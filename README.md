[![Build Status](https://travis-ci.org/shvetsgroup/glvrd-http-api.svg?branch=master)](https://travis-ci.org/shvetsgroup/glvrd-http-api)

#  JavaScript реализация HTTP API Главреда (glvrd.ru)

Эта библиотека упрощает работу с HTTP API (v2) [Главреда](http://glvrd.ru). Библиотека может работать как в браузере, так и на Ноде.

## Подключение

### Браузер

1. Скачиваем и подключаем на страницу скрипт `lib/glvrd.js`:

    ```html
    <script src="glvrd.js"></script>
    ```

2. В своих скриптах теперь доступен глобальный объект Glavred, который можно использовать так:

    ```js
    var glvrd = new Glavred("Joomla plugin v1.2");

    var text = "Несоменно очень важный текст.";
    glvrd.proofread(text).then(function(results){ ... });
    ```

### Node.js/Element

1. Включаем пакет `glvrd-http-api` в свой проект.

    ```
    npm install --save glvrd-http-api
    ```

2. Теперь можно включить АПИ Главреда с помощью require:

    ```js
    var Glavred = require('glvrd-http-api');
    var glvrd = new Glavred("Element plugin v5.0");

    var text = "Несоменно очень важный текст.";
    glvrd.proofread(text).then(function(results){ ... });
    ```

## Реализация

Библиотека сама обеспечивает регистрацию и поддержку сессий связи с сервисом главреда. Таким образом, всё что вам потребуется это два метода:

- Конструктор `new Glavred("Your app name")`: возвращает рабочий экземпляр библиотеки. Каждый экземпляр будет вести собственную сессию связи с главредом.

    Вы всегда должны передавать название своего приложения в конструкторе.

- Метод `proofread("Some text")`: принимает строку для проверки, отдаёт объект [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise), который обещает выполнить коллбек с аргументом вида:

    ```json
    {
      "status": "ok",
      "text": "...",
      "score": "...",
      "fragments": [
        {
          "start": "...",
          "end": "...",
          "hint_id": "...",
          "hint": { "name": "...", "description": "..." }
        },
        "..."
      ]
    }
    ```

    Этот метод принимает второй параметр, через который вы можете запретить автоматическую подгрузку `hints` и `score` в результатах проверки.


### Дополнительные методы

- `getScore(proofreadResults)`: Позволяет синхронно рассчитать общую оценку качества текста для одного или нескольких результатов проверки.

- `getHints(ids)`: Принимает массив идентификаторов описаний проблем с текстом. Отдаёт объект `Promise`, который обещает выполнить коллбек с таким аргументом:

    ```json
    {
        "status": "ok",
        "hints": [
            {"...": {"name": "...", "description": "..."}},
            {"...": {"name": "...", "description": "..."}}
        ]
    }
    ```

- Остальные методы документированы в коде библиотеки (см. файл src/glvrd.js).