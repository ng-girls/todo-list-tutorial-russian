# Класс

Класс - это специальная программная структура. Он определяется с **членами**, которые могут быть **свойствами** \(переменные\)
и **методами** \(функции\). Затем создаются экземпляры класса, для этого обычно вызывается оператор `new` в
классе: `let myInstance = new myClass ();`. Созданный экземпляр - это объект, у которого вы можете вызывать методы класса,
получать или устанавливать значения его свойств. Несколько экземпляров могут быть созданы из одного класса.

## Классы в Angular...

Angular заботится о создании экземпляров классов, которые вы определяете, - если они распознаются как строительные блоки Angular-а.
Для этого необходимы декораторы, с помощью которых вы создаете связь с классом.

Каждый раз, когда вы используете компонент в шаблоне, создается новый экземпляр. Например, здесь будут
созданы три экземпляра класса InputButtonUnitComponent:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
// для примера

template: `
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Давайте посмотрим на класс `InputButtonUnitComponent`.

## implements OnInit

Во-первых, вы видите, что что-то было добавлено в объявление класса:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
export class InputButtonUnitComponent implements OnInit {
  ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`OnInit` это **интерфейс** - структура, которая определенна, но не реализованна как класс. Он определяет, какие свойства и/или методы должны существовать
в классе, который его реализует. В этом случае `OnInit` является интерфейсом для Angular компонентов, которые реализуют
метод `ngOnInit`. Этот метод является **методом жизненного цикла**. Angular вызывает этот метод после создания
экземпляра компонента.

Angular CLI добавляет это утверждение, чтобы напомнить нам, что лучше всего инициализировать вещи в компоненте с
помощью метода `ngOnInit`. Вы можете увидеть также, что он также добавил метод в тело класса:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
ngOnInit() {
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Вы можете использовать этот метод без явного указания того, что класс реализует интерфейс OnInit,
но полезно использовать реализацию. Чтобы узнать, почему, удалите метод `ngOnInit`.
IDE скажет вам, что в коде ошибка - вы должны реализовать `ngOnInit`. Откуда это известно?
Из-за `implements OnInit`.

## constructor

Другой метод, который мы не видели в компоненте `app-root`, является конструктор \(constructor\). Это метод, который вызывается
JavaScript при создании экземпляра класса. Все, что находится внутри этого метода, используется для создания экземпляра.
Поэтому он вызывается до `ngOnInit`.

> Важной особенностью Angular является то, что конструктор используется для инъекций зависимостей \(dependency injection\).
Мы вернемся к этому позже, когда начнем пользоваться сервисами.

## Свойства

Свойство `title`, которое мы добавили, используется для хранения значения в нашем случае с типом строка. Каждый экземпляр
класса будет иметь собственное свойство `title`, а это означает что вы можете изменить значение `title` в одном экземпляре,
при этом в остальных экземплярах оно останется прежним.

В TypeScript мы должны объявлять свойства класса либо внутри него и вне любого метода, либо
передавать их в конструктор - именно таким образом мы будем использовать сервисы.

Вы можете объявить свойство без его инициализации:

```typescript
title: string;
```

Затем вы можете присвоить значение на более позднем этапе, например, в конструкторе или в методе ngOnInit.
Здесь мы явно определили, что `title` имеет тип `string`. \(Когда мы сразу присваиваем значение, то
TypeScript сам опеределяет какой тип у переменной, поэтому в таком случае нет необходимости его добавлять.\)

Когда вы ссылаетесь на свойство класса внутри какого-либо его метода, вы должны добавить префикс `this`.
Это специальное свойство, которое указывает на текущую сущность, т.е. на класс.

Попробуйте установить другое значение для `title` внутри конструктора. Посмотрите результат в браузере:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Попробуйте изменить значение `title` внутри метода `ngOnInit`. Какое значение будет отображаться на экране?

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title: string = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}

ngOnInit() { 
  this.title = 'Angular CLI Rules!';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Методы

Давайте добавим метод, который изменяет значение `title` в соответствии с аргументом,
который мы передаем. Назовем его `changeTitle`. У метода будет один входной параметр типа
`string`. Добавьте метод **внутри класса** \(но не внутри другого метода\):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Примечание:** Функции и методы могут возвращать значение, которое может использоваться после вызова метода. Например:

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
function multiply (x: number, y: number) {
  return x * y;
}

let z = multiptly(4, 5);
console.log(z);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Метод `changeTitle` больше нигде не используется. Мы можем вызвать внутри другого метода
или в шаблоне \(который мы увидим в следующих главах\). Давайте вызовем его внутри конструктора.

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  this.changeTitle('My First Angular App');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![lab-icon](.gitbook/assets/lab%20%281%29.jpg) **Практическое упражнение**:
Вы можете попробовать вызвать метод с разными аргументами внутри `ngOnInit`
\(в данном случае аргумент - это строка, переданная внутри скобок\). Попробуйте вызвать его до или после присвоения
значения непосредственно заголовку. Попробуйте вызвать его несколько раз из того же метода.
Посмотрите результат в браузере.

## Отладка кода

Вы всегда можете использовать `console.log(someValue)` внутри методов класса. Значение, которое
вы передадите в качестве аргумента, будет напечатано в консоли браузера. Таким образом вы можете видеть
порядок выполнения методов и значение аргумента, которое вы передаете \(если это переменная\). Например:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  console.log('in constructor');
  this.changeTitle('My First Angular App');
  console.log(this.title);
}

changeTitle(newTitle: string) {
  console.log(newTitle);
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Консоль браузера входит в состав Dev Tools. Вы можете увидеть, как открыть консоль в разных браузерах здесь:
[https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers](https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers)

