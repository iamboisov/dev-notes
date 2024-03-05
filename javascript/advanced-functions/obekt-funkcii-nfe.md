---
description: Функции — объекты
---

# Объект функции, NFE

Каждое значение в JavaScript имеет свой тип. А функция — это объект.

Функции можно не только вызывать, но и использовать как обычные объекты: удалять/добавлять свойства, передавать их по ссылке и т.д.

## Свойство `name`

Объект функции содержит несколько полезных свойств:

{% code overflow="wrap" %}
```javascript
function sayHi() {
  console.log("Hello")
}

console.log(sayHi.name) // sayHi
```
{% endcode %}

Это свойство позволяет вытащить имя функции.

Это работает даже в случае объявления функции с помощью Function Expression.

{% code overflow="wrap" %}
```javascript
let sayHi = function() {
  console.log("Hello")
}

console.log(sayHi.name) // sayHi
```
{% endcode %}

Это работает даже в случае присваивания значения по умолчанию:

{% code overflow="wrap" %}
```javascript
function f(sayHi = function() {}) {
  alert(sayHi.name); // sayHi
}

f();
```
{% endcode %}

В спецификации это называется «контекстное имя»: если функция не имеет name, то JavaScript пытается определить его из контекста.

Также имена имеют и методы объекта:

{% code overflow="wrap" %}
```javascript
let user = {

  sayHi() {
    // ...
  },

  sayBye: function() {
    // ...
  }

}

alert(user.sayHi.name); // sayHi
alert(user.sayBye.name); // sayBye
```
{% endcode %}

В этом нет никакой магии. Бывает, что корректное имя определить невозможно. В таких случаях свойство name имеет пустое значение. Например:

{% code overflow="wrap" %}
```javascript
// функция объявлена внутри массива
let arr = [function() {}];

alert( arr[0].name ); // <пустая строка>
// здесь отсутствует возможность определить имя, поэтому его нет
```
{% endcode %}

## Свойство `length`

Встроенное свойство length содержит количество аргументов функции при ее объявлении.

{% code overflow="wrap" %}
```javascript
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {}

console.log(f1.length); // 1
console.log(f2.length); // 2
console.log(many.length); // 2
```
{% endcode %}

Основное отличие в том, что если значение `count` живёт во внешней переменной, то оно не доступно для внешнего кода. Изменить его могут только вложенные функции. А если оно присвоено как свойство функции, то мы можем его получить:

## Named Function Expression

Например, давайте объявим Function Expression:

```javascript
let sayHi = function(who) {
  alert(`Hello, ${who}`);
};
```

И присвоим ему имя:

```javascript
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```

Какова цель этого дополнительного имени `func`?

Для начала заметим, что функция всё ещё задана как Function Expression. Добавление `"func"` после `function` не превращает объявление в Function Declaration, потому что оно все ещё является частью выражения присваивания.

Добавление такого имени ничего не ломает.

Есть две важные особенности имени `func`, ради которого оно даётся:

1. Оно позволяет функции ссылаться на себя же (можно использовать для рекурсивных вызовов)
2. Оно не доступно за пределами функции.

Например, ниже функция `sayHi` вызывает себя с `"Guest"`, если не передан параметр `who`:

```javascript
let sayHi = function hello(who) {
  if (who) {
    console.log("Hello", who, "!")
  } else {
    hello("Guest") // ссылается на саму себя
  }
}

sayHi() // Hello, Guest !

sayHi("Amir") // Hello, Amir !

hello("John") // hello is not defined
```

Можно, конечно, использовать sayHi() для вложенного вызова, но у такого метода есть свои минусы. Если функция присвоится другой переменной, то код выдаст ошибку.

```javascript
let sayHi = function(who) {
  if (who) {
    console.log("Hello", who, "!")
  } else {
    sayHi("Guest") // Ошибка: sayHi не является функцией
  }
}

let greeting = sayHi
sayHi = null

greeting() // Вложенный вызов не работает
```

Однако, если мы присвоим имя функции, то всё будет работать:

```javascript
let sayHi = function func(who) {
  if (who) {
    console.log("Hello", who, "!")
  } else {
    func("Guest") // ссылается на саму себя
  }
}

let greeting = sayHi
sayHi = null

greeting() // Hello, Guest !
```

Так происходит, потому что функция берёт `sayHi` из внешнего лексического окружения. Так как локальная переменная `sayHi` отсутствует, используется внешняя. И на момент вызова эта внешняя `sayHi` равна `null`.

Необязательное имя, которое можно вставить в Function Expression, как раз и призвано решать такого рода проблемы.

Всё из кода выше работает, потому что имя `"func"` локальное и находится внутри функции.

{% hint style="info" %}
Однако такой трюк не сработает с Function Declaration.

Синтаксис Function Declaration не предусматривает возможность объявить дополнительное "внутреннее" имя.&#x20;

Когда необходимо надёжное "внутреннее" имя, стоит переписать Function Declaration на Named Function Expression
{% endhint %}
