.. title: Поддержка в oVirt живых снапшотов виртуальных машин
.. slug: поддержка-в-ovirt-живых-снапшотов-виртуальных-машин
.. date: 2012-01-31 02:05:35
.. tags:
.. category:
.. link:
.. description:
.. type: text
.. author: anganar

**Это архивная статья**


В рамках Fedora развивается далеко не только одноименный дистрибутив, но
и много других открытых разработок. Одна из них -
`oVirt <http://www.ovirt.org/>`__. oVirt - это открытая система
управления виртуальной инфраструктурой масштаба центра обработки данных
- десятки и сотни серверов и виртуальных машин, соответствующая
инфраструктура сети и систем хранения. Кстати, именно oVirt является
апстримом для `RHEV <http://ru.redhat.com/products/virtualization/>`__.


Несмотря на то, что в целом oVirt весьма хорош, одной фичи в нем явно не
хватало - поддержки "горячих" снапшотов виртуальных машин без их
остановки. Такая возможность достаточно давно есть в
`qemu-kvm <http://www.linux-kvm.org/>`__, который является основным
гипервизором для oVirt, но выполнить снапшот можно было только вручную
локально. Для живого снапшота из центральной консоли управления не
хватало поддержки такой возможности в
`VDSM <http://www.ovirt.org/wiki/Category:Vdsm>`__, который отвечает за
общение управляющей консоли с гипервизорами.


И вот сегодня в git-репозиторий oVirt `принят
патч <http://gerrit.ovirt.org/gitweb?p=vdsm.git;a=commit;h=f56159d59e29ace09a53bfcf46c5ba836262f730>`__,
который наконец добавляет живые снапшоты в VDSM. Любители острых
ощущений могут собираться из исходников. Все остальные получат
долгожданную фичу в следующем обновлении oVirt.

