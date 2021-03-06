# Хакатон по БЭМ: проект «Сборка»

Привет!

Меня зовут [Андрей Абрамов](http://ru.bem.info/authors/abramov-andrey/). В Яндексе я занимаюсь разработкой БЭМ-инструментов, большая часть которых направлена на сборку проектов, в основе которых лежит методология БЭМ.

Это был мой первый хакатон, и мне сразу же предстояло стать ментором. С одной стороны это определённая ответственность, а с другой — особый опыт, которым я хочу поделиться!

Моя деятельность в рамках Яндекса не могла не отразиться на выборе проекта. Кроме того, я горел желанием сделать мир лучше. Посмотрим, что из этого получилось.

![](https://img-fotki.yandex.ru/get/16138/44214498.bb/0_9bbc7_29c1a57a_XL.jpg)

## Предыстория

Одна из задач, с которой сталкивается практически любой БЭМ-разработчик — это сборка проектов.

В арсенале БЭМ есть два инструмента — старичок bem-tools и ENB с множеством пакетов и расширений. Оба поддерживаются, оба работают.

Но есть у них один недостаток — решениям этим не хватает простоты.

Кроме этого, каждый проект пользователей по-своему уникален, что автоматически означает, что стандартных решений всегда будет не хватать. Пользователям придётся допиливать существующие решения на свой лад.

Напрашивается вывод: инструменты должны быть во-первых простыми, чтобы любой мог в них разобраться, а во-вторых — гибкими, чтобы была возможность написать плагин для своих особых задач.

## Идея модульной сборки

Так возникла идея сделать сборку проектов проще, как для понимания, так и для разработки. Ее я и вынес на хакатон в качестве ментора команды сборки.

Для этого, на мой взгляд, нужно было определить ряд принципов разработки инструментов, соблюдая которые, можно было бы решить поставленные задачи.

После того, как я трижды упомянул слово «боль» в своей презентации, говоря о пережитом опыте использования и разработки текущих решений, я перешёл к перечислению тех самых принципов, которые позволят это слово больше не произносить.

Вот что у меня получилось:

  1. Одна задача — один модуль.
  2. Простота API.
    API решает задачи пользователей, поэтому и говорит API в терминах этих задач, а не предоставляет конструктор из собственных абстракций.
  3. Плагины.
    Модуль предоставляет возможность решать задачу немного иначе, да так, чтобы для этого не пришлось писать модуль заново.
  4. Спецификация.
    Архитекрута модуля устроена так, чтобы весь код удобно покрывался тестами.
  5. Абстрагирование от файловой системы.
    Если задача модуля не связана напрямую с файловой системой, то и сам модуль должен уметь работать в отрыве от неё.
  6. Встраиваемость.
    Расчёт делался на то, что получившиеся модули будут широко использоваться везде, где это возможно: bem-tools, ENB, gulp, grunt, broccoli и т.д..

## Команда

После окончания презентации я и не расчитывал, что ко мне в команду запишется много учасников, ведь проект очень специфичный. К счастью мои опасения не подтвердились, и нас оказалось четверо. Каждый член команды внёс важный и свой особый вклад в получившийся результат.

Наша команда:

  * [Саша Белянский](https://events.yandex.ru/lib/people/610407/) (@belyanskii) работает в команде Яндекс.Директа в симферопольском офисе и представлял проект [BEM IDE](http://ru.bem.info/blog/first-bem-ide/), но в итоге формирования команд перешел к нам. Профиль Саши на bem.info можно найти [здесь](http://ru.bem.info/authors/belyanskii-alexandr/).
  * [Всеволод Струкчинский](https://events.yandex.ru/lib/people/9466/) (@floatdrop) работает в екатеринбургском Яндексе и является автором [gulp-bem](http://github.com/floatdrop/gulp-bem) и ментейнером [getbem.com](https://getbem.com).
  * [Евгений Гаврюшин](https://events.yandex.ru/lib/people/423628/) (@egavr) занимается разработкой инструментов в симферопольском Яндексе и является автором `generator-bem-stub`. Профиль Жени на bem.info можно найти [здесь](http://ru.bem.info/authors/gavryushin-evgeny/).

## Планирование

В отличие от большинства менторов мой план состоял всего из двух пунктов:

  1. Придумать план.
  2. Действовать согласно плану.

Поэтому первое, что мы сделали — забронировали переговорку и несколько часов обсуждали, какие задачи — самые важные и актуальные для каждого из нас, и как они должны быть решены.

![](https://img-fotki.yandex.ru/get/16115/44214498.bc/0_9bbed_a19cf4db_XL.jpg)

Очень приятно было то, что у каждого из участников был свой особый опыт сборки проектов.

Например, Саша поделился тем, как можно использовать модули в его прототипе BEM IDE, а Всеволод рассказал много хороших идей, которые уже используются в `gulp-bem`. Ну, а мы с Женей хорошо понимали, как можно использовать модули в пакетах ENB.

Из обсуждения появился список задач, которые показались самыми важными:

  1. Сканирование уровней блоков.
    Чтобы узнать о том, какие БЭМ-сущности есть в проекте, в каких уровнях переопределения, а также в каких технологиях они реализованы.
  2. Раскрытие зависимостей.
    Возможность дополнять декларацию БЭМ-сущностей недостающими зависимости в желаемом порядке.
  3. Чтение файлов в `deps.js` технологии.

Мы набросали список дел и решили, что лучшей демонстрацией работы модулей будет прототип сборки с помощью `gulp`.

![](https://img-fotki.yandex.ru/get/15517/44214498.bc/0_9bbf0_4f398d3a_XL.jpg)

## Разработка

Перед нами стояли амбициозные планы, а вот время было сильно ограниченно. Чтобы сократить риски на сокрушительный провал в такой нестандартной ситуации, нужно было действовать решительно.

Мы обсуждали архитектуру модулей и согласовывали список требований до тех пор, пока каждый член команды не соглашался, что это именно то, что нужно. И только после этого мы садились стучать по клавиатуре.

Таким образом код писался почти с первой попытки и только 1 раз. Не нужно было тратить время на переписывание модулей из-за непонимания задач или несогласованности между членами команды.

### Test-driven development

Однако, мало было просто договориться о том, как будут устроены модули, нужно было ещё зафиксировать эти знания. Решили, что сначала будем писать тесты, а потом приступать к коду.

Это помогло договориться ещё раз о том, как будет работать модуль в самых изощрённых кейсах. А также зафиксировало то, чего именно мы хотим добиться в процессе работы.

Приятный бонус — был виден прогресс. Перед началом работы известно количество тестов. В любой момент можно понять, сколько уже сделано, а сколько осталось. Когда с каждым усилием количество «зелёных» тестов увеличивается, это здорово мотивирует.

### Парное программирование

Напомню, что мы запланировали 4 задачи, и в команде нас было четверо.

Времени было совсем немного, поэтому логичным было взять по одной задаче и скорее писать код. Но опыт подсказывал: бежать во все стороны невозможно, можно ещё и потерять в качестве.

Мы решили действовать в лучших традициях экстремального программирования и разбиться на пары. Каждая подкоманда брала по одной задаче и занималась ей, пока все тесты не выполнятся. Результат ревьювили оставшиеся коллеги по цеху, вносили свои предложения и правки, а также понимали, как модули будут стыковаться.

Программировать парно было удобно ещё и потому, что очень сложно думать в течении всего дня. Если ты устал думать «архитектурно» или «алгоритмически», можно было брать на себя функции «пилота», писать код, больше следить за синтаксисом и более точечными решениями. И, наоборот, если устал жмякать по клавиатуре и пырить в монитор, становишься штурманом, направляя «пилота».

Чтобы быть уверенными, что мы не потеряем в коммуникации, составы подкоманд менялись после выполнения задач.

### Ошибки случаются

Реализация модуля по раскрытию БЭМ-зависимостей в отсутствии спецификации — это сложная задача. И даже просто портировать решение из ENB согласно принципам модульной сборки оказалось совсем непросто.

В начале второго дня мы поняли, что довести раскрытие зависимостей до качественного решения в оставшееся время не получится. Мы решили доработать прототип и написать небольшую тулзу для работы с декларациями БЭМ-сущностей.

## Результаты

В результате двухдневных стараний у нас было готово 4 модуля.

Каждый из них не был идеальным и не подходил для использования в реальных проектах. Но с точки зрения идей — они прекрасны! =)

Вот они:

  * [bem-walk](https://github.com/bem/bem-walk)
  * [bem-decl](https://github.com/bem/bem-decl)
  * [tech-deps.js](https://github.com/floatdrop/tech-deps.js)
  * [deps-resolver](https://github.com/belyanskii/deps-resolver)

Сейчас проекты находятся в стадии активного развития и не очень стабильны.

Мы будем рассказывать вам про них отдельно по мере их готовности.

А [так](https://github.com/belyanskii/gulp-bem-stub) выглядит прототип сборки с помощью `gulp`.

Пожелания и комментарии к проектам мы как всегда ждем на нашем [форуме](http://ru.bem.info/forum/).

Собирайте без труда и **Stay BEMed**!

![](https://img-fotki.yandex.ru/get/15493/44214498.bd/0_9bc1a_178d2a58_XL.jpg)
