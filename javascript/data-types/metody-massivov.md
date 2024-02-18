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

{% code overflow="wrap" %}
```javascript
arr.forEach(function(item, index, array) {
    // Тело функции
})
```
{% endcode %}

{% code overflow="wrap" %}
```javascript
arr.forEach((item, index, array) => { /* тело функции */})
```
{% endcode %}

Пример:

{% code overflow="wrap" %}
```javascript
let arr = ["I", "go", "home"];

arr.forEach((item, index, array) => { console.log(`У элемента ${item} индекс ${index} и находится в [${array}]`)})
```
{% endcode %}

## `indexOf`/`lastIndexOf`/`includes`

`arr.indexOf(item, form)` ищет `item` начиная с индекса `from` и возвращает номер индекса, на котором был найден искомый элемент, в противном случае `-1`.

`arr.includes(item, from)` ищет `item` начиная с индекса `from` и возвращает `true`, если поиск успешен.

Обычно эти методы используются только с одним аргументом: искомым `item`. По умолчанию поиск ведется с начала.

Обратите внимание, что методы используют строгое сравнение `===`. Таким образом, если мы ищем `false`, он находит именно `false`, а не ноль.

Если мы хотим проверить наличие элемента в массиве и нет необходимости знать его индекс, лучше использовать `arr.includes`.

Метод `arr.lastIndexOf` похож на `arr.indexOf`, но ищет справа налево.

{% hint style="info" %}
`arr.indexOf` неправильно обрабатывает `NaN`. Выводит `-1`.

`arr.includes` правильно обрабатывает `NaN`. Выводит `true/false`.
{% endhint %}

## `find` и `findIndex`/`findLastIndex`

Представим, что у нас есть массив объектов. Как нам найти объект с определенным условием?

На помощь приходит метод `arr.find`.

{% code overflow="wrap" %}
```javascript
let result = arr.find( function(item, index, array) {
    // если true - возвращается текущий элемент и перебор прерывается
    // если все итерации оказались ложными, то возвращает undefined
} )
```
{% endcode %}

{% code overflow="wrap" %}
```javascript
let users = [
    {id: 1, name: "Вася"},
    {id: 2, name: "Петя"},
    {id: 3, name: "Маша"}
];
  
let user = users.find( item => item.name == "Вася" )

console.log(user.id) // возвращает id объекта
```
{% endcode %}

Метод `arr.findIndex` такой же, но возвращает индекс, на котором был найден элемент.

{% code overflow="wrap" %}
```javascript
let users = [
    {id: 1, name: "Вася"},
    {id: 2, name: "Петя"},
    {id: 3, name: "Маша"}
  ];
  
  let user = users.findIndex( item => item.name == "Вася" )

  console.log(user) // 0
```
{% endcode %}

## `filter`

Метод `filter` возвращает массив из элементов, которые удовлетворяют условию. То есть он не возвращает только первый найденный элемент, как это делает `find`.

```javascript
let result = arr.find(function(item, index, array) {
    // если true - возвращается текущий элемент и перебор прерывается
    // если все итерации оказались ложными, возвращается undefined
  });
```

{% code overflow="wrap" %}
```javascript
let users = [
    {id: 1, name: "Вася"},
    {id: 2, name: "Петя"},
    {id: 3, name: "Маша"},
    {id: 4, name: "Петя"}
];
  
let user = users.filter( item => item.name == "Петя" )

console.log(user) // [ { id: 2, name: 'Петя' }, { id: 3, name: 'Петя' } ]
```
{% endcode %}

## `map`

Метод arr.map вызывает функцию для каждого элемента массива и возвращает массив результатов выполнения этой функции.

{% code overflow="wrap" %}
```javascript
let result = arr.map( function(item, index, array) {
    // Создается новый массив и добавляется новое значение на основе результата выполнения функции
} )
```
{% endcode %}

```javascript
let lengths = ["Бильбо", "Гэндальф", "Назгул"]
let result = lengths.map(item => item.length);

console.log(lengths); // [ 'Бильбо', 'Гэндальф', 'Назгул' ]
console.log(result); // [ 6, 8, 6 ] Новый массив
```

## `sort`

Метод arr.sort сортирует массив на месте, меняя в нем порядок элементов.

```javascript
let arr = [1, 2, 15];
arr.sort()

console.log(arr) // [ 1, 15, 2 ] Неправильный порядок
```

По умолчанию элементы сортируются как строки.

Чтобы использовать наш собственный порядок сортировки, нам нужно предоставить функцию в качестве аргумента `arr.sort()`.

Функция должна для пары значений возвращать:

```javascript
function compare(a, b) {
  if (a > b) return 1; // если первое значение больше второго
  if (a == b) return 0; // если равны
  if (a < b) return -1; // если первое значение меньше второго
}
```

```javascript
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15, 56, 52, 46, 10 ];
arr.sort(compareNumeric)

console.log(arr)
```

