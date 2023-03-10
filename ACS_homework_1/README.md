# ABC_IDZ_1
Индивидуальное домашнее задание №1 по АВС

# Отчет
## задание на 4 и 5 баллов:
### _Опции компляции:_
Компиляция программы без оптимизирующих и отладочных опций:
```sh
gcc -S -masm=intel foo1.c
```
Компиляция программы с доп. опциями с целью убрать лишние макросы:
```sh
gcc -masm=intel -fno-asynchronous-unwind-tables -fno-jump-tables -fno-stack-protector -fno-exceptions foo1.c -S -o foo1.s
```

Компиляция и компоновка ассемблерной программы без использования опций отладки:
```sh
as -o foo1.o foo1.s
gcc foo1.o
```

Необходимые файлы предствлены в папке 4_and_5_points.

### _Тестовые прогоны:_
Скриншот запуска тестов на обоих программах:
![Тесты на оценку 4 и 5](tests/tests_4_5.png)


## задание на 6 баллов:
Сделано максимальное использование регистров вместо ОЗУ для хранения лок. переменных (и аргументов, которые требуются для вычисления функции). Комментарии предоставлены в файле foo1.s в папке 6_points. Сишный файл остался без изменений.

### _Тестовые прогоны:_
Скриншот запуска тестов на новой скомпилированной и откомпонованной программе и старой(из предыдущего пункта):
![Тесты на оценку 6](tests/tests_6.png)


## задание на 7 баллов:
Реализована программа на ассемблере с рефакторингом из предыдущего пункта, представленная в виде двух единиц помпиляции - foo1.s & foo2.s. Эти файлы размещены в папке 7_points.
### _Опции компляции:_
Компиляция файлов foo1.c & foo2.c оптимизирующими и отладочными опциями:
```sh
gcc -masm=intel -O0 -fno-asynchronous-unwind-tables -fno-jump-tables -fno-stack-protector -fno-exceptions foo1.c -S -o foo1.s

gcc -masm=intel -O0 -fno-asynchronous-unwind-tables -fno-jump-tables -fno-stack-protector -fno-exceptions foo2.c -S -o foo2.s
```
Ассемблирование программ (с целью рефакторинга):
```sh
gcc foo1.s -c -o foo1.o
gcc foo2.s -c -o foo2.o
```
Компоновка и линковка в исполняемый файл:
```sh
gcc foo1.o foo2.o -o foo.out
```
### _Тестовые прогоны:_
Скриншот запуска тестов на новой программе и старой(из предыдущего пункта):
![Тесты на оценку 7](tests/tests_7_1.png)

Программа, использующая файловую систему ввода-вывода и аргументы командной строки (в данном случае принимающая в консоли два аргумента - названия входного и выходного файлов с расширением), лежит в папке 7_points и называется foo.c (ее ассемблерный код - файл foo.s в той же папке). Ключи компиляции - те же, что и в задачах на 4 и 5 баллов (см. выше). Проведены "примерочные" 2 теста из 10 (см.файлы тест_1_7_points.png и тест_2_7_points.png в папке tests).
P.S. Программа корректно исполняется при валидных входных параметрах (в частности, при наличии файла input.txt с валидным текстом в нем).

## задание на 8 баллов:
В программу добавлен генератор случайных наборов данных (в данном случае массив заполняется случайными числами, размер массива хранится во входном файле - к примеру, output.txt).
Программа теперь ожидает 3 аргумента командной строки:
- название входного файла (к примеру, input.txt), в первой строке которого стоит число - это число элементов в будущем массиве
- название выходного файла (к примеру, output.txt)
- seed, который будет влиять на "случайность" генерируемых элементов массива (при одном и том же seed будут генериться одни и те же наборы данных, при разных seed - разные)

Опции компиляции те же, что и в заданиях выше.

Тесты на производительность (слева юзается озу по максимому, справа - регистры по максимому + добавлено в обоих программах зацикливание, чтобы код работал > 1 сек):
![Тесты на оценку 8](tests/8_points_test.png)

### _Тестовые прогоны (доп.):_
![Тесты на оценку 8](tests/8_first.png)
![Тесты на оценку 8](tests/8_second.png)
