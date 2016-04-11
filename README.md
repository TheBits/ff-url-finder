# ff-url-finder

Экспортирует класс `URLFinder`.

Конструктор: `new URLFinder(tlDomains, localDomains)`, где _tlDomains_ — массив доменов 1-го уровня, которые должны распознаваться без явного указания протокола,
_localDomains_ — массив доменов, ссылки в которых считаются локальными.

Поле: `withHashTags = false` — нужно ли определять в тексте хэш-теги (по умолчанию _false_). Синтаксис хэш-тегов см. в файле _hashtag-syntax.md_.

Метод: `parse(text)` — разбивает строку текста на список блоков следующих типов:

````
{type: "text", text: "aa "} // простой текст
{type: "link", text: "google.com", url: "http://google.com/"} // внешняя ссылка
{type: "atLink", text: "@alice", username: "alice"} // @-ссылка с username
{type: "localLink", text: "freefeed.net/abc", uri: "/abc"} // локальная ссылка (в одном из доменов localDomains)
{type: "email", text: "aa@bb.ru", address: "aa@bb.ru"} // адрес e-mail
{type: "hashTag", text: "#3pm", hashTag: "3pm"} // хэш-тег
````
Везде: _type_ — тип блока, _text_ — текст, который должен показываться пользователю (текст внутри тега A для ссылок). Остальные поля типо-специфичны.

Статический метод `URLFinder.shorten(url, max_length)` — возвращает _url_, укороченный до длины не большей _max_length_ (без учёта протокола).  

## Фичи

* Понимает ссылки без явно указанного протокола (для заданного набора TLD);
* В тексте для пользователя делает urldecode, а также преобразует IDN к человекопонятному виду;
* Правильно обрабатывает концевые скобки в URL.
