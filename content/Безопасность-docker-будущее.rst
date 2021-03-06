.. title: Безопасность Docker - будущее
.. slug: Безопасность-docker-будущее
.. date: 2015-04-06 17:30:08
.. tags: containers, docker, security, selinux, redhat, coreos, gccgo, golang, перепост, перевод
.. category:
.. link:
.. description:
.. type: text
.. author: Peter Lemenkov

**Это архивная статья**

Мы в последнее время много времени проводим на мероприятиях, посвященных
Docker. Технология нам в целом уже знакома, и ничего нового тут по ней услышать
наверное не получится, но вот то, как ее используют люди, что они думают, как
строят свои системы, и что понимают в разных смежных областях - очень
интересно.

Вообще, впечатление от технологии очень двойственно. Изначально Docker родился
в среде пользователей Ubuntu, что отчасти перенацелило его на проблемы
коммьюнити и обусловило mindset разработчиков. Плохой пакетный менеджер,
неряшливый подход к версиям ПО, мусорка из неизвестно чьих PPA,
нефункциональный init-демон, слабое знакомство с глубинными вещами - все эти
вещи до сих пор угадываются в общей архитектуре приложения. Например, очень
странное решение проектировать Docker с активным использованием `AuFS,
технологии, которая была признана устаревшей даже в Ubuntu
</content/overlayfs-включают-в-ядро>`__ (хорошо, что `Red Hat протянул тут руку
помощи </content/Облачные-новости>`__). В системах, спроектированных правильно,
этих проблем (почти) нет, и ряд фич Docker просто не востребован. Недаром тот
же `CoreOS отказывается от Docker в пользу своего Rocket
</content/coreos-отказывается-от-btrfs>`__ из-за функциональной перегруженности
первого. С другой стороны, Docker со всеми его минусами очень хорошо
вписывается в концепцию `Agile
<https://ru.wikipedia.org/wiki/Гибкая_методология_разработки>`__ (см. "тяп-ляп
и готово"), т.к. в современном мире быстро полученный результат ценится гораздо
больше архитектурно правильного, но запоздавшего решения.

Наши коллеги неоднократно говорили, что Docker не только решает задачи, но и
вновь поднимает ряд уже решенных вопросов. Мифическая "проблема с
зависимостями" (напомним, `малофункциональный пакетный менеджер
<https://www.linux.org.ru/forum/talks/9610989/page3#comment-9613050>`__)
переносится им на следующий уровень иерархии, где ее решить уже гораздо
сложнее. Автор этих строк в беседе с представителем Docker на одном из
мероприятий в Москве с удивлением обнаружил, что "отсутствие зависимостей" до
сих пор в их коммьюнити считается достижением, проблематика этой задачи ими
просто не воспринимается, а мэйнтейнеры рассматриваются ими, как чинители
препятствий, а не помощники. Из проблем с зависимостями, и привычек
дуалбутчиков, составляющих заметную часть early adopters, вытекает следующая -
`большая часть приложений собраны неизвестно кем, никак не подписаны, и
непонятно, что там внутри
<http://www.vitavonni.de/blog/201503/2015031201-the-sad-state-of-sysadmin-in-the-age-of-containers.html>`__.

Наши коллеги `грустно замечают
<https://plus.google.com/106519095760339600726/posts/65XPyThLLyY>`__, что пора
бы не удовлетворять пользовательские хотелки, а говорить им "нет"
(инвестировать время в обучение хорошим манерам).

Само по себе `фактическое отсутствие цифровых подписей у образов Docker
<https://lwn.net/Articles/628343/>`__, после неудачной `попытки их реализовать
<https://blog.docker.com/2014/10/docker-1-3-signed-images-process-injection-security-options-mac-shared-directories/>`__
это не хорошо и не плохо - это дизайнерское решение такое. Исправить
изначальное отсутствие подписей, это непростая задача, но иногда и не
требующаяся. Например, `Arch Linux долгое время раздавал никак не подписанные
пакеты <http://allanmcrae.com/2011/12/pacman-package-signing-4-arch-linux/>`__,
и ничего не произошло. Никто же не будет использовать ненадежное решение в
продакшене, ведь так? А вот помноженное на изначальную ставку на дистрибутив
для начинающих (где, например, в принципе нет SELinux), пропагандируемое, как
решение для production, и продвигаемое явочным порядком снизу, это уже гораздо
хуже.

Небезопасность Docker была понята уже давным давно. Пока он используется
frontend-разработчиками для создания сайтов-визиток и для тестов, повторимся,
никаких проблем у вас это не вызовет (скорее всего не вызовет - просто
используйте системы, где есть SELinux, и `не выключайте его
</content/docker-и-selinux>`__). Но для backend-разработчиков лучше сразу
полагаться на более надежные решения. Ну, максимум, развернуть прототип, но
продакшен вам все равно лучше бы деплоить без его использования. В Docker Inc
это тоже поняли, и занялись улучшением ситуации, наняв недавно несколько
специалистов.

К сожалению, `зашли не с той стороны
<http://www.theregister.co.uk/2015/03/04/docker_hiring_and_acquiring/>`__,
пригласив специалистов с опытом проектирования и разработки распределенных
систем, а не Base OS. Повторимся, слепым пятном коммьюнити Docker было и
остается видение архитектуры Linux, связанное с изначальным Agile-решением
ориентации на дистрибутив для начинающих, где специалистов в Base OS заметно
меньше, чем в других сообществах.

Так или иначе, но, Docker набрал популярность, как и множество других
неоптимальных решений, и нужно как то с этим жить. В отличие от головной
компании-разработчика, мы выступаем за другой подход - безопасность Docker не
сверху, а снизу, от пакетного менеджера, используемого при сборке образов, и
SELinux при работе, до `распространения сертифицированных образов через HTTPS
<https://securityblog.redhat.com/2014/12/18/before-you-initiate-a-docker-pull/>`__.

И наш коллега Dan Walsh в очередной раз `написал статью
<https://opensource.com/business/15/3/docker-security-future>`__ о той работе,
которую они ведут по повышению надежности Docker-систем:

  **Безопасность Docker - будущее**

  |image0|

  Я начал этот цикл статей о безопасности Docker с `"контейнеры не
  ограничивают" </content/docker-и-selinux>`__.

  Одна из основных задач как в Red Hat, так и в Docker, это сделать это
  утверждение менее справедливым. Моя команда в Red Hat продолжает использовать
  другие механизмы безопасности, чтобы сделать контейнеры более безопасными.
  Далее перечислю некоторые фичи, над которыми мы сейчас работаем, и как они
  могут в будущем изменить Docker и контейнеры в целом.

  **User namespaces**
  Пространство имен пользователей (user namespace), это еще один namespace в
  ядре, который должен позволить нам добиться более полного разделения между
  хостом и контейнером.

  Идея в том, что вы можете задать интервал UID-ов (например, 60,000-61,000),
  которые вы можете транслировать в интервал 0-1000.

  Тоже самое можно сделать с GID-ами. UID 0 внутри контейнера, ядро будет
  транслировать и обращаться как UID 60,000 снаружи контейнера. Любой другой
  UID файла или процесса, который не попадает в этот интервал, будет
  транслироваться в UID=-1 и не будет доступен в контейнере. Это относится и к
  базовому образу системы. Если вы хотите использовать образ с user namespace,
  тогда вам нужно сменить все UID внутри образа на новый UID, в т.ч. и
  суперпользователя. Еще одна проблема с user namespace, это то, что разделы,
  смонтированные в контейнер с файлами, владельцем которого является
  суперпользователь (UID 0), будут недоступны внутри контейнера. Вам следует
  поменять владельцев на весь контент, который вы будете использовать в
  контейнере:

  ``chown -R 60000:60000 /var/lib/content``

  Другая проблема с user namespaces, это то, что если вы хотите использовать их
  для разделения между контейнерами, то вам понадобится свой интервал UID-ов
  для каждого контейнера. Если у вас сотни контейнеров, то вам будут нужны
  сотни диапазонов. Это же создаст еще одну проблему с разделением хранилища
  между хостовыми машинами.

  Одна из прикольных штук с user namespace, это то, что их можно использовать
  вместе с `capabilities
  <http://man7.org/linux/man-pages/man7/capabilities.7.html>`__.

  Если вы запустите контейнер внутри user namespace, то он больше не будет
  нуждаться в capabilities реальной системы. Это значит, что мы можем
  подправить код, чтобы отказаться от системных capabilities, когда контейнер
  запускает user namespace. Это же позволяет нам отказаться от capabilities в
  метках SELinux.

  **Типичные сценарии использования** Я могу представить как минимум три разных
  варианта использования user namespaces.

  - Улучшить разграничение между контейнерами до такой степени, что мы можем
    отключить все `capabilities
    <http://man7.org/linux/man-pages/man7/capabilities.7.html>`__ снаружи
    контейнера. Если это сделать, то мы улучшим и безопасность системы от
    контейнеров, но, ко сожалению, необязательно безопасность между
    контейнерами. В этом режиме, как я представляю себе, мы бы могли выбрать
    один общий UID для DOCKERROOT, затем настроить все контейнеры на его
    использование.

    Например, если DOCKERROOT будет UID=2, я бы настроил трансляцию для UID0=2
    и GID0=2, а затем транслировал бы все UID-ы больше двух в самих же себя.
    Например, 3-MAX\_UID=3-MAX\_UID, и тоже самое для GID. Сделав это мы бы
    исключили возможность атаковать суперпользователя из контейнера. Это и
    проще реализовать при монтировании разделов.

    Я предложил, что может быть лучше попробовать просто использовать
    трансляцию user namespace по умолчанию, сопоставляя UID с 0-65000 этим же
    UID с 0-65000. Тогда, если вы смонтируете в контейнер файл, принадлежащий
    суперпользователю, как обычный раздел, то это сработает, но процессы
    снаружи контейнера не получат никаких `capabilities
    <http://man7.org/linux/man-pages/man7/capabilities.7.html>`__.

    Так мы сможем более-менее разумно экспериментировать с user namespaces.

  - Метод OpenShift: все файлы внутри контейнера получают одну и ту же пару
    UID/GID. Каждый пользователь в системе получает уникальный UID. Это случай,
    когда пользовательский контейнер требует от процессов запускаться с kernel
    capability. Иначе, толку от этого мало.

  - Каждый контейнер получает отдельный диапазон UID от каждого другого
    контейнера. Это позволит запустить огромное число контейнеров с разделением
    UID-ов. Однако, сложность такого решения просто колоссальна. Монтирование
    разделов будет большой головной болью. Чтобы это заработало, я бы
    порекомендовал нам добавить что-то типа *-v /SRC/DEST:U*, которое бы
    сменило UID:GID /SRC во время монтирования на UID по умолчанию для
    контейнера.
  
  Тем не менее, я не предполагаю, что эти три сценария могут использоваться
  одновременно. Я видел предложения от разработчиков ядра позволить "remapping
  of UIDs" в пределах точки монтирования, когда ее присоединяют к контейнеру,
  возможно даже для "bind mounts", но я оставлю обсуждение возможности
  реализации этого функционала разработчикам ядра, и я бы послушал безопасников
  насчет того, хорошая ли это идея вообще?  Сейчас "user namespace" реализовано
  и включено в libcontainer, и готовятся патчи для Docker.

  **Seccomp**
  Одна из проблем со всеми режимами разделения контейнеров, описанных здесь и в
  других статьях, это то, что они все полагаются на ядро хоста для изоляции. В
  отличие от `систем с воздушным файерволлом
  <https://ru.wikipedia.org/wiki/Воздушный_зазор_%28сети_передачи_данных%29>`__
  или даже виртуалок, процессы в контейнерах напрямую общаются с ядром хостовой
  машины. Если в хостовом ядре есть уязвимость, которую может использовать
  контейнер, то появляется возможность обойти системы защиты и выйти из
  контейнера.

  Ядро Linux для архитектуры x86\_64 предоставляет более чем 600 системных
  вызовов, и ошибка в любом из них может вызвать эскалацию привилегий.
  Некоторые из этих сисколлов вызываются крайне редко, и их стоит исключить из
  списка доступных для контейнера.

  Seccomp был разработан инженерами Google для удаления сисколлов из процесса.
  Google использует его внутри браузера Chrome при выполнении плагинов. Т.к.
  плагины обычно скачиваются из недоверенных источников в интернете,
  пользователям стоит контролировать их безопасность.

  Мой коллега, Paul Moore, решил упростить использование seccomp с помощью
  разработки библиотеки для легкого управления деревом системных вызовов.
  Теперь `libseccomp <https://github.com/seccomp/libseccomp>`__ используется в
  таких проектах, как qemu, systemd, lxc tools и т.д..  Мы также разработали
  биндинги к libseccomp для языка Go, чтобы включить в libcontainer для
  удаления сисколлов из контейнеров.

  Мы предлагаем отказаться от следующих системных вызовов для контейнеров:
  kexec\_load, open\_by\_handle\_at, init\_module, finit\_module,
  delete\_module, iopl, ioperm, swapon, swapoff, sysfs, sysctl, adjtimex,
  clock\_adjtime, lookup\_dcookie, perf\_event\_open, fanotify\_init, and kcmp.

  Мы также ждем от вас предожений о том, какие еще системные вызовы стоит
  сделать по умолчанию недоступными для контейнеров. А также мы обдумываем
  удалить все устаревшие сетевые стандарты в Linux: Amateur Radio X.25 (3), IPX
  (4), Appletalk (5), Netrom (6), Bridge (7), ATM VPC (8), X.25 (9), Amateur
  Radio X.25 PLP (11), DECNet (12), NetBEUI (13), Security (14), PF\_KEY key
  management API (15), и все вызовы socket больше, чем than AF\_NETLINK (16).

  Еще одно последствие от создания фильтра запрещенных сисколлов, это то, что
  он будет блокировать вызовы к другой архитектуре.

  Например, по умолчанию в контейнере с включенным seccomp будет запрещено
  вызывать сисколлы для архитектуры i386. Мы бы хотели сделать это поведение
  умолчальным.

  С удалением сисколлов мы сокращаем поверхность атаки атаки вдвое.

  **Настройка Seccomp**
  Мы также разрабатываем возможность передавать в Docker с помощью аргументов
  командной строки список системных вызовов, которые надо игнорировать,
  аналогично функционалу из `capabilities
  <http://man7.org/linux/man-pages/man7/capabilities.7.html>`__ и меткам
  SELinux. Например, эта команда запретит контейнеру получать его текущую
  рабочую директорию:

  ``docker run -d --security-opt seccomp:deny:getcwd /bin/sh``
  
  Наоборот, эта команда вернет обработку вызова в контейнер:
  
  ``docker run -d --security-opt seccomp:allow:clock_adjtime ntpd``
  
  Инженер Red Hat, Matt Heon, сделал презентацию этого функционала (вы также
  можете `скачать видео в формате OGV
  <http://opensource.com/sites/default/files/seccomp-2.ogv>`__):

  Мы обычно начинаем с составления черного списка вызовов, которые должны быть
  заблокированы, но для по-настоящему смелых, можно начать с отключения всех
  системных вызовов, и постепенно добавлять их обратно.

  ``docker run -d --security-opt seccomp:deny:all --security-opt seccomp:allow:getcwd /bin/sh``
  
  В реальности же вам, конечно, понадобится разрешить гораздо больше сисколлов,
  чтобы этот пример заработал. Сообщения о запретах системных вызовов начнут
  появляться в */var/log/audit/audit.log*, также, как сейчас появляются ошибки
  SELinux, ну или в */var/log/messages*, если audit не запущен.

  **Будущее Docker**
  Мы продолжим изучение других security-фич, которые можно добавить.

  Если новые фичи появятся в Linux, или улучшатся старые, то нам хотелось бы
  быть готовыми к использованию их в контейнерах.

  Еще одна задача, которую мы начали изучать, это администрирование
  контейнеров. Сейчас, если вы можете открывать на чтение и/или запись порт
  Docker, то вы можете делать все, что хотите. Увы, но вы таким образом можете
  легко нарушить безопасность системы, и вот почему мы ограничили права доступа
  к */run/docker.socket* всем непривилегированным пользователям. Мы работаем
  над добавлением авторизации, чтобы администратор контейнеров должен быть
  доказать, что он - некий конкретный пользователь. Мы также работаем над
  добавлением соответствующего журналирования событий, чтобы мы могли
  записывать в Journal/syslog кто запускает контейнеры. И наконец, мы хотим
  добавить управление доступом на основе ролей (Role Based Access Control,
  RBAC), чтобы суперпользователь мог контролировать что могут делать другие
  пользователи. Например:

  - Администратору №1 позволено лишь запускать/останавливать указанные
    контейнеры.

  - Администратору №2 разрешено создавать непривилегированные контейнеры,
    использующие указанный образ(ы).

  - Администратор №3 может запускать привилегированные контейнеры.

  **Выводы**
  Когда весь этот функционал будет полностью реализован, то контейнеры Docker
  будут еще более лучше имунны к опасностям на хостовой системе. Цель одна -
  постоянно повышать возможность контейнеров ограничивать (игра слов - "ability
  for containers to contain").

Из других Docker-новостей - `мы начали экспериментировать с gcc-go для его
сборки вместо Golang от Google
<https://lists.fedoraproject.org/pipermail/devel/2015-April/209606.html>`__.
Если кто не следит за апстримом, то `работы там было много
<https://github.com/docker/docker/issues/9207>`__.

.. |image0| image:: https://opensource.com/sites/default/files/styles/image-full-size/public/images/business/bus-containers2.png

