---
description: Область видимости переменных и замыкание
---

# Область видимости и замыкание

## Блоки кода

Если переменная объявлена внутри блока кода `{...}`, то она видна только внутри этого блока.

{% code overflow="wrap" %}
```javascript
{
  let message = "Hello"; // переменная видна только в этом блоке
  alert(message); // Hello
}

alert(message); // ReferenceError: message is not defined
```
{% endcode %}

Для `if`, `for`, `while` и т.д. переменные, объявленные в блоке кода `{...}`, также видны только внутри:

{% code overflow="wrap" %}
```javascript
if (true) {
  let phrase = "Hello";

  alert(phrase); // Hello
}

alert(phrase); // Ошибка
```
{% endcode %}

{% hint style="info" %}
Переменная, объявленная внутри `(...)`, считается частью блока.
{% endhint %}

## Вложенные функции

{% code overflow="wrap" %}
```javascript
function makeCounter() {
  let count = 0;
  return () => count++;
}

let counter = makeCounter()

console.log(counter()) // 0
console.log(counter()) // 1
```
{% endcode %}

Как вообще счетчик увеличивается? Сейчас разберемся..

## Лексическое окружение

### Переменные

В JavaScript у каждой выполняемой функции, блока кода `{...}` и скрипта есть связанный с ними внутренний (скрытый) объект, называемый лексическим окружением `LexicalEnvironment`.

Объект лексического окружения состоит из:

1. Environment record — объект, в котором как свойства хранятся все локальные переменные
2. Ссылка на внешнее лексическое окружение — то есть то, которое соответствует коду снаружи

Переменная — это свойство специального внутреннего объекта: Environment Record.

"Получить или изменить переменную" — означает "получить или изменить свойство этого объекта".

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```javascript
// Упрощенный пример того, как работает лексическое окружение
let LexicalEnvironment = {
  EnvironmentRecord: {
    variable: "John"
  }
};

let variable = "John";
// То же самое, что и LexicalEnvironment.EnvironmentRecord.variable
// То есть просто считываем свойтсво из объекта

// ---------------------------------------------------------
console.log(variable)
console.log(LexicalEnvironment.EnvironmentRecord.variable)
```
{% endcode %}

* Переменная – это свойство специального внутреннего объекта, связанного с текущим выполняющимся блоком/функцией/скриптом.
* Работа с переменными – это на самом деле работа со свойствами этого объекта.

{% hint style="info" %}
«Лексическое окружение» – это объект спецификации: он существует только «теоретически» в спецификации языка для описания того, как все работает. Мы не можем получить этот объект в нашем коде и манипулировать им напрямую.
{% endhint %}

### Function Declaration

Функция — это тоже значение, что и переменная.

Разница заключается в том, что Function Declaration мгновенно инициализируется полностью.

Когда создается лексическое окружение, Function Declaration сразу же становится функцией, готовой к использованию (в отличие от `let`, который до момента объявления не может быть использован).

Именно поэтому мы можем вызвать функцию, объявленную как Function Declaration, до самого её объявления.

### Внутреннее и внешнее лексическое окружение

Когда запускается выполнение функции, в начале ее вызова автоматически создается новое лексическое окружение для хранения локальных переменных и параметров вызова.

Пример для say("John"):

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Пример:

У нас есть функция, которая выводит на экран переменную

{% code overflow="wrap" %}
```javascript
function say(variable) {
  console.log(variable)
}

say("Hello") // Hello
```
{% endcode %}

Исходя из пояснения про внутреннее и внешнее лексическое окружение, предоставляю код того, как это примерно работает под капотом (я не заглядывал в спецификацию, но написал код для понимания, что происходит при выполнении функции):

{% code overflow="wrap" %}
```javascript
let LexicalEnvironment = {

  EnvironmentRecord: {
    say: function(variable) {
    
      let functionLexicalEnvironment = {
        EnvironmentRecord: {
          name: variable
        }
      }
      
      console.log(functionLexicalEnvironment.EnvironmentRecord.name)
    },
    variable: "Hello"
  },

  ExternalLinks: null
};
```
{% endcode %}

Теперь давайте разбираться что здесь происходит:

