## Получение изображений с Flickr

Сделаем еще один скелет приложения, на этот раз с CSS. Откройте Sublime и пе-
рейдите в вашу папку Chapter5 , которая находится в Projects . Создайте каталог под
названием Flickr . Внутри него создайте привычные папки javascripts и stylesheets , а так-
же файлы app.js и style.css . Сделайте так, чтобы app.js выводил «Hello, World!»
в консоль JavaScript. Создайте также файл index.html , который свяжет все файлы
вместе для создания основы веб-приложения.

Вау! Если вы смогли сделать все это по памяти, то вы далеко пойдете! Если же
пару раз заглядывали в книгу, то все равно неплохо, но попробуйте все-таки запом-
нить этапы основной подготовки и поупражняться выполнять их. Чем больше
основной работы вы сможете делать по памяти, тем лучше сможете фокусировать
свой мозг на сложных и интересных задачах.
Вот как выглядит мой HTML-файл:

```html
<!doctype html>
<html>
<head>
<title>Приложение для Flickr</title>
<link rel="stylesheet" href="stylesheets/style.css">
</head>
<body>
<header>
</header>
<main>
<div class="photos">
</div>
</main>
<footer>
</footer>
<script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
<script src="javascripts/app.js"></script>
</body>
</html>
```

А сейчас мы просто обобщим пример из документации jQuery, которая находится
по адресу http://api.jquery.com . Немного изменим его, чтобы он лучше соответствовал166
Глава 5. Мост
нашим целям. В этом примере мы используем JavaScript для получения изобра-
жений с Flickr, сервиса Yahoo! для публикации изображений. Но перед тем как
сделать это, откройте браузер и введите в адресную строку следующую ссылку:

```
http://api.flickr.com/services/feeds/photos_public.gne?tags=dogs&format=json .
```

Ответ должен появиться прямо в вашем браузере в виде JSON. Мой, например,
выглядит вот так:

```js
jsonFlickrFeed({
"title": "Recent Uploads tagged dogs",
"link": "http://www.flickr.com/photos/tags/dogs/",
"description": "",
"modified": "2013-10-06T19:42:49Z",
"generator": "http://www.flickr.com/",
"items": [
{
"title": "Huck and Moxie ride stroller",
"link": "http://www.flickr.com/photos/animaltourism/10124023233/",
"media": {"m":"http://bit.ly/1bVvkn2"},
"date_taken": "2013-09-20T09:14:25-08:00",
"description": "...description string...",
"published": "2013-10-06T19:45:14Z",
"author": "nobody@flickr.com (animaltourism.com)",
"author_id": "8659451@N03",
"tags": "park dog beagle dogs brooklyn ride stroller prospect hounds"
},
{
"title": "6th Oct Susie: \"You know that thing you\'re eating?\"",
"link": "http://www.flickr.com/photos/cardedfolderol/10123495123/",
"media": {"m":"http://bit.ly/1bVvbQw"},
"date_taken": "2013-10-06T14:47:59-08:00",
"description": "...description string...",
"published": "2013-10-06T19:14:22Z",
"author": "nobody@flickr.com (Cardedfolderol)",
"author_id": "79284220@N08",
"tags": "pets dogs animal mammal canine"
},
{
"title": "6th Oct Susie ready to leave",
"link": "http://www.flickr.com/photos/cardedfolderol/10123488173/",
"media": {"m":"http://bit.ly/1bVvpXJ"},
"date_taken": "2013-10-06T14:49:59-08:00",
"description": "...description string...",
"published": "2013-10-06T19:14:23Z",
"author": "nobody@flickr.com (Cardedfolderol)",
"author_id": "79284220@N08",
"tags": "pets dogs animal mammal canine"
}
]
});
```

## Получение изображений с Flickr

Это выглядит несколько сложнее, чем примеры, виденные вами ранее, но ос-
новная структура и формат остаются теми же самыми. Он начинается с основной
информации о запросе, а затем выводит свойство items , которое представляет собой
массив изображений. Каждый элемент массива содержит другой объект, media ,
у которого есть свойство m , включающее в себя ссылку на изображение. В главе 2
мы изучили, как добавить изображение в HTML-документ с помощью тега <img> —
сейчас он нам и пригодится.
Проделаем все необходимое шаг за шагом. Начнем с добавления строки в функ­
цию main , которая объявляет URL как переменную, после чего можем вызвать
функцию jQuery getJSON точно таким же образом, что и раньше:

```js
var main = function () {
"use strict";
// на самом деле это всего одна строка,
// но я разделил ее на несколько
// для улучшения восприятия
var url = "http://api.flickr.com/services/feeds/photos_public.gne?" +
"tags=dogs&format=json&jsoncallback=?";
$.getJSON(url, function (flickrResponse) {
//пока мы просто выводим ответ в консоль
console.log(flickrResponse);
});
};
$(document).ready(main);
```

Сейчас, если все хорошо, запустив этот код, мы увидим, что выдаваемый Flickr
объект выводится в консоли и можно изучить его с помощью прокрутки вверх-
вниз. Это поможет выявить источник проблем, если таковые появятся в дальней-
шем.


Затем изменим код так, чтобы вместо вывода целого объекта программа выво-
дила каждый URL по отдельности. Другими словами, мы проходим по всем эле-
ментам объекта с помощью цикла forEach :

```js
$.getJSON(url, function (flickrResponse) {
flickrResponse.items.forEach(function (photo) {
console.log(photo.media.m);
});
});
```

Таким образом, в консоли будет выведена последовательность URL — и вы даже
сможете щелкать на них и видеть изображения! Теперь наконец мы можем по-на-
стоящему отправить их в DOM. Чтобы сделать это, используем функцию jQuery
attr , с которой мы пока не сталкивались. Используем ее, чтобы вручную установить
атрибут src для тега <img> :


```js
$.getJSON(url, function (flickrResponse) {
flickrResponse.items.forEach(function (photo) {
// создаем новый элемент jQuery для помещения в него изображения
var $img = $("<img>");
// помещаем в атрибут URL,168
Глава 5. Мост
// хранящийся в ответе Flickr
$img.attr("src", photo.media.m);
// прикрепляем тег img к элементу
// main.photos
$("main .photos").append($img);
});
});
```

Сейчас, перезагрузив страницу, мы и в самом деле увидим изображения с Flickr!
И если изменим в первоначальном URL тег на какой-нибудь другой (вместо dog —
«собака»), целая страница изменится! И как обычно, мы можем значительно улуч-
шить отображение страницы с помощью эффектов jQuery:

```js
$.getJSON(url, function (flickrResponse) {
flickrResponse.items.forEach(function (photo) {
// создаем новый элемент jQuery для хранения изображений
// но пока скрываем его
var $img = $("<img>").hide();
// устанавливаем атрибут для URL,
// находящегося в ответе
$img.attr("src", photo.media.m);
// прикрепляем тег к функции main
// элемента photos, а затем отображаем его
$("main .photos").append($img);
$img.fadeIn();
});
});
```

Сейчас при повторном открытии страницы изображения будут появляться
постепенно. В практических задачах в конце главы мы изменим этот код так, чтобы
изображения появлялись на странице по очереди, одно за другим.
