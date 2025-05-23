---
title: Замыкания
slug: Web/JavaScript/Guide/Closures
---

{{jsSidebar("Intermediate")}}

Замыкание — это комбинация функции и лексического окружения, в котором эта функция была определена. Другими словами, замыкание даёт вам доступ к [Scope](/ru/docs/Glossary/Scope) внешней функции из внутренней функции. В JavaScript замыкания создаются каждый раз при создании функции, во время её создания.

## Лексическая область видимости

Рассмотрим следующий пример:

```js
function init() {
  var name = "Mozilla"; // name - локальная переменная, созданная в init
  function displayName() {
    // displayName() - внутренняя функция, замыкание
    alert(name); // displayName() использует переменную, объявленную в родительской функции
  }
  displayName();
}
init();
```

`init()` создаёт локальную переменную `name` и определяет функцию `displayName()`. `displayName()` — это внутренняя функция — она определена внутри `init()` и доступна только внутри тела функции `init()`. Обратите внимание, что функция `displayName()` не имеет никаких собственных локальных переменных. Однако, поскольку внутренние функции имеют доступ к переменным внешних функций, `displayName()` может иметь доступ к переменной `name`, объявленной в родительской функции `init()`.

{{JSFiddleEmbed("https://jsfiddle.net/78dg25ax/", "js,result", 250)}}

