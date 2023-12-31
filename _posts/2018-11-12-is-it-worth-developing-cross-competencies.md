---
layout: post
date: 2018-11-12
title:  Стоит ли развивать кросс-компетенции
description: |
    Всем привет! Меня зовут Леша. Я работаю системным аналитиком в Альфа-Банке, где занимаюсь развитием электронных каналов, в частности, новым интернет-банком для юридических лиц и индивидуальных предпринимателей.<br><br>

    Кросс-функциональность в Scrum предполагает, что команда обладает всеми компетенциями, необходимыми для самостоятельной разработки продукта. Но должен ли каждый член команды фокусироваться строго на своей основной компетенции? Или стоит развивать кросс-компетенции, на которых специализируются коллеги?<br><br>

    Сегодня я хочу рассказать об опыте развития кросс-компетенций, полученном моей командой при реализации модуля почтовой корреспонденции в новом интернет-банке. Как мы до этого дошли и что из этого вышло? Всех заинтересованных прошу под кат.
author: Алексей Лобзов
place: Москва, Россия
source: https://habr.com/ru/companies/alfa/articles/429502/
---

Развитие сотрудников в Банке включает три основных компонента – саморазвитие, тренинги и развитие на рабочем месте. Саморазвитие предполагает самостоятельное приобретение новых знаний и навыков за счет изучения релевантной литературы, прохождения видеокурсов, участия в митапах и конференциях. Тренинги позволяют в сжатые сроки получить новую информацию от тренера и применить ее на практике (на фото выпускная группа серии тренингов по js). Развитие на рабочем месте направлено на оттачивание навыков за счет решения реальных проектных задачек.

Данная структура справедлива как в отношении основной компетенции сотрудника, так и для кросс-компетенций. Однако развитие последних является не требованием, а возможностью, предоставляемой Банком. И не каждый сотрудник видит потребность в развитии кросс-компетенций.

## Возникновение потребности

На момент старта работ по разработке модуля почтовой корреспонденции команда существовала почти одиннадцать месяцев и имела один реализованный проект. В силу технических особенностей реализации этого проекта наблюдался дисбаланс в нагрузке – на фронт-разработчика приходилось больше задач, чем на любого другого члена команды. И его могли разгрузить только другие фронты, которыми иногда делились смежные команды.

Отсутствие фронт- или миддл-разработчика по причине отпуска или болезни приводило к тому, что команда не могла реализовать новую фичу без помощи разработчиков других команд. Отсутствие аналитика или инженера по тестированию влекло за собой трудности при закрытии пользовательской истории, т.к. в соответствии с принятым DoD требовалось не только выкатить новую фичу в бой, но и покрыть ее документацией и автотестами. Следовательно, отсутствие хотя бы одного члена команды могло привести к недостижению поставленной цели и переносу истории в следующий спринт.

Потребность в развитии кросс-компетенций членами команды сформировалась в процессе реализации первого проекта. Способность разгрузить своего коллегу, подстраховать на период отсутствия могла помочь повысить производительность команды. Поэтому некоторые из нас решили развивать кросс-компетенции.

## Прирост производительности

Задача реализации модуля почтовой корреспонденции в новом интернет-банке во многом отличалась от первого проекта. Во-первых, наблюдалась повышенная нагрузка на миддл-разработчика. Во-вторых, на проекте прошли онбординг одна полноценная команда и еще несколько новых сотрудников. И наконец, для повышения производительности члены команды, развивающие кросс-компетенции, старались подхватывать задачки коллег. Возросла ли производительность команды в результате развития кросс-компетенций?

Для ответа на поставленный вопрос были проанализированы данные за 34 из 35 спринтов второго проекта (данные за один спринт не рассматривались, т.к. он был начат с нулем сторипойнтов). Производительность команды измерялась долей сожженных сторипойнтов на момент закрытия спринта. Расчеты показали, что средняя производительность команды за рассмотренный промежуток составила 45%.

![Динамика сожжения сторипойнтов]({{ site.baseurl }}/assets/img/2018-11-12/pic_1.jpeg)

В 14 спринтах члены команды продемонстрировали кросс-компетенции. Средняя производительность в этих спринтах составила 53%. Значение показателя для спринтов, где кросс-компетенции продемонстрированы не были, оказалось на уровне 40%. Таким образом, был выявлен прирост средней производительности команды в результате применения кросс-компетенций.

## Планы на будущее

Стоит ли развивать кросс-компетенции? Опыт реализации модуля почтовой корреспонденции в новом интернет-банке показывает, что стоит. Способность подхватывать задачки коллег позволила команде добиться 13% прироста средней производительности.

Конечно, повышенный интерес к кросс-компетенциям может негативно сказаться на качестве работы за счет того, что задачу выполняет менее опытный в заданной области член команды. Поэтому каждому стоит фокусироваться в первую очередь на том, что получается лучше всего — на своей основной компетенции. Однако развитие кросс-компетенций могло бы стать приятным бонусом, который позволил бы сгладить просадку, а в некоторых случаях даже повысить производительность вашей команды. Поэтому интересуйтесь тем, что делают ваши коллеги. И, возможно, когда-нибудь эти знания вам действительно пригодятся.

---

[Оригинальный текст]({{ page.source }})