---
description: Планирование вызова функций
---

# Планирование: setTimeout и setInterval

Мы можем вызвать функции не прямо сейчас, а позже с помощью двух методов:

* `setTimeout` позволяет вызвать функцию один раз через определенный промежуток времени
* `setInterval` позволяет вызвать функцию, повторяя вызов через определенный интервал времени.

## `setTimeout`

Синтаксис:

{% code overflow="wrap" %}
```javascript
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...);
```
{% endcode %}

Параметры:

`func|code` Функция или строка кода для выполнения. Обычно это функция. По историческим причинам можно передать и строку кода, но это не рекомендуется.

`delay` Задержка перед запуском в миллисекундах (1000 мс = 1 с). Значение по умолчанию – 0.

`arg1`, `arg2`…Аргументы, передаваемые в функцию.

{% code overflow="wrap" %}
```javascript
function sayHi(a, b) {
  console.log(a + b)
}

setTimeout(sayHi, 1000, 1, 2); // 3
```
{% endcode %}

Можно еще поиграться с деструктуризацией:

{% code overflow="wrap" %}
```javascript
function sayHi({a, b}) {
  console.log(a, b)
}

setTimeout(sayHi, 500, {b:2, a:5}); // Просто меняем местами аргументы
```
{% endcode %}

Это также будет работать:

{% code overflow="wrap" %}
```javascript
setTimeout("alert('Привет')", 1000);
```
{% endcode %}

Но использование строк не рекомендуется. Вместо этого используйте функции. Например, так:

{% code overflow="wrap" %}
```javascript
setTimeout(() => alert('Привет'), 1000);
```
{% endcode %}

{% hint style="info" %}
Передавайте функцию, но не запускайте ее!

Начинающие разработчики иногда ошибаются, добавляя скобки `()` после функции:

{% code overflow="wrap" %}
```javascript
// неправильно!
setTimeout(sayHi(), 1000);
```
{% endcode %}

Это не работает, потому что `setTimeout` ожидает ссылку на функцию. Здесь `sayHi()` запускает выполнение функции, и _результат выполнения_ отправляется в `setTimeout`. В нашем случае результатом выполнения `sayHi()` является `undefined` (так как функция ничего не возвращает), поэтому ничего не планируется.
{% endhint %}

## Отмена через `clearTimeout`

Вызов `setTimeout` возвращает "идентификатор таймера" `timerId`, который можно использовать для отмены дальнейшего выполнения.

Синтаксис для отмены:

{% code overflow="wrap" %}
```javascript
let timerId = setTimeout(...);
clearTimeout(timerId);
```
{% endcode %}

## `setInterval`

Имеет такой же синтаксис как и `setTimeout`:

{% code overflow="wrap" %}
```javascript
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...);
```
{% endcode %}

Все аргументы имеют такое же значение. Но отличие этого метода от `setTimeout` в том, что функция запускается не один раз, а периодически через указанный интервал времени.

Чтобы остановить дальнейшее выполнение функции, необходимо вызвать `clearInterval(timerId)`.

Следующий пример выводит сообщение каждые 2 секунды. Через 5 секунд вывод прекращается:

{% code overflow="wrap" %}
```javascript
// повторить с интервалом 1 секунду
let timerId = setInterval(() => console.log('tick'), 1000);

// остановить вывод через 5 секунд
setTimeout(() => { clearInterval(timerId); console.log('stop'); }, 5000);
```
{% endcode %}

