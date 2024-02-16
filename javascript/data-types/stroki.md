---
description: Самое главное
---

# Строки

## Спецсимволы

<table data-header-hidden><thead><tr><th width="192"></th><th></th></tr></thead><tbody><tr><td><code>\n</code></td><td>Перевод строки</td></tr><tr><td><code>\r</code></td><td>В текстовых файлах Windows для перевода строки используется комбинация символов <code>\r</code>, а на других ОС это просто . Это так по историческим причинам, ПО под Windows обычно понимает и просто .</td></tr><tr><td><code>\'</code>, <code>\"</code>, <code>\`</code></td><td>Кавычки</td></tr><tr><td><code>\\</code></td><td>Обратный слеш</td></tr><tr><td><code>\t</code></td><td>Знак табуляции</td></tr><tr><td><code>\b</code>, <code>\f</code>, <code>\v</code></td><td>Backspace, Form Feed и Vertical Tab — оставлены для обратной совместимости, сейчас не используются.</td></tr></tbody></table>

```javascript
alert( 'I\'m the Walrus!' ); // I'm the Walrus!
```

## Длина строки `length`

```javascript
alert( `My\n`.length ); // 3
```

## Доступ к символам

```javascript
alert( str[0] ); // H
alert( str.at(0) ); // H
```

Однако преимущество метода `.at(pos)` заключается в том, что он допускает отрицательную позицию. Если `pos` – отрицательное число, то отсчет ведется от конца строки.

```javascript
let str = `Hello`;

alert( str[-2] ); // undefined
alert( str.at(-2) ); // l
```

Также можно перебрать строку посимвольно, используя `for..of`:

```javascript
let str = "Hello";

for (letter of str) {
    console.log(letter)
}
```

{% hint style="info" %}
Строки неизменяемы! Нельзя перезаписать символ строки. Однако можно перезаписать новую строку в старую переменную.
{% endhint %}

## Сравнение строк

"Правильный" алгоритм сравнения строк:

```javascript
console.log("Hello".localeCompare("Privet")) // -1
console.log("Hello".localeCompare("asdf")) // 1
console.log("Hello".localeCompare("Hello")) // 0
```

* Отрицательное число, если `str` меньше `str2`.
* Положительное число, если `str` больше `str2`.
* `0`, если строки равны.

{% hint style="info" %}
Строки сравниваются посимвольно в алфавитном порядке (по коду). Код `a` (97) больше кода `Z` (90).
{% endhint %}
