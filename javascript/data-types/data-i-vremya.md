---
description: Работа с датой и временем
---

# Дата и время

Встроенный объект Date содержит дату и время, а также предоставляет методы управления ими.

## Создание

Для создания нового объекта Date нужно вызвать конструктор new Date() с одним из следующих аргументов:

### new Date()

Без аргументов — создать объект Date с текущими датой и временем:

{% code overflow="wrap" %}
```javascript
let date = new Date();
console.log(date); // Fri Feb 23 2024 10:43:02 GMT+0300 (Москва, стандартное время)
```
{% endcode %}

### new Date(milliseconds)

Создать объект `Date` с временем, равным количеству миллисекунд (тысячная доля секунды), прошедших с 1 января 1970 года UTC+0.

{% code overflow="wrap" %}
```javascript
let date = new Date(1503675111372);
console.log(date) // Fri Aug 25 2017 18:31:51 GMT+0300 (Москва, стандартное время)
```
{% endcode %}

```javascript
// 0 соответствует 01.01.1970 UTC+0
let Jan01_1970 = new Date(0);
console.log( Jan01_1970 );

// теперь добавим 24 часа и получим 02.01.1970 UTC+0
let Jan02_1970 = new Date(24 * 3600 * 1000);
console.log( Jan02_1970 );
```

{% hint style="info" %}
Целое число, представляющее собой количество миллисекунд, прошедших с начала 1970 года, называется _таймстамп_ (англ. timestamp).
{% endhint %}

Из таймстампа всегда можно получить дату с помощью `new Date(timestamp)` и преобразовать существующий объект `Date` в таймстамп, используя метод `date.getTime()`

{% code overflow="wrap" %}
```javascript
let date = new Date(); // Fri Feb 23 2024 11:13:52 GMT+0300 (Москва, стандартное время)
console.log(date.getTime()) // 1708675951974. Количество миллисекунд прошедших с 1 января 1970 года UTC+0.
```
{% endcode %}

Датам до 1 января 1970 будут соответствовать отрицательные таймстампы:

{% code overflow="wrap" %}
```javascript
let date = new Date(-1503675111372);
console.log(date) // Tue May 09 1922 11:28:08 GMT+0300 (Москва, стандартное время)
```
{% endcode %}

```javascript
let Dec31_1969 = new Date(-24 * 3600 * 1000);
console.log( Dec31_1969 );
```

### new Date(datestring)

Если аргумент всего один и это строка, то из нее прочитывается дата:

```javascript
let date = new Date("2017-01-26");
alert(date);
// Время не указано, поэтому оно ставится в полночь по Гринвичу и
// меняется в соответствии с часовым поясом места выполнения кода
// Так что в результате можно получить:
// Thu Jan 26 2017 03:00:00 GMT+0300 (Москва, стандартное время)
```

### new Date(year, month, date, hours, minutes, seconds, milliseconds)

Для создания объекта Date с заданными компонентами в местном часовом поясе, необходимы только первые два аргумента — год и месяц. Обязательны только первые два аргумента.

{% hint style="danger" %}
Предоставив только один параметр, объект будет считать его как количество миллисекунд.
{% endhint %}

Неверный код:

{% code overflow="wrap" %}
```javascript
let date = new Date(2024);
console.log(date); // Thu Jan 01 1970 03:00:02 GMT+0300 (Москва, стандартное время)
```
{% endcode %}

Правильно:

```javascript
let date = new Date(2024, 1);
console.log(date); // Thu Feb 01 2024 00:00:00 GMT+0300 (Москва, стандартное время)
```

{% hint style="info" %}
Обратите внимание, что при предоставлении одного параметра, выводится время пройденное после 1 января 1970 года по Гринвичу, учитывая часовой пояс (+ 3 часа). Однако при предоставлении двух параметров, учитывается время по местному часовому поясу.
{% endhint %}

Синтаксис:

* year должен содержать 4 цифры. Для совместимости со старыми версиями языка принимаются 2 цифры, но настоятельно рекомендуется использовать 4 цифры.
* month начинается с 0 (январь) по 11 (декабрь)
* date представляет собой день месяца. Если нет этого параметра, то принимается значение 1 по умолчанию.
* Если параметров hours/minutes/seconds/milliseconds нет, то их значениями становится 0 по умолчанию.

