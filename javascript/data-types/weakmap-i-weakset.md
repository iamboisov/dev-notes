---
description: Предотвращение удаления сборщиком мусора
---

# WeakMap и WeakSet

JavaScript хранит значения в памяти пока эти значения достижимы (то есть значения могут быть использованы).

<pre class="language-javascript" data-overflow="wrap"><code class="lang-javascript"><strong>let john = { name: "John" };
</strong>
// объект доступен, переменная john — это ссылка на него

// перепишем ссылку
john = null;

// объект будет удалён из памяти
</code></pre>

Структура данных считается достижимой до тех пор, пока эта структура данных содержится в памяти. Если мы поместим объект `john` в массив, то до тех пор, пока массив существует, объект будет существовать в памяти, даже если нет других ссылок на него.

{% code overflow="wrap" %}
```javascript
let john = { name: "John" };
let arr = [ john ];
john = null;

console.log(john) // null
console.log(arr) // [ { name: 'John' } ]
```
{% endcode %}

Аналогично, если мы используем объект как ключ в `Map`, то до тех пор, пока существует `Map`, также будет существовать и этот объект.

## WeakMap

Это совсем другая структура, которая не предотвращает удаление объектов сборщиком мусора.

Первое отличие от `Map` в том, что ключи в `WeakMap` должны быть объектами, а не примитивными значениями:

{% code overflow="wrap" %}
```javascript
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, "ok"); // работает (объект в качестве ключа)

// нельзя использовать строку в качестве ключа
weakMap.set("test", "Whoops"); // Invalid value used as weak map key, потому что "test" не объект
```
{% endcode %}

Теперь, если мы используем объект в качестве ключа и если больше нет ссылок на этот объект, то он будет удален из памяти и из `WeakMap` автоматически.

{% code overflow="wrap" %}
```javascript
let weakMap = new WeakMap();
let obj = {};
weakMap.set(obj, "ok"); // работает (объект в качестве ключа)

obj = null

console.log( weakMap.get(obj) ) // undefined
```
{% endcode %}

`WeakMap` не поддерживает перебор и методы `keys()`, `values()`, `entries()`, так что нет способа взять все ключи или значения из неё.

В `WeakMap` присутствуют только следующие методы:

* `weakMap.get(key)`
* `weakMap.set(key, value)`
* `weakMap.delete(key)`
* `weakMap.has(key)`

Зачем такое ограничение? Если объект станет недостижим, то он будет автоматически удалён сборщиком мусора. Но нет информации, _в какой момент произойдёт эта очистка_.

{% hint style="info" %}
Если мы работаем с объектом, который принадлежит другому коду или библиотеке, и хотим сохранить у себя данные для него, которые должны существовать пока существует этот объект, то `WeakMap` — лучшее решение.
{% endhint %}

{% code overflow="wrap" %}
```javascript
weakMap.set(john, "секретные документы");
// если john умрёт, "секретные документы" будут автоматически уничтожены
```
{% endcode %}

#### Пример

Предположим мы ведем учет посещений пользователей. Информация хранится в `Map`. Объект-пользователь — ключ, а кол-во визитов — значение. Когда пользователь нас покидает (его объект удаляется сборщиком мусора), то больше нет смысла хранить счетчик посещений.

{% code overflow="wrap" %}
```javascript
let visitsCountMap = new Map();

function countUser(user) {
        let count = visitsCountMap.get(user) || 0;
        visitsCountMap.set(user, count + 1)
}

let john = { name: "John" };
countUser(john); // ведём подсчёт посещений


john = null;
console.log(visitsCountMap) // Остается в памяти, так как является ключом в visitsCountMap
```
{% endcode %}

Нам же нужно очищать `visitsCountMap` при удалении объекта.

{% code overflow="wrap" %}
```javascript
let visitsCountMap = new WeakMap(); // map: пользователь => число визитов

// увеличиваем счётчик
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(user, count + 1);
}
```
{% endcode %}

Как проверить отличия `Map` от `WeakMap`:

