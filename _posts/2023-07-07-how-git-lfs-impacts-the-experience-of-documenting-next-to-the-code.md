---
layout: post
date: 2023-07-07
title:  Как Git LFS влияет на опыт ведения документации рядом с кодом
description: |
    Подход к ведению документации рядом с кодом используется в Альфа-Банке на протяжении нескольких лет. Он хорошо подходит для документирования API и сервисов, не требующего примеров пользовательского интерфейса. Однако с документацией на мобильные и веб-фронты ситуация немного сложнее.<br><br>

    В статье обозначу проблему, связанную с ведением фронтовой документации рядом с кодом, и приведу одно из решений на базе Git LFS. Затем поделюсь результатами двух пилотов, проведённых в Банке во втором квартале 2023. Их результаты помогут оценить влияние Git LFS на опыт ведения фронтовой документации рядом с кодом. Статья подойдёт всем, кто занимается подготовкой технической документации на программные продукты.
author: Алексей Лобзов
place: Москва, Россия
source: https://habr.com/ru/companies/alfa/articles/745776/
---

Особенность фронтовой документации — это необходимость соблюдения требования функционального сопровождения к наличию в ней примеров пользовательского интерфейса. Они необходимы, чтобы лучше понимать то, что видит конечный пользователь, его действия и результат, к которым эти действия привели.

> Бинарные файлы изображений с примерами пользовательского интерфейса обычно весят больше, чем текстовые файлы с исходниками кода или документации.

Если размер одного скриншота экрана в формате .png весит 500 Кб, а всего в документе 50 изображений, то всего лишь **одна версия документа будет весить 25 Мб**. Для сравнения, вес репозитория с кодом главной страницы интернет-банка, куда многие продукты стремятся встроить свой функционал, со всей историей коммитов, начиная с 2016 года **составляет около 30 Кб**.

Добавление файлов изображений в репозитории приводит к их раздуванию, а следовательно, к возрастанию нагрузки на сеть при клонировании, увеличению времени сборки, расходованию дополнительного дискового пространства.