{% code overflow="wrap" %}
```javascript
new Date(2011, 0, 1, 0, 0, 0, 0); // // 1 Jan 2011, 00:00:00
new Date(2011, 0, 1); // то же самое, так как часы и проч. равны 0
```
{% endcode %}

{% code overflow="wrap" %}
```javascript
let date = new Date(2011, 0, 1, 2, 3, 4, 567);
console.log( date ); // 1.01.2011, 02:03:04.567
```
{% endcode %}

## Получение компонентов даты

Существуют методы получения года, месяца, дня и т.д. из объекта Date:

#### getFullYear()

Получить год (4 цифры).

{% hint style="danger" %}
В старых версиях браузеров всё еще поддерживается метод `getYear()`, который возвращает 2 цифры года по местному времени, но эта возможность была удалена из веб-стандартов. Не используйте её ни в старых, ни в новых проектах.
{% endhint %}

#### getMonth()

Получить месяц, от 0 до 11.

#### getDate()

Получить день месяца, от 1 до 31.

#### getHours(), getMinutes(), getSeconds(), getMilliseconds()

Получить часы, минуты, секунды и миллисекунды.

#### getDay()

Получить день недели, от 0 (воскресенье) до 6 (суббота).

{% hint style="info" %}
Все перечисленные методы возвращают значения в соответствии с местным часовым поясом.
{% endhint %}

## Установка компонентов даты

Методы, которые позволяют установить компоненты даты и времени:

* setFullYear( year, \[month], \[date] )
* setMonth( month, \[date] )
* setDate( date )
* setHours( hour, \[mins], \[sec], \[ms] )
* setMinutes( mins, \[sec], \[ms] )
* setSeconds( sec, \[ms] )
* setMilliseconds( ms )
* setTime( milliseconds ) — устанавливает дату в виде кол-ва миллисекунд, прошедших с 01.01.1970 UTC.

{% hint style="info" %}
У всех методов, кроме setTime(), есть UTC-вариант, например setUTCHours()
{% endhint %}

Как мы видим, некоторые методы могут устанавливать сразу несколько компонентов даты, например: `setHours`. Если какая-то компонента не указана, она не меняется.

{% code overflow="wrap" %}
```javascript
let today = new Date();
let today2 = new Date();

today.setHours(0);
console.log(today); // выводится сегодняшняя дата, но значение часа будет 0

today2.setHours(0, 0, 0, 0);
console.log(today2); // всё ещё выводится сегодняшняя дата, но время будет ровно 00:00:00.
```
{% endcode %}

{% hint style="warning" %}
Учтите, что при выполнении кода в Node.js используется метод `toISOString()` для преобразования в строку и вывода на экран. Для этого метода часовой пояс всегда будет в зоне UTC.
{% endhint %}

## Автоисправление даты

Можно устанавливать компоненты даты вне обычного диапазона значений, а объект сам себя исправит.

```javascript
let date = new Date(2013, 0, 32); // 32 Jan 2013 ?!?
alert(date); // ...1st Feb 2013!
```

Предположим, нам требуется увеличить дату «28 февраля 2016» на два дня. В зависимости от того, високосный это год или нет, результатом будет «2 марта» или «1 марта». Нам об этом думать не нужно. Просто прибавляем два дня. Объект `Date` позаботится об остальном:

```javascript
let date = new Date(2016, 1, 28);
date.setDate(date.getDate() + 2);

alert( date ); // 1 Mar 2016
```

Эту возможность часто используют, чтобы получить дату по прошествии заданного отрезка времени. Например, получим дату «спустя 70 секунд с текущего момента»:

```javascript
let date = new Date();
date.setSeconds(date.getSeconds() + 70);

alert( date ); // выводит правильную дату
```

Также можно установить нулевые или даже отрицательные значения. Например:

{% code overflow="wrap" %}
```javascript
let date = new Date();
date.setDate(date.getDate() - 70); // Fri Feb 23 2024 10:43:02 GMT+0300 (Москва, стандартное время)

console.log( date ); // Fri Dec 15 2023 13:57:47 GMT+0300 (Москва, стандартное время)
```
{% endcode %}

