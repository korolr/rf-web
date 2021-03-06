.. title: Новости ARM
.. slug: Новости-arm
.. date: 2015-01-15 13:43:59
.. tags:
.. category:
.. link:
.. description:
.. type: text
.. author: Peter Lemenkov

**Это архивная статья**


| Как вы уже знаете, наши коллеги плотно занимаются архитектурой ARM (и
  в частности AArch64). Например, участник Fedora ARM SIG, `Rob
  Clark <https://github.com/robclark>`__, потратил несколько лет своей
  жизни, чтоб к декабрю 2014 года у него появилась `возможность играть в
  supertuxkart на ARM-платформе с видеочипом Adreno
  A4xx <http://bloggingthemonkey.blogspot.com/2014/12/a4xx-in-holiday-spirit.html>`__.

  [STRIKEOUT:`А чего добился
  ты? <https://lurkmore.to/Сперва_добейся>`__]
| А вот наш коллега, известный велосипедист, Jon Masters, потратил
  `несколько лет на стандартизацию
  AArch64-архитектуры </content/arm64-те-aarch64-и-непростой-путь-перехода-arm-на-новые-стандарты>`__,
  и теперь современная ARM-платформа не требует специального ядра для
  каждой конкретной материнской платы. Предсказуемо, но унификация была
  принята в штыки embedded-разработчиками, которые трудоустроены в
  хардверных ARM-компаниях, традиционно придерживавшихся `стратегии
  максимально быстрого вывода продукта на
  рынок <http://ic.pics.livejournal.com/droids_life/68035718/427/427_600.jpg>`__.

  Чтобы наконец убедить скептиков `Grant
  Likely <https://uk.linkedin.com/in/grantlikely>`__ из Linaro
  постарался написать уже наверное стотыщпятисотую статью, `почему надо
  взять ACPI, и отказаться от
  DeviceTree <http://www.secretlab.ca/archives/151>`__. Вкратце, ACPI
  выгодно для создателей операционных систем, в частности дистрибутивов
  Linux, т.к. его модель поддержки совершенно отличается от DeviceTree,
  который требует собирать ядро для каждой конфигурации. Действительно,
  DeviceTree вполне достаточно для сборки системы для телефона, которая
  больше не требует апгрейдов (маркетологи требуют сделать так, чтоб
  пользователь не апгрейдил телефон, а каждый раз покупал новый), и
  предназначена, чтоб запускать однопоточную джава-машину и играть там в
  птиц и свиней и веселую ферму. Но чтоб собирать дистрибутив, который
  бы работал без изменений на всех текущих и будущих системах, этого
  мало. Ядро там пересобирать при замене RAID-контроллера или сетевухи
  никто не будет, и удивительно, что embedded-разработчики всерьез это
  предлагают.

| Вообще, вырисовывается интересная картина. ACPI, для не-embedded
  программиста наверное проще всего представить в виде некоей
  runtime-сущности, например, шины данных и/или протокола управления, а
  DeviceTree, это строго compile-time опция. Если так, то становится
  понятна деформация восприятия ACPI людьми, привыкшими "пересобирать
  мир". Интересно, что со статичностью DeviceTree его сторонники борются
  уже давно (например, `мы как-то уже слышали про странную концепцию
  оверлеев </content/Наше-отношение-к-outreach-program-women-emacs-переходит-на-git-и-другие-новости>`__).

| К сожалению, и в этот раз статья Гранта убедила лишь тех, кто и так
  убежден, и `споры начались уже в ленте Google+
  автора <https://plus.google.com/+GrantLikely/posts/UZJeJNDjMjC>`__.

  Более-менее развернутое контр-мнение, без детских вскриков про злой
  Microsoft и драгоценную юниксвэйную свободу выбора из десяти
  вариантов, `привел Pavel
  Machek <https://pavelmachek.livejournal.com/126832.html?nojs=1>`__. Он
  обеспокоен следующим:

-  ACPI требует интерпретатор в ядре (см. чуть выше - ACPI, это
   runtime-сервис, в то время, как DeviceTree, это compile-time фича).

-  Больший объем байткода для поддержки драйверов в ACPI (и, как
   полагает Pavel, более хрупкий) вместо нескольких строчек в описании
   DeviceTree.

-  ARM-серверы явно начинают архитектурно отличаться от embedded-ARM.


| 
| К дискуссии подключился и Jon Masters. В своей ленте Google+ он
  `согласился с большей усложненностью ACPI, но еще раз привел три
  аргумента в его
  пользу <https://plus.google.com/+JonMasters/posts/fx1H1pURHpK>`__ -
  это стандарт, это стандарт, и это стандарт. Если не ACPI, то
  разработчикам дистрибутивов общего пользования придется продираться
  сквозь `Cute Embedded Nonsense
  Hacks </content/cute-embedded-nonsense-hacks>`__, более-менее общий
  перечень которых, как вы могли слышать, `однажды составил Rich WM
  Jones </content/Текущие-недостатки-архитектуры-arm>`__. Терпеть это
  ради юниксвэйной свободы пересобирать ядро для поддержки любого мало
  мальски значимого изменения ядра никто не желает. Поэтому
  [STRIKEOUT:уже неважно, что думают несогласные с нами, и как они это
  аргументируют] будет сопровождающееся овациями повсеместное внедрение
  ACPI и UEFI для ARM-платформы. Ну, примерно так, как это уже было с
  systemd.


| |image0|
| **Jon Masters аргументированно и по пунктам отвечает сторонникам
  DeviceTree**

| 
| Пока в коробке мечутся, забавно сталкиваясь друг с другом, напуганные
  сторонники DeviceTree, наши коллеги, посматривая на них слегка
  свысока, но дружелюбно, продолжают работу. `Marcin
  Juszkiewicz <https://www.openhub.net/accounts/hrw>`__, недавно
  `отмечавший двухлетнюю годовщину своей занятости над
  AArch64 </content/2-года-работы-над-aarch64>`__, сообщает, что
  `запустил X11 на физическом железе
  AArch64 <http://marcin.juszkiewicz.com.pl/2015/01/14/started-x11-on-aarch64-hardware-this-time/>`__.


.. |image0| image:: https://lh3.googleusercontent.com/-HzbWhNI-c4c/VLcgZnmsHfI/AAAAAAABKpg/OdEIub5OTbQ/w506-h380/15%2B-%2B1
   :target: https://plus.google.com/+JonMasters/posts/KXLfhcniihs
