---
description: Копирование объектов и ссылки
---

# Копирование объектов

Объекты отличаются от примитивов тем, что при копировании объекты хранятся как "ссылки", тогда как примитивные копируются как значения.

```javascript
let word = "Hello";
let phrase = word; // в итоге значение копируется в переменную phrase
```

Тогда как переменная, которой присвоен объект, хранит не сам объект, а его "адрес в памяти" — другими словами "ссылку" на него.

```javascript
let person= {
    name: "John",
    age: 30,
}

let user = person;
```

В результате у нас две переменные, которые обращаются к одному и тому же адресу в памяти, чтобы получить доступ к объекту. Другими словами есть объект и две переменные, которые ссылаются на него.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Копирование объекта по ссылке

Если есть необходимость для копирования объекта по ссылке, то можно воспользоваться следующим методом:

{% code overflow="wrap" %}
```javascript
let person= {
    name: "John",
    age: 30,
}

let user = {}

for (let key in person) {
    user[key] = person[key] // Просто копируем каждое свойство в другой объект
}
```
{% endcode %}

Другой способ с использованием метода `Object.assign`.

{% code overflow="wrap" %}
```javascript
Object.assign(user, property1, property2)
```
{% endcode %}

```javascript
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

Object.assign(user, permissions1, permissions2);

console.log(user.canView) // true
```

{% hint style="warning" %}
Если скопированное имя свойства уже существует, то оно будет перезаписано
{% endhint %}

Можно также использовать этот метод вместо цикла:

```javascript
let user = { name: "John" };

let person = Object.assign({}, user)

console.log(person.name) // John
```

Предположим что в объекте находится свойство, которое является объектом. Получается что при копировании в объект `person` мы будем обращаться к свойству по ссылке и при изменении этого свойства, поменяются свойства скопированного объекта. Для полного и независимого копирования объекта нужно использовать глобальный метод `structuredClone()`.

{% code overflow="wrap" %}
```javascript
let user = { name: "John" };
user.itSelf = user // свойство объекта ссылается на себя

let person = structuredClone(user) // при Object.assign() мы бы обращались к .itSelf по ссылке, а нам это  не нужно. 

// В итоге мы провели глубокое копирование с сохранением свойства, которое ссылается себя
console.log(person.name) // John
console.log(person.itSelf === person) // true
```
{% endcode %}

{% hint style="success" %}
Объекты объявленные как `const` могут быть изменены

Значение `user` — константа и оно всегда должно ссылаться на один и тот же объект. Однако свойства объекта могут быть изменены.

```javascript
const user = { name: "John" };
user.name = "Pete";

console.log(user.name) // Pete
```
{% endhint %}
