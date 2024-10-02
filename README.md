# SECOMPP 2024 - Lua 5.4
[Apresentação](https://docs.google.com/presentation/d/10d6hB7PZQrErfJ_yd9yudUiya_61jO63DfGljCeckw0/edit?usp=sharing)

[Arquivos binários](https://luabinaries.sourceforge.net/download.html)

[Manual de referência da versão 5.4](https://lua.org/manual/5.4/)

```lua
print('Insira um nome de arquivo:');
local name = io.read();
name = name..'.txt'

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
        print(('\'\\%3i\'\t: %i'):format(key:byte(), value));
    else
        print(('\'%s\'\t: %i'):format(key, value));
    end
end
```
