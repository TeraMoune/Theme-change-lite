# Change-theme-lite
Если Вы задаётесь вопросом как сделать альтернативное оформление сайта и всё еще не нашли понятного и простого решения, то это для Вас.

>Текущий вариант плагина подразумевает наличие префиксов тёмной темы в основном файле стилей.

В `main.tpl` применимы следующие теги.

|Тег(и)             | Описание             |
| ----------------- | -------------------- |
| {night-mode}      | Устанавливает в значение `checked` если активно альтернативное оформление |
| [dark-mode-auto] ... [/dark-mode-auto]  | В случае если пользователю допущено включение автоматической смены темы, то всё между блоков будет скрыто и переключение будет недоступным. |
|{theme-class}|Принимает значение `dark` для дополнительного использование в разметке. Например в объекте body чтобы указать префикс.|

## Примерная HTML разметка переключателя.
В прошлом я предоставлял вариант без тега `{night-mode}` в этом он присутствует и будет сохранять положение **checkbox'a**.
```html
[dark-mode-auto]
<div class="night-mode">
  <label for="night-mode">Тёмное оформление</label>
  <div class="tw-toggle">
    <input type="checkbox" class="checkbox" id="night-mode" onchange="changeTheme();"{night-mode}>
  </div>
</div>
[/dark-mode-auto]
```
Ну а дальше всё зависит от вашей фантазии, и навыков вёрстки и стилизации. В интернете можно найти примеры всяких красивых кнопочек.

## JavaScript функция
Её размещаете где угодно, в тегах `<script>` или в файлах с расширением `js`. Но за пределами конструкций `$(function() { ... })` и любых других вариаций.

```js
function changeTheme() {
	
	setTimeout(function(){	
		if( $('body').hasClass('dark') ) {
			
			$('body').removeClass('dark');
			setcookie('theme-class', 'light');
			
		} else {
			
			$('body').addClass('dark');
			setcookie('theme-class', 'dark');	
			
		}
	}, 16);
	
}
```

## Автоматическая смена.
>Если в DLE этого типа поля нет

Для начала потребуется плагин [checkbox-userxfields.xml](https://github.com/TeraMoune/Different-hacks-DLE#checkbox-userxfieldsxml) Для реализации дополнительного поля типа **checkbox** в профиль пользователя.

Далее если создать дополнительное поле в профиль пользователя с именем `auto-dark-mode` то станут доступны блочные теги 
`[dark-mode-auto] ... [/dark-mode-auto]` в которых будет скрыто содержимое разметки между блоками если пользователь включил автоматическую смену.


Корректировка окна тёмного времени суток. Я не стал ничего придумывать, простое условие. В данном примере используемом в плагине выделяется окно в период с `20` часов вечера по `6` утра.

```php
if( $a_hour >= 0 AND ($a_hour > 20 OR $a_hour < 6) ) {
  $theme_class = 'dark';
} else $theme_class = '';
```

>Так же в этом коде можно заметить и переменную `$theme_class` в которой прописан класс который будет использоватся в шаблоне и являтся префиксом для стилей. Так же можно поменять на своё значение с учётом ваших пожеланий. И не забудьте внести правки в js функцию и поменять имя класса.