{% code overflow="wrap" %}
```javascript
// Создается лексическое окружение (объект)
let LexicalEnvironment = {

// Одно из свойств нужно для хранения переменных и параметров вызова
  EnvironmentRecord: {
    say: function(variable) {
    
    // Лексическое окружение для функции
      let functionLexicalEnvironment = {
      
      // Для хранения переменных и параметров вызова
        EnvironmentRecord: {
          name: variable
        },
      
      // Для ссылок на внешнее окружение  
        ExternalLinks: null
      }

      console.log(functionLexicalEnvironment.EnvironmentRecord.name)
    },
    variable: "Hello"
  },

// Другое свойство нужно для доступа ко внешнему коду
  ExternalLinks: null
};
```
{% endcode %}

По сути эти два вызова функции одно и то же:

{% code overflow="wrap" %}
```javascript
say("Hello") // Hello
LexicalEnvironment.EnvironmentRecord.say("Hello"); // Hello
```
{% endcode %}

В процессе вызова у нас есть два лексических окружения:

**Внутреннее (для вызываемой функции):**

В нем находится переменная name, аргумент функции. Мы вызываем `say("Hello")`, так что значение переменной name равно `"Hello"`

**Внешнее (глобальное лексическое окружение):**

В нем находится переменная `variable` со значением `"Hello"` и сама функция.

У внутреннего лексического окружения есть ссылка на внешнее `outer`.

Когда код хочет получить доступ к переменной — сначала происходит поиск во внутреннем лексическом окружении, затем во внешнем, а затем в следующем и так далее до глобального.

Если переменная не была найдена, то будет ошибка в строгом режиме (use strict). Без строгого режима, для обратной совместимости, присваивание несуществующей переменной создает новую глобальную переменную с таким же именем.

## Возврат функции

Вернемся к примеру со счетчиком:

{% code overflow="wrap" %}
```javascript
function makeCounter() {
    let count = 0;
  
    return function() {
      return count++;
    };
  }
  
  let counter = makeCounter();
```
{% endcode %}

В начале каждого вызова `makeCounter()` создается новый объект лексического окружения, в котором хранятся переменные для конкретного запуска `makeCounter()`.

Таким образом у нас два вложенных лексических окружения:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Во время выполнения makeCounter() создается крошечная вложенная функция, состоящая из одной строки `return count++`. Мы ее не запускаем, а только создаем.

Все функции помнят лексическое окружение, в котором они были созданы. Технически они имеют скрытое свойство `[[Environment]]`, которое хранит ссылку на внешнее лексическое окружение.

Таким образом `counter[[Environment]]` имеет ссылку на `{count: 0}` лексического окружения.

{% code overflow="wrap" %}
```javascript
function makeCounter() {
    let count = 0;
  
    return function() {
      return count++;
    };
  }
  
  // Создается лексическое окружение с переменной и функцией, которая имеет внутреннее лексическое окружение с переменной и вложенной функцией.
  let counter = makeCounter();
```
{% endcode %}

Ссылка на `[[Environment]]` устанавливается только один раз при создании функции.

При вызове counter() создается новое лексическое окружение, а его внешняя ссылка на лексическое окружение берется из `counter.[[Environment]]`.

Представим, что мы вызвали функцию `counter()` 3 раза. Что происходит под капотом языка: (это примерная абстракция того, как работает код со счетчиком)

{% code overflow="wrap" %}
```javascript
let anotherLexEnvironment = {
    EnvironmentRecord: {
        count: 0,
        makeCounter: function() {
          return this.count++
        }
    },
    ExternalLinks: null
}

let LexicalEnvironment = {
    
    EnvironmentRecord: {
      counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
    },

    ExternalLinks: null
  };

let LexicalEnvironment2 = {
    
  EnvironmentRecord: {
    counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
  },

  ExternalLinks: null
};

let LexicalEnvironment3 = {
    
  EnvironmentRecord: {
    counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
  },

  ExternalLinks: null
};

console.log(LexicalEnvironment.EnvironmentRecord.counter)
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n")

console.log(LexicalEnvironment2.EnvironmentRecord.counter)
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n")

console.log(LexicalEnvironment3.EnvironmentRecord.counter)
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n")
```
{% endcode %}

Давайте разбираться:

