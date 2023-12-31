---
layout: post
date: 2021-12-16
title:  Как мы управляем техническим долгом аналитики
description: |
    В рамках одного из первых проектов в Альфе мы с командой делали приложение по работе с группами платежей. Приложение позволяло вместо поштучной обработки каждой отдельной платёжки выбирать сразу несколько платёжных поручений, подписывать их и отправлять в Банк на исполнение. Всего за несколько кликов. Очень удобный функционал для клиентов, работающих со множеством платежей одновременно.<br><br>

    В текущем спринте нам с командой надо было реализовать операцию по отправке группы платежей в Банк (подписание и прочие подготовительные операции выполнялись в старой версии системы). Времени — всего неделя. Из наработок, которые мы могли бы переиспользовать, API, позволяющий отправлять в Банк на исполнение единичный платёж.<br><br>

    Команда принимает решение — для каждого платёжного поручения группы, выбранного на фронте, делать вызов существующего API для поштучной отправки платежей. Спустя неделю отчитываемся о достижении цели спринта. Новый функционал открыт на клиентов. Теперь они могут за пару кликов отправлять сразу десять, двадцать и больше платежей в Банк на исполнение. Ценность определённо есть.<br><br>
    
    Но какой ценой была достигнута цель спринта? Ростом нагрузки на сеть. Увеличением времени обработки запросов клиентов. Таймаутами. Решение было неоптимальным. У команды образовался техдолг.
author: Алексей Лобзов
place: Москва, Россия
source: https://habr.com/ru/companies/alfa/articles/595823/
---

Технический долг — это накопленные в программном коде или архитектуре проблемы, связанные с пренебрежением к качеству программного продукта. Эти проблемы мы фиксируем и обещаем когда-то устранить. Поэтому технический долг сопряжён с дополнительными будущими затратами на его устранение.

Техническим долгом надо управлять, стремясь к его минимизации. Ведь чем меньше техдолг, тем выше качество программного продукта. Однако единовременно его очень трудно устранить в связи с потребностью в дополнительных затратах и желанием Бизнеса (да и некоторых участников команд развития) заниматься преимущественно разработкой новых фич. Именно поэтому устранение технического долга надо планировать, выделяя необходимые ресурсы и время.

Технический долг может относиться к продуктам, развитием которых занимаются выделенные команды. Однако и по продуктам без закреплённой команды развития также может существовать техдолг. Кто в таком случае отвечает за его устранение?

В нашей Дирекции за развитие функционального слоя и обеспечение качества разрабатываемых решений отвечают Центры компетенций — аналитики, java и javascript-разработки, тестирования. Соответственно, если есть продукт без команды развития, по которому имеется техдолг, то ресурсы на его устранение следовало бы запрашивать в Центрах компетенций. Но какой из них должен отвечать за устранение технического долга по продукту без команды развития?

В корпоративном направлении Центра компетенций аналитики удалённых каналов доступа мы решили взять технический долг под контроль. Начали с определения понятия техдолга аналитики. Под ним мы договорились понимать то же, что и обычный технический долг, только применительно к артефактам работы аналитика, а именно архитектурной и проектной документации. При этом техдолг аналитики мог проявляться в трёх аспектах, связанных с выкаченным в бой функционалом и влияющих на приоритет его устранения:

1. Отсутствие документации — высокий приоритет.

2. Неактуальность документации — средний приоритет.

3. Несоответствие документации стандарту её оформления — низкий приоритет.

После определения понятия мы решили сформировать бэклог технического долга аналитики. Начали с высокоприоритетного техдолга (Priority = High), связанного с отсутствием документации на выкаченный в бой функционал. Информация по всем выкаченным в бой программным продуктам, в том числе наличию архитектурной и проектной документации на них, находится у функционального сопровождения. Мы запросили у коллег из сопровождения информацию по наличию документации по всем продуктам нового интернет-банка (НИБ) для юридических лиц и индивидуальных предпринимателей. В результате мы получили довольно внушительный перечень отсутствующих документов. Для каждого такого документа была создана отдельная задача с признаком “техдолг” (Issue Type = Тех. долг) в специальном проекте в JIRA.

Развитием большинства продуктов с отсутствующими документами занимались выделенные команды развития. Поэтому большая часть техдолга нашла ответственных за его устранение среди аналитиков соответствующих команд (Assignee, Alfa Teams). Верификатором по задаче устранения техдолга решили сделать техлида аналитика, ответственного за задачу.

Однако были и продукты, за которыми не была закреплена ни одна команда развития. Устранять техдолг аналитики по таким продуктам решили в рамках личных целей аналитиков Центра компетенций. Чуть больше информации про личные цели можно получить в [докладе](https://www.youtube.com/watch?v=P8XjS8Lgocs) Алексея Лобзова на митапе Гильдии Аналитиков.

Выкатка поставок в бой в НИБ выполняется через отдельную доску в JIRA. В задачи на выкатку мы добавили поле для указания ссылки на документацию на поставку, если она имеется, либо на задачу техдолга, если документацию только предстоит написать (Priority = High)/ актуализировать (Priority = Medium)/ привести к стандарту (Priority = Low). Таким образом, чтобы не тормозить разработку, мы решили разрешить выкатываться в бой с техническим долгом по документации, но при условии, что он будет зафиксирован в виде отдельной задачи в JIRA. Это позволило бы нам как минимум не забыть о том, что у нас есть техдолг аналитики.

Конечно, аналитики могли бы хитрить, указывая в новом поле ссылку на старую версию документации, которая могла быть неактуальной или не соответствующей стандарту, либо на документацию по совершенно другому программному продукту вместо того, чтобы заводить и указывать ссылку на задачу по устранению технического долга аналитики. Однако мы решили исходить из того, что наши коллеги работают честно и не скрывают проблемы по своим проектам.

В любом случае, никто не запрещает техлиду либо специалисту функционального сопровождения перед раскаткой ознакомиться с описанием поставки, в том числе приложенной документацией на неё. Даже если приведена ссылка на нужную документацию, её актуальность можно легко проверить как минимум по дате изменения. А соответствие стандарту — по полученным аппрувам и комментариям от коллег-аналитиков, которые обращают на это внимание при ревью документов.

Таким образом, мы сделали первый большой шаг к централизованному управлению техническим долгом аналитики в корпоративном направлении нашего Центра компетенций. Мы создали правила учёта техдолга и сформировали бэклог задач на его устранение. Мы даже определили ответственных по большинству из созданных задач.

Обладая данными по техдолгу аналитики в разрезе команд развития, мы можем совместно с Бизнесом планировать его устранение за счёт включения соответствующих мероприятий в квартальные цели команд. Что же касается техдолга по продуктам без команд развития, пока планируем закрывать его в рамках личных целей аналитиков Центра компетенций.

Больше информации по теме можно получить из [доклада](https://www.youtube.com/watch?v=GNHc7MSHR30) Анатолия Олейнера на недавно прошедшем Open Analysis System Meetup. Как только наберём достаточно статистики по устранению техдолга, поделимся в нашем блоге.

А сейчас вопрос к вам, читателям. Есть ли в вашей компании процесс управления техдолгом в целом и техдолгом аналитики в частности? Как он организован? Какими инструментами пользуетесь?

---

[Оригинальный текст]({{ page.source }})