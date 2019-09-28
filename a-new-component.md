# Новый компонент

В этой главе вы узнаете как создать новый компонент, который позволит нам добавлять новые записи в todo list. Новый компонент будет состоять из `input` и `button` HTML элементов. И будет называться `input-button-unit`.

Мы будем использовать Angular CLI для того, чтобы сгенерировать нужные файлы и шаблон компонента для нас. Так как Angular CLI - инструмент командной строки, он принимает команды через терминал. Это не значит, что нам нужно остановить процесс `ng serve`. Вместо этого, мы можем открыть дополнительную вкладку в окне терминала и запустить все нужные команды там. Таким образом, все внесенные изменения будут сразу же отображены в браузере.

Откройте новую вкладку в терминале и выполните следующую команду:

```text
ng g c input-button-unit
```

Как мы уже знаем, `ng` - это команда, которая запускает Angular CLI. `g` - это сокращение для `generate`. `c` - это сокращение для `component`. `input-button-unit` - это имя компонента, который мы хотим создать.

Полная версия команды получается следующая (ее не нужно запускать):

```text
ng generate component input-button-unit
```
Давайте посмотрим, что Angular CLI сгенерировал для нас.

Он создал новую директорию: `src/app/input-button-unit`, которая содержит три файла:

`input-button-unit.component.css` - файл, содержащий стили для компонента.
`input-button-unit.component.spec.ts` - файл, в содержащий тесты для компонента (мы не будем использовать его для этого туториала).
* `input-button-unit.component.ts` - в этом файле мы будем описывать логику компонента.
* `input-button-unit.component.html` - файл, содержащий шаблон компонента.

Откройте `input-button-unit.component.ts`. Обратите внимание, что Angular CLI создал и сконфигурировал для нас компонент, включая его селектор, который состоит из имени, которое мы дали компоненту: `input-button-unit.component.ts` и префикса `app`. Вот что должно получиться:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
    </p>
  `,
  styleUrls: ['./input-button-unit.component.css']
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Префикс `app` будет добавлен ко всем компонентам, которые вы сгенерируете. Это сделано для того, чтобы избежать коллизий с другими HTML элементами. Например, если вы создадите компонент `input`, то коллизии не произойдет со стандартным элементом `input`, так как селектором созданного компонента будет `app-input`.

> `app` - это стандартный префикс для главного приложения. Однако, если вы пишите библиотеку компонентов, которая будет использоваться в других проектах, нужно будет выбрать другой префикс. Например, библиотека компонентов [Angular Material](https://material.angular.io/) использует префикс `mat`. Вы можете создать проект с другим префиксом, используя флаг `—prefix` или изменить префикс после создания проекта в файле `angular.json`.

Давайте используем созданные компонент, чтобы увидеть результат!

Откройте `app.component.ts` файл и добавьте `app-input-button-unit` тэг в его шаблон:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
template: `
  <h1>
    Welcome to {{ title }}!
  </h1>

  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Откройте браузер и посмотрите, что изменилось!

Теперь, давайте добавим контент в новый компонент. Сначала, давайте создадим свойство `title` в классе компонента, которое мы будем использовать как название задачи:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Свойство `title` в `input-button-unit` компоненте, не будет пересекаться со свойством `title` в `app-root` компоненте, так как каждый компонент имеет свой контекст и все его свойства изолированы внутри.

Далее, добавьте вывод `title` свойства в шаблон компонента:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Посмотрите на результат!

Получившийся компонент делает не очень много прям сейчас. Однако, в следующих частях мы добавим логику для этого компонента.
