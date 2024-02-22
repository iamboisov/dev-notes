# Деструктурирующее присваивание

Деструктурирующее присваивание — это специальный синтаксис, который "распаковывает" массивы и объекты в несколько переменных и позволяет манипулировать ими в удобном нам формате.

## Деструктуризация массива

{% code overflow="wrap" %}
```javascript
let arr = ["Elem1", "Elem2"]

let [odd, even] = arr

console.log(odd)
console.log(even)
```
{% endcode %}

Деструктуризация это просто другой вариант написания:

```javascript
let odd = arr[0]
let even = arr[1]
```

Нежелательные элементы могут быть проигнорированы с помощью дополнительной запятой:

```javascript
let arr = ["Elem1", "Elem2", "Elem4"]

let [odd, ,even] = arr // Дополнительная запятая

console.log(odd)
console.log(even)
```

На самом деле мы можем использовать любой перебираемый объект, не только массивы:

```javascript
let [a, b, c] = "abc";
let [one, two, three] = new Set([1, 2, 3]);
```

Мы можем использовать что угодно «присваивающее» с левой стороны. Можно даже присвоить значения свойствам объекта:

```javascript
let user = {};

[user.name1, user.lastName1] = "Amir Boisov".split(" ");
[user.name2, user.lastName2] = ["John", "Connor"];

console.log(user.name1, user.lastName1); // Amir Boisov
console.log(user.name2, user.lastName2); // John Connor
```

## Цикл с `entries`

Можно использовать с деструктуризацией объекта:

{% code overflow="wrap" %}
```javascript
let user = {
  name: "John",
  age: 30
};

for (let [key, value] of Object.entries(user)) { // Перебираем ключи и значения по структуре
  console.log(`Ключ ${key} со значением ${value}`)
}
```
{% endcode %}

То же самое для map:

{% code overflow="wrap" %}
```javascript
let user = new Map();
user.set("name", "John");
user.set("age", "30");

// Map перебирает как пары [ключ, значение], что очень удобно для деструктурирования
for (let [key, value] of user) {
  alert(`${key}:${value}`); // name:John, затем age:30
```
{% endcode %}

## Обмен переменных

С помощью деструктуризации можно производить обмен значениями переменных:

{% code overflow="wrap" %}
```javascript
let user = "John";
let admin = "Amir";

[user, admin] = [admin, user];

console.log(user);
console.log(admin);
```
{% endcode %}

Здесь мы создаём временный массив из двух переменных и деструктурируем его в порядке замены.

## Остаточные параметры

Если мы хотим не просто получить первые значения, но и собрать все остальные, то мы можем добавить ещё один параметр, который получает остальные значения, используя оператор «остаточные параметры» – троеточие (`"..."`):

{% code overflow="wrap" %}
```javascript
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

// rest это массив элементов, начиная с 3-го
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```
{% endcode %}

Вместо `rest` можно использовать любое другое название переменной, просто убедитесь, что перед переменной есть три точки и она стоит на последнем месте в деструктурирующем присваивании.

{% code overflow="wrap" %}
```javascript
let quotes = ["Hello", "World", "This is", "the rest of the quote"];

let [firstElem, secondElem, ...other] = quotes;

console.log(firstElem)
console.log(secondElem)
console.log(other)
```
{% endcode %}

## Значения по умолчанию

Можно присваивать значения по умолчанию для параметров:

{% code overflow="wrap" %}
```javascript
let quotes = ["Hello"];

let [firstElem="No data", secondElem="No data"] = quotes;

console.log(firstElem) // Hello
console.log(secondElem) // No data
```
{% endcode %}

Значения по умолчанию позволяют выводить элемент, даже если его значение отсутствует в массиве.

Можно использовать даже функции:

{% code overflow="wrap" %}
```javascript
// prompt запустится только для surname
let [name = prompt('name?'), surname = prompt('surname?')] = ["Julius"];

alert(name);    // Julius (из массива)
alert(surname); // результат prompt
```
{% endcode %}

## Деструктуризация объекта

{% code overflow="wrap" %}
```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

let {title, width, height} = options; // Имена переменных должны совпадать со свойствами объекта. Здесь также можно установить значения по умолчанию при желании

// let {title="Text", width=50, height=150} = options;

console.log(title, width, height);
```
{% endcode %}

Если же вы хотите дать переменным другие имена, то необходимо использовать двоеточие:

{% code overflow="wrap" %}
```javascript
let sizes = {
  title: "Menu",
  width: 100,
  height: 200
};

let {title:prop1, width:prop2, height:prop3} = sizes; // Задаем новые имена переменным

console.log(prop1, prop2, prop3);
```
{% endcode %}

