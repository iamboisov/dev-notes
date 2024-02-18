---
description: Главное о методах массивов
---

# Методы массивов

## `splice`

Как удалить элемент из массива?

Так как массивы — это объекты, то можно использовать `delete`.

```javascript
let arr = ["I", "go", "home"];
delete arr[1]

console.log(arr[1])
console.log(arr.length) // Однако длина массива 3
```

Мы удалили элемент, но длина массива 3. Это происходит из-за того, что мы удаляем значение по ключу. Это хорошо для объектов, но для массивов мы хотим, чтобы другие элементы сдвинулись.

Поэтому нужно использовать специальные методы.

Метод `splice` — это универсальный "швейцарский нож" для работы с массивами. Этот метод умеет всё: добавлять, удалять, заменять.

```javascript
let arr = ["I", "go", "home"];

arr.splice(start, deleteCount, elem1, elemN)
```

Метод изменяет массив `arr` начиная с индекса `start`, удаляет `deleteCount` элементов и затем вставляет `elem1`, `element` на их место. Возвращает массив из удалённых элементов.

Пример:

```javascript
let arr = ["I", "go", "home"];
arr.splice(1, 1) // Начиная с индекса 1, он убрал 1 элемент

console.log(arr) // [ 'I', 'home' ]
```

{% code overflow="wrap" %}
```javascript
let arr = ["I", "go", "home"];
arr.splice(1, 2, "cook", "dinner") // Начиная с индекса 1, удаляем 2 элемента и заменяем на два других

console.log(arr) // [ 'I', 'cook', 'dinner' ]
```
{% endcode %}

Как поместить удаленные элементы в переменную:

{% code overflow="wrap" %}
```javascript
let arr = ["I", "go", "home"];
let removed = arr.splice(1, 2)

console.log(removed) // [ 'go', 'home' ]
```
{% endcode %}

Метод также может вставлять элементы без удаления. Достаточно установить `deleteCount` в `0`:

```javascript
let arr = ["I", "go", "home"];
arr.splice(3, 0, "and", "cook", "dinner")

console.log(arr)
```

{% hint style="info" %}
Отрицательные индексы разрешены.
{% endhint %}

```javascript
let arr = [1, 2, 5];
arr.splice(-1, 0, 3, 4)

console.log(arr)
```

## `slice`

Метод `slice` намного проще.

```javascript
let arr = ["I", "go", "home"];
let sliced = arr.slice(1, 3)

console.log(sliced) // [ 'go', 'home' ]
```

Метод возвращает новый массив, в который копирует все элементы с индекса `start` и до `end` (не включая `end`). `start` и `end` могут быть отрицательными, в этом случае отсчет будет вестись с конца массива.

```javascript
let arr = ["I", "go", "home"];
let sliced = arr.slice(-2)

console.log(sliced) // [ 'go', 'home' ]
```

## `concat`

Этот метод создает новый массив, в который копирует данные из других массивов и дополнительные значения.

```javascript
let arr = [1, 2,];
let newArr = arr.concat([3, 4])
let newArr2 = newArr.concat(5, 6)

console.log(arr) // [ 1, 2 ]
console.log(newArr) // [ 1, 2, 3, 4 ]
console.log(newArr2) // [ 1, 2, 3, 4, 5, 6 ]
```

Объекты добавляются как есть:

{% code overflow="wrap" %}
```javascript
let arr = [1, 2];

let arrayLike = {
  0: "что-то",
  length: 1
};

alert( arr.concat(arrayLike) ) // 1,2,[object Object]. Однако Node.js добавит в массив весь объект и результатом будет: [ 1, 2, { '0': 'что-то', length: 1 } ]
```
{% endcode %}

Однако если массивоподобный объект имеет специальное свойство `Symbol.isConcatSpreadable`, то он обрабатывается как массив:

{% code overflow="wrap" %}
```javascript
let arr = [1, 2];

let arrayLike = {
  0: "что-то",
  1: "еще",
  [Symbol.isConcatSpreadable]: true,
  length: 2
};

alert( arr.concat(arrayLike) ) // [ 1, 2, 'что-то', 'еще' ]
```
{% endcode %}

## Перебор `forEach`

Метод позволяет запускать функцию для каждого элемента массива.
