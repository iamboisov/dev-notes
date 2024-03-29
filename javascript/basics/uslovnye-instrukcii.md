---
description: Конструкции "if" и условный оператор "?".
---

# Условные инструкции

## Инструкция "if"

Инструкция `if` выполняет условие в скобках, если  результат `true`, то выполняет блок кода.

```javascript
let num = 10;

if (num == 10) {
console.log("Num is 10")
}
```

Можно также передать в `if` заранее вычисленное в переменной логическое значение:

```javascript
let condition = (num == 10);

if (condition) {
...
}
```

## Блок "else"

Инструкция `if` может содержать необязательный блок `else`, которое выполнится при противоположном результате вычисления условия.

<pre class="language-javascript"><code class="lang-javascript">let num = 10;

if (num == 5) {
...
} else if (num == 7) {
<strong>...
</strong><strong>} else {
</strong>  console.log("Этот код выполнится")
}
</code></pre>

## Условный оператор "?"

Если есть необходимость определить переменную в зависимости от условия, то можно использовать следующую конструкцию:

```javascript
let res = condition ? resultIfTrue : resultIfFalse;

// То же самое, что и:
let res = 10;
if (res == condition) {
res = resultIfTrue
} else {
res = resultIfFalse
}
```

При постановке условия можно убрать скобки, так как оператор `?`  имеет низкий приоритет, поэтому он выполнится после вычисления условия. Однако скобки делают код более читабельным.

{% hint style="info" %}
В некоторых случаях можно избежать использования оператора `?`

```javascript
let accessAllowed = age > 18 ? true : false;

// или же, потому что сравнение само по себе уже возвращает true/false
let accessAllowed = age > 18;
```
{% endhint %}

### Несколько операторов "?"

```javascript
let num = 10;

let res = num >= 20 ? "Число больше или равно 20" :
    (num >= 15) ? "Число больше или равно 15, но меньше 20" :
    (num >= 10) ? "Число больше или равно 10, но меньше 15" : "Число меньше 10";
    
console.log(res)
```

```javascript
// То же самое, но с конструкцией if/else

let num = 16;
let res = null;

if (num >= 20) {
res = "Число больше или равно 20"
} else if (num >= 15) {
res = "Число больше или равно 15, но меньше 20"
} else if (num >= 10) {
res = "Число больше или равно 10, но меньше 15"
} else {
res = "Число меньше 10"
};
    
console.log(res)
```

### Нетрадиционное использование оператора "?"

Оператор `?` можно использовать вместо конструкции `if`, то есть в качестве замены.

```javascript
let num = 11;

(num == 10) ? console.log("Равно 10") : console.log("Не равно 10")
```

{% hint style="danger" %}
Не рекомендуется использовать оператор таким образом из-за плохой читабельности. Смысл оператора `?` - вернуть значение в зависимости от условия. Используйте оператор только именно для этого. Когда нужно выполнить ветви кода, лучше использовать конструкцию `if`.
{% endhint %}
