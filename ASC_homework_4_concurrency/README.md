# ABC_IDZ_4_Concurrency

# ABC_IDZ_4
Вариант №18
Фортов Егор, БПИ214

# Отчет
## задание на 4, 5, 6, 7 & 8 баллов (на 9 баллов см. ниже):
### Условие задачи:
18. Задача о болтунах. N болтунов имеют телефоны. Они либо ждут звон- ков, либо звонят друг другу, чтобы побеседовать. Если телефон случайного абонента занят, болтун будет звонить другому абоненту, пока ему кто-нибудь не ответит. Побеседовав некоторое время, болтун или ждет звонка, или зво- нит на другой случайный номер. Создать многопоточное приложение, мо- делирующее поведение болтунов. Для решения задачи использовать мью-тексы.

Модель параллельных вычислений - аппроксимированный тип "Взаимодействующие равные".

Входные данные программы - количество болтунов, seed генерации псевдослучайных чисел и входн./выход. файлы.
Основной синхропримитив - мьютексы.
Результаты работы - вывод всех этапов параллельных вычислений, по которым можно сказать, что программа написана верно, так как все результаты этапов были выведены в правильном порядке + на одних и тех же входных данных имеем одинаковый вывод => состояние гонки не возникает.

В приведенном решении мьютексы - это разговор болтунов, если мьютекс закрыт => болтун говорит по телефону, открыт - этот болтун свободен и ему можно звонить. Потоки - это сами болтуны, которые решают, кому и когда звонить (исполняются по возможности одновременно, так как в условии задачи прослеживается одномоментная связь "многие ко многим". Синхронизируются потоки при помощи мьютексов. Идея синхронизции такая: каждый из N болтунов звонит другим N болтунам (для упрощения понимания полагаем, что у нас 2N болтунов). В цикле создаются N потоков, которые почти параллельно входят в нашу функцию call(). Первый поток звонит 1-ому абоненту, блокирует его мьютекс. Соответственно, 2-ой поток при входе в функцию call() не может позвонить 1-ому абоненту (конечно, если еще не истекло время разговора и "замок" не открылся). Итак, второй поток (болтун) звонит второму абоненту и так далее. Каж i-ый поток сначала смотерит на первые (i+1) мьютексов и звонит первому освободившемуся абоненту.

### _Опции компляции:_
Компиляция программы без оптимизирующих и отладочных опций (для оптимизации можно просто включить один из флагов -O1, -O2, -O3):
```sh
gcc main.c -g -pthread
```

Описание опций компиляции:
- -pthread - позволяет нам работать с потоками ОС
- -g - предоставляет отладочную информацию (в данном случае не нужен, но при отладке может помочь)

То, что ждет на вход моя программа в командной строке - описано в коде самой программы.

Тесты (проверяем на одинаковость результатов, чтоб отследить возможную гонку, а также на корректность вывода):
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207384082-2f8e6031-ba0e-49b7-8161-536d9e1334fa.png">
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207384564-9bae64d3-c271-4875-859f-c9116459c7f8.png">
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207384710-0ff64192-38b8-49af-b17e-dbc5752247ad.png">
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207389534-353063a6-b61f-4e5f-91b3-0977218cdde7.png">





## задание на 9 баллов:
Некорректное поведение программы обеспечивается отключением pthread_join, тогда программа завершится раньше, чем отработают все потоки. Тест:
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207386444-ea434e9c-b53f-43ca-a44e-02cfec2c33d9.png">
Также некорректно будет убрать pthread_mutex_destroy; Test (тут такое же поведение как в предыдущем тесте, так как дестрой влияет на внутренне поведение программы, куски потоков в ОС):
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/69436000/207387092-e483f976-41b9-46e3-a1cb-b59938018509.png">