{% code overflow="wrap" %}
```javascript
// Создается лексическое окружение для функции makeCounter()
let anotherLexEnvironment = {
    EnvironmentRecord: {
        count: 0,
        makeCounter: function() {
          return this.count++
        }
    },
    ExternalLinks: null
}


// Создается лексическое окружение для функции counter(), которая ссылается на лексическое окружение anotherLexEnvironment
let LexicalEnvironment = {
    
    EnvironmentRecord: {
    // Свойство, которое ссылается на свойство другого объекта
      counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
    },

    ExternalLinks: null
  };

// Дубликат функции
let LexicalEnvironment2 = {
    
  EnvironmentRecord: {
    counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
  },

  ExternalLinks: null
};

// Дубликат функции
let LexicalEnvironment3 = {
    
  EnvironmentRecord: {
    counter: anotherLexEnvironment.EnvironmentRecord.makeCounter()
  },

  ExternalLinks: null
};


console.log(LexicalEnvironment.EnvironmentRecord.counter) // 0. То же самое, что и counter()
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n") // 3. Потому что мы создали 3 лексических окружения, которые ссылаются на функцию, которая прибавляет 1 каждый раз. См. постфиксный инкремент ++

console.log(LexicalEnvironment2.EnvironmentRecord.counter) // 1. То же самое, что и counter()
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n")

console.log(LexicalEnvironment3.EnvironmentRecord.counter) // 2. То же самое, что и counter()
console.log(anotherLexEnvironment.EnvironmentRecord.count, "\n")
```
{% endcode %}

Теперь, когда код внутри `counter()` ищет переменную `count`, он сначала ищет ее в собственном лексическом окружении (пустом, так как там нет локальных переменных), а затем в лексическом окружении внешнего вызова `makeCounter()`, где находит `count` и изменяет ее.

**Переменная обновляется в том лексическом окружении, в котором она существует.**

## Замыкания

Замыкание — это функция, которая запоминает свои внешние переменные и может получить к ним доступ. В JavaScript все функции изначально являются замыканиями. То есть они автоматически запоминают где были созданы, с помощью скрытого свойства `[[Environment]]` и могут получить доступ ко внешним данным.

## Сборка мусора

Обычно лексическое окружение удаляется из памяти вместе со всеми переменными после завершения вызова функции. Это связано с тем, что на него нет ссылок. Как и любой объект в JS, оно хранится в памяти до тех пор, пока к нему можно обратиться.

Однако если существует вложенная функция, которая доступна извне, то она имеет свойство `[[Environment]]` ссылающееся на лексическое окружение.

В этом случае лексическое окружение остается доступным даже после завершения работы функции.

{% code overflow="wrap" %}
```javascript
function f() {
  let value = 123;

  return function() {
    alert(value);
  }
}

let g = f(); // g.[[Environment]] хранит ссылку на лексическое окружение
// из соответствующего вызова f()
```
{% endcode %}

В приведенном ниже коде после удаления вложенной функции ее окружающее лексическое окружение (а значит, и `value`) очищается из памяти:

{% code overflow="wrap" %}
```javascript
function f() {
  let value = 123;

  return function() {
    alert(value);
  }
}

let g = f(); // пока существует функция g, value остается в памяти

g = null; // ...и теперь память очищена.
```
{% endcode %}

## Оптимизация

Как мы видели, в теории, пока функция жива, все внешние переменные тоже сохраняются.

Но на практике движки JavaScript пытаются это оптимизировать. Они анализируют использование переменных и, если легко по коду понять, что внешняя переменная не используется – она удаляется.

**Одним из важных побочных эффектов в V8 (Chrome, Edge, Opera) является то, что такая переменная становится недоступной при отладке.**

{% code overflow="wrap" %}
```javascript
function f() {
  let value = Math.random();

  function g() {
    debugger; // в консоли: напишите alert(value); Такой переменной нет!
  }

  return g;
}

let g = f();
g();
```
{% endcode %}

В теории, она должна быть доступна, но попала под оптимизацию движка.

Это может приводить к забавным (если удаётся решить быстро) проблемам при отладке. Одна из них – мы можем увидеть не ту внешнюю переменную при совпадающих названиях:

{% code overflow="wrap" %}
```javascript
let value = "Сюрприз!";

function f() {
  let value = "ближайшее значение";

  function g() {
    debugger; // в консоли: напишите alert(value); Сюрприз!
  }

  return g;
}

let g = f();
g();
```
{% endcode %}

Пример замыкания. Необходимо вывести сумму чисел:

{% code overflow="wrap" %}
```javascript
function sum(a) {
  return (b) => { return a + b} // Берем переменную из внешнего лексического окружения
}

console.log(sum(1)(2)) // Используем двойные круглые скобки, чтобы передать параметры
```
{% endcode %}

Пример с подвохом:

{% code overflow="wrap" %}
```javascript
let x = 1;

function func() {
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 2; // С самого начала переменная видна, но не инициализирована
}

func();
```
{% endcode %}

