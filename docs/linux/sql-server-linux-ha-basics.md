---
title: Основные сведения о доступности SQL Server для Linux развертываний | Документы Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 11/27/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: f311d9c3116083120b97fa78486242939a438de9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-availability-basics-for-linux-deployments"></a>Основные сведения о доступности SQL Server для Linux развертываний

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Начиная с [!INCLUDE[sssql17-md](../includes/sssql17-md.md)], [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] поддерживается на Windows и Linux. На основе Windows, например [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертываниях [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] баз данных и экземпляров должны быть высокодоступной под Linux. В этой статье рассматриваются технические аспекты планирование и развертывание высокой надежности на основе Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] баз данных и экземпляров, а также некоторые отличия от установок на основе Windows. Поскольку [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] могут быть неизвестны для специалистов по Linux и Linux могут быть впервые [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] специалистов по ИТ, статьи в некоторых случаях понятия, которое может быть знакома некоторые и знакомы пользователям.

## <a name="includessnoversion-mdincludesssnoversion-mdmd-availability-options-for-linux-deployments"></a>[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Параметры доступности для развертываний Linux
Помимо резервного копирования и восстановления те же три функции доступности доступны в Linux и для развертывания на основе Windows.
-   Всегда в группах доступности (и действий)
-   Всегда на экземплярах отказоустойчивого кластера (FCI)
-   [Доставка журналов](sql-server-linux-use-log-shipping.md)

