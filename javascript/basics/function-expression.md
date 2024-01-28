---
description: Другого вида синтаксис создания функции в JavaScript.
---

# Function expression

Функция в JavaScript — это особого типа значение. Синтаксис, которым мы использовали до этого, называется Function Declaration (Объявление функции). Однако существует другой способ создания функции, который называется Function Expression (Функциональное выражение).

```javascript
let newFunc = function() {
console.log("Функциональное выражение")
}

newFunc()
```

## Функция — это значение

```javascript
function newFunc() {
return "Функция"
}

console.log(newFunc) // вернет [Function: newFunc]
```

Можно также скопировать функцию в другую переменную:

{% code overflow="wrap" %}
```javascript
function newFunc() {
    console.log("Функция")
    }

let anotherFunc = newFunc // Нет круглых скобок, иначе записали бы в переменную результат вызова функции newFunc
anotherFunc()
newFunc()
```
{% endcode %}

&#x20;Или даже используя _Function Expression:_

```javascript
let newFunc = function() {
    console.log("Функция")
    }

let anotherFunc = newFunc // Нет круглых скобок, иначе записали бы в переменную результат вызова функции newFunc
anotherFunc()
newFunc()
```

{% hint style="info" %}
Для чего нужна точка с запятой в конце?

Точка с запятой не является частью синтаксиса. Она нужна для присваивания.&#x20;

Как в `let x = 1;` так и для `let newFunc = function(...) {...};`
{% endhint %}

## Функции callback

Предположим нам необходимо создать функцию, которая при определенных ответах пользователя должна вызывать другие функции.

Например вы должны проверить введенный пользователем пароль.

{% code overflow="wrap" %}
```javascript
function checkPassword(password, yes, no) { // Если пароль правильный, то вызываем функцию yes, иначе no
    if(password == "1234") yes()
    else no()
}

function correct() {
    console.log("Добро пожаловать!")
}

function incorrect() {
    console.log("Неверный пароль")
}

checkPassword("1235", correct, incorrect) // В аргументах передаем пароль и необходимые функции
```
{% endcode %}

{% hint style="info" %}
Аргументы `correct` и `incorrect` функции `checkPassword` называются _функциями-колбэками_ или просто _колбэками_.
{% endhint %}

Ключевая идея в том, что мы передаем функцию в качестве аргумента и ожидаем, что она будет вызвана обратно когда-нибудь позже, если это будет необходимо. В данном случае, `correct` становится коллбэком для ответа `yes`, а `incorrect` — для ответа `no`.

{% hint style="info" %}
Функция называется анонимной, если она не присвоена к какой-либо переменной.
{% endhint %}

## Отличия Function Declaration от Function Expression

* Function Expression создается, когда выполнение доходит до него, и затем уже может использоваться.
* Function Declaration может быть вызвана раньше, чем она объявлена.

Другими словами, когда движок JavaScript готовится выполнять скрипт или блок кода, прежде всего он ищет в нем Function Declaration и создает функции. В результате функции, созданные как Function Declaration, могут быть вызваны раньше своих определений.

```javascript
sayHi();

function sayHi() {
console.log("Привет")
}
```

Функции, объявленные при помощи Function Expression, создаются тогда, когда выполнение доходит до них. То есть их сначала надо объявить и поместить в переменную, а уже после получится вызвать.

```javascript
sayHi(); // Вызовет ошибку: Cannot access 'sayHi' before initialization

let sayHi = function() {
console.log("Привет")
}
```

{% hint style="info" %}
Функции объявленные внутри тела других функций или блоков кода, доступны только внутри тела этих функций и блоков кода. То есть невозможно вызвать функцию снаружи, которая находится внутри другой.
{% endhint %}

Пример:

```javascript
function usesrGreeting() {
    function sayHi() {
        console.log()
    }

    sayHi() // Вызовется без ошибок
}

sayHi() // Выдаст ошибку
```

Чтобы вызвать функцию снаружи, необходимо объявить ее с помощью Function Expression.

```javascript
let sayHi

function userGreeting() {
    sayHi = function() {
        console.log("Привет")
    }
}

userGreeting() // Необходимо вызвать эту функцию, чтобы присвоить функцию переменной
sayHi() // Вызовется без ошибок
```
