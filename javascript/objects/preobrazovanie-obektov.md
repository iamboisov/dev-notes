---
description: Работа с объектам
---

# Преобразование объектов

Что произойдет, если мы сложим два объекта или вычтем один из другого?

В случае таких операций, объекты автоматически превращаются в примитивы. После выполняется операция над этими примитивами. На выходе мы получаем примитив.

Сейчас рассмотрим как объект превращается в примитив и как это можно настроить.

## Правила преобразования

1. Не существует преобразования к логическому выражению. Все объекты `true`.
2. Числовое преобразование происходит, когда мы вычитаем объекты или применяем математические функции. Например можно вычитать объекты с датами и результатом будет разница между этими датами.
3. Преобразование к строке происходит, когда мы выводим объект на экран.

### Преобразование к строке

Для преобразования объекта к строке, мы можем использовать операцию над объектом, которая ожидает строку.

```javascript
console.log(obj)
```

### Преобразование к числу

Большинство математических функций включают в себя такое преобразование.

```javascript
// Явное преобразование
let num = Number(obj);

// Унарный плюс
let n = +obj
let delta = date1 - date2;

let greater = user1 > user2
```

### `default` (хинт)

Когда мы производим математические операции с объектами и оператор не "уверен" какой тип ожидать, то он обращается к `default`.

Для выполнения преобразования, JavaScript пытается найти и вызвать три следующих метода объекта:

1. Вызвать `obj[Symbol.toPrimitive](hint)` — метод с символьным ключом, если такой метод существует.
2. Иначе хинт равен `string` и выполняется преобразование к строке.
3. Иначе, хинт равен `number` или `default`.

{% code overflow="wrap" %}
```javascript
let user = {
    name: "Vasya",
    age: 30,
    
    [Symbol.toPrimitive](hint) {
        return hint == "string" ? this.name : this.age 
    }
};

alert(user) // выводя объект на экран мы должны преобразовать в строку. Однако функция не уверена, как преобразовать объект в строке и обращается к хинту. Пытается найти метод с символьным ключом [Symbol.toPrimitive](hint). Результат: Vasya

alert(+user) // 30
```
{% endcode %}

### `toString/valueOf`

Если нет `Symbol.toPrimitive`, тогда JavaScript пытается найти методы `toString` и `valueOf`.

По умолчанию объект имеет методы `toString` и `valueOf`:

* Метод `toString` возвращает строку `"[object Object]"`.
* Метод `valueOf` возвращает сам объект.

Давайте настроим преобразование:

```javascript
let user = {
  name: "John",
  money: 1000,

  // для хинта равного "string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // для хинта равного "number" или "default"
  valueOf() {
    return this.money;
  }

};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
```

{% hint style="info" %}
Преобразование может вернуть любой примитивный тип. Нет никакого контроля, вернет ли `toString` строку. Единственное обязательное условие: эти методы должны возвращать примитив, а не объект.&#x20;
{% endhint %}
