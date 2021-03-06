# Функции в Lua и квестах
Функции позволяют выполнять определенный набор или последовательность действий. Основное назначение встроенных функций &mdash; это совершение уникальных действий, а назначение самопальных функций &mdash; возможность сократить количество кода, пакуя повторящийся код в функции.

## Встроенные функции
Основное назначение квестов &mdash; это использование квестовых функций. Квестовые функции вшиты в ядро сервера и могут сообщать ту или иную информацию. Например, функция [pc.get_name](../pc/pc.get_name.md)() сообщает имя игрока:

````lua
quest example begin
	state start begin
	
		--[[
			Если ник игрока "terron", то представить в своей голове данную строку можно так:

			when login with "terron" == "terron" begin

			Вместо функции как бы подставляется значение, которое она сообщает.
		]]
		
		when login with pc.get_name() == "terron" begin
			syschat("Привет, terron! ")
		end
	end
end
````

На самом деле, правильнее говорить не &laquo;сообщают&raquo;, а &laquo;возвращают&raquo;, поэтому дальше мы будем придерживаться именно этого термина.

Встроенные функции могут возвращать данные любого типа. Вот несколько примеров:

````lua
quest example begin
	state start begin
		when login begin
			syschat("Привет, " .. pc.get_name() .. "! ")
			syschat("Ваш уровень - " .. pc.get_level()) -- сообщает уровень игрока (тип number)
			syschat("Ваш баланс - " .. pc.get_gold() .. " янг. ") -- сообщает количество янг игрока (тип number)

			if pc.is_gm() then -- сообщает, является ли игрок администратором (тип boolean)
				syschat("Хорошего рабочего дня, уважаемый администратор! ")
			else
				syschat("Отличного фарма и удачи, уважаемый игрок! ")
			end
		end
	end
end
````

Также с помощью квестовых функций можно менять информацию об игроке. В данном примере мы при каждом входе в игру функцией [pc.change_gold](../pc/pc.change_gold.md)() будем увеличивать баланс янг игрока на 500:

````lua
quest example begin
	state start begin
		when login begin
			local gold = 500

			syschat("Привет, " .. pc.get_name() .. "! Вам было начислено " .. gold .. " за то, что вы крутой! ")

			pc.change_gold(gold)
		end
	end
end
````

Т.к. функция [pc.change_gold](../pc/pc.change_gold.md)() ничего не возвращает, то бишь возвращает `nil` (вы не забыли, что `nil` &mdash; это &laquo;ничего&raquo; или просто &laquo;пустота&raquo;?), то заключать ее в переменную или объединять с какой-либо строкой смысла нет. Она просто меняет баланс игрока и всё.

Мы передали в данную функцию число `500` как параметр. Функции могут принимать так называемые параметры (также известны как &laquo;аргументы функции&raquo;). Параметры заключаются в скобки, идущие после названия функции. Если параметров несколько, то они разделяются запятыми:

````lua
quest example begin
	state start begin
		when login begin		
			syschat("Привет, " .. pc.get_name() .. "! Вы получили 5 красных зелий за то, что вы крутой! ")

			--[[
				27001 - это vnum красного зелья
				5 - это количество
			]]

			pc.give_item2(27001, 5) -- данная функция выдает игроку определенный предмет в определенном количестве
		end
	end
end
````

Некоторые функции возвращают не одно, а несколько значений. Это одна из особенностей языка Lua. Правда, подобных встроенных функций очень мало. Например, функция [pc.get_start_location](../pc/pc.get_start_location.md)() сообщает индекс локации и координаты площади первого города игрока в зависимости от его империи. Работать с такими функциями нужно так:

````lua
quest example begin
	state start begin
		when login begin
			local index, coordinate_x, coordinate_y = pc.get_start_location()

			syschat("Индекс вашего первого города: " .. index)
			syschat("Координата по оси X: " .. coordinate_x)
			syschat("Координата по оси Y: " .. coordinate_y)
		end
	end
end
````

Подробнее о том, сколько значений и вообще что возвращает функция, можно узнать из статьи о ней &mdash; в этом репозитории задокументировано большинство квестовых функций.

## Самопальные функции
Вы можете самостоятельно создавать функции в Lua, например, чтобы не писать один и тот же код по несколько раз. Самый простой способ создать функцию:

