---
description: Перебор объектов, коллекций, множеств
---

# Object.keys, values, entries

Мы уже видели методы `map.keys()`, `map.values()`, `map.entries()`.

Методы поддерживаются для структур:

* `Map`
* `Set`
* `Array`

## Перебор простых объектов

* `Object.keys` — возвращает массив ключей
* `Object.values` — возвращает массив значений
* `Object.entries` — возвращает массив пар `[key, value]`

Первое отличие в том, что мы должны вызвать `Object.keys(obj)`, а не `obj.keys()`.

У нас может быть объект `data`, который реализует свой собственный метод `data.values()`. И мы всё ещё можем применять к нему стандартный метод `Object.values(data)`.

Второе отличие в том, что методы вида `Object.*` возвращают «реальные» массивы, а не просто итерируемые объекты.

Например:

{% code overflow="wrap" %}
```javascript
let user = {
  name: "John",
  age: 30
};
```
{% endcode %}

* `Object.keys(user) = ["name", "age"]`
* `Object.values(user) = ["John", 30]`
* `Object.entries(user) = [ ["name","John"], ["age",30] ]`

Перебор результата:

{% code overflow="wrap" %}
```javascript
let user = {
    name: "John",
    age: 30
};

for (let value of Object.values(user)) {
    console.log(value);
}
```
{% endcode %}

{% hint style="warning" %}
`Object.keys/values/entries` игнорируют символьные свойства.
{% endhint %}

Так же, как и цикл `for..in`, эти методы игнорируют свойства, использующие `Symbol(...)` в качестве ключей.

## Трансформация объекта

У объектов нет множества методов, которые есть в массивах, например `map`, `filter` и других.

Если мы хотели бы их применить, то можно использовать `Object.entries` с последующим вызовом `Object.fromEntries`:

1. Вызов `Object.entries(obj)` возвращает массив пар ключ/значение для `obj`.
2. На нём вызываем методы массива, например, `map`.
3. Используем `Object.fromEntries(array)` на результате, чтобы преобразовать его обратно в объект.

Например, у нас есть объект с ценами, и мы хотели бы их удвоить:

{% code overflow="wrap" %}
```javascript
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // преобразовать в массив, затем используем метод map, затем fromEntries обратно объект
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

alert(doublePrices.meat); // 8
```
{% endcode %}

