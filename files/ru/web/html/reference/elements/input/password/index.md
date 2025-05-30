---
title: <input type="password">
slug: Web/HTML/Reference/Elements/input/password
---

{{HTMLSidebar}}

{{HTMLElement("input")}} элементы типа **`"password"`** предоставляют пользователю возможность безопасного ввода пароль. Элемент представлен как однострочный текстовый редактор, в котором текст затенён, чтобы его нельзя было прочитать, как правило, путём замены каждого символа другим символом, таким как звёздочка ("\*") или точка ("•"). Этот символ будет меняться в зависимости от {{Glossary("user agent")}} и {{Glossary("OS")}}.

{{InteractiveExample("HTML Demo: &lt;input type=&quot;password&quot;&gt;", "tabbed-standard")}}

```html interactive-example
<div>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" />
</div>

<div>
  <label for="pass">Password (8 characters minimum):</label>
  <input type="password" id="pass" name="password" minlength="8" required />
</div>

<input type="submit" value="Sign in" />
```

```css interactive-example
label {
  display: block;
}

input[type="submit"],
label {
  margin-top: 1rem;
}
```

Особенности работы процесса ввода могут отличаться от браузера к браузеру; мобильные устройства, например, часто отображают вводимый символ на мгновение, прежде чем закрывать его, чтобы позволить пользователю быть уверенным, что они нажали клавишу, которую они хотели нажать; это полезно, учитывая небольшой размер клавиш и лёгкость, с которой может быть нажата неправильная, особенно на виртуальных клавиатурах.

> [!NOTE]
> Любые формы, содержащие конфиденциальную информацию, такую как пароли (например, формы входа), должны обслуживаться через HTTPS; В Firefox теперь реализованы несколько механизмов для предупреждения от небезопасных форм входа в систему - см. [Небезопасные пароли](/ru/docs/Web/Security/Insecure_passwords). Другие браузеры также реализуют аналогичные механизмы.

