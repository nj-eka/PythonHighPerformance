# pythonHighPerformance

## Базовый пример
### Описание функций для иследования
В данном пункте приведем пример простой функции на языке python. В качестве функции рассматривалась функция, которая суммирует числа от ***0** до **N***. В эксперименте сравнивается скорость работы данной функции в следующих случаях: 
* ```PythonFunc(N)``` --- функция реализована на python без предварительной компиляции; 
* ```PyPyFunc(N)``` --- функция реализована на pypy;
* ```CythonFunc(N)``` --- функция реализована на python но предварительно скомпилирована;
* ```CythonTypedFunc(N)``` --- функция реализована на cython с типизацией объектов;
* ```CythonOpenMPFunc(N)``` --- функция реализована на cython с типизацией объектов а также распаралеливанием на OpenMP;
* ```NuitkaFunc(N)``` --- функция которая компилируется с помощью Nuitka

### Вычислительный эксперимент
Для сравнения рассматривается ***N = 100000000*** для всех моделей, а также каждая функция вызывается ***200*** раз для усреднения результата.

### Результаты
В данном простом примере получили следующие оценки времени работы функций:
| Функция  | Время |
| ------------- | ------------- |
| PythonFunc  | 4515 ms  |
| CythonFunc  | 3335 ms  |
| CythonTypedFunc  | 0.032 ms  |
| CythonOpenMPFunc  | 0.010 ms  |
| NuitkaFunc  | 4035 ms  |

Весь код доступен по [ссылке](https://github.com/andriygav/cythonExample/blob/master/example/SimpleExample.ipynb) и [ссылке](https://github.com/andriygav/cythonExample/blob/master/example/SimpleExamplePypy.ipynb).

## Пример на основе CountVectorizer с пакета sklearn
### Описание функций для иследования
В данном пункте сравним скорости работы простого векторного представления предложений. В качестве базового решения выбран ```CountVectorizer``` из пакета ```sklearn```.

На основе ```CountVectorizer``` был написан сообвественый класс ```Vectorizer``` на python. Данный класс повторяет базовые возможности класса ```CountVectorizer```, такие как ```fit``` и ```transform```. Код доступен по [ссылке](https://github.com/andriygav/cythonExample/blob/master/example/CountVectorizer.ipynb) и [ссылке](https://github.com/andriygav/cythonExample/blob/master/example/CountVectorizerPypy.ipynb).

Сревнение производительности проводится для класса ```Vectorizer``` который работает в следующих случаях:
* ```Vectorizer``` --- простая реалзиция на python без компиляции;
* ```CythonVectorizer``` --- простая компиляция класса при помощи cython без использования типизации;
* ```CythonTypedVectorizer``` --- простая компиляция класса при помощи cython c использованием типизации;
* ```PyPyVectorizer``` --- компииляция функции на pypy3.

### Вычислительный эксперимент
В эксперименте сравнивается время метода ```fit```, а также метода ```transform``` для всех моделей. Для оценки времени производится усреднение по нескольким независимым вызовам данных методов. Каждый раз вызов метода ```fit``` производится на новом объекте рассматриваемого класса. Метод ```transform``` вызывается также каждый раз на новом объекте рассматриваемого класса после вызова метода ```fit```.
### Результаты
Оценка времени выполнялась для выборки, которая состояла из **79582** строк (**16.8mb** текста). Всего в данном тексте содержится **76777** различных токенов, которые были найдены всемы моделями и добавлены в словарь.

Для чистоты эксперимента время представленное в таблице является устредненным по **200** вызовам функции ```fit``` и **200** вызовам функции ```transform```.

| Функция  | Время ```fit``` | Время ```transform``` |
| ------------- | ------------- | ------------- |
| Vectorizer  | 492 ms | 1602 ms |
| CythonVectorizer  | 456 ms | 1513 ms |
| CythonTypedVectorizer  | 441 ms | 1510 ms |
| NuitkaVectorizer  | 572 ms | 1597 ms |




## Список источников
* Kurt W. Smith. Cython: A Guide for Python Programmers. Sebastopol: O’Reilly, 2015.
