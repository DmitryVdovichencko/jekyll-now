---
layout: post
title: Гайд по SASS.
images:

  - url: /images/sass.png
    alt: SASS
    title: SASS

---

SASS -- (Syntactically Awesome Stylesheets - Синтаксически Шикарные Таблицы Стилей) препроцессор для CSS -- позволяет писать код, соответствующий принципам DRY, который будет быстрее, эффективнее и проще в обслуживании. При подготовке использовалась книга [Dan Cederholm Sass for Web Designers](https://abookapart.com/products/sass-for-web-designers)

 
## SASS синтаксис.

Для SASS есть два разных синтаксиса последний -- SCSS наиболее удобен по нескольким причинам:

* Позволяет использовать синтаксис CSS при этом все будет работать отлично.
* Можно легко преобразовать действующий CSS в SASS.
* Не требует расстановки отступов при форматировании кода.

Пример синтаксиса SCSS

```scss
$pink: #ea4c89; //создаем переменную с цветом
p {
	font-size: 12px;
	color: $pink; //применяем переменную
}
p strong {
	text-transform: uppercase;
}
```
Это будет скомпилировано в такой CSS:

```css
p {
	font-size: 12px;
	color: #ea4c89;
}
p strong {
	text-transform: uppercase;
}
```
Оригинальный SASS синтаксис выглядит по другому:

```scss
$pink: #ea4c89
p
	font-size: 12px
	color: $pink
p strong
	text-transform: uppercase
```
Прощайте фигурные скобки и точки с запятой. Остаются только идентификаторы и пробелы.

## Настраиваем рабочее окружение для SASS.

Для Windows необходима предварительная установка Ruby с помощью [RubyInstaller](https://rubyinstaller.org/).
Затем нужно выполнить установку `gem SASS`.

Для отслеживания изменений и компиляции в css используем команду :

```
$ sass --watch werewolf.scss:vampire.css
```
Хорошей практикой будет считаться хранить SASS файлы в отдельной папке и неплохо было бы следить за ее изменениями:

```
$ sass --watch stylesheets/sass:stylesheets
```
В этом случае в `stylesheets` хранятся CSS файлы и папка SASS, где лежат SASS исходники.

Также можно определить стиль вывода для CSS это делается с помощью `--style    `

* По умолчанию `nested`. Вложенный стиль. Вложенность соответствует иерархии элементов `HTML`

``` scss
ol {
 margin: 10px 0;
 padding: 10px 0; }
  ol li {
   font-size: 2em;
   line-height: 1.4; }
    ol li p {
     color: #333; }
```
* `Expanded`. Выглядит как `CSS`, написанный вручную.

``` scss
ol {
margin: 10px 0;
padding: 10px 0;}
ol li {
font-size: 2em;
line-height: 1.4;
}
ol li p {
color: #333;
```
* `Compact`. Правила `CSS` сгруппированы в одну строку, селекторы друг под другом.

``` scss
ol { margin: 10px 0; padding: 10px 0; }
ol li { font-size: 2em; line-height: 1.4; }
ol li p { color: #333; }
```
```
* `Compressed`. Все ненужные пробелы и переводы строк удалены для уменьшения размера выходного файла.

```scss
ol{margin:10px 0;padding:10px 0;}ol li{font-size:2em;line-height:1.4;}ol li p{color:#333;}
```

## Используем SASS.

### Вложенные правила.

Вместо того, чтобы повторять селекторы  -- мы можем использовать вложенные правила в соответствие со структурой разметки `HTML`.

``` scss
header[role="banner"] {
 margin: 20px 0 30px 0;
 border-bottom: 4px solid #333;
 #logo {
  float: left;
  margin: 0 20px 0 0;
 img {
  display: block;
  opacity: .95;
  }
}
```
Также мы можем создавать вложенные имена свойств которые указаны через дефис, например:

``` scss
font: {
	size: 54px;
	family: Jubilat, Georgia, serif;
	weight: bold;
}
```

### Ссылаемся на родительские селекторы через &

Вместо `&` при компиляции будет подставлен родительский селектор.

``` scss
a {
	font-weight: bold;
	text-decoration: none;
	color: red;
	border-bottom: 2px solid red;
	&:hover {
		color: maroon;
		border-color: maroon;
	}
}
```
### Комментарии в SASS 

Комментируем так же как в `CSS`, но если хотим чтобы комментарий сохранился даже при компиляции в `compressed style`

``` scss
/* ! This is a multi-line comment that will
appear in the final .css file. Even in compressed style. */
```

### Переменные

Все значения свойств `CSS` которые постоянно повторяются в стилях могут быть назанчены переменным в `SASS`.
При компиляции все переменные будут заменены на их значения.

``` scss
$color-main: #333;
$color-light: #999;
$color-accent: #ea4c89;
$font-sans: "Proxima Nova", "Helvetica Neue", Helvetica, Arial, sans-serif;
$font-serif: Jubilat, Georgia, serif;
```
Кстати для цветов есть функции `darken`  и  `lighten` позволяющие получить более темные или светлые оттенки выбранного цвета соответственно.

``` scss
section.primary {
	background: lighten($color-accent, 30%);
}
section.secondary {
	background: darken($color-accent, 30%);
}
```

### Миксины

Если нам нужно определять и многократно использовать не только значения свойств но и целые блоки стилей -- здесь есть работа для миксинов

``` scss
//объявляем mixin
@mixin title-style {
	margin: 0 0 20px 0;
	font-family: $font-serif;
	font-size: 20px;
	font-weight: bold;
	text-transform: uppercase;
}
//и используем
section.main h2 {
	@include title-style;
}
```
Миксины могут принимать аргументы и даже назначать значения по умолчанию, если вдруг при использовании какой-то из аргументов не задан

```scss
@mixin title-style($color, $background: #eee) {
//назначили два аргумента для миксина
	margin: 0 0 20px 0;
	font-family: $font-serif;
	font-size: 20px;
	font-weight: bold;
	text-transform: uppercase;
	color: $color;
	background: $background;
}

section.main h2 {
	@include title-style(#c63);
// передан только аргумент $color значит $background получит значение по умолчанию
}
section.secondary h3 {
	@include title-style(#39c, #333);
// переданы оба аргумента значит оба получат действующие значения
}
```