| **[Value](#value)**               | {{domxref("DOMString")}} представляет пароль или пустую строку                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **События**                       | [`change`](/ru/docs/Web/API/HTMLElement/change_event) и [`input`](/ru/docs/Web/API/Element/input_event)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **Общие поддерживаемые атрибуты** | [`autocomplete`](/ru/docs/Web/HTML/Reference/Elements/input#autocomplete), [`inputmode`](/ru/docs/Web/HTML/Reference/Elements/input#inputmode), [`maxlength`](/ru/docs/Web/HTML/Reference/Elements/input#maxlength), [`minlength`](/ru/docs/Web/HTML/Reference/Elements/input#minlength), [`pattern`](/ru/docs/Web/HTML/Reference/Elements/input#pattern), [`placeholder`](/ru/docs/Web/HTML/Reference/Elements/input#placeholder), [`readonly`](/ru/docs/Web/HTML/Reference/Elements/input#readonly), [`required`](/ru/docs/Web/HTML/Reference/Elements/input#required), and [`size`](/ru/docs/Web/HTML/Reference/Elements/input#size) |
| **IDL атрибуты**                  | `selectionStart`, `selectionEnd`, `selectionDirection`, и `value`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **Методы**                        | {{domxref("HTMLInputElement.select", "select()")}}, {{domxref("HTMLInputElement.setRangeText", "setRangeText()")}}, и {{domxref("HTMLInputElement.setSelectionRange", "setSelectionRange()")}}                                                                                                                                                                                                                                                                                                                                                                                                                                          |

## Значения

Атрибут [`value`](/ru/docs/Web/HTML/Reference/Elements/input#value) содержит {{domxref("DOMString")}} , значение которого является текущим содержимым элемента редактирования текста, используемого для ввода пароля. Если пользователь ещё ничего не указал, это значение представляет собой пустую строку (`""`). Если указано свойство [`required`](/ru/docs/Web/HTML/Reference/Global_attributes#required), то поле ввода пароля должно содержать значение, отличное от пустой строки, которое должно быть действительным.

Если указан атрибут [`pattern`](/ru/docs/Web/HTML/Reference/Elements/input#pattern), содержимое элемента управления `"password"` считается действительным только в том случае, если значение проходит проверку; см. [Validation](#validation) для получения дополнительной информации.

> [!NOTE]
> Символы строки (U+000A) и возврата каретки(U+000D) недопустимы в значении поля `"password"`. При вставке пароля, возвращаемые символы удаляются из значения.

## Использование полей ввода пароля

Поля ввода пароля обычно работают так же, как и другие текстовые поля ввода; основное отличие состоит в том, чтобы скрывать введённый контент, чтобы люди, не знакомые с пользователем, не могли прочитать его пароль.

### Простое поле ввода пароля

Здесь мы видим самый простое поле ввода пароля, с меткой, установленной с помощью элемента {{HTMLElement("label")}}.

```html
<label for="userPassword">Пароль:</label>
<input id="userPassword" type="password" />
```

{{EmbedLiveSample("Простое_поле_ввода_пароля", 600, 40)}}

### Поддержка автозаполнения

Чтобы менеджер паролей пользователя автоматически вводил пароль, укажите атрибут [`autocomplete`](/ru/docs/Web/HTML/Reference/Elements/input#autocomplete). Для паролей должно быть одно из следующих значений:

- `"on"`
  - : Разрешить браузеру или менеджеру паролей автоматически заполнять поле пароля. Это не так информативно, как использование `"current-password"` или `"new-password"`.
- `"off"`
  - : Запрещает браузеру или менеджеру паролей автоматически заполнять поле пароля.
- `"current-password"`
  - : Разрешить браузеру или менеджеру паролей вводить текущий пароль для сайта. Это даёт больше информации, чем `"on"`, так как позволяет браузеру или менеджеру паролей знать, что в настоящее время известен пароль для сайта в поле, а не используется новый.
- `"new-password"`
  - : Разрешить браузеру или менеджеру паролей автоматически вводить новый пароль для сайта. Он может быть автоматически сгенерирован на основе других атрибутов элемента управления или может просто указать браузеру представить виджет «предлагаемого нового пароля».

```html
<label for="userPassword">Пароль:</label>
<input id="userPassword" type="password" autocomplete="current-password" />
```

{{EmbedLiveSample("Autocomplete_sample1", 600, 40)}}

### Обязательное заполнение пароля

Чтобы сообщить браузеру пользователя, что поле пароля должно иметь действительное значение перед отправкой формы, просто укажите Boolean атрибут [`required`](/ru/docs/Web/HTML/Reference/Elements/input#required).

```html
<label for="userPassword">Пароль:</label>
<input id="userPassword" type="password" required />
```

{{EmbedLiveSample("Обязательное_заполнение_пароля", 600, 40)}}

### Указание режима ввода

Если ваше приложение лучше обслуживается с использованием другого режима ввода, чем по умолчанию, вы можете использовать атрибут [`inputmode`](/ru/docs/Web/HTML/Reference/Elements/input#inputmode) для определённого запроса. Наиболее очевидным вариантом использования является то, что ваше приложение использует в качестве пароля числовое значение (например, ПИН). Например, мобильные устройства с виртуальными клавиатурами могут переключаться на макет цифровой клавиатуры вместо полной клавиатуры, чтобы облегчить ввод пароля.

```html
<label for="pin">ПИН:</label>
<input id="pin" type="password" inputmode="numeric" />
```

{{EmbedLiveSample("Указание_режима_ввода", 600, 40)}}

### Настройка длины пароля

Как обычно, вы можете использовать атрибуты [`minlength`](/ru/docs/Web/HTML/Reference/Elements/input#minlength) и [`maxlength`](/ru/docs/Web/HTML/Reference/Elements/input#maxlength), чтобы установить минимальную и максимальную допустимую длину пароля , Этот пример дополняет предыдущий, указав, что PIN-код пользователя должен быть не менее четырёх и не более восьми цифр. Атрибут [`size`](/ru/docs/Web/HTML/Reference/Elements/input#size) используется для обеспечения того, чтобы элемент управления ввода пароля имел ширину в восемь символов.

```html
<label for="pin">ПИН:</label>
<input
  id="pin"
  type="password"
  inputmode="numeric"
  minlength="4"
  maxlength="8"
  size="8" />
```

{{EmbedLiveSample("Настройка_длины_пароля", 600, 40)}}

### Выделение текста

Как и другие элементы управления текстовой записью, вы можете использовать метод {{domxref("HTMLInputElement.select", "select()")}} для выбора всего текста в поле пароля.

#### HTML

```html
<label for="userPassword">Пароль</label>
<input id="userPassword" type="password" size="12" />
<button id="selectAll">Выделить все</button>
```

#### JavaScript

```js
document.getElementById("selectAll").onclick = function (event) {
  document.getElementById("userPassword").select();
};
```

#### Результат

{{EmbedLiveSample("Выделение_текста", 600, 40)}}

Вы также можете использовать {{domxref("HTMLInputElement.selectionStart", "selectionStart")}} и {{domxref("HTMLInputElement.selectionEnd", "selectionEnd")}}, чтобы получить (или установить), какой диапазон символов в элементе управления, и {{domxref("HTMLInputElement.selectionDirection", "selectionDirection")}}, чтобы узнать, какой выбор направления произошёл (или будет расширен в зависимости от вашей платформы, см. его документацию для объяснения) , Однако, учитывая, что текст затенён, их полезность несколько ограничена.

## Валидация

Если ваше приложение имеет ограничения по набору символов или любые другие требования для фактического содержимого введённого пароля, вы можете использовать атрибут [`pattern`](/ru/docs/Web/HTML/Reference/Elements/input#pattern), чтобы установить регулярное выражение, чтобы автоматически гарантировать, что ваши пароли соответствуют этим требованиям.

В этом примере допустимы только значения, состоящие как минимум из четырёх и не более восьми шестнадцатеричных цифр.

```html
<label for="hexId">Hex ID:</label>
<input
  id="hexId"
  type="password"
  pattern="[0-9a-fA-F]{4,8}"
  title="Enter an ID consisting of 4-8 hexadecimal digits" />
```

{{EmbedLiveSample("Валидация", 600, 40)}}

- `disabled`
  - : Этот Boolean атрибут указывает, что поле пароля недоступно для взаимодействия. Кроме того, отключённые значения полей не отправляются с формой.

## Примеры

### Запрос номера социального страхования

В этом примере принимается только ввод, который соответствует формату [действительного номера социального страхования Соединённых Штатов](https://en.wikipedia.org/wiki/Social_Security_number#Structure). Эти цифры, используемые для целей налогообложения и идентификации в США, представлены в форме «123-45-6789». Также существуют различные правила, для которых допустимы значения в каждой группе.

#### HTML

```html
<label for="ssn">SSN:</label>
<input
  type="password"
  id="ssn"
  inputmode="number"
  minlength="9"
  maxlength="12"
  pattern="(?!000)([0-6]\d{2}|7([0-6]\d|7[012]))([ -])?(?!00)\d\d\3(?!0000)\d{4}"
  required
  autocomplete="off" />
<br />
<label for="ssn">Value:</label>
<span id="current"></span>
```

Здесь используется [`pattern`](/ru/docs/Web/HTML/Reference/Elements/input#pattern) , который ограничивает введённое значение строками, представляющими номера юридической информации Социальной защиты. Очевидно, что это регулярное выражение не гарантирует действительный SSN, но гарантирует, что число может быть единым; он обычно избегает недопустимых значений. Кроме того, он позволяет разделять три группы цифр пробелом, тире ("-") или ничем.

В [`inputmode`](/ru/docs/Web/HTML/Reference/Elements/input#inputmode) установлено значение `"number"`, чтобы побудить устройства с виртуальными клавиатурами переключаться на макет цифровой клавиатуры для облегчения ввода. Для атрибутов [`minlength`](/ru/docs/Web/HTML/Reference/Elements/input#minlength) и [`maxlength`](/ru/docs/Web/HTML/Reference/Elements/input#maxlength) установлено значение 9 и 12 соответственно, чтобы требовалось, чтобы значение было не менее девяти и не более 12 символов (первый не разделяет символы между группами цифр и последними с ними). Атрибут [`required`](/ru/docs/Web/HTML/Reference/Elements/input#required) используется для указания того, что этот элемент управления должен иметь значение. Наконец, [`autocomplete`](/ru/docs/Web/HTML/Reference/Elements/input#autocomplete) установлен `"off"`, чтобы избежать попыток установить пароли менеджеров паролей.

#### JavaScript

```js
var ssn = document.getElementById("ssn");
var current = document.getElementById("current");

ssn.oninput = function (event) {
  current.innerHTML = ssn.value;
};
```

#### Результат

{{EmbedLiveSample("Запрос_номера_социального_страхования", 600, 60)}}

## Спецификации

{{Specifications}}

## Совместимость с браузерами

{{Compat}}
