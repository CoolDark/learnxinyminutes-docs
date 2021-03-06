---
language: lua
filename: learnlua-ru.lua
contributors:
    - ["Tyler Neylon", "http://tylerneylon.com/"]
translators:
    - ["Max Solomonov", "https://vk.com/solomonovmaksim"]
    - ["Max Truhonin", "https://vk.com/maximmax42"]	
lang: ru-ru
---

```lua
-- Два дефиса начинают однострочный комментарий.

--[[
    Добавление двух квадратных скобок
    делает комментарий многострочным.
--]]
--------------------------------------------------------------------------------
-- 1. Переменные, циклы и условия.
--------------------------------------------------------------------------------

num = 42  -- Все числа являются типом double.
--[[ 
    Не волнуйся, 64-битные double имеют 52 бита
    для хранения именно целочисленных значений;
    точность не является проблемой для
    целочисленных значений, занимающих меньше
    52 бит.
--]]

s = 'walternate'  -- Неизменные строки как в Python.
t = "Двойные кавычки также приветствуются"
u = [[ Двойные квадратные скобки
       начинают и заканчивают
       многострочные значения.]]
t = nil  -- Удаляет определение переменной t; Lua имеет мусорку.

-- Циклы и условия имеют ключевые слова, такие как do/end:
while num < 50 do
  num = num + 1  -- Здесь нет ++ или += операторов.
end

-- Условие "если":
if num > 40 then
  print('больше 40')
elseif s ~= 'walternate' then  -- ~= обозначает "не равно".
  -- Проверка равенства это == как в Python; работает для строк.
  io.write('не больше 40\n')  -- По умолчанию стандартный вывод.
else
  -- По умолчанию переменные являются глобальными.
  thisIsGlobal = 5  -- Стиль CamelСase является общим.

  -- Как сделать локальную переменную:
  local line = io.read()  -- Считывает введённую строку.

  -- Для конкатенации строк используется оператор .. :
  print('Зима пришла, ' .. line)
end

-- Неопределённые переменные возвращают nil.
-- Этот пример не является ошибочным:
foo = anUnknownVariable  -- Теперь foo = nil.

aBoolValue = false

-- Только значения nil и false являются ложными; 0 и '' являются истинными!
if not aBoolValue then print('это значение ложно') end

--[[
     Для 'or' и 'and' действует принцип "какой оператор дальше, 
     тот и применяется". Это действует аналогично a?b:c 
     операторам в C/js:
--]]
ans = aBoolValue and 'yes' or 'no'  --> 'no'

karlSum = 0
for i = 1, 100 do  -- Здесь указан диапазон, ограниченный с двух сторон.
  karlSum = karlSum + i
end

-- Используйте "100, 1, -1" как нисходящий диапазон:
fredSum = 0
for j = 100, 1, -1 do fredSum = fredSum + j end

-- В основном, диапазон устроен так: начало, конец[, шаг].

-- Другая конструкция цикла:
repeat
  print('путь будущего')
  num = num - 1
until num == 0

--------------------------------------------------------------------------------
-- 2. Функции.
--------------------------------------------------------------------------------

function fib(n)
  if n < 2 then return n end
  return fib(n - 2) + fib(n - 1)
end

-- Вложенные и анонимные функции являются нормой:
function adder(x)
  -- Возращаемая функция создаётся когда adder вызывается, тот в свою очередь
  -- запоминает значение переменной x:
  return function (y) return x + y end
end
a1 = adder(9)
a2 = adder(36)
print(a1(16))  --> 25
print(a2(64))  --> 100

-- Возвраты, вызовы функций и присвоения, вся работа с перечисленным может иметь 
-- неодинаковое кол-во аргументов/элементов. Неиспользуемые аргументы являются nil и 
-- отбрасываются на приёме.

x, y, z = 1, 2, 3, 4
-- Теперь x = 1, y = 2, z = 3, и 4 просто отбрасывается.

function bar(a, b, c)
  print(a, b, c)
  return 4, 8, 15, 16, 23, 42
end

x, y = bar('zaphod')  --> выводит "zaphod  nil nil"
-- Теперь x = 4, y = 8, а значения 15..42 отбрасываются.

-- Функции могут быть локальными и глобальными. Эти строки делают одно и то же:
function f(x) return x * x end
f = function (x) return x * x end

-- Эти тоже:
local function g(x) return math.sin(x) end
local g = function(x) return math.sin(x) end
-- Эквивалентно для local function g(x)..., кроме ссылки на g в теле функции
-- не будет работать как ожидалось.
local g; g  = function (x) return math.sin(x) end
-- 'local g' будет прототипом функции.

-- Так же тригонометрические функции работсют с радианами.

-- Вызов функции с одним текстовым параметром не требует круглых скобок:
print 'hello'  -- Работает без ошибок.

-- Вызов функции с одним табличным параметром так же не требуют круглых скобок (про таблицы в след.части):
print {} -- Тоже сработает.

--------------------------------------------------------------------------------
-- 3. Таблицы.
--------------------------------------------------------------------------------

-- Таблицы = структура данных, свойственная только для Lua; это ассоциативные массивы.
-- Похоже на массивы в PHP или объекты в JS
-- Так же может использоваться как список.


-- Использование словарей:

-- Литералы имеют ключ по умолчанию:
t = {key1 = 'value1', key2 = false}

-- Строковые ключи выглядят как точечная нотация в JS:
print(t.key1)  -- Печатает 'value1'.
t.newKey = {}  -- Добавляет новую пару ключ-значение.
t.key2 = nil   -- Удаляет key2 из таблицы.

-- Литеральная нотация для любого (не пустой) значения ключа:
u = {['@!#'] = 'qbert', [{}] = 1729, [6.28] = 'tau'}
print(u[6.28])  -- пишет "tau"

-- Ключ соответствует нужен не только для значения чисел и строк, но и для
-- идентификации таблиц.
a = u['@!#']  -- Теперь a = 'qbert'.
b = u[{}]     -- Мы ожидали 1729, но получили nil:
-- b = nil вышла неудача. Потому что за ключ мы использовали
-- не тот же объект, который использовали в оригинальном значении.
-- Поэтому строки и числа больше подходят под ключ.

-- Вызов фукцнии с одной таблицей в качестве аргумента
-- не нуждается в кавычках:
function h(x) print(x.key1) end
h{key1 = 'Sonmi~451'}  -- Печатает 'Sonmi~451'.

for key, val in pairs(u) do  -- Итерация цикла с таблицей.
  print(key, val)
end

-- _G - это таблица со всеми глобалями.
print(_G['_G'] == _G)  -- Печатает 'true'.

-- Использование таблиц как списков / массивов:

-- Список значений с неявно заданными целочисленными ключами:
v = {'value1', 'value2', 1.21, 'gigawatts'}
for i = 1, #v do  -- #v это размер списка v.
  print(v[i])  -- Начинается с ОДНОГО!
end

-- Список это таблица. v Это таблица с последовательными целочисленными
-- ключами, созданными в списке.

--------------------------------------------------------------------------------
-- 3.1 Мета-таблицы и мета-методы.
--------------------------------------------------------------------------------

-- Таблицы могут быть метатаблицами, что дает им поведение
-- перегрузки-оператора. Позже мы увидим, что метатаблицы поддерживают поведение
-- js-прототипов.
f1 = {a = 1, b = 2}  -- Представляет фракцию a/b.
f2 = {a = 2, b = 3}

-- Это не сработает:
-- s = f1 + f2

metafraction = {}
function metafraction.__add(f1, f2)
  local sum = {}
  sum.b = f1.b * f2.b
  sum.a = f1.a * f2.b + f2.a * f1.b
  return sum
end

setmetatable(f1, metafraction)
setmetatable(f2, metafraction)

s = f1 + f2  -- вызывает __add(f1, f2) на мета-таблице f1

-- f1, f2 не имеют ключей для своих метатаблиц в отличии от прототипов в js, поэтому
-- ты можешь извлечь данные через getmetatable(f1). Метатаблицы это обычные таблицы с 
-- ключем, который в Lua известен как __add.

-- Но следущая строка будет ошибочной т.к s не мета-таблица:
-- t = s + s
-- Шаблоны классов приведенные ниже смогут это исправить.

-- __index перегружет в мета-таблице просмотр через точку:
defaultFavs = {animal = 'gru', food = 'donuts'}
myFavs = {food = 'pizza'}
setmetatable(myFavs, {__index = defaultFavs})
eatenBy = myFavs.animal  -- работает! спасибо, мета-таблица.

--------------------------------------------------------------------------------
-- Прямой табличный поиск не будет пытаться передавать с помощью __index
-- значения, и её рекурсии.

-- __index значения так же могут быть function(tbl, key) для настроенного
-- просмотра.

-- Значения типа __index,add, ... называются метаметодами.
-- Полный список. Здесь таблицы с метаметодами.

-- __add(a, b)                     для a + b
-- __sub(a, b)                     для a - b
-- __mul(a, b)                     для a * b
-- __div(a, b)                     для a / b
-- __mod(a, b)                     для a % b
-- __pow(a, b)                     для a ^ b
-- __unm(a)                        для -a
-- __concat(a, b)                  для a .. b
-- __len(a)                        для #a
-- __eq(a, b)                      для a == b
-- __lt(a, b)                      для a < b
-- __le(a, b)                      для a <= b
-- __index(a, b)  <fn or a table>  для a.b
-- __newindex(a, b, c)             для a.b = c
-- __call(a, ...)                  для a(...)

--------------------------------------------------------------------------------
-- 3.2 Классы и наследования.
--------------------------------------------------------------------------------

-- В Lua нет поддержки классов на уровне языка;
-- Однако существуют разные способы их создания с помощью
-- таблиц и метатаблиц.

-- Пример классам находится ниже.

Dog = {}                                   -- 1.

function Dog:new()                         -- 2.
  local newObj = {sound = 'woof'}          -- 3.
  self.__index = self                      -- 4.
  return setmetatable(newObj, self)        -- 5.
end

function Dog:makeSound()                   -- 6.
  print('I say ' .. self.sound)
end

mrDog = Dog:new()                          -- 7.
mrDog:makeSound()  -- 'I say woof'         -- 8.

-- 1. Dog похоже на класс; но это таблица.
-- 2. "function tablename:fn(...)" как и 
--    "function tablename.fn(self, ...)", Просто : добавляет первый аргумент
--    перед собой. Читай 7 и 8 чтоб понять как self получает значение.
-- 3. newObj это экземпляр класса Dog.
-- 4. "self" есть класс являющийся экземпляром. Зачастую self = Dog, но экземляр
--    может поменять это. newObj получит свои функции, когда мы установим newObj как
--    метатаблицу и __index на себя.
-- 5. Помни: setmetatable возвращает первый аргумент.
-- 6. ":" Работает в 2 стороны, но в этот раз мы ожидмаем, что self будет экземпляром
--    а не классом.
-- 7. Dog.new(Dog), тоже самое что self = Dog in new().
-- 8. mrDog.makeSound(mrDog) будет self = mrDog.
--------------------------------------------------------------------------------

-- Пример наследования:

LoudDog = Dog:new()                           -- 1.

function LoudDog:makeSound()
  local s = self.sound .. ' '                 -- 2.
  print(s .. s .. s)
end

seymour = LoudDog:new()                       -- 3.
seymour:makeSound()  -- 'woof woof woof'      -- 4.

--------------------------------------------------------------------------------
-- 1. LoudDog получит методы и переменные класса Dog.
-- 2. self будет 'sound' ключ для new(), смотри 3й пункт.
-- 3. Так же как "LoudDog.new(LoudDog)" и переделанный в "Dog.new(LoudDog)"
--    LoudDog не имеет ключ 'new', но может выполнить "__index = Dog" в этой метатаблице
--    Результат: Метатаблица seymour стала LoudDog и "LoudDog.__index = Dog"
--    Так же seymour.key будет равна seymour.key, LoudDog.key, Dog.key, 
--    в зависимости от того какая таблица будет с первым ключем.
-- 4. 'makeSound' ключ найден в LoudDog; и выглдяит как "LoudDog.makeSound(seymour)".

-- При необходимости, подкласс new() будет базовым.
function LoudDog:new()
  local newObj = {}
  -- set up newObj
  self.__index = self
  return setmetatable(newObj, self)
end

--------------------------------------------------------------------------------
-- 4. Модули.
--------------------------------------------------------------------------------


--[[ Я закомментировал этот раздел так как часть скрипта остается 
--   работоспособной.
```

