# Методы примитивов

Каждый примитив имеет свой собственный "объект-обертку", которые называются: `String`, `Number`, `Boolean`, `Symbol` и `BigInt`. Таким образом они имеют набор методов.

```javascript
let str = "Hello";

console.log(str.toUpperCase())
```

{% hint style="info" %}
`null/undefined` не имеют методов.&#x20;
{% endhint %}

```javascript
console.log(null.toUpperCase())
```
