---
title: Возвращаемые значения функций
slug: Learn_web_development/Core/Scripting/Return_values
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/JavaScript/Building_blocks/Build_your_own_function","Learn/JavaScript/Building_blocks/События", "Learn/JavaScript/Building_blocks")}}

Для нас в этом курсе имеется ещё один важный момент. Посмотрим внимательнее на возвращаемое значение функций. Некоторые функции не возвращают существенное значение после завершения, но некоторые возвращают, и важно понимать что это за значение и как использовать его в своём коде и как сделать так чтобы ваши собственные функции возвращали полезные значения. Мы объясним всё это ниже.

| Необходимые навыки: | Базовая компьютерная грамотность, знание основ HTML и CSS, [JavaScript first steps](/ru/docs/conflicting/Learn_web_development/Core/Scripting), [Functions — reusable blocks of code](/ru/docs/Learn_web_development/Core/Scripting/Functions). |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Цели:               | Понять что такое возвращаемое значение функции и как его использовать.                                                                                                                                                                          |

## Что из себя представляют возвращаемые значения?

**Возвращаемые значения** - это на самом деле просто значения, которые функция возвращает после своего завершения. Вы уже неоднократно встречали возвращаемые значения, хотя, возможно, и не осознавали этого. Напишем небольшой код:

```js
var myText = "I am a string";
var newString = myText.replace("string", "sausage");
console.log(newString);
// функция replace() принимает строку,
// заменяет одну подстроку другой и возвращает
// новую строку со сделанными заменами
```

Мы уже видели этот блок кода в нашей первой статье про функции. Мы вызываем функцию [replace()](/ru/docs/Web/JavaScript/Reference/Global_Objects/String/replace) на строке `myText` и передаём ей 2 параметра — заменяемую подстроку и подстроку, которой будем заменять. Когда функция завершит выполнение, она вернёт значение, которым является новая строка со сделанными в ней заменами. В коде выше мы сохраняем это возвращаемое значение как значение переменной `newString`.

