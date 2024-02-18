---
description: >-
  Объекты для хранения именованных коллекций и массивы для хранения
  упорядоченных коллекций.
---

# Map и Set

## Map

Это коллекция ключ/значение, как и объект. Но Map позволяет использовать ключи любого типа.

Методы и свойства Map:

* `new Map()` — создает коллекцию
* `map.set(key, value)` — записывает по ключу `key` значение `value`.
* `map.get(key)` — возвращает значение по ключу или `undefined`, если ключа нет.
* `map.has(key)` — возвращает `true`, если `key` присутствует в коллекции, иначе `false`.
* `map.delete(key)` — удаляет элемент по ключу.
* `map.clear()` — очищает коллекцию от элементов.
* `map.size` — возвращает текущее количество элементов.

```javascript
let map = new Map();

map.set("1", "one")
map.set(1, "two")
map.set(true, "three")

console.log(map.get("1")) // one
console.log(map.get(1)) // two
console.log(map.get(true)) // three
```

**Map может использовать объекты в качестве ключей.**

Например:

```javascript
let john = { name: "John" };

// давайте сохраним количество посещений для каждого пользователя
let visitsCountMap = new Map();

// объект john - это ключ для значения в объекте Map
visitsCountMap.set(john, 123);

alert(visitsCountMap.get(john)); // 123
```

{% hint style="info" %}
Каждый вызов map.set возвращает объект map, так что мы можем объединить вызовы в одну цепочку.
{% endhint %}

```javascript
let map = new Map()

map.set("1", "str1")
  .set(1, "num1")
  .set(true, "bool1");

console.log(map)
```

### Перебор Map

Для перебора есть 3 метода:

* `map.keys()` — возвращает итерируемый объект по ключам
* `map.values()` — возвращает итерируемый объект по значениям
* `map.entries()` — возвращает итерируемый объект по парам `[ключ, значение]`. Это то же самое, что и `for..of`

Например:

```javascript
let recipeMap = new Map([
  ["огурец", 500],
  ["помидор", 350],
  ["лук",    50]
]);

// перебор по ключам (овощи)
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // огурец, помидор, лук
}

// перебор по значениям (числа)
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// перебор по элементам в формате [ключ, значение]
for (let entry of recipeMap) { // то же самое, что и recipeMap.entries()
  alert(entry); // огурец,500 (и так далее)
}
```

Кроме этого,  Map имеет встроенный forEach:

```javascript
let recipeMap = new Map([
    ["огурец", 500],
    ["помидор", 350],
    ["лук",    50]
  ]);
  
recipeMap.forEach((value, key, map) => {
    console.log(`${key}: ${value}`); // огурец: 500 и так далее
  });
```

Если мы хотим создать коллекцию из существующего объекта, то достаточно использовать встроенный метод `Object.entries(obj)`, который получает объект и возвращает массив пар ключ-значение:

```javascript
let obj = {
    name: "John",
    age: 30
  };
  
  let map = new Map(Object.entries(obj));
  
  alert( map.get('name') ); // John
```

Есть также обратный метод, который получает массив пар вида `[ключ, значение]`, он создаёт из них объект:

```javascript
let prices = Object.fromEntries([
    ['banana', 1],
    ['orange', 2],
    ['meat', 4]
  ]);
  
  // prices = { banana: 1, orange: 2, meat: 4 }
  
  alert(prices.orange); // 2
  
```

Мы можем использовать `Object.fromEntries`, чтобы получить обычный объект из `Map`.

```javascript
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries()); // создаём обычный объект (*)

// готово!
// obj = { banana: 1, orange: 2, meat: 4 }

console.log(obj); // 2
```

Можно было написать строчку еще короче:

```javascript
let obj = Object.fromEntries(map); // убрать .entries()
```

Это то же самое, так как `Object.fromEntries` ожидает перебираемый объект в качестве аргумента, не обязательно массив. А перебор `map` как раз возвращает пары ключ/значение, так же, как и `map.entries()`.

## Set

Это особый вид коллекции, где каждое значение может появляться только один раз.

Основные методы:

* `new Set(iterable)` — создает Set, и если в качестве аргумента был представлен итерируемый объект, то копирует значения в новый Set.
* `set.add(value)` — добавляет значение (ничего не делает, если уже есть) и возвращает новый объект `set`.
* `set.delete(value)` — удаляет значение, возвращает `true`, если `value` было в множестве на момент вызова, иначе `false`.
* `set.has(value)` — возвращает `true`, если значение присутствует в множестве, иначе `false`.
* `set.clear()` — удаляет все значения
* `set.size` — возвращает кол-во элементов в множестве.

Например, мы ожидаем посетителей, и нам необходимо составить их список. Но повторные визиты не должны приводить к дубликатам. Каждый посетитель должен появиться в списке только один раз.

Множество `Set` – как раз то, что нужно для этого:

```javascript
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// считаем гостей, некоторые приходят несколько раз
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// set хранит только 3 уникальных значения
alert(set.size); // 3

for (let user of set) {
  alert(user.name); // John (потом Pete и Mary)
}
```

Мы можем перебрать содержимое объекта set как с помощью метода `for..of`, так и используя `forEach`:

```javascript
let set = new Set(["апельсин", "яблоко", "банан"]);

for (let value of set) alert(value);

// то же самое с forEach:
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

Заметим забавную вещь. Функция в `forEach` у `Set` имеет 3 аргумента: значение `value`, потом _снова то же самое значение_ `valueAgain`, и только потом целевой объект. Это действительно так, значение появляется в списке аргументов дважды.

Это сделано для совместимости с объектом `Map`, в котором колбэк `forEach` имеет 3 аргумента. Выглядит немного странно, но в некоторых случаях может помочь легко заменить `Map` на `Set` и наоборот.

`Set` имеет те же встроенные методы, что и `Map`:

* `set.keys()` – возвращает перебираемый объект для значений,
* `set.values()` – то же самое, что и `set.keys()`, присутствует для обратной совместимости с `Map`,
* `set.entries()` – возвращает перебираемый объект для пар вида `[значение, значение]`, присутствует для обратной совместимости с `Map`.
