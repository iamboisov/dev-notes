---
description: Преобразование объектов
---

# Формат JSON, метод toJSON

Представим, у нас есть сложный объект, и мы хотели бы преобразовать его в строку, чтобы отправить его по сети или просто вывести для логирования.

Можно сделать так:

{% code overflow="wrap" %}
```javascript
let user = {
  name: "John",
  age: 30,

  toString() {
    return `{ name: ${this.age}, age: ${this.age}}`
  }
}

alert(user)
```
{% endcode %}

Если объект становится сложным и со вложенными свойствами, то такой метод становится неудобным.

Существует другой метод преобразования объектов и он более простой в реализации.

## JSON.stringify

JSON — это общий формат для представления значений и объектов.

JavaScript предоставляет методы:

* JSON.stringity для преобразования объектов в JSON
* JSON.parse для преобразования JSON обратно в объект

{% code overflow="wrap" %}
```javascript
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(json)
```
{% endcode %}

Метод `JSON.stringify(student)` берёт объект и преобразует его в строку.

Полученная строка json называется JSON-форматированным или сериализированным объектом. Мы можем отправить его по сети или поместить в хранилище данных.

Отличия JSON от объектного литерала:

* Строки используют двойные кавычки. Никаких одинарных кавычек или обратных кавычек в JSON. 'John' становится "John".
* Имена свойств заключаются в двойные кавычки.

JSON.stringify может быть применен и к примитивам.

JSON поддерживает следующие типы данных:

* Объекты `{ ... }`
* Массивы `[ ... ]`
* Примитивы:
  * строки,
  * числа,
  * логические значения `true/false`,
  * `null`.

Например:

{% code overflow="wrap" %}
```javascript
// число в JSON остаётся числом
alert( JSON.stringify(1) ) // 1

// строка в JSON по-прежнему остаётся строкой, но в двойных кавычках
alert( JSON.stringify('test') ) // "test"

alert( JSON.stringify(true) ); // true

alert( JSON.stringify([1, 2, 3]) ); // [1,2,3]
```
{% endcode %}

JSON является независимой от языка спецификацией для данных, поэтому JSON.stringify пропускает некоторые специфические свойства объектов JavaScript:

* Свойства-функции (методы)
* Символьные ключи и значения
* Свойства, содержащие undefined

Важное ограничение: не должно быть циклических ссылок.

Например:

{% code overflow="wrap" %}
```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: ["john", "ann"]
};

meetup.place = room;       // meetup ссылается на room
room.occupiedBy = meetup; // room ссылается на meetup

JSON.stringify(meetup); // Ошибка: Преобразование цикличной структуры в JSON
```
{% endcode %}

## Исключаем и преобразуем: replacer

Полный синтаксис `JSON.stringify`:

{% code overflow="wrap" %}
```javascript
let json = JSON.stringify(value[, replacer, space])
```
{% endcode %}

#### value

Значение для кодирования

#### replacer

Массив свойств для кодирования или функция соответствия function(key, value)

#### space

Дополнительное пространство (отступы), используемое для форматирования

***

В большинстве случаев `JSON.stringify` используется только с первым аргументом. Но если нам нужно настроить процесс замены, например, отфильтровать циклические ссылки, то можно использовать второй аргумент `JSON.stringify`.

Если мы передадим ему массив свойств, будут закодированы только эти свойства.

Например:

{% code overflow="wrap" %}
```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup ссылается на room
};

room.occupiedBy = meetup; // room ссылается на meetup

alert( JSON.stringify(meetup, ['title', 'participants']) );
// {"title":"Conference","participants":[{},{}]}
```
{% endcode %}

Список свойств применяется ко всей структуре объекта. Так что внутри `participants` – пустые объекты, потому что `name` нет в списке.

Давайте включим в список все свойства, кроме `room.occupiedBy`, из-за которого появляется цикличная ссылка:

{% code overflow="wrap" %}
```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup ссылается на room
};

room.occupiedBy = meetup; // room ссылается на meetup

alert( JSON.stringify(meetup, ['title', 'participants', 'place', 'name', 'number']) );
/*
{
  "title":"Conference",
  "participants":[{"name":"John"},{"name":"Alice"}],
  "place":{"number":23}
}
*/
```
{% endcode %}

