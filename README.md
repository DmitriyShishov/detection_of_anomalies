## Задание
  Необходимо разработать алгоритм/алгоритмы фильтрации выбросов, связанных со снижением потребления газа из-за ремонтных работ на примере приложенных к заданию наборов временных рядов по трем ситуациям:

1. Равномерное потребление;

2. Растущее потребление;

3. Сезонное потребление.

## Решение

### Функкция определения пороговых значений
  Оценивать пороговые значения для фильтрации выбросов будем с помощью метода интерквартильного размаха. На вход подается столбец датафрейма, рассчитываются значения первого и третьего квартиля, затем рассчитывается их разность.
После этого определяются пороговые значения для выбросов:
Q1 - 1.5 * IQR для нижней границы;
Q3 + 1.5 * IQR для верхней границы.

Так как по условию нам необходимо отфильтровать выбросы, связанные с приостановкой работ, в данной задаче будем использовать только нижнюю границу.

### Основная функция
1) Определяем окно для анализа пороговых значений;
2) Рассчитываем пороговые значения для выбросов в текущем окне;
3) Проходим каждым значением в этом окне и сравниваем его нижним порогом. Если выше порога, значение пропускается. Если ниже, его индекс заносится в список, а само значение заменяется на медину в окне;
4) Берем следующее окно с перекрытием. Размер окна и процент перекрытия определяется на входе функции.
5) Проходим по полученному списку. Если в нем присутствуют 5 значений слева или справа от текущего, то это значение добавляется в новый список индексов-выбросов. 
Таким образом в новый список не попадают локальные одиночные выбросы.

### Функция редактирования и визуализации
  На вход функции подается путь до csv. файла, размер окна для анализа и процент перекрытия окон. Затем файл преобразуется в датафрейм, пригодный для форматирования в библиотеке pandas. Далее создается новый столбец, который заполняется единицами на тех индексах, которые вышли списком из предыдущей функции. Остальные значения заполняются нулями. Такой подход реализуется для каждого ряда.
  Затем следует визуализация графика с отмеченными исходными и новыми выбросами.
  На выходе функции получаем датасет новыми пометками выбросов.
