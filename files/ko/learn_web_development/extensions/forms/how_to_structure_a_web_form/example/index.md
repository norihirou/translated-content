---
title: Example
slug: Learn_web_development/Extensions/Forms/How_to_structure_a_web_form/Example
original_slug: Learn/Forms/How_to_structure_a_web_form/Example
l10n:
  sourceCommit: bb026bcb88b7f45374d602301b7b0db5a49ff303
---

[HTML 폼 구성 방법](/ko/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form)의 기본 결제 폼 예제 입니다.

## 결제 폼

### HTML

```html-nolint
<form method="post">
  <h1>Payment form</h1>
  <p>
    Required fields are followed by
    <strong><span aria-label="required">*</span></strong>.
  </p>
  <section>
    <h2>Contact information</h2>
    <fieldset>
      <legend>Title</legend>
      <ul>
        <li>
          <label for="title_1">
            <input type="radio" id="title_1" name="title" value="A" />
            Ace
          </label>
        </li>
        <li>
          <label for="title_2">
            <input type="radio" id="title_2" name="title" value="K" />
            King
          </label>
        </li>
        <li>
          <label for="title_3">
            <input type="radio" id="title_3" name="title" value="Q" />
            Queen
          </label>
        </li>
      </ul>
    </fieldset>
    <p>
      <label for="name">
        <span>Name: </span>
        <strong><span aria-label="required">*</span></strong>
      </label>
      <input type="text" id="name" name="username" />
    </p>
    <p>
      <label for="mail">
        <span>Email: </span>
        <strong><span aria-label="required">*</span></strong>
      </label>
      <input type="email" id="mail" name="usermail" />
    </p>
    <p>
      <label for="pwd">
        <span>Password: </span>
        <strong><span aria-label="required">*</span></strong>
      </label>
      <input type="password" id="pwd" name="password" />
    </p>
  </section>
  <section>
    <h2>Payment information</h2>
    <p>
      <label for="card">
        <span>Card type:</span>
      </label>
      <select id="card" name="usercard">
        <option value="visa">Visa</option>
        <option value="mc">Mastercard</option>
        <option value="amex">American Express</option>
      </select>
    </p>
    <p>
      <label for="number">
        <span>Card number:</span>
        <strong><span aria-label="required">*</span></strong>
      </label>
      <input type="tel" id="number" name="cardnumber" />
    </p>
    <p>
      <label for="expiration">
        <span>Expiration date:</span>
        <strong><span aria-label="required">*</span></strong>
      </label>
      <input
        type="text"
        id="expiration"
        required="true"
        placeholder="MM/YY"
        pattern="^(0[1-9]|1[0-2])\/([0-9]{2})$" />
    </p>
  </section>
  <section>
    <p><button type="submit">Validate the payment</button></p>
  </section>
</form>
```

### CSS

```css
h1 {
  margin-top: 0;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

form {
  margin: 0 auto;
  width: 400px;
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 1em;
}

div + div {
  margin-top: 1em;
}

label span {
  display: inline-block;
  width: 120px;
  text-align: right;
}

input,
textarea {
  font: 1em sans-serif;
  width: 250px;
  box-sizing: border-box;
  border: 1px solid #999;
}

input[type="checkbox"],
input[type="radio"] {
  width: auto;
  border: none;
}

input:focus,
textarea:focus {
  border-color: #000;
}

textarea {
  vertical-align: top;
  height: 5em;
  resize: vertical;
}

fieldset {
  width: 250px;
  box-sizing: border-box;
  margin-left: 136px;
  border: 1px solid #999;
}

button {
  margin: 20px 0 0 124px;
}

label {
  position: relative;
}

label em {
  position: absolute;
  right: 5px;
  top: 20px;
}
```

### 결과

{{ EmbedLiveSample('A_payment_form', '100%', 620) }}