```lua
-- Предположим файл mod.lua будет выглядеть так:
local M = {}

local function sayMyName()
  print('Hrunkner')
end

function M.sayHello()
  print('Why hello there')
  sayMyName()
end

return M

-- Иные файлы могут использовать функционал mod.lua:
local mod = require('mod')  -- Запустим файл mod.lua.

-- require - подключает модули.
-- require выглядит так:     (если не кешируется; смотри ниже)
local mod = (function ()
  <contents of mod.lua>
end)()
-- Тело функции mod.lua является локальным, поэтому
-- содержимое не видимо за телом функции.

-- Это работает так как mod здесь = M в mod.lua:
mod.sayHello()  -- Скажет слово Hrunkner.

-- Это будет ошибочным; sayMyName доступен только в mod.lua:
mod.sayMyName()  -- ошибка

-- require возвращает значения кеша файла вызванного не более одного раза, даже когда
-- требуется много раз.

-- Предположим mod2.lua содержит "print('Hi!')".
local a = require('mod2')  -- Напишет Hi!
local b = require('mod2')  -- Не напишет; a=b.

-- dofile работает без кэша:
dofile('mod2')  --> Hi!
dofile('mod2')  --> Hi! (напишет снова)

-- loadfile загружает lua файл, но не запускает его.
f = loadfile('mod2')  -- Вызовет f() запустит mod2.lua.

-- loadstring это loadfile для строк.
g = loadstring('print(343)')  -- Вернет функцию.
g()  -- Напишет 343.

--]]

```
## Примечание (от автора)

