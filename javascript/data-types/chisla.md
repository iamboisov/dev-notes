---
description: Про числа и методы
---

# Числа

## Способы записи числа

```javascript
let bigNum = 1000000000;

// Для удобства
let bigNum2 = 1_000_000_000;

// Короткая запись
let bigNum3 = 1.2e6 // 1.2 * 1 000 000. 6 нулей

let smallNum = 1e-6 // 0.000001 — 1 / 1 000 000
```

Нижнее подчеркивание — "синтаксический сахар". Движок языка игнорирует нижнее подчеркивание.

## Шестнадцатеричные, двоичные и восьмеричные числа

```javascript
console.log( 0xff ) // Шестнадцатеричная система
console.log( 0b111111 ) // Двоичная система
console.log( 0o377 ) // Восьмеричная система
```

## `toString(base)`

Метод `num.toString()` возвращает строковое представление числа в системе счисления `base` (от 2 до 36).



{% code overflow="wrap" %}
```javascript
let num = 101238394;

console.log( num.toString(16) )
console.log( num.toString(2) )
console.log( num.toString(8) )
```
{% endcode %}

{% hint style="warning" %}
Две точки для метода, который применяется прямо к числу.
{% endhint %}

```javascript
12345..toString(16)

(12345).toString(16)
```

## Сравнение `Object.is`

Существует метод `Object.is`, который сравнивает значения примерно как `===`, но более надежен в двух особых случаях:

1. Работает с `NaN`: `Object.is(NaN, NaN) === true`
2. Значения `0` и `-0` разные. `Object.is(0, -0) === false`

## `parseInt` и `parseFloat`

Функции `parseInt` и `parseFloat` "читают" число из строки.&#x20;

```javascript
alert( parseInt('100px') ); // 100
alert( parseFloat('12.5em') ); // 12.5

alert( parseInt('12.3') ); // 12, вернётся только целая часть
alert( parseFloat('12.3.4') ); // 12.3, произойдёт остановка чтения на второй точке
```

Функции `parseInt/parseFloat` вернут `NaN`, если не смогли прочитать ни одну цифру:

{% code overflow="wrap" %}
```javascript
alert( parseInt('a123') ); // NaN, на первом символе происходит остановка чтения
```
{% endcode %}

К функциям можно добавить второй параметр для определения системы счисления:

```javascript
alert( parseInt('0xff', 16) ); // 255
alert( parseInt('ff', 16) ); // 255, без 0x тоже работает

alert( parseInt('2n9c', 36) ); // 123456
```

{% hint style="info" %}
Другие математические функции встроены в объект `Math`.
{% endhint %}
