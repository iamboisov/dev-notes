---
description: >-
  Конструкция "switch" представляет собой способ сравнить выражение сразу с
  несколькими вариантами.
---

# Конструкция "switch"

## Синтаксис

Эта конструкция имеет один или более блок `case` и необязательный блок `default`. Она заменяет собой сразу несколько `if`.

```javascript
switch (x) {
    case 'value 1':
    ...;
    [break];
    
    case 'value 2':
    ...;
    [break];
    
    case 'value 3':
    ...;
    [break];
    
    default:
    ...;
    [break];
}
```

* Переменная `x` проверяется на строгое равенство первому значению `value 1`, а затем второму `value 2` и так далее.
* Если соответствие установлено, то `switch` выполняется от следующей директивы `case` и далее, до ближайшего `break` (или до конца `switch`)
* Если ни одни вариант не сработал, то выполняется вариант `default`.

Пример использования конструкции:

```javascript
let x = "Телефон";
switch(x) {
case "Телевизор":
console.log("Телевизор");
break;

case "Приставка":
console.log("Приставка");
break;

case "Телефон":
console.log("Телефон");
break;

case "Монитор":
console.log("Монитор");
break;
}
```

Если нет `break`, то выполнение пойдет ниже по следующим `case`, при этом остальные проверки игнорируются.

{% hint style="info" %}
Любое выражение может быть аргументом для `switch/case`

```javascript
let x = "1";
let y = 0;

switch(+x) { // преобразуем единицу в число
case b+1: // прибавляем 1 к b
console.log("В точку!");
break;

default:
console.log("Код не выполнится")
}
```
{% endhint %}

## Группировка `case`

Несколько вариантов `case`, которые выполняют один код, можно сгруппировать:

{% code overflow="wrap" %}
```javascript
let x = 3;

switch(x) {
case 2:
console.log(2);
break;

case 3: // Выполнение case 3 начинается на этой строке, но продолжается в case 4, так как нет break
case 4:
console.log("3 или 4");
break;

case 5:
console.log(5);
break;
}
```
{% endcode %}

## Тип имеет значение

Нужно отметить, что проверка на равенство всегда строгая. Значения должны быть одного типа, чтобы равенство выполнилось.&#x20;

Пример мертвого кода:

```javascript
let x = "3";

switch(x) {
case 2:
console.log(2);
break;

case 3:
console.log(3);
break;

case 4:
console.log(4);
break;

default:
console.log(NaN) // Так равенство не выполнилось, то выполнится default
}
```
