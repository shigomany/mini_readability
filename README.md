# mini_readability
Extraction of useful information from the URL
## Описание алгоритма
После получения основного контента, происходит отчистка `мусора` в документе по
следующим правилам (класс **Replacer**):
  1. Отчистка `не нужных` тегов, которые никак не влияют на читабельность текста.
  Например: <script>, <style>, <link>.
  2. Удаление тегов с пустым контентом.
  3. Форматирования документа через файл настроек форматирования.
 

Следющим шагом идет поиск `основного` контента на странице (класс **FinderMeaningfulContent**).
Поиск осуществляется через рекурсивных обход каждого тега.
На каждом теге вычисляется оценка по следующей формуле:
![screenshot of sample](https://habrastorage.org/getpro/habr/post_images/3ae/ecf/a3e/3aeecfa3ea7a7504ba69e01c86b25b26.png)

После к чистой оценке добавляется/вычитается определенное значение за нахождение определенного слова в указанном атрибуте.
Все параметры находятся в файле с основными настройками.

## Файлы настроек
Если данные файлы не будут найдены, они создадутся автоматически в папке **settings**.

`main_settings.json` - файл основных настроек тегов.
Содержит настройки для удаления определенных тегов и для наград/штрафов.

`format_settings.json` - файл настроек формата тегов.
Содержит настройки для форматирования определенных тегов.

## Сборка
В корне каталога находится файл `compilestring.txt`.
При выполнении команды из указанного файла, соберется EXE-файл, который можно использовать
в качестве готовой утилиты.

## Команды
Ниже будет представлен список команд для запуска утилиты параметр url или file должен обязятельно присутствовать
при запуске.

Парсинг одной новости.
```
--url=http://site.ru/news/1/
```

Парсинг одной списка новостей из файла.
При таком виде некотрые сайты не парсятся, выводится только набор ссылок.
```
--file=checked.txt
```
URL статей в каждой новой строчке.
```
https://lenta.ru/news/2019/09/12/knopkodavi/
https://lenta.ru/articles/2019/09/11/eralash/
...
```

Подключение проксей.
Пример файла: `proxy.json`
```
--proxy-json=proxy.json
```

Подключение фиктивных user-agent'ов.
Пример файла: `user-agents.txt`
```
--user-agent-txt=user-agents.txt
```

## Результаты
В ходе проверки готовой утилиты выявлены следующие закономерности для каждого сайта:

`www.gazeta.ru` - отсутствует заголовок.

`russian.rt.com` - осутствует заголовок, присутствуют лишние пробелы.

`lenta.ru` - без повреждений, присутствует заголовок, отсутствует мусор.

`iz.ru` - отсутсвует заголовок, присутствует некоторый мусор ниже основной статьи.

Лучше сделать полность методы отчистки мусора. И алгоритм поиска нужного блока может давать сбои при наличии на сайте
большого комментария.

С помощью поиска в ширину подсчитывать оценку тега на определенном уровне, возможно улучшит результаты обнаружения
нужного блока с контентом.

## Cсылки
<https://github.com/words/flesch-kincaid>
<https://habr.com/ru/post/220983/>
<https://tuhrig.de/extracting-meaningful-content-from-raw-html/>
