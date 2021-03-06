# d.spawn_group()
Функция **d.spawn_group** призывает группу монстров в подземелье.

## Параметры функции
### group_vnum
Тип *number*. **Обязательный параметр**. ID группы монстров из файла `group.txt`.

### x
Тип *number*. **Обязательный параметр**. Локальная координата по оси X без двух нулей на конце, куда будет призвана группа монстров.

### y
Тип *number*. **Обязательный параметр**. Локальная координата по оси Y без двух нулей на конце, куда будет призвана группа монстров.

### radius
Тип *number*. **Обязательный параметр**. Допустимая погрешность в координатах при появлении группы монстров. Если параметр равен, допустим, 15, то координаты появления группы монстров будут равны ([x](#x) &plusmn; 15, [y](#y) &plusmn; 15); тоже самое в виде квеста:

````lua
local group_vnum = 101
local x = pc.get_local_x()
local y = pc.get_local_y()
local radius = 15
local count = 3
d.spawn_group(
	group_vnum,
	x + number(radius * -1, radius),
	y + number(radius * -1, radius),
	0,
	true,
	count
)
````

### agressive
Тип *boolean*. **Обязательный параметр**. Агрессивны ли будут монстры или нет.

### count
Тип *number*. **Обязательный параметр**. Количество призываемых групп.

## Возвращаемые значения
### mob_vid
Тип *number*. VID лидера одной из групп.

## Примечания
Функция может быть вызвана анонимно.

Если было вызвано несколько групп, возвращается VID только лидера одной из них.

Эта функция работает только в подземельях.