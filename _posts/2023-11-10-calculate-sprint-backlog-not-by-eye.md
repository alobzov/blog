---
layout: post
date: 2023-11-10
title:  Рассчитываем бэклог спринта не на глаз
description: |
    <b>А с использованием методов линейного программирования.</b><br><br>

    Сталкивались ли вы с понятием линейного программирования? А его применением на практике? В университете мы изучаем разные разделы математики, нам рассказывают про математические модели и методы, однако вопросу их практического применения часто уделяется недостаточно внимания.<br><br>

    В статье я поделюсь основными тезисами <a href="https://www.youtube.com/watch?v=Zcp0V9eTyDI&list=PL_XScYmjXxkfu1GeVUhg_37DJxeSta6jh&index=48">моего доклада</a>, представленного на конференции Analyst Days №16. В нём я постарался показать, как методы линейного программирования могут быть применены в работе команды, живущей спринтами. Под катом вас ждет альтернативный взгляд на планирование спринта.
author: Алексей Лобзов
place: Москва, Россия
source: https://habr.com/ru/companies/alfa/articles/772754/
---

Планирование спринта и проблемы
В Альфа-Банк я [пришел в 2017](https://habr.com/ru/companies/alfa/articles/762462/) году и начал с позиции системного аналитика в продуктовой команде: мы с коллегами занимались развитием интернет-банка для юридических лиц и использовали в своей работе Scrum.

Вначале мы старались полностью соответствовать этому фреймворку — выполнять все необходимые ритуалы. Однако постепенно пришло осознание, что соблюдать их все затратно, это отъедает время, которое можно было бы уделить разработке продукта. У нас исчезло ревью спринта, а ретроспектива стала проводиться по необходимости. Но, что осталось неизменным, так это планирование.

В процессе планирования на входе имели бэклог продукта, а на выходе — бэклог спринта. Естественно, планирование не проходило гладко. И сейчас расскажу, почему.

Есть такая штука как ёмкость.

> Она показывает вес работ, который команда (или её член), в среднем, закрывает за спринт в сторипойнтах (далее — sp).

Обычно у нас она составляла в среднем 20 sp. Соответственно, на планировании мы начинали из бэклога продукта набирать задачи, пока их суммарный вес не превышал ёмкость команды.

На практике возникали ситуации, когда мы набирали 20 sp и стартовали спринт, а всего через пару дней часть членов команды уже закрывали все свои задачи. Ну и всё — дел больше нет, можно отдыхать. Или нет?

Как поступали в таких ситуациях?

1. В спринт из бэклога продукта брали дополнительные элементы, под которыми на планинге никто не коммитился и, следовательно, не обещал закрыть за спринт.

2. Занимались задачами развития, что, конечно, хорошо, но не всегда было ценно для продукта.

Бывали и противоположные ситуации: команда набирала задач на 20 sp, причём нагрузка распределена неравномерно — кто-то из членов команды загружен полностью, а кто-то, наоборот, недогружен. И тут приходил владелец продукта и говорил: «А давайте возьмём ещё одну-две задачки сверх? Очень надо, прям горит». И те, кто оказывался недогружен, были более склонны согласиться на его просьбу. Но, кто сказал, что новые задачи для них? В результате в спринте задач не на 20 sp, а на 25 sp, и кто-то из членов команды перерабатывает, чтобы выполнить заявленные коммиты.

Давайте посмотрим почему так получается.

## Первый нюанс при планировании: специализация

Перед вами пример бэклога продукта из Jira.

<details>
  <summary>Совершенно тестовый пример, User Story просто пронумеровал</summary>
  <img src="{{ site.baseurl }}/assets/img/2023-11-10/pic_1.png" alt="Совершенно тестовый пример, User Story просто пронумеровал"/>
</details>

Мы видим перечень задач/элементов. В данном случае это пользовательские истории, но в общем случае могут быть любые: обычные задачи, ошибки, задачи на устранение техдолга и всё остальное. У элементов есть ответственный и оценка в sp.

Как традиционно формируется бэклог спринта?

Мы начинаем выбирать элементы сверху до тех пор, пока не упрёмся в ёмкость команды. Наша задача набрать ровно столько элементов, чтобы смочь закрыть их все в течение спринта. В противном случае велика вероятность нарушить коммитмент.

![Бэклог продукта]({{ site.baseurl }}/assets/img/2023-11-10/pic_2.png)

Представим, что мы отобрали задач на 16 sp. Ёмкость команды 20 sp, значит в «запасе» остаётся 4 sp. У нас есть несколько вариантов действий.

* С одной стороны, можно остановиться, потому что добавив следующий элемент, мы превысим нашу ёмкость. В этом случае мы смиряемся с тем, что команда будет недозагружена: закроет три задачи, а в оставшееся время позанимается чем-то еще.

* С другой стороны, можем пойти дальше и добрать задачи снизу: 7 и 9-ю. В результате мы набёрем задач ровно на 20 sp. Это такой классический подход. Но здесь появляется нюанс.

В нашей команде участники имеют свою ключевую компетенцию, свою специализацию. Это значит, что системный аналитик должен быть хорош при выполнении задач анализа, но не обязан уметь закрывать задачи фронтенд-разработчика, потому что это не его специализация. И это касается всех остальных.

Например, наша команда состояла из системного аналитика, двух разработчиков (Java и JS), QA, владельца продукта и скрам-мастера.

![Команда]({{ site.baseurl }}/assets/img/2023-11-10/pic_3.png)

Допустим, аналитик, два разработчика и QA закрывают по 5 sp. Команда сформировала спринт из задач с суммарным весом 20 sp, полностью заняв свою емкость. Однако только на одного фронтенд-разработчика приходится 15 sp. Какова вероятность, что в заданных условиях команда выполнит коммитмент и закроет все задачи в спринт?

Вот так и получается, что специализация членов имеет важную роль при планировании спринта команды.

## Второй нюанс: составные задачи

Элементы бэклога продукта могут быть составными — содержать в себе подзадачи/сабтаски, распределенные по членам команды. Например, на скрине приведена пользовательская история, которая состоит из четырех подзадач.

![Составной элемент бэклога продукта]({{ site.baseurl }}/assets/img/2023-11-10/pic_4.png)

Каждый член команды дает по ним свою оценку. Внимание, вопрос! Сколько весит пользовательская история?

* Вариант А: 16 sp — пользовательская история весит как сумма оценок подзадач.

* Вариант Б: 5 sp — берём максимальную оценку.

* Вариант В: 4 sp — средняя из оценок составляющих элементов.

**Я встречал подходы, когда одна команда выбирала максимальную оценку, а другая среднюю.**

На этом месте должен ворваться скрам-мастер и напомнить, что у нас есть скрам-планирование и каждый участник команды должен давать оценку не своей части работы, а всей пользовательской истории. И в итоге мы должны прийти к тому, что все согласятся с какой-то общей оценкой.

**Но, на мой взгляд, этот подход имеет существенное ограничение**. Всё потому, что каждый член команды будет выполнять свою часть работы в соответствии со своей специализацией. Как я, как системный аналитик, могу адекватно оценить работу фронтенд-разработчика? Я могу адекватно оценить задачи анализа, да и то не всегда, потому что в наших задачах есть высокая доля неопределённости.

Проще давать оценку по своей компетенции, а по чужой сложнее. **Поэтому скрам-планирование, на мой взгляд, отлично применимо, когда участники более-менее взаимозаменяемы**. В других случаях оно не сработает, надо давать оценку по каждому элементу отдельно.

Таким образом, мы приходим к тому, что прежде чем отбирать задачи в спринт, стоит держать в уме следующее:

**Над сложным элементом бэклога могут работать сразу несколько членов команд с разными компетенциями**. Это не ответственность одного. То, что сделает разработчик нужно протестировать, и задачу на тестирование нужно либо вынести отдельно, либо учесть затраты на тестирование в задаче разработки.

**Не всегда и не все члены команды могут полноценно заменить друг друга**. Это история про специализацию — я, как системный аналитик, вряд ли сделаю микросервис, который соответствует всем паттернам проектирования. Хотя есть мнение, что в командах развиваются Т-образные компетенции и со временем системный аналитик может начать помогать бекенд-разработчику закрывать его задачи. Но я в такое не верю — со сложными задачами это невозможно. Полноценной замены членов команд не бывает! А если бывает — покажите мне такую команду, хоть одну.

**Ёмкость есть не только у всей команды, а у каждого её члена**. Даже если суммарный вес задач в собранном спринте удовлетворяет ёмкости вашей команды, вы всё равно с большой вероятностью нарушите коммитмент, если не учтете ёмкость каждого отдельного её члена.

## Вводим понятие «ценность»

Однажды мы работали над проектом интернет-банка для юридических лиц. Мы делали приложение, которое позволяло выполнять групповые операции над платежами. Обычно, когда работают с одним платежом, открывают форму платежного поручения, вносят в неё данные, подписывают, отправляют на исполнение. Вот это тоже самое, но можно обрабатывать платежи массово, а не по одному.

На экране фрагмент бэклога продукта этого проекта.

![Бэклог продукта по группам платежей]({{ site.baseurl }}/assets/img/2023-11-10/pic_5.png)

**У нас есть шесть элементов: четыре из них относятся к разработке нового функционала, один — актуализация документов, и еще один касается дизайна.**

**У каждого элемента есть оценка от всех членов команды, которых нужно привлечь к выполнению соответствующей задачи, причём:**

* Есть составные элементы бэклога, например, первая задача «Отправка группы платежей в Банк», которые требуют привлечения нескольких компетенций («составной» — значит, что для его реализации привлекаются не один, а несколько членов команды).

* Есть задачи, которые требуют привлечения только одного члена команды, например, предпоследняя — «Актуализация архитектурных документов». Здесь нам нужен только системный аналитик (SA).

Также мы вводим еще один параметр — «ценность». **Вводим с допущением (!)**. Мы будем считать, что чем выше элемент в бэклоге, тем он ценнее. Ведь самые ценные элементы мы и так перемещаем наверх, где они на виду. Таким образом, получается, что у первого элемента ценность 6 (потому что у нас всего 6 элементов), у второго 5, и так далее.

Также мы предположим, что у каждого члена команды одинаковая ёмкость в 15 sp.

**Дисклеймер**. Предложение привязывать ценность к рангу элемента — лишь один из многих вариантов. Эту величину вы можете привязывать к другим параметрам, например, к деньгам — сколько вы заработаете или сэкономите на фиче, полученной в результате закрытия элемента бэклога продукта. Но здесь вас ждет ещё большая сложность в виде финансовой оценки задачи.

А теперь соберём все вместе.

* У нас есть оценки по всем элементам бэклога в sp.

* Есть ценность по всем элементам бэклога.

* Есть ёмкость членов команды.

## Задача формирования оптимального бэклога спринта

Мы уже почти подошли к тому, чтобы начать формировать бэклог спринта. Делать это мы будем не просто так, а с двумя важными акцентами.

Первое — будем стараться избегать простоев участников команды. Например, не будем закидывать в спринт задачи только для фронтендера. Будем стремиться загрузить всех членов команды.

Второе — будем пытаться поставить максимальную ценность по результатам выполнения спринта. Нужно больше ценности!

Итак, мы подошли к задаче линейного программирования. С учетом заданных параметров получим математическую модель формирования бэклога спринта:

![ЗЛП с независимыми элементами]({{ site.baseurl }}/assets/img/2023-11-10/pic_6.png)

**Первый компонент модели (первая строка) — функция ценности**. Показывает ценность, которую мы будем нести в спринт. Наша задача — максимизировать эту функцию.

* us — элемент бэклога продукта. Рядом с ними — его порядковый номер. Например, us1 — самый верхний элемент бэклога.

* Коэффициенты при us (слева) показывают ценность элементов бэклога, например, «6» — это ценность первого, а «5» — второго элемента.

**Второй компонент — система ограничений**. Показывает как распределяется загрузка членов команды. У нас их четверо, поэтому уравнений тоже четыре.

* В правой части стоит цифра «15» — это ёмкость одного участника, больше в спринт взять не может.

* Слева — распределение загрузки каждого члена команды на всех элементах бэклога продукта. Например, первое уравнение описывает системного аналитика (SA). Мы видим, что он потратит 3 sp на выполнение первого элемента бэклога продукта, 5 sp — второго, и т.д.

**Третий компонент — система нулей и единиц**. Ноль означает, что мы не возьмем элемент в спринт, а единица — возьмём. Фактически, нам остается понять, какие переменные примут значение «единица», эти элементы и возьмем в спринт.

Однако, данная задача справедлива для независимых элементов бэклога продукта, когда старт работы над одним не зависит от завершения работы над другим. Но у нас есть и зависимые элементы — когда нельзя выполнить одну задачу, не выполнив другую, связанную с ней.

В реальности независимых задач почти не бывает. Поэтому нам нужно как-то определить зависимости элементов. Мы будем это делать также через дополнительные неравенства. Добавил их ниже.

![ЗЛП со связанными элементами]({{ site.baseurl }}/assets/img/2023-11-10/pic_7.png)

На картинке указано, что us5 — задача актуализации архитектурных документов. По процессу ни одну из четырех новых фич мы не сможем реализовать, пока у нас архитектурные документы не будут отражать эти функции. Архитектурная документация — это пререквизит для разработки, поэтому элемент us5 больше, либо равен четырём первым элементам бэклога продукта.

Итого: у нас получилась задача линейного программирования. Как её решить?

Самый простой способ — взять Excel, включить надстройку «Поиск решения» и найти ответ. Он представляет собой множество нулей и единиц, по которым понятно, какие задачи нужно взять в спринт. В данном случае мы возьмём в работу первую, третью, пятую и шестую задачи. Ценность характеризуется условным числом «13». Это наше оптимальное решение.

![Решение ЗЛП]({{ site.baseurl }}/assets/img/2023-11-10/pic_8.png)

Конечно, можно всё посчитать и на бумажке, если у вас есть время. Либо воспользоваться альтернативными инструментами для решения задач линейного программирования. Вариантов много.

## Выводы

> Задачу формирования бэклога спринта можно формализовать.

Иногда создается впечатление, что спринт формируется хаотично: владелец продукта и команда собираются на планирование, решают какие элементы взять в спринт. Причём иногда решение выглядит субъективно: «Возьмем первую, вторую, третью задачу, а четвертая мне не нравится».

Но на самом деле задачу формирования бэклога спринта можно формализовать и помочь команде принять решение, что им следует взять в спринт. Не просто полагаться на субъективную оценку, а посчитать.

> Второй важный вывод — данную задачу можно автоматизировать.

Причем автоматизировать не только средствами Excel, но и что-то свое напрограммировать.

Большая часть данных, если не все, уже лежат у вас в таск-трекере: оценки, приоритеты и т.д. Остаётся только ёмкость. Но и её, с определенными допущениями, можно вычислить ретроспективно, либо задать директивно.

> Третье — при наличии прогрумленного бэклога можно оптимально спланировать почти **ВЕСЬ** проект.

Это в теории. На практике я встречал бэклог, прогрумленный максимум на два или три спринта вперед. Если ваша команда всё грумит идеально, задачи понятны, оценены, то запуская этот алгоритм итеративно, можно спланировать сразу несколько спринтов, а в теории и весь проект.

---

На этом все. Если вам понравилась идея и вы хотите углубиться в тему, рекомендую познакомиться с моей статьей «[Использование методов линейного программирования для формирования бэклога спринта Scrum-команды](https://grebennikon.ru/article-v19f.htmlhttps://grebennikon.ru/article-v19f.html)», опубликованной в журнале «Управление проектами и программами». В ней приведено теоретическое обоснование описанной выше модели.

Отдельно отмечу, что данная модель предназначена для того, чтобы вы не тратили часы на обсуждение того, какую задачу взять в ближайший спринт. Она позволяет по кнопке сформировать оптимальный бэклог спринта. Останется лишь потратить немного времени и решить, вносить ли в него изменения.

Также оставляю [ссылку на доклад](https://www.youtube.com/watch?v=Zcp0V9eTyDI&list=PL_XScYmjXxkfu1GeVUhg_37DJxeSta6jh&index=48), по материалам которого написана эта статья

---

[Оригинальный текст]({{ page.source }})