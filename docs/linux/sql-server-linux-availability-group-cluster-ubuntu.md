---
title: Настройка Ubuntu кластера для группы доступности SQL Server | Документы Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 04/30/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: d9e41a09fdd76f060fcf34d33d7463984ae942a6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2018
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>Настройка кластера Ubuntu и ресурс группы доступности

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

В этом документе описывается создание трех узлов кластера на Ubuntu и добавьте ранее созданную группу доступности в качестве ресурса кластера. Для обеспечения высокой доступности группы доступности для Linux требует трех узлов — см. [высокий уровень доступности и защиты данных в конфигурации группы доступности](sql-server-linux-availability-group-ha.md).

> [!NOTE] 
> На этом этапе интеграции SQL Server с Pacemaker в Linux не, а также как WSFC в Windows. Из в SQL, неизвестно о наличии кластера, все orchestration находится за пределами в и служба управляется как отдельный экземпляр Pacemaker. Кроме того имя виртуальной сети доступен только в WSFC, не имеет эквивалента в одной и той же в Pacemaker. Всегда на динамические административные представления, запросить сведения о кластере возвращают пустые строки. Вы можете создать прослушиватель, чтобы использовать его для прозрачного переподключения после отработки отказа, но необходимо вручную зарегистрировать имя прослушивателя на DNS-сервере с помощью IP-адрес, используемый для создания виртуального IP-ресурс (как описано в следующих разделах).

Следующие разделы выполните эти шаги для настройки решения отказоустойчивого кластера. 

## <a name="roadmap"></a>Стратегия

Инструкции по созданию группы доступности на серверах Linux для обеспечения высокой доступности, отличаются от действия, указанные на отказоустойчивом кластере Windows Server. Ниже приводятся общие действия. 

1. [Настройка SQL Server на узлах кластера](sql-server-linux-setup.md).

2. [Создание группы доступности](sql-server-linux-availability-group-configure-ha.md). 

