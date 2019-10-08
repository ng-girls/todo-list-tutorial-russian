# Привязка событий

Мы хотим, чтобы наше приложение реагировало определенным образом на определенные действия пользователя. Далее в этом туториале мы создадим возможность переименовывать элементы списка и добавлять новые по клику на кнопку `Save` или клавишу `Enter`.

Пока у нас еще списка нет, поэтому мы будем использовать другой способ продемонстрировать как это работает. А позже мы заменим его на нужное действие.

Компонент `input-button-unit` сейчас должен выглядеть так:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>

    <input [value]="title">
    <button>Save</button>
  `,
  styleUrls: ['./input-button-unit.component.css']
})
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';

  constructor() { }

  ngOnInit() {
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Метод

Для начала, давайте реализуем метод `changeTitle`. В качестве аргумента он будет принимать новый заголовок. Удобнее всего наши собственные методы расположить после методов жизненного цикла компонента \(в данном случае `ngOnInit`\):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Привязка событий

Так же, как и в случае привязки значений к свойствами элементов, мы можем привязать и действия к событиям, производимым элементами. Опять же, в Angular это делается довольно просто. **Вы заключаете имя события в круглые скобки и передаете ему метод, который должен быть выполнен при отправке события**.

Давайте попробуем реализовать простой пример: сделаем так, чтобы заголовок изменился по клику на кнопку. Обратите внимание на круглые скобки вокруг события `click`. \(Мы также изменяем привязку значения элемента ввода input обратно на `title`.\)

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>

  <input [value]="title">
  <button (click)="changeTitle('Button Clicked!')">
    Save
  </button>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Обратите внимание, что событие называется `click`, а не` onClick` - в Angular вы удаляете префикс `on` из названий событий в html элементах.

Зайдите в браузер и протестируйте результат - после клика на кнопку Save заголовок должен обновиться.

## Данные события

На данном этапе мы всегда передаем статическую строку в вызов метода: `'Button Clicked!'` Но наша цель передать значение, введенное пользователем в поле ввода!

В следующей главе мы узнаем, как использовать свойства одного элемента в другом элементе внутри одного шаблона. Это позволит нам завершить реализацию события `click` кнопки `Save`.
А пока мы привяжем тот же метод к еще одному событию: нажатию клавиши `Enter`.

### Событие 'keyup'

Когда пользователь печатает, производятся события клавиатуры, такие как `keydown` и `keyup`. Сейчас мы будем ловить событие `keyup` \(когда отпущена нажатая клавиша\) и менять заголовок:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" (keyup)="changeTitle('Button Clicked!')">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь, когда пользователь печатает в поле ввода, заголовок меняется на `'Button Clicked!'`. Но это все еще статическая строка.

**Совет:** Когда элемент становится слишком длинным из-за большого количества атрибутов, для восприятия гораздо лучше разделить его на несколько строк:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title"
       (keyup)="changeTitle('Button Clicked!')">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Объект $event

На данном этапе мы просто реагируем на любое событие `keyup`. Но Angular позволяет нам получить сам объект события. Он передается привязке как `$event` и мы можем использовать этот объект при вызове `changeTitle()`.

Объект, производимый в событиях типа `keyup`, содержит ссылку на элемент, который произвел его - элемент `input`. Ссылка хранится в свойстве `target`. Как мы уже видели, элемент input имеет свойство `value`, которое содержит текущую строку, находящуюся в поле ввода. Таким образом мы можем передать `$event.target.value` методу:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title"
       (keyup)="changeTitle($event.target.value)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Посмотрите как это работает в браузере. Теперь при каждом нажатии клавиши вы можете видеть как заголовок меняется на введенное значение.

### Нажатие клавиши Enter

You can limit the change to only a special key stroke, in our case it's the Enter key. Angular makes it really easy for us. The `keyup` event has properties which are more specific events. So just add the name of the key you'd like to listen to - `keyup.enter`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title"
       (keyup.enter)="changeTitle($event.target.value)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now the title will change only when the user hits the Enter key while typing in the input.

### Explore the $event

 ![lab-icon](.gitbook/assets/lab%20%281%29.jpg)**Playground: **You can change the changeTitle method to log the `$event` object in the console. This way you can explore it and see what properties it has.

Change the method `changeTitle`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(event): void {
  console.log(event);
  this.title = event.target.value; // the original functionality still works
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now change the argument you're passing in the template, to pass the whole `$event`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title"
       (keyup.enter)="changeTitle($event)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Try it out!

You can do the same with the `button` element:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
  <button (click)="changeTitle($event)">
    Save
  </button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Don't forget to change back the code before we go on!**

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>

    <input [value]="title"
           (keyup.enter)="changeTitle($event.target.value)">

    <button (click)="changeTitle('Button Clicked!')">
      Save
    </button>
  `,
  styleUrls: ['./input-button-unit.component.css']
})
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';

  constructor() { }

  ngOnInit() {
  }

  changeTitle(newTitle: string) {
    this.title = newTitle;
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