{% hint style="info" %}
На самом деле функция может вернуть любое положительное число. Это позволяет нам писать более короткие функции.
{% endhint %}

```javascript
let arr = [ 1, 2, 15 ];
arr.sort(function(a, b) { return a - b; });

alert(arr);  // 1, 2, 15
```

```javascript
let arr = [ 1, 2, 15 ];

arr.sort( (a, b) => a - b);

alert(arr);  // 1, 2, 15
```

## `reverse`

Этот метод меняет порядок элементов на обратный.

```javascript
let arr = [1, 2, 3, 4, 5];
arr.reverse();

alert( arr ); // 5,4,3,2,1
```

## `split` и `join`

Метод `str.split(delim)` разбивает строку на массив по заданному разделителю `delim`.

```javascript
let names = 'Вася, Петя, Маша';

let arr = names.split(', ');

for (let name of arr) {
  alert( `Сообщение получат: ${name}.` ); // Сообщение получат: Вася (и другие имена)
}
```

**Разбивка по буквам**

Вызов `split(s)` с пустым аргументом `s` разбил бы строку на массив букв:

```javascript
let str = "тест";

alert( str.split('') ); // т,е,с,т
```

Вызов `arr.join(glue)` делает ровно обратное. Он создаёт строку из элементов `arr`, вставляя `glue` между ними.

Например:

```javascript
let arr = ['Вася', 'Петя', 'Маша'];

let str = arr.join(';'); // объединить массив в строку через ;

alert( str ); // Вася;Петя;Маша
```

## `reduce`/`reduceRight`

Эти методы используются для вычисления единого значения на основе всего массива.

```javascript
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```

Функция применяется по очереди ко всем элементам массива и «переносит» свой результат на следующий вызов.

Аргументы:

* `accumulator` – результат предыдущего вызова этой функции, равен `initial` при первом вызове (если передан `initial`),
* `item` – очередной элемент массива,
* `index` – его позиция,
* `array` – сам массив.

```javascript
let arr = [1, 2, 3, 4, 5];

// убрано начальное значение (нет 0 в конце)
let result = arr.reduce((sum, current) => sum + current);

alert( result ); // 15
```

При отсутствии `initial` в качестве первого значения берётся первый элемент массива, а перебор стартует со второго.

Но такое использование требует крайней осторожности. Если массив пуст, то вызов `reduce` без начального значения выдаст ошибку. Поэтому рекомендуется всегда указывать начальное значение.

## `Array.isArray`

Обычный `typeof` не сможет отличить объект от массива, потому что массивы основаны на объектах.

Поэтому мы используем этот метод:

```javascript
alert(Array.isArray({})); // false

alert(Array.isArray([])); // true
```

## thisArg

Почти все методы массивов, кроме `sort` принимают необязательный параметр `thisArg`, который передает контекст.

```javascript
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
```

Значение параметра `thisArg` становится `this` для `func`.

Например, тут мы используем метод объекта `army` как фильтр, и `thisArg` передаёт ему контекст:

```javascript
let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age >= this.minAge && user.age < this.maxAge;
  }
};

let users = [
  {age: 16},
  {age: 20},
  {age: 23},
  {age: 30}
];

// найти пользователей, для которых army.canJoin возвращает true
let soldiers = users.filter(army.canJoin, army);

alert(soldiers.length); // 2
alert(soldiers[0].age); // 20
alert(soldiers[1].age); // 23
```

Вызов `users.filter(army.canJoin, army)` можно заменить на `users.filter(user => army.canJoin(user))`, который делает то же самое.

## Array.from

Метод, который принимает итерируемый объект или псевдомассив и делает из него «настоящий» `Array`. После этого мы уже можем использовать методы массивов.

`Array.from` в строке `(*)` принимает объект, проверяет, является ли он итерируемым объектом или псевдомассивом, затем создаёт новый массив и копирует туда все элементы.

Полный синтаксис `Array.from` позволяет указать необязательную «трансформирующую» функцию:

```javascript
Array.from(obj[, mapFn, thisArg])
```

Необязательный второй аргумент может быть функцией, которая будет применена к каждому элементу перед добавлением в массив, а `thisArg` позволяет установить `this` для этой функции.

Например:

{% code overflow="wrap" %}
```javascript
let arr = Array.from(range, num => num * num);

alert(arr); // 1,4,9,16,25. Предположим в объекте есть итерируемые свойства с числами
```
{% endcode %}

Здесь мы используем `Array.from`, чтобы превратить строку в массив её элементов:

```javascript
let str = 'AB';

// разбивает строку на массив её элементов
let chars = Array.from(str);

alert(chars[0]); // A
alert(chars[1]); // B
alert(chars.length); // 2
```

В отличие от `str.split`, этот метод в работе опирается на итерируемость строки, и поэтому, как и `for..of`, он корректно работает с суррогатными парами.

Методы также можно комбинировать.