Я был взволнован, когда узнал что с Lua я могу делать игры при помощи <a href="http://love2d.org/">Love 2D game engine</a>. Вот почему.

Я начинал с <a href="http://nova-fusion.com/2012/08/27/lua-for-programmers-part-1/">BlackBulletIV's Lua for programmers</a>.
Затем я прочитал официальную <a href="http://www.lua.org/pil/contents.html">Документацию по Lua</a>.

Так же может быть полезным <a href="http://lua-users.org/files/wiki_insecure/users/thomasl/luarefv51.pdf">Lua short
reference</a> на lua-users.org.

Основные темы не охваченные стандартной библиотекой:

* <a href="http://lua-users.org/wiki/StringLibraryTutorial">string library</a>
* <a href="http://lua-users.org/wiki/TableLibraryTutorial">table library</a>
* <a href="http://lua-users.org/wiki/MathLibraryTutorial">math library</a>
* <a href="http://lua-users.org/wiki/IoLibraryTutorial">io library</a>
* <a href="http://lua-users.org/wiki/OsLibraryTutorial">os library</a>

Весь файл написан на Lua; сохрани его как learn.lua и запусти при помощи "lua learn.lua" !

Это была моя первая статья для tylerneylon.com, которая так же доступна тут <a href="https://gist.github.com/tylerneylon/5853042">github gist</a>.

Удачи с Lua!

