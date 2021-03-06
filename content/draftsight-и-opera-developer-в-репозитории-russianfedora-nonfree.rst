.. title: DraftSight и Opera Developer в репозитории RussianFedora-NonFree
.. slug: draftsight-и-opera-developer-в-репозитории-russianfedora-nonfree
.. date: 2014-07-19 12:32:15
.. tags:
.. category:
.. link:
.. description:
.. type: text
.. author: carasin

**Это архивная статья**


Многие из вас неоднократно слышали от приверженцев коммерческих ОС
доводы в свою пользу, дескать в мире ОС с открытым исходным кодом
вообще, и в GNU/Linux в частности, катастрофически не хватает
специализированного (профессионального) программного обеспечения.

Контраргументы типа "зато у нас виртуализация, облака и вообще
суперкомпьютеры" в таких случаях не работают: "простому пользователю"
подавай фотошопы™, автокады™ и иже с ними. Если фотошопам™ можно хоть
как-то противопоставить `GIMP <http://www.gimp.org/>`__, то с
`САПР <https://ru.wikipedia.org/wiki/Система_автоматизированного_проектирования>`__
(`CAD <https://en.wikipedia.org/wiki/CAD>`__) всё несколько сложнее. Дело
в том, что свободные проекты, например,
`FreeCAD <http://freecadweb.org/>`__ и
`LibreCAD <http://librecad.org/cms/home.html>`__, будь они хоть трижды
функциональными программами, `не могут обеспечить
работу <https://www.linux.org.ru/news/opensource/7361666>`__ с чертежами
в де факто стандартном формате
`DWG <https://ru.wikipedia.org/wiki/DWG>`__. Не будем подробно
останавливаться на том, почему и как в области автоматизированного
проектирования сложилась ситуация с повсеместным засильем проприетарного
формата чертежей. Являясь инженером-электриком, скажу лишь одно: даже в
электротехническом проектировании, где от инженерной графики, как
правило, требуется лишь изображение прямоугольников, окружностей и
прямых линий, зачем-то используют
`AutoCAD <http://www.autodesk.ru/products/autocad/overview>`__. В этой
ситуации сложно что-либо изменить, поэтому разработчики других
коммерческих САПР, несмотря на наличие вполне себе открытого стандарта
`DXF <https://ru.wikipedia.org/wiki/DXF>`__, вынуждены обеспечивать хоть
какую-нибудь поддержку формата DWG. Возвращаясь к теме отсутствия (по
мнению некоторых приверженцев коммерческих ОС) специализированного софта
в GNU/Linux, отмечу, что ситуация всё же отличается от фантазий
обитателей форумов — по крайней мере в сфере автоматизированного
проектирования. Помимо указанных выше свободных CAD'ов в нашем
распоряжении также имеется ряд проприетарных САПР (с поддержкой DWG):
`BricsCAD <https://www.bricsys.com/ru_RU/bricscad/>`__,
`Medusa4 <http://www.cad-schroer.com/products/medusa4.html>`__,
`DraftSight <http://www.3ds.com/ru/produkty-i-uslugi/draftsight/>`__.

Последние две, к слову, имеют бесплатные версии для некоммерческого
использования, а DraftSight, помимо того, доступен также и в виде
`пакета
RPM <http://www.3ds.com/ru/produkty-i-uslugi/draftsight/zagruzit-draftsight/>`__.

Но получен этот пакет путём конвертирования из
`DEB <https://ru.wikipedia.org/wiki/Deb_(формат_файлов)>`__
`небезызвестным "костылём" <http://joeyh.name/code/alien/>`__. Конечно,
речи об удачной установке такого пакета в Fedora и быть не может. Все
эти обстоятельства в итоге привели к тому, что в репозитории
RussianFedora-NonFree появился `переработанный
пакет <http://redmine.russianfedora.pro/issues/1348>`__ с последней на
момент написания этой статьи версией DraftSight. Кроме того, в нём
частично решена проблема (присутствующая в оригинале) с невозможностью
открытия файла, если путь к нему содержит пробелы и нелатинские символы
(в текущем виде возможно открыть только один файл за раз). Ну и, раз уж
пошла речь о проприетарном ПО, стоит упомянуть `долгожданное
появление <http://blogs.opera.com/desktop/2014/06/opera-24-linux-released-developer-stream/>`__
сборки под GNU/Linux "хромированного" браузера Opera. На данный момент
существует только находящаяся в разработке ветка 24.x, причём пакеты
собираются исключительно в формате DEB. Это упущение мы `постарались
исправить <http://redmine.russianfedora.pro/issues/1354>`__, результатом
чего стало включение в репозиторий RussianFedora-NonFree перепакованного
обновлённого сурового норвежского браузера.