Четыре года назад мы с коллегами пытались решить данную проблему за счёт автоматической генерации документации, в том числе примеров пользовательского интерфейса, из автотестов. Решение было представлено на [AnalyzeIT MeetUp #2](https://habr.com/ru/companies/alfa/articles/455441/), однако не получило развития в связи с ограничениями на доработку под требования функционального сопровождения и стандарты ведения документации Банка.

В прошлом году было предложено альтернативное решение. Для хранения файлов изображений предлагалось использовать хранилище Artifactory, взаимодействие с которым было бы реализовано средствами Git LFS. Решение было опробовано на моей локальной машине и описано в статье [Как мы ведём документацию рядом с кодом](https://habr.com/ru/companies/alfa/articles/680556/).

В этом году появилась возможность реализовать описанное решение в Банке, но с небольшой модификацией — хранилище LFS предоставляется корпоративным Bitbucket. В качестве CI/CD используется собственная система на базе Jenkins.

![Процесс ведения документации на фронт]({{ site.baseurl }}/assets/img/2023-07-07/pic_1.png)

Более подробную информацию о том, что получилось, можно узнать из [доклада](https://www.youtube.com/watch?v=IBjTFx5HNas) Игоря Савинова, представленного на недавно прошедшем Alfa Analyze IT Meetup.

Во втором квартале 2023 мы запустили пилот данного решения на нескольких командах. Параллельно на других командах проводился пилот похожего решения, но без Git LFS, предполагающего размещение файлов изображений прямо в репозитории. Таким образом, у нас появилась возможность оценить влияние Git LFS на опыт ведения фронтовой документации.

В конце квартала мы подготовили анкету для сбора обратной связи с участников пилотов, среди вопросов которой, были следующие:

1. Удобно ли вести документацию?

2. Удобно ли читать документацию?

3. Удобно ли искать документацию?

4. Насколько трудозатратно вести документацию по сравнению с Confluence?

Со стороны **Решения А** (без Git LFS) обратную связь оставили 5 человек, со стороны **Решения В** (с Git LFS) — 7. Далее результаты опроса.

## Удобно ли вести документацию?

В обоих пилотах респонденты разделились пополам. Одна половина находит решение удобным для ведения фронтовой документации, другая так не считает.

![Удобно ли вести документацию?]({{ site.baseurl }}/assets/img/2023-07-07/pic_2.png)

В качестве преимуществ участники пилотов отметили процесс ревью документации и возможность отслеживания истории изменений. Вот несколько мнений на этот счёт.

> «Удобный процесс ревью (добавление аппруверов, комментарии, отображение диффов)»

> «Удобно работать с историей изменения документации»

> «Более структурированное хранение, возможность отследить кто и что конкретно менял»

В качестве недостатков были указаны отсутствие WYSIWYG-редактора, возможности вставки изображений из буфера и ограниченные возможности по форматированию документов:

> «Тяжело писать именно фронтовую документацию в asciidoc из-за отсутствия wysiwyg-редактора»

> «Самое неудобное — это добавление картинок, их надо скачать, пронумеровать, переместить в IDEA»

> «В Конфлюенсе больше возможностей для красивого ведения документации, их проще реализовать»

## Удобно ли читать документацию?

Как и в вопросе про ведение документации, мнения разделились. Для одной половины решение удобно, для другой — нет.

![Удобно ли читать документацию?]({{ site.baseurl }}/assets/img/2023-07-07/pic_3.png)

По мнению участников пилотов, итоговой версией документации могут пользоваться не только эксперты функционального сопровождения, но и представители бизнеса:

> «Документацию могут использовать как саппорт, так и бизнес»

Недостаток увидели в расположении примеров пользовательского интерфейса, отдельном от текстового описания, вызванного отсутствием возможности увеличения изображения в итоговой версии документа.

> «Тяжело читать готовую документацию, так как макеты располагаются после таблиц»

## Удобно ли искать документацию?

Участники обоих пилотов в большинстве своём считают решение удобным для поиска документации.

![Удобно ли искать документацию?]({{ site.baseurl }}/assets/img/2023-07-07/pic_4.png)

В качестве комментариев к ответам были указаны следующие:

> «Не нужно искать документацию по фронту по всем пространствам, найти репозиторий всегда проще»

> «Более понятный поиск нужной документации»

## Насколько трудозатратно вести документацию по сравнению с Confluence?

Большинство участников пилотов считают решение более трудозатратными. Причём решение без Git LFS оценивается большинством как сильно более трудозатратное.

![Насколько трудозатратно вести документацию по сравнению с Confluence?]({{ site.baseurl }}/assets/img/2023-07-07/pic_5.png)

Основная причина, по-видимому, это недостаток знаний и опыта работы респондентов с используемым языком текстовой разметки. Один из полученных комментариев это достаточно хорошо иллюстрирует:

> «Если плохо знаешь asciidoc, то на внесение изменений уходит большое количество времени»

## Что в итоге?

> Анализ полученной от участников пилотов обратной связи позволяет сделать вывод, что использование Git LFS не влияет на опыт ведения фронтовой документации.

Данная надстройка функционирует незаметно для члена команды и не меняет флоу его работы с репозиторием. Git LFS позволяет избежать раздувания репозитория, поэтому решение с его использованием (**Решение В**) может стать подходящим вариантом для ведения документации на мобильные и веб-фронты.

Вместе с этим отношение респондентов к ведению фронтовой документации рядом с кодом неоднозначное.

> Половина из них считает пилотные решения неудобными для ведения и чтения документации. Плюс их использование более трудозатратно, если сравнивать с Confluence.

Поэтому прежде чем принять решение о масштабном внедрении рассмотренного решения нужно ещё раз взвесить все «за» и «против», а также попытаться устранить недостатки, подсвеченные участниками пилотов.

---

В наших командах ведением технической документации на программные продукты занимаются системные аналитики. Часто они мне задают вопрос, зачем вести документацию рядом с кодом. Ответ на этот вопрос не является темой настоящей статьи. Однако тем, кому интересно разобраться, рекомендую ознакомиться с материалами:

* [Грановская Ю., Маркелов И. Версионирование требований - применение аналитиками классических практик разработчиков](https://vimeo.com/268472953)

* [Новикова Л., Валеев К., Факторович С., Волынкин Н., Поташников Н., Белянина А. DocOps в работе системного аналитика](https://flowconf.ru/talks/e01ce8c8594242f58260d6a71b26de95/)

* [Поташников Н., Лобзов А., Зингер Е., Гришанов С., Волынкин Н. [Flow live] Docs as code для аналитика](https://www.youtube.com/watch?v=ok9KMRCbrq8)

---

[Оригинальный текст]({{ page.source }})