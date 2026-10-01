# SECOMPP 2026
[Apresentação Dia 1](https://docs.google.com/presentation/d/1_ty3S2MjSL1y3AWqb_kZnwaK51FecTfo2a3E4xoJanE/edit?usp=sharing)
[Apresentação Dia 2](https://docs.google.com/presentation/d/1KMhBTjarxXWs_A2CZBXyepU1o5tcl-VafBHIBR5WGsw/edit?usp=sharing)

[Arquivos binários](https://luabinaries.sourceforge.net/download.html)

[Extensão VSCode](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)

```c
#include <stdio.h>
#include "include/lua.h"
#include "include/lauxlib.h"
#include "include/lualib.h"

int soma_dois(lua_State *L) {
    double a = lua_tonumber(L, -1);
    double b = lua_tonumber(L, -2);
    double somado = a+b;
    lua_pushnumber(L, somado);
    return 1;
}

void mostrar_pilha(lua_State *L) {
    int tam_pilha = lua_gettop(L);
    printf("tamanho da pilha: %d\n", tam_pilha);
    printf("Pilha: [\n");
    for(int i = -1; i >= -tam_pilha; i--) {
        int tipo = lua_type(L, i);
        switch (tipo) {
            case LUA_TNUMBER: {
                printf("\t%8s: %lf\n", "number", lua_tonumber(L,i));
                break;
            }
            case LUA_TBOOLEAN: {
                const char *truth_value = lua_toboolean(L,i) ? "true" : "false";
                printf("\t%8s: %s\n", "boolean", truth_value);
                break;
            }
            case LUA_TSTRING: {
                printf("\t%8s: \"%s\"\n", "string", lua_tostring(L,i));
                break;
            }
            case LUA_TTABLE: {
                printf("\t%8s.\n", "table");
                break;
            }
            case LUA_TNIL: {
                printf("\t%8s.\n", "nil");
                break;
            }
            case LUA_TFUNCTION: {
                printf("\t%8s.\n", "function");
                break;
            }
            case LUA_TTHREAD: {
                printf("\t%8s.\n", "thread");
                break;
            }
            default: {
                printf("\tDEFAULTCASE.\n");
                break;
            }
        }
    }
    printf("]\n");
}

void itens_na_pilha(lua_State *L) {

    printf("temos %d itens na pilha.\n", lua_gettop(L));
}

int erro(lua_State *L) {
    printf("Nao eh funcao.\n");
    return 1;
}

int main() {

    lua_State *L = luaL_newstate();
    luaL_openlibs(L);
    if(!lua_checkstack(L, 100)) {
        printf("moiô\n");
        return 1;
    }
    // lua_pushnil(L);
    // lua_pushboolean(L, 0); //false
    // itens_na_pilha(L);
    // lua_pushnumber(L, 404);
    // lua_pushinteger(L, 4041);
    // itens_na_pilha(L);
    // int aaaa = 30;
    // lua_pushfstring(L, "%d\n", aaaa);
    // lua_setglobal(L, "oq_sobra");
    // mostrar_pilha(L);
    // luaL_dofile(L, "teste.lua");
    // mostrar_pilha(L);
    // lua_getglobal(L, "o");
    // int o = lua_tonumber(L, -1);
    // printf("o: %d\n", o);
    // lua_getglobal(L, "carro");
    // const char *carro = lua_tostring(L, -1);
    // int len_carro = lua_strlen(L, -1);
    // printf("carro: %s\ntamanho do carro: %d\n", carro, len_carro);
    // int carroint = lua_tonumber(L, -1);
    // printf("carro int: %d\n", carroint);
    // lua_getglobal(L, "idade");
    // itens_na_pilha(L);
    // int idade = lua_tonumber(L, -1);
    // printf("idade: %d\n", idade);
    // int tipo = lua_type(L, -3);
    // switch(tipo) {
    //     case LUA_TBOOLEAN:
    //         printf("booleano\n");
    //         break;
    //     case LUA_TNUMBER:
    //         printf("number\n");
    //         break;
    //     case LUA_TFUNCTION:
    //         printf("funcao\n");
    //         break;
    //     case LUA_TTABLE:
    //         printf("table\n");
    //         break;
    //     case LUA_TSTRING:
    //         printf("string\n");
    //         break;
    //     case LUA_TNIL:
    //         printf("nil\n");
    //         break;
    //     default:
    //         printf("ué\n");
    //         break;
    // }
    // mostrar_pilha(L);
    // lua_pop(L, 4);
    // itens_na_pilha(L);
    // lua_remove(L, 1);
    // itens_na_pilha(L);
    // mostrar_pilha(L);
    // lua_insert(L, 1);
    // mostrar_pilha(L);
    // lua_insert(L, 2);
    // mostrar_pilha(L);
    // lua_replace(L, 1);
    // mostrar_pilha(L);
    // lua_pushvalue(L, -1);
    // lua_pushvalue(L, 1);
    // mostrar_pilha(L);
    // luaL_dostring(L, "printf(\"sintaxe incorreta\");");
    // mostrar_pilha(L);
    luaL_dofile(L, "teste.lua");
    lua_getglobal(L, "os");
    lua_pcall(L, 0, 1, 0); // chama os.date()
    printf("%s\n", lua_tostring(L, -1));
    lua_getglobal(L, "multiplica_dois");
    lua_pushnumber(L, 2);
    lua_pushstring(L, "4");
    mostrar_pilha(L);
    lua_pcall(L, 2, 2, 0);
    mostrar_pilha(L);
    printf("%d\n%s\n", lua_tointeger(L, -2), lua_tostring(L, -1));
    lua_pushcfunction(L, soma_dois);
    lua_setglobal(L, "soma_dois");
    lua_pop(L, -1);
    luaL_dofile(L, "teste.lua");
    mostrar_pilha(L);
    lua_close(L);
    return 0;
}
```

```c
#include <stdio.h>
#include <string.h>
#include "include/lua.h"
#include "include/lauxlib.h"
#include "include/lualib.h"

int main() {
    lua_State *L = luaL_newstate();
    luaL_openlibs(L);
    char linha[256];
    int msg;
    printf("Interpretador de Lua caseiro 1.0\n");
    while(1) {
        printf("> ");
        fgets(linha, 255, stdin);
        msg = luaL_dostring(L, linha);
        switch(msg) {
            case LUA_OK: //deu certo
                break;
            case LUA_ERRRUN:
                printf("Erro de execução\n");
                break;
            case LUA_ERRSYNTAX:
                printf("Erro de sintaxe\n");
                break;
            case LUA_ERRFILE:
                printf("O arquivo não foi encontrado!\n");
                return 1;
            case LUA_ERRMEM:
                printf("Erro: memória insuficiente!\n");
                return 2;
            default:
                printf("Erro desconhecido.\n");
                break;
        }
        if(!strcmp(linha, "exit\n"))
            break;
    }
    lua_close(L);
    return 0;
}
```

# SECOMPP 2025
[Pasta do projeto](https://drive.google.com/file/d/1dJYh1sfdClNqbaZERplWl1F3GcWO0Uhp/view?usp=sharing)

[Apresentação Lua 5.4](https://docs.google.com/presentation/d/10d6hB7PZQrErfJ_yd9yudUiya_61jO63DfGljCeckw0/edit?usp=sharing)

# Amostra TCC 2024
[Apresentação](https://docs.google.com/presentation/d/1fd-xqXsNJgiyUJzoBC_wH_K_UTlIA_tIHHOtHEegOmo/edit?usp=sharing)

# SECOMPP 2024 - Lua 5.4
[Apresentação](https://docs.google.com/presentation/d/10d6hB7PZQrErfJ_yd9yudUiya_61jO63DfGljCeckw0/edit?usp=sharing)

[Arquivos binários](https://luabinaries.sourceforge.net/download.html)

[Manual de referência da versão 5.4](https://lua.org/manual/5.4/)

```lua
print('Insira um nome de arquivo:');
local name = io.read();
name = name..'.txt';

local f = io.open(name, 'w');
assert(f ~= nil, 'Falha ao abrir');

print('Inicie a escrita:');
local ln = io.read();
while ln ~= '$EXIT' do
    f:write(ln..'\n');
    ln = io.read();
end
f:close();
print('Escrita finalizada');

local freq = {};

f = io.open(name, 'r');
local chr = f:read(1);
while chr ~= nil do
    if freq[chr] == nil then
        freq[chr] = 1;
    else
        freq[chr] = freq[chr] + 1;
    end
    chr = f:read(1);
end

for key, value in pairs(freq) do
    if key:byte() < 32 then
        print(('\'\\%i\'\t: %i'):format(key:byte(), value));
    else
        print(('\'%s\'\t: %i'):format(key, value));
    end
end
```