Если вы посмотрите на функцию replace() на MDN reference page, вы увидите секцию под названием [Return value](/ru/docs/Web/JavaScript/Reference/Global_Objects/String/replace#return_value). Очень важно знать и понимать какие значения возвращаются функциями, так что мы пытаемся включать эту информацию везде, где это возможно.

Некоторые функции не возвращают значения( на наших reference pages, возвращаемое значение обозначено как `void` или `undefined` в таких случаях). Например, в функции [displayMessage()](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/functions/function-stage-4.html#L50) которую мы сделали в прошлой статье, в результате выполнения функции не возвращается никакого значения. Функция всего лишь отображает что-то где-то на экране.

В основном, возвращаемое значение используется там, где функция является чем-то вроде вспомогательного звена при вычислениях. Вы хотите получить результат, который включает в себя некоторые значения. Эти значения вычисляются функцией, которая возвращает результат так, что он может быть использован в следующих стадиях вычисления.

### Использование возвращаемых значений в ваших собственных функциях

Чтобы вернуть значение своей функции, вы должны использовать ключевое слово [return](/ru/docs/Web/JavaScript/Reference/Statements/return). Мы видели это в действии недавно - в нашем примере [random-canvas-circles.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/loops/random-canvas-circles.html). Наша функция `draw()` отрисовывает где-то на экране 100 случайных кружков.

{{htmlelement("canvas")}}:

```js
function draw() {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);
  for (var i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgba(255,0,0,0.5)";
    ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
    ctx.fill();
  }
}
```

Внутри каждой итерации есть 3 вызова функции `random()`. Это сделано чтобы сгенерировать случайное значение для текущей координаты x, y и для радиуса. Функция `random()` принимает 1 параметр (целое число) и возвращает случайное число в диапазоне от 0 до этого числа. Выглядит это вот так:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Тоже самое может быть написано вот так:

```js
function random(number) {
  var result = Math.floor(Math.random() * number);
  return result;
}
```

Но первую версию написать быстрее и она более компактна.

Мы возвращаем результат вычисления `Math.floor(Math.random()*number)` каждый раз когда функция вызывается. Это возвращаемое значение появляется в момент вызова функции и код продолжается. Так, например, если мы выполним следующую строчку:

```js
ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
```

и 3 вызова `random()` вернут значения 500, 200 и 35, соответственно, строчка будет выполнена как если бы она была такой:

```js
ctx.arc(500, 200, 35, 0, 2 * Math.PI);
```

Сначала выполняются вызовы функции `random()`, на место которых подставляются возвращаемые ей значения, а затем выполнятся сама строка.

## Активное обучение: наша собственная, возвращающая значение функция

Теперь напишем нашу собственную возвращающую значение функцию.

1. Первым делом, сделайте локальную копию файла [function-library.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/functions/function-library.html) из GitHub. Это простая HTML страничка, содержащая текстовое поле {{htmlelement("input")}} и параграф Также там есть элемент {{htmlelement("script")}} в котором мы храним в 2ух переменных ссылки на оба HTML-элемента. Это маленькая страничка позволит вам ввести число в text box и отобразит различные, относящиеся к нему числа в параграфе ниже.
2. Теперь добавим несколько полезных функций в элемент `<script>` . Ниже двух существующих строчек JavaScript, добавьте следующие описания функций:

   ```js
   function squared(num) {
     return num * num;
   }

   function cubed(num) {
     return num * num * num;
   }

   function factorial(num) {
     var x = num;
     while (x > 1) {
       num *= x - 1;
       x--;
     }
     return num;
   }
   ```

   `Ф` функции `squared()` и `cubed()` довольно очевидны— они возвращают квадрат или куб переданного как параметр числа. Функция `factorial()` возвращает [factorial](https://en.wikipedia.org/wiki/Factorial) переданного числа.

3. Далее мы добавим способ выводить нашу информацию введённым в text input числе. Добавьте обработчик событий ниже существующих функций:

   ```js
   input.onchange = function () {
     var num = input.value;
     if (isNaN(num)) {
       para.textContent = "You need to enter a number!";
     } else {
       para.textContent =
         num +
         " squared is " +
         squared(num) +
         ". " +
         num +
         " cubed is " +
         cubed(num) +
         ". " +
         num +
         " factorial is " +
         factorial(num) +
         ".";
     }
   };
   ```

   Здесь мы создаём обработчик событий `onchange` который срабатывает когда меняется когда новое значение вводится в text input и подтверждается (введите значение и, например, нажмите tab). Когда анонимная функция срабатывает, введённое в input значение сохраняется в переменной `num` .

4. Далее мы делаем условный тест — если введённое значение не является числом, мы выводим в параграф сообщение об ошибке. Тест смотрит возвращает ли выражение `isNaN(num)` true. Мы используем функцию [isNaN()](/ru/docs/Web/JavaScript/Reference/Global_Objects/isNaN) чтобы проверить что значение переменной num не число — если так то функция возвращает `true`, если нет- `false`.
5. Если тест возвращает `false`, значение переменной `num` число, и поэтому мы выводим сообщение внутри параграфа о значениях квадрата, куба и факториала числа. Предложение вызывает функции `squared()`, `cubed()` и `factorial()` чтобы получить нужные значения. Сохраните ваш код, загрузите его в браузере и посмотрите на то что получилось.

> [!NOTE]
> Если у вас проблемы с работой данного примера, не стесняйтесь сверить ваш код с [работающей версией](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/functions/function-library-finished.html) (или [смотрите живой пример](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-library-finished.html)), или спросите нас.

К этому моменту мы хотели бы чтобы вы написали парочку собственных функций и добавили их в библиотеку. Как на счёт квадратного или кубического корня числа или длины окружности круга с длиной радиуса равной числу `num`?

Это упражнение привнесло парочку важных понятий в изучении того, как использовать ключевое слово `return` . В дополнение:

- Приведите другой пример написание обработчика ошибок. Это довольно хорошая идея проверять что важные параметры предоставлены в правильном типе и если они опциональны то предусматривать для них значения по умолчанию. В таком случая ваша программа с меньшей вероятность подвержена ошибкам.
- Поразмышляйте о идее создания библиотеки функций. Чем дальше вы будите расти в профессиональном плане, тем больше будете сталкиваться с однотипными вещами. Это хорошая идея начать собирать свою собственную библиотеку функций, которые вы часто используют — в таком случае вы сможете просто скопировать их в ваш новый код или просто добавить их в любую HTML страничку, где это требуется.

## Заключение

Функции очень полезны и несмотря на то, что об их синтаксисе и функциональности можно говорить долго, у нас есть довольно понятные статьи для дальнейшего обучения.

Если в статье есть что-то что вы не поняли, не стесняйтесь перечитать статью ещё раз или [свяжитесь с нами](/ru/docs/Learn_web_development#contact_us) для получения помощи.

## Смотрите также

- [Функции более подробно](/ru/docs/Web/JavaScript/Reference/Functions) — подробное руководство, охватывающее более продвинутую информацию, связанную с функциями.
- [Колбэк-функции в JavaScript](https://www.impressivewebs.com/callback-functions-javascript/) — распространённый паттерн JavaScript для передачи функции в другую функцию как аргумент, который затем вызывается внутри первой функции.

{{PreviousMenuNext("Learn/JavaScript/Building_blocks/Build_your_own_function","Learn/JavaScript/Building_blocks/Events", "Learn/JavaScript/Building_blocks")}}

## In this module

- [Making decisions in your code — conditionals](/ru/docs/Learn_web_development/Core/Scripting/Conditionals)
- [Looping code](/ru/docs/Learn_web_development/Core/Scripting/Loops)
- [Functions — reusable blocks of code](/ru/docs/Learn_web_development/Core/Scripting/Functions)
- [Build your own function](/ru/docs/Learn_web_development/Core/Scripting/Build_your_own_function)
- [Function return values](/ru/docs/Learn_web_development/Core/Scripting/Return_values)
- [Introduction to events](/ru/docs/Learn_web_development/Core/Scripting/Events)
- [Image gallery](/ru/docs/Learn_web_development/Core/Scripting/Image_gallery)
