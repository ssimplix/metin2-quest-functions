# d.purge_area()
Функция **d.purge_area** удаляет всех монстров и NPC из заданной прямоугольной области.

## Параметры функции
### x1
Тип *number*. **Обязательный параметр**. Серверная координата без двух нулей на конце.

### y1
Тип *number*. **Обязательный параметр**. Серверная координата без двух нулей на конце.

### x2
Тип *number*. **Обязательный параметр**. Серверная координата без двух нулей на конце.

### y2
Тип *number*. **Обязательный параметр**. Серверная координата без двух нулей на конце.

## Примечания
Неизвестно, может ли эта функция быть вызвана анонимно.

4 параметра формируют прямоугольную область: [x1](#x1) и [y1](#y1) &mdash; левый верхний угол, а [x2](#x2) и [y2](#y2) &mdash; правый нижний.

Возможно, что питомцы не удаляются.

Существует аналогичная функция, работающая вне подземелья &mdash; [purge_area](../global/purge_area.md)().

Эта функция работает только в подземельях.