{% code overflow="wrap" %}
```javascript
let visitsMap = new Map(); // Создаем Map
let visitsWeakMap = new WeakMap(); // Создаем WeakMap

// Счетчики
// -----------------------------------------------
function countMap(user) {
        let count = visitsMap.get(user) || 0;
        visitsMap.set(user, count + 1)
}
function countWeakMap(user) {
    let count = visitsWeakMap.get(user) || 0;
    visitsWeakMap.set(user, count + 1)
}
// -----------------------------------------------


// Создаем объекты и добавляем в Map и WeakMap
let john1 = { name: "John1" };
let john2 = { name: "John2" };

// Вызываем счетчики
countMap(john1);
countWeakMap(john2);


// Удаляем объекты
john1 = null;
john2 = null;

// Ждем пока выполнится сборка мусора. Надо запустить код несколько раз, так как нет гарантий, что сборщик мусора запустится с первого раза.
setTimeout(function(){
    console.log("3000ms после... ждем сборку мусора");
    console.log(visitsMap);
    console.log(visitsWeakMap);
    }, 3000);

setTimeout(function(){
    console.log("6000ms после... ждем сборку мусора");
    console.log(visitsMap);
    console.log(visitsWeakMap);
    }, 6000);

setTimeout(function(){
    console.log("10000ms после... ждем сборку мусора");
    console.log(visitsMap);
    console.log(visitsWeakMap);
    }, 10000);

setTimeout(function(){
    console.log("13000ms после... ждем сборку мусора");
    console.log(visitsMap);
    console.log(visitsWeakMap);
    }, 13000);
```
{% endcode %}

**Результат:**

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Здесь мы видим, что после того, как мы удалили объекты и подождали 13 секунд, сборщик мусора движка JavaScript удалил из `WeakMap` объект, но не удалил из `Map`.

{% hint style="info" %}
Теперь нет необходимости вручную очищать `visitsCountMap`. После того, как объект `john` стал недостижим другими способами, кроме как через `WeakMap`, он удаляется из памяти вместе с информацией по такому ключу из `WeakMap`.
{% endhint %}

## Применение для кэширования

`WeakMap` может использоваться для кэширования, когда результат вызова функции должен где-то запоминаться ("кэшироваться") для того, чтобы дальнейшие ее вызовы на том же объекте могли просто брать уже готовый результат, повторно используя его.

Пример с использованием `Map`:

{% code overflow="wrap" %}
```javascript
// 📁 cache.js
let cache = new Map();

// вычисляем и запоминаем результат
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* тут какие-то вычисления результата для объекта */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// Теперь используем process() в другом файле:

// 📁 main.js
let obj = {/* допустим, у нас есть какой-то объект */};

let result1 = process(obj); // вычислен результат

// ...позже, из другого места в коде...
let result2 = process(obj); // ранее вычисленный результат взят из кеша

// ...позже, когда объект больше не нужен:
obj = null;

alert(cache.size); // 1 (Упс! Объект всё ещё в кеше, занимает память!)
```
{% endcode %}

Многократные вызовы `process(obj)` с тем же самым объектом в качестве параметра ведут к тому, что результат вычисляется только в первый раз, а затем последующие вызовы берут его из кэша. Недостатком является то, что необходимо вручную очищать `cache` от ненужных объектов.

Однако если мы будем использовать `WeakMap` вместо `Map`, то эта проблема исчезнет: закэшированные результаты, которые уже не нужны, будут автоматически удалены из памяти сборщиком мусора.

{% code overflow="wrap" %}
```javascript
// 📁 cache.js
let cache = new WeakMap();

// вычисляем и запоминаем результат
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* вычисляем результат для объекта */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 📁 main.js
let obj = {/* какой-то объект */};

let result1 = process(obj);
let result2 = process(obj);

// ...позже, когда объект больше не нужен:
obj = null;

// Нет возможности получить cache.size, так как это WeakMap,
// но он равен 0 или скоро будет равен 0
// Когда сборщик мусора удаляет obj, связанные с ним данные из кеша тоже удаляются
```
{% endcode %}

## WeakSet

Коллекция `WeakSet` ведет себя похоже:

* Она аналогична `Set`, но мы можем добавлять в `WeakSet` только объекты.
* Объект присутствует во множестве только до тех пор, пока доступен где-то еще.
* Как и `Set`, она поддерживает `add`, `has` и `delete`, но не `size`, `keys()` и не является перебираемой.

Например, мы можем добавлять пользователей в `WeakSet` для учёта тех, кто посещал наш сайт:

{% code overflow="wrap" %}
```javascript
let visitedSet = new WeakSet();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

visitedSet.add(john); // John заходил к нам
visitedSet.add(pete); // потом Pete
visitedSet.add(john); // John снова

// visitedSet сейчас содержит двух пользователей

// проверим, заходил ли John?
console.log(visitedSet.has(john)); // true

// проверим, заходила ли Mary?
console.log(visitedSet.has(mary)); // false
console.log(visitedSet)

john = null;

// структура данных visitedSet будет очищена автоматически (объект john будет удалён из visitedSet)
```
{% endcode %}

Наиболее значительным ограничением `WeakMap` и `WeakSet` является то, что их нельзя перебрать или взять всё содержимое. Это может доставлять неудобства, но не мешает `WeakMap/WeakSet` выполнять их главную задачу – быть **дополнительным хранилищем** данных для объектов, управляемых из каких-то других мест в коде.
