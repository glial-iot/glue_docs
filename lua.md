# Язык Lua

Драйвера и скрипты в Glue пишутся на языке Lua. 

## О языке

Lua (лу́а, с порт. — «луна») — скриптовый язык программирования, разработанный в подразделении Tecgraf (Computer Graphics Technology Group) Католического университета Рио-де-Жанейро (Бразилия). Интерпретатор языка является свободно распространяемым, с открытыми исходными текстами на языке Си.  

По идеологии и реализации язык Lua ближе всего к JavaScript, в частности, он также реализует прототипную модель ООП, но отличается Паскале-подобным синтаксисом и более мощными и гибкими конструкциями. Характерной особенностью Lua является реализация большого числа программных сущностей минимумом синтаксических средств. Так, все составные пользовательские типы данных (массивы, структуры, множества, очереди, списки) реализуются через механизм таблиц, а механизмы объектно-ориентированного программирования, включая множественное наследование — с использованием метатаблиц, которые также отвечают за перегрузку операций и ряд других возможностей.  


Ниже представлены некоторые ссылки, которые помогут вам познакомиться с языком:  
[Wikipeadia: Lua](https://ru.wikipedia.org/wiki/Lua)  
[Lua за 60 минут ](https://zserge.wordpress.com/2012/02/23/lua-%D0%B7%D0%B0-60-%D0%BC%D0%B8%D0%BD%D1%83%D1%82/)  
[Программирование на языке Lua](http://f.aui.su/data/uploads/lua-2015.pdf)(книга от автора языка)  

Ниже находится краткое введение в язык. Текст основан на [этом](https://github.com/adambard/learnxinyminutes-docs/blob/master/ru-ru/lua-ru.html.markdown) источнике, все права принадлежат его авторам.  

Версия языка, используемая в Glue — 5.1.  


## Краткий курс Lua

Два дефиса начинают однострочный комментарий:
```lua
-- Комментарий
```

Добавление двух квадратных скобок делает комментарий многострочным:
```lua
--[[
    Многострочный
    комментарий
--]]
```

Можно быстро расскомментировать этот кусок кода добавлением к нему еще одного тире
```lua
---[[
    Многострочный
    не комментарий
--]]
```
### Переменные

В основнов, в Lua вам придется пользоваться 6 типами переменных : nil, boolean, number, string, table, function. 
```lua
num = 42 --Все числа имеют тип double. 
s = 'walternate'  -- Неизменные строки, как в Python.
t = "Двойные кавычки также приветствуются"
u = [[ Двойные квадратные скобки
       начинают и заканчивают
       многострочные значения.]]
```

Функция type возвращает имя типа любого заданного значения:
```lua
print(type("Hello world")) --> string
print(type(10.4*3)) --> number
print(type(print)) --> function
```

Присвоение переменно значениея nil удаляет определение переменной: 
```lua
t = nil  
```
Тип nil — это тип с единственным значением, nil, основная задача которого состоит в том, чтобы отличаться от всех остальных значений.  
Lua использует nil как нечто, не являющееся значением, чтобы изобразить отсутствие подходящего значения.  

В Lua есть сборка мусора.

### Условия

Блоки обозначаются ключевыми словами, такими как do/end:
```lua
while num < 50 do
  num = num + 1  -- Операторов ++ и += нет.
end
```

Ветвление "если":
```lua
if num > 40 then
  print('больше 40')
elseif s ~= 'walternate' then  -- "~=" обозначает "не равно".
  -- Проверка равенства это "==", как в Python; работает для строк.
else
  -- По умолчанию переменные являются глобальными. Как сделать переменную локальной:
  local line = "username"

  -- Для конкатенации строк используется оператор .. :
  print('Зима пришла, ' .. line)
end
```

Неопределённые переменные возвращают nil. Этот пример не является ошибочным:
```lua
foo = anUnknownVariable  -- Теперь foo = nil.
```

Только значения nil и false являются ложными; 0 и '' являются истинными!
```lua
aBoolValue = false

if not aBoolValue then print('это значение ложно') end
```

Для 'or' и 'and' действует принцип "какой оператор дальше, тот и применяется". Это действует аналогично оператору a?b:c в C/js:
```lua
ans = aBoolValue and 'yes' or 'no'  --> 'no'
```

### Циклы
Цикл for:
```lua
karlSum = 0
for i = 1, 100 do  -- Здесь указан диапазон, ограниченный с двух сторон.
  karlSum = karlSum + i
end
```
Используйте "100, 1, -1" как нисходящий диапазон. В основном, диапазон устроен так: начало, конец[, шаг].
```lua
fredSum = 0
for j = 100, 1, -1 do fredSum = fredSum + j end
```

Другая конструкция цикла:
```lua
repeat
  print('путь будущего')
  num = num - 1
until num == 0
```

### Функции

```lua
function fib(n)
  if n < 2 then return n end
  return fib(n - 2) + fib(n - 1)
end
```

Вложенные и анонимные функции являются нормой:
```lua
function adder(x)
  -- Возращаемая функция создаётся, когда вызывается функция adder,
  -- и запоминает значение переменной x:
  return function (y) return x + y end
end
a1 = adder(9)
a2 = adder(36)
print(a1(16))  --> 25
print(a2(64))  --> 100
```

Возвраты, вызовы функций и присвоения работают со списками, которые могут иметь разную длину.  
Лишние получатели принимают значение nil, а лишние значения игнорируются.
```lua
x, y, z = 1, 2, 3, 4
-- Теперь x = 1, y = 2, z = 3, а 4 просто отбрасывается.

function bar(a, b, c)
  print(a, b, c)
  return 4, 8, 15, 16, 23, 42
end

x, y = bar('zaphod')  --> выводит "zaphod  nil nil"
-- Теперь x = 4, y = 8, а значения 15..42 отбрасываются.


a, b = b, a -- Так можно менять значения переменных местами
```

Функции могут быть локальными и глобальными. Эти строки делают одно и то же:
```lua
function f(x) return x * x end
f = function (x) return x * x end
```
Эти тоже:
```lua
local function g(x) return math.sin(x) end
local g = function(x) return math.sin(x) end
```
Эквивалентно для local function g(x)..., однако ссылки на g в теле функции не будут работать, как ожидалось:
```lua
local g -- 'local g' будет прототипом функции.
g  = function (x) return math.sin(x) end

```

Вызов функции с одним строковым параметром не требует круглых скобок:
```lua
print 'hello'  -- Работает без ошибок.
```

Вызов функции с одним табличным параметром также не требует круглых скобок (про таблицы в след. части):
```lua
print {} -- Тоже сработает.
```

### Таблицы
Таблица — единственная составная структура данных в Lua, представляет собой ассоциативный массив.  
Подобно массивам в PHP или объектам в JS, они представляют собой хеш-таблицы, которые также можно использовать в качестве списков.

Использование словарей:
```lua
t = {key1 = 'value1', key2 = false} -- Литералы имеют ключ по умолчанию
```

Строковые ключи используются, как в точечной нотации в JS:
```lua
print(t.key1)  -- Печатает 'value1'.
t.newKey = {}  -- Добавляет новую пару ключ/значение.
t.key2 = nil   -- Удаляет key2 из таблицы.
```

Литеральная нотация для любого значения ключа (кроме nil):
```lua
-- 
u = {['@!#'] = 'qbert', [{}] = 1729, [6.28] = 'tau'}
print(u[6.28])  -- пишет "tau"
```

Ключ соответствует значению для чисел и строк, но при использовании таблицы в качестве ключа берётся её экземпляр.
```lua
a = u['@!#']  -- Теперь a = 'qbert'.
b = u[{}]     -- Вы могли ожидать 1729, но получится nil:
-- b = nil, т.к. ключ не будет найден.
-- Это произойдёт потому, что за ключ мы использовали не тот же самый объект,
-- который был использован для сохранения оригинального значения.
-- Поэтому строки и числа удобнее использовать в качестве ключей.
```

Вызов функции с одной таблицей в качестве аргумента не требует круглых скобок:
```lua
function h(x) print(x.key1) end
h{key1 = 'Sonmi~451'}  -- Печатает 'Sonmi~451'.

for key, val in pairs(u) do  -- Цикл по таблице.
  print(key, val)
end
```

Использование таблиц, как списков / массивов:
```lua
v = {'value1', 'value2', 1.21, 'gigawatts'} -- Список значений с неявно заданными целочисленными ключами
for i = 1, #v do  -- #v - размер списка v.
  print(v[i])  -- Нумерация начинается с 1!
end
```
Список не является отдельным типом. "v" - всего лишь таблица с последовательными целочисленными ключами, воспринимаемая как список.

### Шаблоны
В Lua есть поддержка регулярных выражений, которые называются шаблонами. Они используются для поиска в функциях `string.find`, `string.gfind`, `string.match`,`string.gmatch` и `string.gsub`  
`%a` — латинская буква `[A-Za-z]`  
`%с` — контрольные символы  
`%d` — десятичная цифра `[0-9] [0123456789]`  
`%u` — латинская буква верхнего регистра `[A-Z]`  
`%l` — латинская буква нижнего регистра `[a-z]`  
`%p` — знак пунктуации  
`%s` — символ пробела  
`%w` — латинская буква или арабская цифра `[A-Za-z0-9]`  
`%z` — нулевой символ  

`%A` — не латинская буква `[^A-Za-z]`  
`%C` — не контрольные символы  
`%D` — не десятичная цифра `[^%d] [^0-9]`  
`%U` — не латинская буква верхнего регистра `[^A-Z]`  
`%L` — не латинская буква нижнего регистра `[^a-z]`  
`%P` — не знак пунктуации  
`%S` — не символ пробела  
`%W` — не латинская буква и не арабская цифра `[^A-Za-z0-9]`  
`%Z` — не нулевой символ `[^%z]`

`%b` — сбалансированная строка: `%b()` = "(text)", `%b%{%}` = "{text}"  
`.` — любой символ (точка — `%.`)  

Применение квантификаторов `+`/`-`/`*`/`?`:  
`%s` — 1 символ пробела  
`%s+` — 1 или более символов пробела (жадный)  
`%s-` — 0 или более символов пробела (ленивый)  
`%s*` — 0 или более символов пробела (жадный)  
`%s?` — необязательное наличие символа пробела  

`^` — начало строки, `$` — конец строки, `()` — захват  

Символы, которые должны экранироваться %: `( ) . % + - * ? [ ^ $`


### Захваты
Захват — это способ выделить подстроку из строки, пользуясь шаблонами с помощью `string.find`. То, что надо поместить в переменную, ограничивается скобочками. Первые два параметра, возвращаемые `string.find` — начальная и конечная позиция найденных подстрок, следующий — сами подстроки. В данном случае строка, содержащая где-то внутри себя текст "`similarity = 60%`" будет разбита таким образом, что в переменной `similarity` окажется числовое значение из одной или нескольких (жадный квантификатор `+`) цифр (шаблон `%d`), находящихся между строками "`similarity = `" (шаблон `similarity%s=%s`, где `%s` это символ пробоела) и "`%`" (шаблон `%%`, где `%` — экранирование следующего за ним служебного символа). 
```lua
local string = "person id = 0 ; similarity = 60% ; cam_id = 1"
local start_pos, end_pos, similarity = string.find(string, "similarity%s=%s(%d+)%%")
print(similarity) -- 60
```

