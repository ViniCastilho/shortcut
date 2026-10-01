# SECOMPP 2026
[Apresentação Dia 1](https://docs.google.com/presentation/d/1_ty3S2MjSL1y3AWqb_kZnwaK51FecTfo2a3E4xoJanE/edit?usp=sharing)
[Apresentação Dia 2](https://docs.google.com/presentation/d/1KMhBTjarxXWs_A2CZBXyepU1o5tcl-VafBHIBR5WGsw/edit?usp=sharing)

[Arquivos binários](https://luabinaries.sourceforge.net/download.html)

[Extensão VSCode](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)

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