{% code overflow="wrap" %}
```javascript
let date = new Date(2016, 0, 2); // 2 Jan 2016

date.setDate(1); // задать первое число месяца
alert( date );

date.setDate(0); // первый день месяца -- это 1, так что выводится последнее число предыдущего месяца
alert( date ); // 31 Dec 2015
```
{% endcode %}

## Преобразование к числу, разность дат

Если объект Date преобразовать в число, то получим таймстамп по аналогии с date.getTime():

{% code overflow="wrap" %}
```javascript
let date = new Date();
alert(+date); // количество миллисекунд, то же самое, что date.getTime()
```
{% endcode %}

Важный побочный эффект: даты можно вычитать, в результате получаем разность в миллисекундах.

Этот приём можно использовать для измерения времени:

{% code overflow="wrap" %}
```javascript
let start = new Date(); // начинаем отсчёт времени

// выполняем некоторые действия
for (let i = 0; i < 10000000; i++) {
  let doSomething = i * i;
}

let end = new Date(); // заканчиваем отсчёт времени

console.log( `Цикл отработал за ${end - start} миллисекунд` );
```
{% endcode %}

## Date.now()

Если нужно просто измерить время, объект Date нем не нужен.

Существует особый метод `Date.now()`, возвращающий текущую метку времени.

Семантически он эквивалентен `new Date().getTime()`, однако метод не создаёт промежуточный объект `Date`. Так что этот способ работает быстрее и не нагружает сборщик мусора.

Данный метод используется из соображений удобства или когда важно быстродействие, например, при разработке игр на JavaScript или других специализированных приложений.

Вероятно, предыдущий пример лучше переписать так:

{% code overflow="wrap" %}
```javascript
let start = Date.now(); // количество миллисекунд с 1 января 1970 года

// выполняем некоторые действия
for (let i = 0; i < 10000000; i++) {
  let doSomething = i * i;
}

let end = Date.now(); // заканчиваем отсчёт времени

alert( `Цикл отработал за ${end - start} миллисекунд` ); // вычитаются числа, а не даты
```
{% endcode %}

## Разбор строки с датой

Метод Date.parse() считывает дату из строки.

Формат строки должен быть следующим: `YYYY-MM-DDTHH:mm:ss.sssZ`, где:

* `YYYY-MM-DD` – это дата: год-месяц-день.
* Символ `"T"` используется в качестве разделителя.
* `HH:mm:ss.sss` – время: часы, минуты, секунды и миллисекунды.
* Необязательная часть `'Z'` обозначает часовой пояс в формате `+-hh:mm`. Если указать просто букву `Z`, то получим UTC+0.

Возможны и более короткие варианты, например, `YYYY-MM-DD` или `YYYY-MM`, или даже `YYYY`.

Вызов `Date.parse(str)` обрабатывает строку в заданном формате и возвращает таймстамп (количество миллисекунд с 1 января 1970 года UTC+0). Если формат неправильный, возвращается `NaN`.

Например:

{% code overflow="wrap" %}
```javascript
let ms = Date.parse('2012-01-26T13:51:50.417-07:00');

alert(ms); // 1327611110417 (таймстамп)
```
{% endcode %}

Можно тут же создать объект `new Date` из таймстампа:

{% code overflow="wrap" %}
```javascript
let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') ); // результат будет по вашему местному времени

alert(date);
```
{% endcode %}

Другой пример:

{% code overflow="wrap" %}
```javascript
let date1 = new Date(Date.parse('2012-01-26T13:51Z')); // Дата по местному времени

// Можно переписать по-другому
let date2 = new Date('26 January 2012 13:51 UTC')

console.log(date1);
console.log(date2);
```
{% endcode %}

{% code overflow="wrap" %}
```javascript
// Можно сразу создать объект Date из таймстампа
let date1 = new Date(Date.parse('2012-01-26T14:51+01:00')); // Воспринимается как дата по часовому поясу. +01:00 смещение по UTC. 
let date2 = new Date('26 January 2012 13:51 UTC')

console.log(date1); // Результат: 2012-01-26T13:51:00.000Z
console.log(date2); // Результат: 2012-01-26T13:51:00.000Z
```
{% endcode %}
