# Page

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