В Windows FCI всегда требуют базовый отказоустойчивый кластер Windows Server (WSFC). В зависимости от сценария развертывания группы Доступности обычно требуется базовый WSFC с исключением, является новой None variant в [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. WSFC не существует в Linux. Реализация в Linux кластеризации рассматривается в разделе [Pacemaker для группы доступности AlwaysOn и отказоустойчивого кластера экземпляров в Linux](#pacemaker-for-always-on-availability-groups-and-failover-cluster-instances-on-linux).

## <a name="a-quick-linux-primer"></a>Краткий учебник Linux
При некоторых установках Linux могут быть установлены с помощью интерфейса, большинство не, это означает, что практически полностью на уровне операционной системы выполняется через командную строку. — Это общий термин для эта командная строка в мире Linux *bash оболочки*.

В Linux многие команды, необходимо выполнить с повышенными привилегиями, подобно множество различных действий необходимо выполнить в Windows Server с правами администратора. Существует два основных способа выполняться с повышенными привилегиями:
1. Выполняются в контексте пользователя, соответствующие. Чтобы изменить на другого пользователя, используйте команду `su`. Если `su` выполняется сам по себе без имени пользователя, при условии, что необходимо знать пароль, вы попадете в оболочке как *корневой*.
   
2. Дополнительные общие и безопасности обоснованное способ запуска действия — использовать `sudo` перед выполнением ничего. Многие примеры в этой статье используется `sudo`.

Некоторые распространенные команды, каждый из которых имеют различные переключатели и параметры, которые можно проверять через Интернет:
-   `cd` — Перейдите в каталог
-   `chmod` — изменить разрешения для файла или каталога
-   `chown` — Смена владельца файла или каталога
-   `ls` — Показать содержимое каталога
-   `mkdir` — Создайте папку (каталог) на диске
-   `mv` — Перемещение файла из одного расположения в другое
-   `ps` — Показывать все процессы работы
-   `rm` — удалить файл локально на сервере
-   `rmdir` — удалить папку (каталог)
-   `systemctl` — Запуск, остановка или включить службы
-   Текст команды редактора. В Linux существуют различные параметры текстового редактора, таких как vi и emacs.

## <a name="common-tasks-for-availability-configurations-of-includessnoversion-mdincludesssnoversion-mdmd-on-linux"></a>Общие задачи для конфигураций доступности [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] в Linux
В этом разделе рассматриваются задачи, которые являются общими для всех на основе Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертываний.

### <a name="ensure-that-files-can-be-copied"></a>Убедитесь, что файлы могут быть скопированы
Копирование файлов с одного сервера на другой — это задача, любой пользователь, с помощью [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] в Linux должно быть способно выполнить. Эта задача очень важно для конфигурации группы Доступности.

Таких вещей, как разрешение проблемы может существовать в Linux, а также для установки на основе Windows. Тем не менее если вы знакомы с тем, как копировать с сервера на сервер в Windows может быть незнакомы как это можно сделать в Linux. Распространенный способ — использовать программу командной строки `scp`, который обозначает безопасного. В фоновом `scp` использует OpenSSH. SSH означает безопасный оболочки. В зависимости от дистрибутив Linux OpenSSH сам не могут быть установлены. Если это не так, необходимо сначала установить OpenSSH. Дополнительные сведения о настройке OpenSSH см. сведения по следующим ссылкам для каждого распределения:
-   [Red Hat Enterprise Linux (RHEL)](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-OpenSSH.html)
-   [SUSE Linux Enterprise Server (SLES)](https://en.opensuse.org/SDB:Configure_openSSH)
-   [Ubuntu](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

При использовании `scp`, необходимо указать учетные данные сервера, если это не источник или назначение. Например с помощью

```bash
scp MyAGCert.cer username@servername:/folder/subfolder
```

копирует файл MyAGCert.cer в папке, указанной на другом сервере. Обратите внимание, что необходимо иметь разрешения — и, возможно, владельцем-файла скопируйте его, поэтому `chown` может также необходимо применять перед копированием. Аналогичным образом на принимающей стороне нужному пользователю требуется доступ к работы файлом. Например, чтобы восстановить этот файл сертификата `mssql` пользователь должен быть возможность доступа к ним.

Samba, являющийся вариант Linux блок сообщений сервера (SMB), можно также использовать для создания общих папок, таких как доступ к UNC-пути `\\SERVERNAME\SHARE`. Дополнительные сведения о настройке Samba см. сведения по следующим ссылкам для каждого распределения:
-   [RHEL](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Confined_Services/chap-Managing_Confined_Services-Samba.html)
-   [SLES](https://www.suse.com/documentation/sles11/book_sle_admin/data/cha_samba.html)
-   [Ubuntu](https://help.ubuntu.com/community/Samba)

Также можно использовать общие папки SMB на основе Windows; Общие папки SMB не обязательно должны быть под управлением Linux, при условии, что правильно настроены на сервере Linux, где размещен клиентскую часть Samba [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] и общего ресурса имеет права доступа. Для них в смешанной среде, это будет одним из способов использовать существующую инфраструктуру для под управлением Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертываний.

Единственное, что важно является версия Samba развертывания следует совместимые SMB 3.0. При добавлении поддержки SMB в [!INCLUDE[sssql11-md](../includes/sssql11-md.md)], ему требуется все общие папки для поддержки SMB 3.0. Если для общей папки и не Windows Server с помощью Samba, общий ресурс на основе Samba следует использовать Samba 4.0 или более поздней версии и в идеале 4.3 или более поздней версии, которая поддерживает SMB 3.1.1. Является хорошим источником информации о SMB и Linux [SMB3 в Samba](http://events.linuxfoundation.org/sites/events/files/slides/smb3-in-samba.pr__0.pdf).

Наконец с помощью сетевой общей папке системы (NFS) является параметром. С помощью NFS не подходит для развертывания на основе Windows из [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]и может использоваться только для развертывания на основе Linux.

### <a name="configure-the-firewall"></a>Настройка брандмауэра
Как и в Windows, дистрибутивы Linux имеют встроенного брандмауэра. Если ваша компания использует внешнего брандмауэра для серверов, отключение брандмауэров в Linux может быть приемлемым. Тем не менее независимо от того, где включен брандмауэр, порты должны быть открыты. Следующая таблица Документирует общие порты, необходимые для высокой доступности [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертываний в Linux.

| Номер порта | Тип     | Описание                                                                                                                 |
|-------------|----------|-----------------------------------------------------------------------------------------------------------------------------|
| 111         | TCP/UDP  | NFS. `rpcbind/sunrpc`                                                                                                    |
| 135         | TCP      | Samba (если используется) — конечных точек                                                                                          |
| 137         | UDP      | Samba (если используется) — служба имен NetBIOS                                                                                      |
| 138         | UDP      | Samba (если используется)-датаграмм NetBIOS                                                                                          |
| 139         | TCP      | Samba (если используется) — сеанса NetBIOS                                                                                           |
| 445         | TCP      | Samba (если используется) — SMB по протоколу TCP                                                                                              |
| 1433        | TCP      | [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] — порт; по умолчанию При необходимости можно изменить с помощью `mssql-conf set network.tcpport <portnumber>`                       |
| 2049        | TCP, UDP | NFS (если используется)                                                                                                               |
| 2224        | TCP      | Pacemaker — используемый `pcsd`                                                                                                |
| 3121        | TCP      | Pacemaker — обязательных Pacemaker удаленных узлов                                                                    |
| 3260        | TCP      | iSCSI Initiator (если используется) — может быть изменено в `/etc/iscsi/iscsid.config` (RHEL), но должен соответствовать порту цели iSCSI) |
| 5022        | TCP      | [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] — по умолчанию порт, используемый для конечной точки группы Доступности; можно изменить при создании конечной точки                                |
| 5403        | TCP      | Pacemaker                                                                                                                   |
| 5404        | UDP      | Pacemaker — предусмотренного Corosync при использовании многоадресной рассылки UDP.                                                                     |
| 5405        | UDP      | Pacemaker — предусмотренного Corosync                                                                                            |
| 21064       | TCP      | Pacemaker — предусмотренного ресурсов с помощью управление жизненным ЦИКЛОМ                                                                                 |
| Переменная    | TCP      | Порт конечной точки группы Доступности; значение по умолчанию — 5022                                                                                           |
| Переменная    | TCP      | NFS-порт для `LOCKD_TCPPORT` (в `/etc/sysconfig/nfs` на RHEL)                                              |
| Переменная    | UDP      | NFS-порт для `LOCKD_UDPPORT` (в `/etc/sysconfig/nfs` на RHEL)                                              |
| Переменная    | TCP/UDP  | NFS-порт для `MOUNTD_PORT` (в `/etc/sysconfig/nfs` на RHEL)                                                |
| Переменная    | TCP/UDP  | NFS-порт для `STATD_PORT` (в `/etc/sysconfig/nfs` на RHEL)                                                 |

Другие порты, которые могут использоваться Samba, в разделе [использование порта Samba](https://wiki.samba.org/index.php/Samba_Port_Usage).

И наоборот имя службы под Linux можно также добавить исключение вместо порта; например `high-availability` для Pacemaker. Ссылаться на распределение для имен в случае направление, в которую вы хотите продолжить. Например на RHEL команду, чтобы добавить в Pacemaker —

```bash
sudo firewall-cmd --permanent --add-service=high-availability
```

**Документация по брандмауэра:**
-   [RHEL](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/s1-firewalls-haar)
-   [SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html)

### <a name="install-includessnoversion-mdincludesssnoversion-mdmd-packages-for-availability"></a>Установка [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] пакетов для обеспечения доступности
На основе Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] установки, некоторые компоненты установлены даже в базовый механизм установки, а другие нет. Под Linux, только [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] engine устанавливается как часть процесса установки. Все остальные является необязательным. Для высокой доступности [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] экземпляров в Linux два пакета должен быть установлен с [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]: [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агента (*mssql-server-agent*) и высокий уровень доступности (HA) пакет ( *MSSQL-server-ha*). Хотя [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агента технически необязателен, это [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]планировщика заданий и в необходимых при доставке журналов, поэтому рекомендуется установить. В рамках установки ОС Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агента не является обязательным.

>[!NOTE]
>Для тех, Кому [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агент [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]встроенные задания планировщика. Он является стандартным способом для администраторов баз данных для планирования резервного копирования и других [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] обслуживания. В отличие от установки на основе Windows из [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] где [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агент является совершенно другой службой, в Linux, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] агент выполняется в контексте [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] сам.

Если групп доступности или экземпляры отказоустойчивого кластера будут настроены в конфигурации на основе Windows, они имеют кластеры. Поддержка кластеров означает, что [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] имеет конкретный ресурс библиотеки DLL, которые известны WSFC (файл sqagtres.dll и sqsrvres.dll для FCI hadrres.dll для групп доступности) и используемые WSFC, чтобы убедиться, что [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] работает кластеризованный функциональные возможности, запущена, и работает неправильно. Поскольку кластеризации является внешним не только к [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] , но сам Linux, корпорация Майкрософт пришлось кода эквивалент библиотека DLL ресурсов для развертывания на основе Linux AG и отказоустойчивого Кластера. Это *mssql-server-ha* пакет, также известный как [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] ресурсов агента для Pacemaker. Чтобы установить *mssql-server-высокой надежности* пакета см. в разделе [установить пакеты высокого уровня ДОСТУПНОСТИ и агент SQL Server](sql-server-linux-deploy-pacemaker-cluster.md#install-the-sql-server-ha-and-sql-server-agent-packages).

Дополнительные пакеты для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] в Linux, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Full-Text Search (*mssql-server-fts*) и [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] службы Integration Services (*mssql сервер является*), не являются требуется для обеспечения высокой доступности, либо на экземпляре отказоустойчивого Кластера или группе Доступности.

## <a name="pacemaker-for-always-on-availability-groups-and-failover-cluster-instances-on-linux"></a>Pacemaker для группы доступности AlwaysOn и экземпляров отказоустойчивых кластеров в Linux
Как в предыдущих отмечалось, Pacemaker с Corosync — единственный механизм кластеризации, в настоящее время поддерживается корпорацией Майкрософт для групп доступности и отказоустойчивые кластеры. В этом разделе рассматриваются основные сведения для понимания решения, а также планирование и развернуть его, чтобы [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] конфигурации.

### <a name="ha-add-onextension-basics"></a>Основы расширения и надстройки в высокой ДОСТУПНОСТИ
Все распределения, поддерживаемых в настоящее время поставки высокого уровня доступности надстройки-в-модуль, который основан на Pacemaker, кластеризация стека. Стек включает в себя два основных компонентов: Pacemaker и Corosync. Все компоненты стека являются:
-   Pacemaker — основной компонент, который выполняет действия, такие как координаты на нескольких компьютерах кластеризованный кластеризации.
-   Corosync — framework и набор интерфейсов API, который предоставляет таких вещей, как кворум, возможность Сбой перезапуска процессов и т. д.
-   libQB — предоставляет такие операции, как ведение журнала.
-   Агент ресурсов — конкретные функции, обеспечиваемые, чтобы приложения можно интегрировать с Pacemaker.
-   Забора агент — скриптов или функций, которые описывать узлов и работать с ними, если они возникли проблемы.
    
> [!NOTE]
> Стек кластера часто называют Pacemaker в системе Linux.

Это решение является в некотором смысле аналогично, но во многом отличается от процесса развертывания кластерных конфигураций с помощью Windows. В Windows доступности формы кластеризации, называется отказоустойчивым кластером Windows Server (WSFC), встроено в операционную систему и функции, которая позволяет создавать WSFC, отказоустойчивая кластеризация, по умолчанию отключена. В Windows построены на основе WSFC групп доступности и отказоустойчивые кластеры и совместно использовать тесной интеграции из-за конкретный ресурс библиотеки DLL, которая обеспечивается [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Это решение тесно связанные возможна в целом так как это из одного поставщика.

![](./media/sql-server-linux-ha-basics/image1.png)

В Linux во время каждой поддерживаемой распределение имеет Pacemaker доступно, каждого распространителя можно настроить и немного разные реализации и версий. Некоторые различия, будут отражены в инструкциях данной статьи. Представляет уровень кластеризации с открытым кодом, несмотря на то, что он поставляется с распределения, он не интегрирована тесно таким же образом WSFC под управлением Windows. Поэтому корпорация Майкрософт предоставляет *mssql-server-ha*, после чего [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] и стека Pacemaker может предоставить близко к, но не в точности одинаковую функциональность для групп доступности и отказоустойчивые кластеры под управлением Windows.

Полную документацию по Pacemaker включая более подробное объяснение того, что все, что имеет с полные данные ссылки, RHEL и SLES:
-   [RHEL](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-overview-HAAR.html)
-   [SLES](https://www.suse.com/documentation/sle_ha/book_sleha/data/book_sleha.html)

Ubuntu имеет руководство для обеспечения доступности.

Дополнительные сведения о стеке целиком см. также официальные [страницы документации Pacemaker](http://clusterlabs.org/doc/) на сайте Clusterlabs.

### <a name="pacemaker-concepts-and-terminology"></a>Pacemaker основные понятия и терминология
В данном разделе описываются общие понятия и терминология для реализации Pacemaker.

#### <a name="node"></a>Узел
Узел — это сервер, участвующих в кластере. Кластер Pacemaker поддерживает до 16 узлов. Это число может быть превышено, если Corosync не запущена на дополнительных узлах, но Corosync является обязательным для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Таким образом, максимальное количество узлов, кластер может сохранять для любого [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-конфигурации на основе 16; это ограничение Pacemaker и никак не связано с максимального ограничения для групп доступности и отказоустойчивые кластеры, обусловленной [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. 

#### <a name="resource"></a>Ресурс
WSFC и Pacemaker кластера имеют понятие ресурса. Ресурс — это особые функциональные возможности, выполняется в контексте кластера, например диск или IP-адрес. Например под Pacemaker можно получить создать ресурсы отказоустойчивого Кластера и группы Доступности. Это не разнородных сходен в WSFC, где вы видите [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] для либо FCI или ресурса группы Доступности при настройке группы Доступности, но не в точности равен из-за базовых различий в том, как [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] интегрируется с Pacemaker.

Pacemaker имеет стандартный и клон ресурсов. Клон ресурсы, которые одновременно работать на всех узлах. Примером такого IP-адрес, который выполняется на нескольких узлах в целях балансировки нагрузки. Любой ресурс, который создается для FCI использует стандартный ресурс, так как только один узел может содержать экземпляр отказоустойчивого Кластера в любой момент времени.

При создании группы Доступности требуется особый вид клон ресурсу с именем ресурса многими состояниями. Хотя AG имеет только одну первичную реплику, группы Доступности, сама выполняется на всех узлах, на котором он настроен для работы в и, потенциально могут позволить такие элементы, как доступ только для чтения. Так как это «live» использование узла ресурсов существует понятие два состояния: главного и подчиненного. Дополнительные сведения см. в разделе [многими состояниями ресурсов: ресурсы, имеющие несколько режимов](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/s1-multistateresource-HAAR.html).

#### <a name="resource-groupssets"></a>Наборы групп ресурсов
Аналогично роли в WSFC Pacemaker кластер использует концепцию группы ресурсов. Группа ресурсов (называемую набором в SLES) — это совокупность ресурсов, которые работать вместе и могут выполнять переход с одного узла на другой, как единое целое. Группы ресурсов не может содержать ресурсы, которые настроены как ведущий и ведомый; Таким образом они не может использоваться для групп доступности. Группа ресурсов может использоваться для FCI, но не обычно Рекомендуемая конфигурация.

#### <a name="constraints"></a>Ограничения
WSFCs имеют различные параметры для ресурсов, а также таких вещей, как зависимости, которые настраивают WSFC иерархическое отношение между двумя ресурсами на разных. Зависимость — просто это правило, предписывая WSFC ресурса, который должен быть сначала online.

Кластер Pacemaker не поддерживает концепцию зависимостей, но существуют ограничения. Существуют три типа ограничений: совместное размещение, расположение и порядок.
-   Ограничение совместного размещения, обеспечивает ли два ресурса должен быть запущен на том же узле.
-   Ограничение расположения предписывает Pacemaker кластеру, где можно (или не может) запускаться ресурс.
-   Ограничением упорядочивания предписывает кластеру Pacemaker порядок, в котором следует начать ресурсы.

> [!NOTE]
> Ограничения на совместное размещение не являются обязательными для ресурсов в группе ресурсов, так как все эти действуют как единое целое.

#### <a name="quorum-fence-agents-and-stonith"></a>Кворум, граница агентов и STONITH
Кворум в Pacemaker похож на WSFC концепцией. Механизм кворума кластера специально чтобы убедиться, что кластер остается запущен и работает. WSFC и надстройки высокого уровня ДОСТУПНОСТИ для дистрибутивы Linux существует понятие голосование, где каждый узел учитывается при подсчете кворума. Требуется большинство голосов, в противном случае в худшем случае, кластер завершит работу.

В отличие от WSFC отсутствует ресурс следящий сервер для работы с кворумом. Как WSFC предназначена для того, чтобы свести число голосующих нечетным. Конфигурация кворума имеет особенности для групп доступности, чем экземпляры отказоустойчивого кластера.

WSFCs отслеживания состояния узлов, входящих и их обработки при возникновении проблемы. Более поздние версии WSFCs предоставляют возможности, такие как карантин узла, некорректно работающие или недоступен (не для узла, сетевого взаимодействия — вниз, и т. д.). На стороне Linux функциональность обеспечивается агент разграничения. Понятие иногда называют разграничения. Однако эти агенты граница относятся к развертыванию и часто, предоставляемые поставщиками оборудования и некоторые поставщики программного обеспечения, например те, которые предоставляют низкоуровневые оболочки. Например VMware предоставляет агент разграничения, который может использоваться для виртуальных машин Linux, виртуализованных с помощью vSphere.

Кворума и ограждения ties в другое понятие вызове STONITH или прокрутить на другой узел в заголовке. Необходимо иметь кластера все дистрибутивы Linux, поддерживаемые Pacemaker STONITH. Дополнительные сведения см. в разделе [ограждения Red Hat высокого уровня доступности кластера](https://access.redhat.com/solutions/15575) (RHEL).

#### <a name="corosyncconf"></a>corosync.conf
`corosync.conf` Файл содержит конфигурации кластера. Он расположен в `/etc/corosync`. Во время обычных ежедневных операций этот файл не следует необходимо изменить, если кластер настроен правильно.

#### <a name="cluster-log-location"></a>Расположение журнала кластера
Расположение журнала для кластеров Pacemaker различаться в зависимости от распределения.
-   RHEL и SLES- `/var/log/cluster/corosync.log`
-   Ubuntu — `/var/log/corosync/corosync.log`

Чтобы изменить расположение по умолчанию ведения журнала, измените `corosync.conf`.

## <a name="plan-pacemaker-clusters-for-includessnoversion-mdincludesssnoversion-mdmd"></a>Кластеры Pacemaker плана для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
В этом разделе рассматриваются важные моменты планирования для Pacemaker кластера.

### <a name="virtualizing-linux-based-pacemaker-clusters-for-includessnoversion-mdincludesssnoversion-mdmd"></a>Виртуализации под управлением Linux Pacemaker кластеры для [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Использование виртуальных машин для развертывания на основе Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертывания для групп доступности и отказоустойчивые кластеры, связанная с тем же правилам, что и для их аналоги под управлением Windows. Имеется базовый набор правил для поддержки виртуализированных [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] развертываний, предоставляемой корпорацией Майкрософт в [956893 КБ поддержки Microsoft](https://support.microsoft.com/en-us/help/956893/support-policy-for-microsoft-sql-server-products-that-are-running-in-a-hardware-virtualization-environment). Различных низкоуровневых оболочек, например Microsoft Hyper-V и VMware ESXi могут иметь различные дисперсии Кроме того, у из-за различий в самих платформы.

Когда дело доходит до групп доступности и отказоустойчивые кластеры, в области виртуализации, убедитесь, что для узлов кластера заданного Pacemaker запрет близости. При настройке для обеспечения высокой доступности в конфигурации группы Доступности или экземпляр отказоустойчивого Кластера, размещение виртуальных машин [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] никогда не должен выполняться на одном узле низкоуровневой оболочки. Например, при развертывании отказоустойчивого Кластера с двумя узлами требуется входить *по крайней мере* три узлы низкоуровневой оболочки, поэтому, где-либо для одной из виртуальных машин размещение узла для перехода в случае сбоя узла, особенно в том случае, если с помощью функции, например Live Миграция или vMotion.

Дополнительные сведения см.:
-   Документация по Hyper-V — [использование гостевой кластеризации для обеспечения высокой доступности](https://technet.microsoft.com/library/dn440540(v=ws.11).aspx)
-   Технический документ (написаны для развертывания на основе Windows, но большинство понятий, по-прежнему применять) — [планирования высокой доступности, критически важных развертываний SQL Server с VMware vSphere](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/solutions/vmware-vsphere-highly-available-mission-critical-sql-server-deployments.pdf)

>[!NOTE]
>RHEL с кластером Pacemaker с STONITH Hyper-v еще не поддерживается. Пока, поддерживаемый для дополнительных сведений и обновлений, обратитесь к [политики поддержки для высокой доступности кластеров RHEL](https://access.redhat.com/articles/29440#3physical_host_mixing).

### <a name="networking"></a>Работа с сетью
В отличие от WSFC Pacemaker не требует выделенного имени или по крайней мере один выделенный IP-адрес для самого кластера Pacemaker. Групп доступности и отказоустойчивые кластеры требуют IP-адресов (см. документацию по каждому Дополнительные сведения), но не имена, так как отсутствует ресурс сетевого имени. SLES позволяют настроить IP-адрес для административных целей, но не является обязательным, как можно увидеть в [создать кластер Pacemaker](sql-server-linux-deploy-pacemaker-cluster.md#create).

Как WSFC, Pacemaker бы предпочли бы избыточных сетевых подключениях, то есть различных сетевых адаптеров (сетевых адаптеров или pNICs для физического) с отдельных IP-адресов. С точки зрения конфигурации кластера каждый IP-адрес будет иметь как свой собственный кольца. Тем не менее как с WSFCs в настоящее время многие виртуализованных или в общедоступном облаке, где на самом деле существует только один виртуализировать представленные на сервер сетевого Адаптера (vNIC). Если все pNICs и vNICs подключены к тому же коммутатору физическим или виртуальным, нет не true избыточности на сетевом уровне, поэтому Настройка нескольких сетевых адаптеров — немного иллюзии к виртуальной машине. Избыточность сети обычно встроена в низкоуровневой оболочки для виртуализованных развертываний и определенно встроено в общедоступном облаке.

Один с несколькими сетевыми адаптерами и Pacemaker и WSFC отличается, Pacemaker разрешает несколько IP-адресов в одной и той же подсети, а WSFC — нет. Дополнительные сведения о нескольких подсетей и кластеры Linux см. в статье [настроить несколько подсетей](sql-server-linux-configure-multiple-subnet.md).

### <a name="quorum-and-stonith"></a>Кворум и STONITH
Требования к конфигурации кворума и связаны с развертывания группы Доступности или с FCI [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

STONITH является обязательным для поддерживаемых Pacemaker кластера. В документации, предоставляемой распределение используется для настройки STONITH. Например, в [разграничения на основе хранилища](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_storage_protect_fencing.html) для SLES. Имеется также агент STONITH для VMware vCenter для решений на основе ESXI. Дополнительные сведения см. в разделе [Stonith подключаемый модуль агента для VMWare виртуальных Машин VCenter SOAP ограждения (Unofficial)](https://github.com/olafrv/fence_vmware_soap).

> [!NOTE]
> На момент написания этой статьи, Hyper-V не имеет решения для STONITH. Это верно и для на локальном развертывании, также влияет на развертывания Pacemaker на основе Azure, с помощью определенных распределения, например RHEL.

### <a name="interoperability"></a>Совместимость
В данном разделе описываются способы взаимодействия с WSFC или других дистрибутивов Linux кластере под управлением Linux.

#### <a name="wsfc"></a>WSFC
В настоящее время нет прямой возможности для WSFC и Pacemaker кластер для совместной работы. Это означает, что нет возможности для создания группы Доступности или экземпляр отказоустойчивого Кластера, который работает на разных WSFC и Pacemaker. Тем не менее существует два решения взаимодействия, которые предназначены для групп доступности. Экземпляр отказоустойчивого Кластера может участвовать в конфигурации кросс платформенных можно только если он участвует как экземпляр одного из этих двух сценариев:
-   Группе Доступности с типом кластера нет. Дополнительные сведения см. в разделе Windows [документации с группами доступности](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).
-   Распределенные группы Доступности, которой — это специальный тип группы доступности, которая позволяет двух разных групп доступности, должен быть настроен как свои собственные группы доступности. Дополнительные сведения о распределенных групп доступности см. в документации [распределенные группы доступности](../database-engine/availability-groups/windows/distributed-availability-groups.md).

#### <a name="other-linux-distributions"></a>Другие версии ОС Linux
В Linux все узлы кластера Pacemaker должен быть в одном распространения. Например это означает, что узел RHEL не может быть частью кластера Pacemaker с узлом SLES. Основная причина этого было отмечено ранее,: распределения могут иметь различные версии и функции, поэтому вещей может работать неправильно. Смешивание распределения имеет же статье, что смешивание WSFCs и Linux: не используются или распределенных групп доступности.

## <a name="next-steps"></a>Следующие шаги
[Развертывание кластера Pacemaker для SQL Server в Linux](sql-server-linux-deploy-pacemaker-cluster.md)