````lua
quest example begin
	state start begin
		function calculate_ab(a, b)
			return a + b
		end

		when login begin
			local beer, vodka = 500, 800
			syschat(example.calculate_ab(beer, vodka)) --> 1300
		end
	end
end
````

Правила именования функций такие же, как у переменных, ведь функции тоже являются переменными, просто вместо числел и строк они хранят в себе тип данных `function`. Внутри квестов функции могут задаваться только внутри `state`.

В примере выше мы задали функцию внутри `state`. Когда функция задается внутри `state`, то при ее вызове обязательно надо указывать перед ней название квеста (`example.calculate_ab()`). Область видимости у функций ограничивается всем квестом, независимо от того, в какой стадии была задана функция:

````lua
--[[
	При каждом входе в игру игрок будет видеть разные сообщения:
]]

quest example begin
	state start begin
		function calculate_ab(a, b)
			return a + b
		end

		when login begin	
			syschat("Start: " .. example.calculate_ab(1, 2)) --> Start 3

			set_state("test")
		end
	end

	state test begin
		when login begin	
			syschat("Test: " .. example.calculate_ab(3, 4)) --> Test 7

			set_state("start")
		end
	end
end
````

Функции создаются при помощи ключевого слова `function`. После ключевого слова идет название функции и в скобках заключаются параметры функции. Параметры указываются как переменные и область видимости у этих переменных будет локальной только внутри функции. Наша функция `calculate_ab()` принимает 2 числа и считает их, а затем возвращает значение ключевым словом `return`. Код, идущий внутри секции ниже `return`, уже не исполняется:

````lua
quest example begin
	state start begin
		function calculate_ab(a, b)
			local val = a + b

			return val

			syschat("Посчитали и вот: " .. val) -- данное сообщение мы не увидим, т.к. выполнение секции закончилось на return
		end

		when login begin		
			local beer, vodka = 500, 800
			syschat(example.calculate_ab(beer, vodka)) --> 1300
		end
	end
end
````

Если не передавать в `return` каких-либо значений, то функция ничего не вернет, то бишь `nil`:

````lua
quest example begin
	state start begin
		function calculate_ab(a, b)
			local val = a + b

			syschat("Посчитали и вот: " .. val)

			return

			syschat("Второе сообщение... ") -- это сообщение мы не увидим
		end

		when login begin		
			local beer, vodka = 500, 800

			local how_much = example.calculate_ab(beer, vodka) -- в этот момент появится сообщение "Посчитали и вот: 1300", так как в этот момент исполняется функция calculate_ab()

			if how_much then
				syschat(how_much) -- мы не увидим это сообщение т.к. how_much == nil
			end
		end
	end
end
````

В отличие от оригинального Lua, внутри квестов динамически создавать функции через переменные нельзя. Зато функции можно создавать глобально. Глобальные функции можно будет использовать во всех квестах, а не только в том, в котором она была создана. Такие функции создаются в файле `questlib.lua`. Особенность этого файла в том, что код, находящийся в нем, исполняется только один раз во время запуска сервера. То есть мы объявляем в этом файле функцию один раз, файл запускается сервером один раз и на время работы сервера он будет помнить о том, что существуют такие-то функции и переменные, заданные в `questlib.lua`. Добавьте в самый конец файла нашу функцию:

````lua
function calculate_ab(a, b)
	local val = a + b

	return val
end
````

Также функции в `questlib.lua` можно задавать как переменные (а в самих квестах нельзя!). Результат данного примера будет идентичен примеру выше:

````lua
local calculate_ab = function(a, b)
	local val = a + b

	return val
end
````

И чтобы функция начала работать в квестах, ее необходимо добавить в файл `quest_functions` (без расширения). Просто в конец файла добавьте `calculate_ab` и всё. Не забудьте про то, что последние строки в файлах должны быть пустыми! Теперь нашу функцию можно использовать в квестах:

````lua
quest example begin
	state start begin
		when login begin		
			local beer, vodka = 500, 800

			syschat(calculate_ab(beer, vodka)) --> 1300
		end
	end
end
````

Обратите внимание на то, что при использовании функций, заданных глобально, использовать приставку в виде названия квеста не нужно.