[Выполните](https://jsfiddle.net/xAFs9/3/) этот код и обратите внимание, что команда `alert()` внутри `displayName()` благополучно выводит на экран содержимое переменной `name` объявленной в родительской функции. Это пример так называемой лексической области видимости _(lexical_ _scoping)_: в JavaScript область действия переменной определяется по её расположению в коде (это очевидно _лексически_), и вложенные функции имеют доступ к переменным, объявленным вовне. Этот механизм и называется Lexical scoping (область действия, ограниченная лексически).

## Замыкание

Рассмотрим следующий пример:

```js
function makeFunc() {
  var name = "Mozilla";

  function displayName() {
    alert(name);
  }

  return displayName;
}

var myFunc = makeFunc();
myFunc();
```

Если выполнить этот код, то результат будет такой же, как и выполнение `init()` из предыдущего примера: строка "Mozilla" будет показана в JavaScript alert диалоге. Что отличает этот код и представляет для нас интерес, так это то, что внутренняя функция `displayName()` была возвращена из внешней до того, как была выполнена.

На первый взгляд, кажется неочевидным, что этот код правильный, но он работает. В некоторых языках программирования локальные переменные-функции существуют только во время выполнения этой функции. После завершения выполнения `makeFunc()` можно ожидать, что переменная _name_ больше не будет доступна. Однако, поскольку код продолжает нормально работать, очевидно, что это не так в случае JavaScript.

Причина в том, что функции в JavaScript формируют так называемые _замыкания_. _Замыкание_ — это комбинация функции и лексического окружения, в котором эта функция была объявлена. Это окружение состоит из произвольного количества локальных переменных, которые были в области действия функции во время создания замыкания. В рассмотренном примере `myFunc` — это ссылка на экземпляр функции `displayName`, созданной в результате выполнения `makeFunc`. Экземпляр функции `displayName` в свою очередь сохраняет ссылку на своё лексическое окружение, в котором есть переменная `name`. По этой причине, когда происходит вызов функции `myFunc`, переменная `name` остаётся доступной для использования и сохранённый в ней текст "Mozilla" передаётся в `alert`.

А вот немного более интересный пример — функция `makeAdder`:

```js
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2)); // 7
console.log(add10(2)); // 12
```

Здесь мы определили функцию `makeAdder(x)`, которая получает единственный аргумент `x` и возвращает новую функцию. Эта функция получает единственный аргумент `y` и возвращает сумму `x` и `y`.

По существу `makeAdder` — это фабрика функций: она создаёт функции, которые могут прибавлять определённое значение к своему аргументу. В примере выше мы используем нашу фабричную функцию для создания двух новых функций — одна прибавляет 5 к своему аргументу, вторая прибавляет 10.

`add5` и `add10` — это примеры замыканий. Эти функции делят одно определение тела функции, но при этом они сохраняют различные окружения. В окружении функции `add5` `x` — это 5, в то время как в окружении `add10` `x` — это 10.

## Замыкания на практике

Замыкания полезны тем, что позволяют связать данные (лексическое окружение) с функцией, которая работает с этими данными. Очевидна параллель с объектно-ориентированным программированием, где объекты позволяют нам связать некоторые данные (свойства объекта) с одним или несколькими методами.

Следовательно, замыкания можно использовать везде, где вы обычно использовали объект с одним единственным методом.

Такие ситуации повсеместно встречаются в web-разработке. Большое количество front-end кода, который мы пишем на JavaScript, основано на обработке событий. Мы описываем какое-то поведение, а потом связываем его с событием, которое создаётся пользователем (например, клик мышкой или нажатие клавиши). При этом наш код обычно привязывается к событию в виде обратного/ответного вызова (callback): _callback функция - функция выполняемая в ответ на возникновение события_.

Давайте рассмотрим практический пример: допустим, мы хотим добавить на страницу несколько кнопок, которые будут менять размер текста. Как вариант, мы можем указать свойство font-size на элементе body в пикселах, а затем устанавливать размер прочих элементов страницы (таких, как заголовки) с использованием относительных единиц em:

```css
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 12px;
}

h1 {
  font-size: 1.5em;
}

h2 {
  font-size: 1.2em;
}
```

Тогда наши кнопки будут менять свойство font-size элемента body, а остальные элементы страницы просто получат это новое значение и отмасштабируют размер текста благодаря использованию относительных единиц.

Используем следующий JavaScript:

```js
function makeSizer(size) {
  return function () {
    document.body.style.fontSize = size + "px";
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);
```

Теперь `size12`, `size14`, и `size16` - это функции, которые меняют размер текста в элементе body на значения 12, 14, и 16 пикселов, соответственно. После чего мы цепляем эти функции на кнопки примерно так:

```js
document.getElementById("size-12").onclick = size12;
document.getElementById("size-14").onclick = size14;
document.getElementById("size-16").onclick = size16;
```

```html
<a href="#" id="size-12">12</a>
<a href="#" id="size-14">14</a>
<a href="#" id="size-16">16</a>
```

{{JSFiddleEmbed("https://jsfiddle.net/vnkuZ/7726/","","200")}}

## Эмуляция частных (private) методов с помощью замыканий

Языки вроде Java позволяют нам объявлять частные (private) методы . Это значит, что они могут быть вызваны только методами того же класса, в котором объявлены.

JavaScript не имеет встроенной возможности сделать такое, но это можно эмулировать с помощью замыкания. Частные методы полезны не только тем, что ограничивают доступ к коду, это также мощное средство глобальной организации пространства имён, позволяющее не засорять публичный интерфейс вашего кода внутренними методами классов.

Код ниже иллюстрирует, как можно использовать замыкания для определения публичных функций, которые имеют доступ к закрытым от пользователя (private) функциям и переменным. Такая манера программирования называется [модульное программирование](https://www.google.com/search?q=javascript+module+pattern):

```js
var Counter = (function () {
  var privateCounter = 0;

  function changeBy(val) {
    privateCounter += val;
  }

  return {
    increment: function () {
      changeBy(1);
    },
    decrement: function () {
      changeBy(-1);
    },
    value: function () {
      return privateCounter;
    },
  };
})();

alert(Counter.value()); /* Alerts 0 */

Counter.increment();
Counter.increment();

alert(Counter.value()); /* Alerts 2 */

Counter.decrement();

alert(Counter.value()); /* Alerts 1 */
```

Тут много чего поменялось. В предыдущем примере каждое замыкание имело свой собственный контекст исполнения (окружение). Здесь мы создаём единое окружение для трёх функций: `Counter.increment`, `Counter.decrement`, и `Counter.value`.

Единое окружение создаётся в теле анонимной функции, которая исполняется в момент описания. Это окружение содержит два приватных элемента: переменную `privateCounter` и функцию `changeBy(val)`. Ни один из этих элементов не доступен напрямую, за пределами этой самой анонимной функции. Вместо этого они могут и должны использоваться тремя публичными функциями, которые возвращаются анонимным блоком кода (anonymous wrapper), выполняемым в той же анонимной функции.

Эти три публичные функции являются замыканиями, использующими общий контекст исполнения (окружение). Благодаря механизму lexical scoping в Javascript, все они имеют доступ к переменной `privateCounter` и функции `changeBy`.

Заметьте, мы описываем анонимную функцию, создающую счётчик, и тут же запускаем её, присваивая результат исполнения переменной `Counter`. Но мы также можем не запускать эту функцию сразу, а сохранить её в отдельной переменной, чтобы использовать для дальнейшего создания нескольких счётчиков вот так:

```js
var makeCounter = function () {
  var privateCounter = 0;

  function changeBy(val) {
    privateCounter += val;
  }

  return {
    increment: function () {
      changeBy(1);
    },
    decrement: function () {
      changeBy(-1);
    },
    value: function () {
      return privateCounter;
    },
  };
};

var Counter1 = makeCounter();
var Counter2 = makeCounter();

alert(Counter1.value()); /* Alerts 0 */

Counter1.increment();
Counter1.increment();

alert(Counter1.value()); /* Alerts 2 */

Counter1.decrement();

alert(Counter1.value()); /* Alerts 1 */
alert(Counter2.value()); /* Alerts 0 */
```

Заметьте, что счётчики работают независимо друг от друга. Это происходит потому, что у каждого из них в момент создания функцией `makeCounter()` также создавался свой отдельный контекст исполнения (окружение). То есть приватная переменная `privateCounter` в каждом из счётчиков это действительно отдельная, самостоятельная переменная.

Используя замыкания подобным образом, вы получаете ряд преимуществ, обычно ассоциируемых с объектно-ориентированным программированием, таких как изоляция и инкапсуляция.

## Создание замыканий в цикле: Очень частая ошибка

До того, как в версии ECMAScript 6 ввели ключевое слово [`let`](/ru/docs/Web/JavaScript/Reference/Statements/let), постоянно возникала следующая проблема при создании замыканий внутри цикла. Рассмотрим пример:

```html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email" /></p>
<p>Name: <input type="text" id="name" name="name" /></p>
<p>Age: <input type="text" id="age" name="age" /></p>
```

```js
function showHelp(help) {
  document.getElementById("help").innerHTML = help;
}

function setupHelp() {
  var helpText = [
    { id: "email", help: "Ваш адрес e-mail" },
    { id: "name", help: "Ваше полное имя" },
    { id: "age", help: "Ваш возраст (Вам должно быть больше 16)" },
  ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function () {
      showHelp(item.help);
    };
  }
}

setupHelp();
```

{{JSFiddleEmbed("https://jsfiddle.net/v7gjv/13026/", "", 200)}}

Массив `helpText` описывает три подсказки для трёх полей ввода. Цикл пробегает эти описания по очереди и для каждого из полей ввода определяет, что при возникновении события `onfocus` для этого элемента должна вызываться функция, показывающая соответствующую подсказку.

Если вы запустите этот код, то увидите, что он работает не так, как мы ожидаем интуитивно. Какое поле вы бы ни выбрали, в качестве подсказки всегда будет высвечиваться сообщение о возрасте.

Проблема в том, что функции, присвоенные как обработчики события `onfocus`, являются замыканиями. Они состоят из описания функции и контекста исполнения (окружения), унаследованного от функции `setupHelp`. Было создано три замыкания, но все они были созданы с одним и тем же контекстом исполнения. К моменту возникновения события `onfocus` цикл уже давно отработал, а значит, переменная `item` (одна и та же для всех трёх замыканий) указывает на последний элемент массива, который как раз в поле возраста.

В качестве решения в этом случае можно предложить использование функции, фабричной функции (function factory), как уже было описано выше в примерах:

```js
function showHelp(help) {
  document.getElementById("help").innerHTML = help;
}

function makeHelpCallback(help) {
  return function () {
    showHelp(help);
  };
}

function setupHelp() {
  var helpText = [
    { id: "email", help: "Ваш адрес e-mail" },
    { id: "name", help: "Ваше полное имя" },
    { id: "age", help: "Ваш возраст (Вам должно быть больше 16)" },
  ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}

setupHelp();
```

{{JSFiddleEmbed("https://jsfiddle.net/v7gjv/13024/", "", 200)}}

Вот это работает как следует. Вместо того, чтобы делить на всех одно окружение, функция `makeHelpCallback` создаёт каждому из замыканий своё собственное, в котором переменная `item` указывает на правильный элемент массива `helpText`.

## Соображения по производительности

Не нужно без необходимости создавать функции внутри функций в тех случаях, когда замыкания не нужны. Использование этой техники увеличивает требования к производительности как в части скорости, так и в части потребления памяти.

Как пример, при написании нового класса есть смысл помещать все методы в прототип его объекта, а не описывать их в тексте конструктора. Если сделать по-другому, то при каждом создании объекта для него будет создан свой экземпляр каждого из методов, вместо того, чтобы наследовать их из прототипа.

Давайте рассмотрим не очень практичный, но показательный пример:

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();

  this.getName = function () {
    return this.name;
  };

  this.getMessage = function () {
    return this.message;
  };
}
```

Поскольку вышеприведённый код никак не использует преимущества замыканий, его можно переписать следующим образом:

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}

MyObject.prototype = {
  getName: function () {
    return this.name;
  },
  getMessage: function () {
    return this.message;
  },
};
```

Методы вынесены в прототип. Тем не менее, переопределять прототип — само по себе является плохой привычкой, поэтому давайте перепишем всё так, чтобы новые методы просто добавились к уже существующему прототипу.

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}

MyObject.prototype.getName = function () {
  return this.name;
};

MyObject.prototype.getMessage = function () {
  return this.message;
};
```

Код выше можно сделать аккуратнее:

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}

(function () {
  this.getName = function () {
    return this.name;
  };

  this.getMessage = function () {
    return this.message;
  };
}).call(MyObject.prototype);
```

В обоих примерах выше методы определяются один раз — в прототипе. И все объекты, использующие данный прототип, будут использовать это определение без дополнительного расхода вычислительных ресурсов. Смотрите подробное описание в статье [Подробнее об объектной модели](/ru/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain).

{{PreviousNext("Web/JavaScript/Equality_comparisons_and_sameness")}}
