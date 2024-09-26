# CodeToImageGenerator
### Веб-приложение на ASP NET MVC для генерации изображения (скриншота) из предоставленного пользователем фрагмента кода.
Посмотреть, как приложение реализовано, можно в боте [Codepic](https://t.me/codepicbot).

Видео демонстрация на [RuTube](https://rutube.ru/video/private/0d26b2c7e8fb4f6be7204b3356ba43df/?p=vbdK4Bv8BKoXn1pV1xEefw)

## Состав репозитория:
- `CodeToImageGenerator.Demo` : Консольное приложение для быстрой демонстрации возможностей. Внутри Program.cs нужно только поменять значения переменных `code` и `programmingLanguage` на, соответственно, фрагмент кода и язык программирования. После запуска консольного приложения в папке `~/bin/Debug/net8.0` будет сохранен файл с изображением.
- `CodeToImageGenerator.Common` : Бибилотека классов, в которой происходит создание изображения.
- `CodeToImageGenerator.Web` : Веб-приложение, в котором запускается сайт, для веб-версии, и телеграм бот с мини-приложением. Функционально сайт от мини-приложения не отличаются. Разница во внешнем виде - на сайте есть header и footer, в мини приложении они отсутствуют за ненадобностью.

## Сборка и запуск

### Локальный запуск
Локальная отладочная сборка от релизной версии отличается тем, что для размещения на сервере используется docker-контейнер. Внутри приложения для релизной версии используется kestrel и есть различия в работе библиотеки, отвечающей за генерацию скриншотов.
Для запуска приложения нужно задать две переменные окружения:
```
"BOT_TOKEN" - токена вашего бота телеграм
"WEB_APP_URL" - адрес мини приложения
```
Важно, что обе переменные обязательны и без них приложение не стартует. Для пробного запуска адрес мини приложения может быть любым, он используется внутри приложения только для формирования ссылки на mini-app внутри бота.

### Запуск на сервере
Для запуска на сервере собирается docker образ. При запуске необходимо передать две переменные окружения, например в файле `.env` в формате:
```
BOT_TOKEN=your_token
WEB_APP_URL=your_url
```
Затем при старте образа добавить `--env-file .env` , например команда запуска может выглядеть так:
```
docker run --env-file .env -d -p 8080:80 codetoimagegenerator:latest
``` 

## Технологии
Для создания скриншотов используется библиотека [Puppeteer.Sharp](https://github.com/hardkoded/puppeteer-sharp) -> скачивает (или использует предварительно скачанный дистрибутив) Chromium

Для работы Телеграм бота - [Telegram.Bot](https://github.com/TelegramBots/telegram.bot)

Для отдельных операций с json - [Newtonsoft.Json](https://www.newtonsoft.com/json)

Для подсветки синтаксиса на скриншоте используется бибилотека [HLJS](https://github.com/highlightjs/highlight.js) 

## Схема работы

Пользователь открывает мини-приложение в Телеграм одним из четырех доступных способов:
- По прямой ссылке (создается в BotFather)
- По нажатию на кнопку клавиатуры в чате с ботом

![](https://github.com/algmironov/CodeToImageGenerator/blob/master/Previews/KeyboardButton.png)

- По нажатию на кнопку Меню в чате с ботом

- По нажатию на кнопку "Открыть приложение" в профиле бота

![](https://github.com/algmironov/CodeToImageGenerator/blob/master/Previews/Description_button.png)

Так же, можно зайти на страницу веб-приложения на сайте.

Далее выбираете в выпадающем меню один из языков программирования. 
На данный момент Доступно ~20 самых популярных языков, плюс `cmd`, `powerShell` и `bash`. Можно добавить все, доступные в `hljs`, их там более 100.
В поле ввода вставляете фрагмент кода и нажимаете "Скачать изображение". 

![MiniApp](https://github.com/algmironov/CodeToImageGenerator/blob/master/Previews/MiniApp.png)

Результат в чате:

![](https://github.com/algmironov/CodeToImageGenerator/blob/master/Previews/Result.png)

Итоговое изображение:

![](https://github.com/algmironov/CodeToImageGenerator/blob/master/Previews/csharp_6ttaXfYP3F.png)
> На картинке выше код метода, генерирующего уникальное имя для скачиваемого файла

Для пользователей телеграм изображение будет отправлено в бота.
Для посетителей веб-версии изображение скачается на устройство.

## TODO
На данный момент остается реализовать:
- Прием обновлений Телеграм через WebHook
- Всплывающее окно или уведомление об успешном создании изображения внутри интерфейсас мини приложения
- Поддержка мультиязычности
- Темы (светлая/темная)

### Как научиться создавать своих ботов на .NET
[Курс PRO C#. Создание Telegram бота](https://stepik.org/205788)
 

   
