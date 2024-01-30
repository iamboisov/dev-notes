---
description: Методы объектов, ключевое слово "this, ООП
---

# Методы объекта "this"

## Методы объектов

{% hint style="info" %}
Методы объекта — это функции, которые являются свойствами этого объекта
{% endhint %}

Например:

{% code overflow="wrap" %}
```javascript
let user = {
    name: "Вася",
    age: 30,
    // Объявляем метод
    sayHi: function() {
        console.log("Привет!");
    }
};
```
{% endcode %}

Или же:

{% code overflow="wrap" %}
```javascript
let user = {
    name: "Вася",
    age: 30,
    // Объявляем метод сокращенным способом
    sayHi() {
        console.log("Привет!");
    }
};
```
{% endcode %}

Если необходимо получить доступ к свойству `name` объекта `user`, то нужно использовать ключевое слово `this`.

{% hint style="info" %}
Значение `this` — это объект, который используется для вызова метода
{% endhint %}

{% code overflow="wrap" %}
```javascript
let user = {
    name: "Вася",
    age: 30,
    sayHi() {
        console.log("Привет,", this.name); // Привет, Вася
    }
};
```
{% endcode %}

{% hint style="danger" %}
Можно, конечно, использовать `user.name`, но если мы будем ссылаться на объект с помощью другой переменной, а из `user` присвоим что-нибудь другое, то при вызове `admin.name` будет выводиться ошибка.
{% endhint %}

{% code overflow="wrap" %}
```javascript
let user = {
  name: "Вася",
  age: 30,
  sayHi() {
      console.log("Привет,", user.name);
  }
};

let admin = user;
user = null;
admin.sayHi() // Cannot read properties of null
```
{% endcode %}

Но если поменяем на `this.name`:

{% code overflow="wrap" %}
```javascript
let user = {
  name: "Вася",
  age: 30,
  sayHi() {
      console.log("Привет,", this.name);
  }
};

let admin = user;
user = null;
admin.sayHi() // Привет, Вася
```
{% endcode %}

## `this` не является фиксированным

Значение `this` вычисляется во время выполнения кода в зависимости от контекста.

{% code overflow="wrap" %}
```javascript
let user = {name: "Вася"};
let admin = {name: "Админ"}

function sayHi() {
    console.log("Привет,", this.name)
};

user.greet = sayHi; // Присваиваем свойству объекта функцию
admin.greet= sayHi; // Присваиваем свойству объекта функцию

user.greet() // Привет, Вася
admin.greet() // Привет, Админ
```
{% endcode %}

{% hint style="danger" %}
**Вызов метода без объекта выводит** `undefined`

{% code overflow="wrap" %}
```javascript
function sayHi() {
    console.log("Привет,", this.name) // Если же уберем .name, то значением this будет являться глобальный объект window (в браузере) или global (в Node.js)
};

sayHi() // Привет, undefined
```
{% endcode %}
{% endhint %}

## Стрелочные функции и `this`

У таких функций нет своего `this`. Если мы ссылаемся на `this` внутри такой функции, то она берет `this` из внешней "обычной" функции.

Пример:

```javascript
let user = {
    name: "Вася",
    
    sayHi() {
        let greet = () => console.log(this.name);
        greet()
    }
};

user.sayHi()
```
