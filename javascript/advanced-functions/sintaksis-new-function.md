---
description: Другой вариант объявления функции
---

# Синтаксис new Function

Сам синтаксис:

{% code overflow="wrap" %}
```javascript
let func = new Function([arg1, arg2, ...argN], functionBody);
```
{% endcode %}

Функция создаётся с заданными аргументами `arg1...argN` и телом `functionBody`.

{% code overflow="wrap" %}
```javascript
let sum = new Function('a', 'b', 'return a + b');

console.log( sum(1, 2) ); // 3
```
{% endcode %}

Функция без аргументов, в этом случае достаточно указать только тело:

{% code overflow="wrap" %}
```javascript
let sayHi = new Function('console.log("Hello")');

sayHi(); // Hello
```
{% endcode %}

`new Function` позволяет превратить любую строку в функцию. Например, можно получить новую функцию с сервера и затем выполнить её:

{% code overflow="wrap" %}
```javascript
let str = // ... код, полученный с сервера динамически ...

let func = new Function(str);
func();
```
{% endcode %}

Обычно функция запоминает, где родилась, в специальном свойстве `[[Environment]]`. Это ссылка на лексическое окружение.

Но когда функция создаётся с использованием `new Function`, в её `[[Environment]]` записывается ссылка не на внешнее лексическое окружение, в котором она была создана, а на глобальное. Поэтому такая функция имеет доступ только к глобальным переменным.

{% code overflow="wrap" %}
```javascript
function getFunc() {
  let value = "test"; // Область видимости ограничивается блоком кода

  let func = new Function('alert(value)'); // Просто не видит переменную потому, что есть доступ только к глобальным переменным.

  return func;
}

getFunc()(); // ошибка: value не определено
```
{% endcode %}

Обычное объявление функции:

```javascript
function getFunc() {
  let value = "test";

  let func = function() {console.log(value)};

  return func;
}

let funcCall = getFunc(); // Или getFunc()() для вызова вложенной функции
funcCall()
```
