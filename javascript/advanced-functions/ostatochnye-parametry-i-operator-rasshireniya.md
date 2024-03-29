---
description: О передаваемых аргументах и массивах
---

# Остаточные параметры и оператор расширения

Многие встроенные функции языка поддерживают произвольное количество аргументов.

Например:

* `Math.max(arg1, arg2, ..., argN)` – вычисляет максимальное число из переданных.
* `Object.assign(dest, src1, ..., srcN)` – копирует свойства из исходных объектов `src1..N` в целевой объект `dest`.
* …и так далее.

## Остаточные параметры

Вызывать функцию можно  с любым количеством параметров независимо от того, как она была определена. Лишние аргументы не вызовут ошибку, но не будут учитываться при выполнении.

Остаточные параметры могут быть обозначены через `...`

Эти три точки означают "собери оставшиеся параметры в массив".

```javascript
function sumAll(...args) { // args — имя массива
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

alert( sumAll(1) ); // 1
alert( sumAll(1, 2) ); // 3
alert( sumAll(1, 2, 3) ); // 6
```

Мы можем положить первые несколько параметров в переменные, а остальные – собрать в массив.

{% hint style="warning" %}
Остаточные параметры должны быть в конце. Бессмысленно писать что-либо после них. Это вызовет ошибку.
{% endhint %}

## Переменная arguments

Все аргументы функции находятся в псевдомассиве arguments под своими порядковыми номерами.

```javascript
function showName() {
  alert( arguments[0] );
  alert( arguments[1] );

  // Объект arguments можно перебирать
  for (let arg of arguments) console.log(arg);
}

// Вывод: 2, Юлий, Цезарь
showName("Первый", "Второй");

// Вывод: 1, Третий, undefined (второго аргумента нет)
showName("Третий");
```

Но у него есть один недостаток. Хотя `arguments` похож на массив, и его тоже можно перебирать, это всё же не массив. Он не поддерживает методы массивов, поэтому мы не можем, например, вызвать `arguments.map(...)`.

К тому же, `arguments` всегда содержит все аргументы функции — мы не можем получить их часть. А остаточные параметры позволяют это сделать.

{% hint style="info" %}
Стрелочные функции не имеют `"arguments".`
{% endhint %}

Как мы помним, у стрелочных функций нет собственного `this`. Теперь мы знаем, что нет и своего объекта `arguments`.

Если мы обратимся к `arguments` из стрелочной функции, то получим аргументы внешней «нормальной» функции.

Пример:

```javascript
function f() {
  let showArg = () => alert(arguments[0]);
  showArg(2);
}

f(1); // 1
```

## Оператор расширения

Когда `...arr` используется при вызове функции, он «расширяет» перебираемый объект `arr` в список аргументов.

```javascript
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(...arr1, ...arr2) ); // 8
```

Мы даже можем комбинировать оператор расширения с обычными значениями:

```javascript
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25
```

Например, оператор расширения подойдёт для того, чтобы превратить строку в массив символов:

```javascript
let str = "Привет";

alert( [...str] ); // П,р,и,в,е,т
```

{% hint style="warning" %}
Оператор расширения работает только с итерируемыми объектами.
{% endhint %}
