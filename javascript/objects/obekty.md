---
description: >-
  Объекты используются для хранения коллекций различных значений и боле сложных
  сущностей.
---

# Объекты

Объект может быть создать с помощью фигурных скобок `{...}` с необязательным списком свойств.

{% hint style="info" %}
Свойство – это пара "ключ-значение".
{% endhint %}

{% code overflow="wrap" %}
```javascript
let person = new Object(); // конструктор, который нужен для создания объектов по шаблону
let user = {...};
```
{% endcode %}

## Литералы и свойства

```javascript
let person = {
    name: "Имя",
    age: 22
    "likes me": true
};
```

Можно также читать свойства в объекты или доабвлять в них новые.

```javascript
console.log(person.name);
console.log(person["likes me"];

// Или же добавить новое свойство
person.isSitting = true;

// Можем также удалить свойство объекта
delete person.name

// Можем изменить значение свойства
person.age = 25;
```

Доступ к свойству через переменную

```javascript
let person = {
    name: "Имя",
    age: 22
    "likes me": true
};

let quote = prompt("Напишите фразу");

console.log(person[key])
```

{% hint style="danger" %}
Запись через точку такого не позволит

```javascript
console.log(person.key) // undefined
```
{% endhint %}

## Вычисляемые свойства

Можно создать  вычисляемые свойства заключив переменную в квадратные скобки в литеральной нотации.

```javascript
let fruit = prompt("Какой фрукт купить?", "Банан");

let bag = {
    [friut] = 5;
} // То есть название свойства будет вычисляться по ответу пользователя
```

Другой пример:

```javascript
let fruit = prompt("Какой фрукт купить?", "Банан");

let bag = {
    [friut + "Electronics"] = 5;
} // То есть название свойства будет вычисляться по ответу пользователя
```

## Свойство из переменной

В реальном коде нам часто необходимо использовать переменные как значения для свойств с тем же именем.

{% code overflow="wrap" %}
```javascript
function makeUser(name, age) {
    return {
        name: name,
        age: age,
    };
}

let user = makeUser("John", 30); // Здесь мы вызываем фукнцию, результатом которой является объект
user.isSitting = true

console.log(user.isSitting)
```
{% endcode %}

Такод подход настолько популярен, что существуют даже короткие свойства для упрощения этой записи.

```javascript
function makeUser(name, age) {
    return {
        name,
        age,
        isSitting: true,
    };
}
```

## Ограничения на имена свойств

Как мы знаем, имя переменной не может совпадать с зарезервированными JavaScript словами. Однако такого правила нет для свойств объектов.

```javascript
let obj = {
    for: 1,
    let: 2,
    while: 3,
    return: 4,
}

console.log(obj.for + obj.let + obj.while + obj.return)
```

{% hint style="info" %}
Все ключи объектов других типов будут преобразованы в строки
{% endhint %}

```javascript
let obj = {
    0: "Привет",
}

console.log(typeof obj[0]) // string
```

{% hint style="danger" %}
Есть подводный камень связанный со специальным свойством `__proto__`. Мы не можем установить его в необъектное значение.
{% endhint %}

```javascript
let obj = {}

obj.__proto__ = 5;
console.log(typeof obj.__proto__) // object
```

## Проверка существования свойств, оператор `in`

В отличие от других языков, в JavaScript есть особенность обратиться к любому свойству объекта, даже если его не существует. К ошибке такое действие не приведет, а лишь выведется `undefined`, что скажет об отсутствии такого свойства.&#x20;

```javascript
let obj = {}
console.log(obj.property)
```

&#x20;Также существует специальный оператор `in` для проверки существования свойства в объекте. Оператор возвращает логическое выражение `true/false`.

Синтаксис:

```javascript
"key" in object
```

Пример:

```javascript
let user = { name: "John", age: 30 };

console.log( "name" in user) // true
```

{% hint style="warning" %}
Свойство должно быть в кавычках. Если мы убираем кавычки, то JavaScript думает, что мы указываем на переменную, в которой находится имя свойства.
{% endhint %}

{% hint style="info" %}
Так для чего нужен оператор `in`, если можно просто проверить свойство на `undefined`?

Он нужен в случае, если свойство существует, но его значение `undefined`.
{% endhint %}

Пример:

```javascript
let obj = {
    test: undefined,
}

console.log(obj.test); // undefined, значит свойства не существует??
console.log("test" in obj); // true, свойство существует
```

## Цикл `for (key in obj) {...}`

Для перебора всех свойств объекта используется цикл `for...in`. Этот цикл отличается от изученного цикла.

Синтаксис

```javascript
for (key in obj) {
    // тело цикла выполняется для каждого свойства.
}
```

Пример:

```javascript
let person = {
    name: "Имя",
    age: 22,
    isSitting: true,
    profession: "Software Engineer",
};

for (let key in person) {
    console.log(key)
}
```

{% hint style="warning" %}
Свойства с целочисленными ключами (только целые числа) сортируются по возрастанию, а все остальные располагаются в порядке создания.
{% endhint %}

{% hint style="success" %}
Целочисленное свойство — означает строку, которая может быть преобразована в целое число и обратно без изменений.

`"49"` может быть преобразовано в число и обратно в строку без изменений, а `"+49"` или `"1.2"` нет.
{% endhint %}

```javascript
let codes = {
  "49": "Германия",
  "41": "Швейцария",
  "44": "Великобритания",
  // ..,
  "1": "США"
};

for (let num in codes) {
  console.log(num) // 1, 41, 44, 49
}
```

Если же есть необходимость сделать коды стран, то достаточно сделать целочисленные свойства нецелочисленными, добавив `+` перед числом.