3. Настройте диспетчер ресурсов кластера, например Pacemaker. Эти инструкции приведены в этом документе.
   
   Способ настройки диспетчер ресурсов кластера зависит от конкретных дистрибутивах Linux. 

   >[!IMPORTANT]
   >Рабочих средах требуется агент разграничения, например STONITH для обеспечения высокой доступности. Демонстрации в этой документации не используйте разграничения агентов. Для тестирования и проверки только являются демонстраций. 
   
   >Кластер Linux использует разграничения для возвращения известного состояния кластера. Способ настройки разграничения зависит от того, распределение и среды. В настоящее время разграничения недоступна в некоторых средах облака. В разделе [политики поддержки для высокой доступности кластеров RHEL - платформ виртуализации](https://access.redhat.com/articles/29440) для получения дополнительной информации.

5.  [Добавление группы доступности в качестве ресурса кластера](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource). 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Установка и настройка Pacemaker на каждом узле кластера

1. На всех узлах откройте порты брандмауэра. Откройте порт для службы высокого уровня доступности Pacemaker, экземпляр SQL Server и конечной точке группы доступности. TCP-порт по умолчанию для сервера SQL Server — 1433.  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   Кроме того можно просто отключить брандмауэр:
        
   ```bash
   sudo ufw disable
   ```

1. Установите пакеты Pacemaker. На всех узлах выполните следующие команды:

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. Задайте пароль для пользователя по умолчанию, который создается при установке пакетов Pacemaker и Corosync. Используйте на всех узлах один и тот же пароль. 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Включить и запустить службу pcsd и Pacemaker

Следующая команда включает и запускает службу pcsd и pacemaker. Запустите на всех узлах. Это позволяет узлам приходящийся после перезагрузки. 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>Команды включения pacemaker может завершиться с ошибкой «pacemaker запуска по умолчанию содержит не runlevels прерывание.» Это не опасное, можно продолжить настройку кластера. 

## <a name="create-the-cluster"></a>Создание кластера

1. Удалите все существующие конфигурации кластера со всех узлов. 

   Выполнение «ПК apt get установки sudo» устанавливает pacemaker, corosync и ПК в то же время и начинает работать все три службы.  Запуск corosync создает шаблон "/ etc/cluster/corosync.conf" файла.  Возможность успешного выполнения этот файл дальнейшие действия не должно существовать —, необходимо остановить pacemaker / corosync и удалите "/ etc/cluster/corosync.conf", а затем выполните следующие шаги. «уничтожить кластер ПК» делает то же самое, и его можно использовать как временной этап установки исходного кластера.
   
   Следующая команда удаляет все существующие файлы конфигурации кластера и останавливает все службы кластеров. Это окончательно удаляет кластера. Запустите его в качестве первого шага в предварительной рабочей среде. Обратите внимание что «компьютеры кластера уничтожить» отключены pacemaker служба и необходимо повторно включать. Выполните на всех узлах следующую команду.
   
   >[!WARNING]
   >Команда удаляет все существующие ресурсы кластера.

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. Создание кластера. 

   >[!WARNING]
   >Из-за известная проблема, которую кластеризации поставщика над этой проблемой, начиная со следующей ошибкой перехода кластера («компьютеры кластера start»). Это так, как настроить файл журнала в /etc/corosync/corosync.conf которого создается при запуске команды установки кластера, не так. Чтобы обойти эту проблему, измените файл журнала: /var/log/corosync/corosync.log. В качестве альтернативы можно создать файл /var/log/cluster/corosync.log.
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
Следующая команда создает трех узлов кластера. Перед выполнением данного сценария замените значения между `< ... >`. Выполните следующую команду на основном узле. 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2…> <node3>
   sudo pcs cluster start --all
   ```
   
   >[!NOTE]
   >Если кластер был настроен ранее на тех же узлах, используйте параметр --force при выполнении команды pcs cluster setup. Обратите внимание, что это эквивалентно команде pcs cluster destroy. Службу Pacemaker необходимо включить повторно с помощью команды sudo systemctl enable pacemaker.


## <a name="configure-fencing-stonith"></a>Настройка разграничения (STONITH)

Pacemaker кластера поставщиков требуют STONITH должно быть включено и разграничения устройству, настроенному для поддерживаемых кластера. Если диспетчер ресурсов кластера не удается определить состояние узла или ресурса на узле, разграничения позволяет снова подключить известного состояния кластера. Ресурс уровня разграничения главным образом, гарантируется без повреждения данных в случае сбоя путем настройки ресурса. Можно использовать разграничения уровня ресурсов, например, с DRBD (распределенных реплицируются блоков) для пометки диска на узле как устаревший, когда канал связи выходит из строя. Узел уровня разграничения гарантирует узла не выполняет какие-либо ресурсы. Это делается путем сброса узел и его реализация Pacemaker вызывается STONITH (что означает «прокрутить другой узел в начало»). Pacemaker поддерживает выполнения разнообразных разграничения устройства, например бесперебойного питания или управления интерфейсные карты для серверов. Дополнительные сведения см. в разделе [Pacemaker кластеры с нуля](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-plugin/html/Clusters_from_Scratch/ch05.html) и [разграничения и Stonith](http://clusterlabs.org/doc/crm_fencing.html) 

Так как уровень узла ограждения конфигурации сильно зависит от среды, мы отключаем для этого учебника (он может быть настроен на более позднее время). Выполните следующий скрипт на основном узле: 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>Отключение STONITH — только для целей тестирования. Если вы планируете использовать Pacemaker в рабочей среде, следует планировать реализацию STONITH в зависимости от среды и хранить включена. Обратите внимание, что на этом этапе отсутствуют разграничения агенты для облачных сред (включая Azure) или Hyper-V. Следовательно поставщика кластера не обеспечивает поддержку для запуска рабочих кластерах в этих средах. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Набор кластеров свойства кластера интервал повторной проверки-

`cluster-recheck-interval` Указывает интервал опроса, по которому проверяется кластера изменения в параметры ресурсов, ограничений или другие параметры кластера. Если реплика отключается, кластера пытается перезапустить реплики с интервалом, ограничивается `failure-timeout` значение и `cluster-recheck-interval` значение. Например если `failure-timeout` составляет 60 секунд и `cluster-recheck-interval` задается 120 секунд, предпринимается попытка перезагрузки с интервалом, больше 60 секунд, но менее 120 секунд. Рекомендуется задать время ожидания сбоя 60s и кластера-повторной проверки interval, значение которого больше, чем 60 секунд. Задавать интервал для повторной проверки кластера на малое значение не рекомендуется.

Чтобы обновить значение свойства `2 minutes` запуска:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> При наличии группы доступности, под управлением кластера Pacemaker Обратите внимание, что все распределения, использующих последние доступные Pacemaker пакета 1.1.18-11.el7 внести изменение поведения для начала сбой является Неустранимая кластера, параметр, если его имеет значение false. Это изменение затрагивает рабочий процесс отработки отказа. Если первичная реплика сбоя, ожидается кластера отработки отказа для одной из доступных вторичных реплик. Вместо этого пользователи заметят кластер отслеживает попытке запуска сбоя первичной реплики. Если этой основной никогда не переходит в оперативный режим (из-за постоянного сбоя), кластера никогда не переключается на другой доступной вторичной реплике. Из-за этого изменения, ранее рекомендуемые конфигурации для задания начала сбой является Неустранимая больше не является допустимым, и параметр должен вернуть значение по умолчанию `true`. Кроме того, должен быть обновлен для включения ресурсов AG `failover-timeout` свойство. 
>
>Чтобы обновить значение свойства `true` запуска:
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>Обновить существующие свойство ресурса группы Доступности `failure-timeout` для `60s` запуска (Замените `ag1` на имя вашей группы доступности):
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>Установка агента ресурсов SQL Server для интеграции с Pacemaker

Выполните следующие команды на всех узлах. 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>Создайте имя входа SQL Server для Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Создание группы доступности

Чтобы создать ресурс группы доступности, используйте `pcs resource create` команды и задать свойства ресурса. Ниже команда создает `ocf:mssql:ag` ведущий и ведомый типа ресурса для группы доступности с именем `ag1`. 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s --master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>Создайте виртуальные IP-адреса

Чтобы создать виртуальный ресурс IP-адреса, выполните следующую команду на одном узле. Используйте статический IP-адрес доступен по сети. Перед выполнением скрипта замените значения между `< ... >` допустимый IP-адрес.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Отсутствует эквивалент в Pacemaker имя виртуального сервера. Чтобы использовать строку подключения, указывающая на строку имя сервера и не использовать IP-адрес, зарегистрируйте IP-адрес ресурса и имя нужного виртуального сервера в DNS. Для конфигураций аварийного восстановления Зарегистрируйте имя нужного виртуального сервера и IP-адрес в DNS-серверов на основном сервере и сайт аварийного восстановления.

## <a name="add-colocation-constraint"></a>Добавить ограничение совместного размещения

Практически каждое решение в кластере Pacemaker, такие как выбор, где должны запускаться ресурс выполняется путем сравнения оценок. Вычисления оценки для каждого ресурса, и диспетчер ресурсов кластера выбирает узел с высший показатель для конкретного ресурса. (Если узел имеет отрицательный показатель для ресурса, ресурс не может выполняться на этом узле.) Ограничения можно используйте для настройки решения кластера. Ограничения имеют оценку. Если ограничение имеет показатель меньше БЕСКОНЕЧНОСТИ, это только рекомендация. Оценка бесконечность означает, что он является обязательным. И убедитесь, что первичная реплика и виртуальный IP-адрес ресурса, на одном узле, определите ограничение совместного размещения с оценкой бесконечность. Чтобы добавить ограничение совместного размещения, выполните следующую команду на одном узле. 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Добавить ограничение порядка сортировки

Ограничение совместного размещения имеет неявное ограничение порядка сортировки. Он перемещает виртуальные IP-адреса, прежде чем перейти в ресурс группы доступности. По умолчанию является последовательность событий:

1. Пользователь выполняет `pcs resource move` группой доступности первичной из узел1 и узел2.
1. Виртуальные IP-адреса останавливается на node1.
1. Узел2 запускает виртуальные IP-адреса.

   >[!NOTE]
   >На этом этапе IP-адрес временно указывает узел2 пока узел2 по-прежнему pre-отработки отказа Вторичная. 
   
1. Источник на node1, группы доступности будет понижен до получателя.
1. Группы доступности, вторичная на node2, повышается до основного. 

Чтобы предотвратить временно указывающие на узел с данными дополнительных pre-отработки отказа IP-адрес, добавьте ограничением порядка сортировки. 

Чтобы добавить упорядочивания ограничение, выполните следующую команду на одном узле:

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>После настройки кластера и добавить группу доступности в качестве ресурса кластера, Transact-SQL нельзя использовать для отработки отказа ресурсов группы доступности. Ресурсы кластера SQL Server в Linux не связаны тесно как с операционной системой, как и на кластере отработки отказа Windows Server (WSFC). Службы SQL Server не имеет сведений о присутствии кластера. Все взаимодействие осуществляется с помощью средства управления кластером. В RHEL или Ubuntu использовать `pcs`. 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Следующие шаги

[Работать с высокой ДОСТУПНОСТИ группы доступности](sql-server-linux-availability-group-failover-ha.md)

