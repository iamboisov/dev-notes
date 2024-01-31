---
description: Способ создания множества однотипных объектов с помощью оператора "new"
---

# Конструктор, оператор "new"

Для создания множества похожих и однотипных объектов используют оператор `new`.

## Функция-конструктор

Функции-конструкторы — это обычные функции. Однако есть два правила:

1. Имя функции-конструктора должно начинаться с большой буквы.
2. Функция-конструктор должна выполняться только с помощью оператора `new`.

{% code overflow="wrap" %}
```javascript
function User(name, permissions) {
  this.name = name;
  this.isAdmin = permissions;
}

let user = new User("Вася", false) // Передаем аргументы функции-конструктору
console.log(`${user.name} — ${user.isAdmin === true ? "Администратор" : "Пользователь"}`) // Выводим результат. Если permissions === true, то Вася - Администратор
```
{% endcode %}

{% hint style="success" %}
Когда функция вызывается как `new User(...)`, происходит следующее:

1. Создается новый пустой объект и он присваивается `this`
2. &#x20;Выполняется тело функции
3. Возвращается значение `this`
{% endhint %}

Другими словами это происходит так:

{% code overflow="wrap" %}
```javascript
function User(name, permissions) {
  // this = {}
  
  // Добавляются свойства к объекту this
  this.name = name;
  this.isAdmin = permissions;
  
  // return this;
}
```
{% endcode %}

{% hint style="success" %}
`new function() {...}`

Если в коде много строк, которые создают один сложный объект, то код можно обернуть в функцию-конструктор, которая будет сразу же вызвана.
{% endhint %}

{% code overflow="wrap" %}
```javascript
let user = new function() {
  this.name = "Вася";
  this.isAdmin = true;
}

console.log(`${user.name} — ${user.isAdmin === true ? "Администратор" : "Пользователь"}`) // Вася — Администратор
```
{% endcode %}

{% hint style="warning" %}
Такой конструктор не может быть вызван снова, так как нигде не сохраняется. Просто создается и вызывается. Такой способ направлен на инкапсуляцию кода, который создает новый объект без возможности повторного использования.
{% endhint %}

## Проверка на вызов в режиме конструктора

Чтобы проверить была ли вызвана функция с помощью оператора `new` или без него, необходимо использовать свойство `new.target` внутри функции. Если функция была вызвана без `new`, то результатом выполнения будет `undefined`.

{% code overflow="wrap" %}
```javascript
function User() {
    console.log(new.target)
}

User(); //undefined

new User() // [Function: User]
```
{% endcode %}

Можно сделать так, чтобы вызов функции осуществлялся только с `new`, то есть результат никак не зависел:

{% code overflow="wrap" %}
```javascript
function User(name) {
  if (!new.target) {
    return new User(name)
  }

  this.name = name
  console.log("Hello,", this.name)
}

User("Петя");
```
{% endcode %}

## Возврат значения из конструктора и `return`

Обычно конструкторы не имеют оператора `return`. Из задача — записать необходимое в `this`, и это автоматическим становится результатом.

Однако если есть `return`, то применяются правила:

1. При вызове `return` с объектом, вместо `this` вернется объект.
2. При вызове `return` с примитивным значением, оно проигнорируется.

```javascript
function User(name) {
  this.name = name
  return {name: "Другой объект"}
}

console.log(new User("Ваня").name) // Другой объект
```

Пример, в котором примитивное значение игнорируется:

```javascript
function User(name) {
  this.name = name
  return "Другой объект"
}

console.log(new User("Ваня").name) // Ваня
console.log(new User("Ваня")) // User { name: 'Ваня' }
```

{% hint style="info" %}
Можно не ставить круглые скобки после `new`
{% endhint %}

```javascript
let user = new User;
// То же самое, что и
let user = new User();
```

## Создание методов в конструкторе

К `this` можно добавлять не только свойства, но и методы.

```javascript
function User(name) {
  this.name = name
  this.greet = function() { // Добавили метод
    console.log("Привет,", this.name)
  }
}

let user = new User("Ваня"); // Создаем объект с помощью конструктора

user.greet() // Вызываем метод
```

Можно немного переписать часть кода, чтобы было понятнее. Код выше — это то же самое, что и:

```javascript
let user = {
  name: "Ваня",
  greet: function() {
      console.log("Привет,", this.name)
  }
}

user.greet() // Привет, Ваня
```