Список свойств получился длинный. К счастью, в качестве `replacer` мы можем использовать функцию, а не массив.

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup ссылается на room
};

room.occupiedBy = meetup; // room ссылается на meetup


alert( JSON.stringify(meetup, (key, value) => {
  return (key == 'occupiedBy') ? undefined : value;
}));
```

## Форматирование: space

Третий аргумент в `JSON.stringify(value, replacer, space)` – это количество пробелов, используемых для удобного форматирования. Аргумент `space` используется исключительно для вывода в удобочитаемом виде.

Ниже `space = 2` указывает JavaScript отображать вложенные объекты в несколько строк с отступом в 2 пробела внутри объекта:

```javascript
let user = {
  name: "John",
  age: 25,
  roles: {
    isAdmin: false,
    isEditor: true
  }
};

alert(JSON.stringify(user, null, 2));
/* отступ в 2 пробела:
{
  "name": "John",
  "age": 25,
  "roles": {
    "isAdmin": false,
    "isEditor": true
  }
}
*/
```

Третьим параметром может быть строка:

```javascript
let user = {
  name: "John",
  age: 25,
  roles: {
    isAdmin: false,
    isEditor: true
  }
};

alert(JSON.stringify(user, null, "**"));
/*
{
**"name": "John",
**"age": 25,
**"roles": {
****"isAdmin": false,
****"isEditor": true
**}
}
*/
```

## Пользовательский toJSON

Как и `toString` для преобразования строк, объект может предоставлять метод `toJSON` для преобразования в JSON. `JSON.stringify` автоматически вызывает его, если он есть.

Например:

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  date: new Date(Date.UTC(2017, 0, 1)),
  room
};

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "date":"2017-01-01T00:00:00.000Z",  // (1)
    "room": {"number":23}               // (2)
  }
*/
```

Как видим, `date` `(1)` стал строкой. Это потому, что все объекты типа `Date` имеют встроенный метод `toJSON`, который возвращает такую строку.

Теперь давайте добавим собственную реализацию метода `toJSON` в наш объект `room` `(2)`:

```javascript
let room = {
  number: 23,
  toJSON() { // Создаем правила преобразования в JSON
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

alert( JSON.stringify(room) ); // 23

alert( JSON.stringify(meetup) );
```

Как видите, `toJSON` используется как при прямом вызове `JSON.stringify(room)`, так и когда `room` вложен в другой сериализуемый объект.

## JSON.parse

Чтобы декодировать JSON-строку, нужен другой метод JSON.parse.

Синтаксис:

```javascript
let value = JSON.parse(str[, reviver]);
```

#### str

JSON для преобразования в объект

#### reviver

Необязательная функция, которая будет вызываться для каждой пары (ключ, значение) и может преобразовывать значение.

Например:

```javascript
// строковый массив
let numbers = "[0, 1, 2, 3]";

numbers = JSON.parse(numbers);

alert( numbers[1] ); // 1
```

```javascript
let user = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

user = JSON.parse(user);

alert( user.friends ); // 1,2,3,4
```

{% hint style="danger" %}
JSON не поддерживает комментарии. Добавление комментария в JSON делает его недействительным.
{% endhint %}

## Использование reviver

Представьте, что мы получили объект `meetup` с сервера в виде строки данных.

Вот такой:

```javascript
// title: (meetup title), date: (meetup date)
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';
```

А теперь нам нужно _десериализовать_ её, т.е. снова превратить в объект JavaScript.

Давайте сделаем это, вызвав `JSON.parse`:

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str);

alert( meetup.date.getDate() ); // Ошибка!
```

Значением `meetup.date` является строка, а не `Date` объект.

Давайте передадим `JSON.parse` функцию восстановления вторым аргументом, которая возвращает все значения «как есть», но `date` станет `Date`:

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getUTCHours() ); // 12 - теперь работает!
```

Кстати, это работает и для вложенных объектов:

```javascript
let schedule = `{
  "meetups": [
    {"title":"Conference","date":"2017-11-30T12:00:00.000Z"},
    {"title":"Birthday","date":"2017-04-18T12:00:00.000Z"}
  ]
}`;

schedule = JSON.parse(schedule, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( schedule.meetups[1].date.getDate() ); // 18 - отлично!
```
