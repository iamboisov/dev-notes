---
description: Преобразование типов данных разными способами.
---

# Преобразование типов

Обычно операторы и функции автоматически приводят переданные им значения к нужному типу.

Например, `alert` автоматически приводит любое значение в строковый тип. Математические функции преобразуют значения к числовому типу.

## Строковое преобразование

Преобразование значения к строковому типу производится с помощью функций. Напрямую это можно сделать с помощью функции `String(value)`.

```javascript
let num = 5;
let intoString = String(num)
```

## Численное преобразование

Преобразование значения к числовому типу данных производится с помощью математических функций и выражений.

Например:

```javascript
alert( "2" / "1" ); // 1, строки преобразуются в числа
```

Есть также возможность преобразовать напрямую с помощью функции `Number(value)`.

```javascript
let num = "10";
let intoNum = Number(num)
```

Если строку невозможно преобразовать в числовой тип, то результатом будет `NaN`.

```javascript
let num = "Число";
let intoNum = Number(num) // NaN
```

{% hint style="info" %}
```javascript
let intoNum1 = Number(undefined) // NaN
let intoNum2 = Number(null) // 0
let intoNum3 = Number(true) // 1
let intoNum4 = Number(false) // 0
let intoNum5 = Number(" ") // 0
```
{% endhint %}

## Логическое преобразование

Преобразование происходит с помощью функции `Boolean(value)`.

```javascript
let intoBool1 = Boolean("") // false, однако с пробелом " " будет true.
let intoBool2 = Boolean(0) // false
let intoBool3 = Boolean(null) // false
let intoBool = Boolean(undefined) // false
let intoBool = Boolean(NaN) // false
```

Всё остальное будет `true`. Любая непустая строка будет `true`.