{% hint style="info" %}
Как и в случае с массивами, значениями по умолчанию могут быть любые выражения или даже функции.
{% endhint %}

Мы также можем совмещать `:` и `=`:

{% code overflow="wrap" %}
```javascript
let options = {
  title: "Menu"
};

let {width: w = 100, height: h = 200, title} = options;

console.log(w, h, title) // 100 200 Menu
```
{% endcode %}

{% hint style="info" %}
Остаточные параметры тоже работают с объектами.
{% endhint %}

В примерах выше переменные были объявлены в присваивании: `let {…} = {…}`. Можно, конечно, не писать `let`, но так код не будет работать. Проблема в том, что JavaScript обрабатывает `{...}` в основном потоке кода (не внутри другого выражения) как блок кода.

{% code overflow="wrap" %}
```javascript
let title, width, height;

// ошибка будет в этой строке
{title, width, height} = {title: "Menu", width: 200, height: 100};
```
{% endcode %}

Такие блоки кода могут быть использованы для группировки операторов, например:

{% code overflow="wrap" %}
```javascript
{
  // блок кода
  let message = "Hello";
  // ...
  alert( message );
}
```
{% endcode %}

Так что здесь JavaScript считает, что видит блок кода, отсюда и ошибка. На самом-то деле у нас деструктуризация.

Чтобы показать JavaScript, что это не блок кода, мы можем заключить выражение в скобки `(...)`:

```javascript
let title, width, height;

// Заключаем в круглые скобки
({title, width, height} = {title: "Menu", width: 200, height: 100});

console.log(title, width, height)
```

## Вложенная деструктуризация

Если объект или массив содержит вложенные объекты, массивы или свойства, то можно использовать более сложные шаблоны с левой стороны, чтобы извлечь эти вложенные свойства.

```javascript
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};


let {
  size: {
    width: w,
    height: h,
  },

  items: [value1, value2],
  extra: ex,
} = options;


console.log(w) // 100
console.log(h) // 200
console.log(value1) // Cake
console.log(value2) // Donut
console.log(ex) // true
```

Можно также брать сами вложенные объекты или свойства:

```javascript
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};

let {
  size: s,
  items: i,
  extra: ex,
  title: t,
} = options;

console.log(s)
console.log(i)
console.log(t)
```

Можно также присваивать свойствам значения по умолчанию :

```javascript
let options = {
  size: 100
};

let {
  size: s,
  title: t="Default title",
} = options;

console.log(s)
console.log(t)
```

## Умные параметры функции

Есть ситуации, когда функция имеет много параметров, большинство из которых необязательны. Представьте себе функцию, которая создаёт меню. Она может иметь ширину, высоту, заголовок, список элементов и так далее. Запомнить порядок параметров — сложно.

Вот так – плохой способ писать подобные функции:

```javascript
function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
  // ...
}
```

Другая проблема заключается в том, как вызвать функцию, когда большинство параметров передавать не надо, и значения по умолчанию вполне подходят.

Разве что вот так?

```javascript
// undefined там, где подходят значения по умолчанию
showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])
```

Это выглядит ужасно. И становится нечитаемым, когда мы имеем дело с большим количеством параметров.

На помощь приходит деструктуризация!

Мы можем передать параметры в объект и функция деструктурирует его в переменные:

{% code overflow="wrap" %}
```javascript
let options = {
  width: 100,
  height: 200,
  description: "Some text here"
};

// Заключаем в скобки
function destructuring({height=150, width=75, title="Hello", description="Text"}) {
  console.log(height, width, title, description)
}

destructuring(options)
```
{% endcode %}

Теперь с массивом:

```javascript
let options = ["Hello", "World"]

function destructuring([value1, value2]) {
  console.log(value1, value2)
}

destructuring(options)
```

Пожалуйста, обратите внимание, что такое деструктурирование подразумевает, что в `showMenu()` будет обязательно передан аргумент. Если нам нужны все значения по умолчанию, то нам следует передать пустой объект:

```javascript
showMenu({}); // Ок, все значения - по умолчанию

showMenu(); // Так была бы ошибка
```

Мы можем исправить это, сделав `{}` значением по умолчанию для всего объекта параметров:

{% code overflow="wrap" %}
```javascript
let options = {
  width: 100,
  height: 200,
  description: "Some text here"
};

// После деструктуризации присваиваем пустой объект {}
function destructuring({height=150, width=75, title="Hello", description="Text"} = {}) {
  console.log(height, width, title, description)
}

destructuring() // Теперь не будет ошибки при вызове без параметров
```
{% endcode %}

В приведённом выше коде весь объект аргументов по умолчанию равен `{}`, поэтому всегда есть что-то, что можно деструктурировать.
