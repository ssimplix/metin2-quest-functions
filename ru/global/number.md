# number()
Функция **number** выбирает случайное целое число в указанном диапазоне.

## Параметры функции
### min_value
Тип *number*. **Обязательный параметр**. Минимальное значение.

### max_value
Тип *number*. **Обязательный параметр**. Максимальное значение.

## Возвращаемые значения
### rand
Тип *number*. Случайное целое число в указанном диапазоне. Если один из параметров не указан или не является числом, то возвращется `0`.

## Примечания
Функция может быть вызвана анонимно.

Функция аналогична встроенной в Lua функции [math.random](../math/math.random.md)().