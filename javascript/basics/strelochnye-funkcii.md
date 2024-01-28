---
description: >-
  Лаконичный способ создания функций, который лучше, чем использование Function
  Expression.
---

# Стрелочные функции

Синтаксис:

```javascript
let func = (param1, param2, ..., paramN) => expression
```

Другими словами, это сокращенная версия:

```javascript
let func = function(param1, param2, ..., paramN) {
    return expression
}
```

Пример:

```javascript
let sum = (a, b) => console.log(a+b)

sum(1, 2) // 3
```

Если в функции только один параметр, то круглые скобки можно опустить:

```javascript
let userName = name => console.log("Привет,", name)

userName("Вася") // Привет, Вася
```

Если аргументов нет, то круглые скобки пустые:

```javascript
let greeting = () => console.log("Привет!")

greeting()
```

Стрелочные функции можно использовать как Function Expression. Например для динамического создания функций:

```javascript
let age = 22;

let greeting = (age > 18) ? 
() => console.log("Привет, старец!") : 
console.log("Привет, мелкий!")

greeting() // Привет, старец!
```
