---
title: Управление рабочими нагрузками в Analytics Platform System | Документы Microsoft
description: Управление рабочими нагрузками в Analytics Platform System.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: a4f748ed39705f865a303f1b59ae352068f93431
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707102"
---
# <a name="workload-management-in-analytics-platform-system"></a>Управление рабочими нагрузками в система платформы аналитики

Возможности управления SQL Server PDW рабочей нагрузки позволяет пользователям и администраторам назначать запросы, чтобы предварительно задать конфигурация памяти и параллелизма. Используйте Управление рабочей нагрузки для повышения производительности рабочей нагрузки, согласованных или смешанных, позволяя запросы на получение необходимых ресурсов без лишить любые запросы, бесконечно.  
  
Например с помощью методов управления рабочей нагрузки в SQL Server PDW, можно выполнить следующее.  
  
-   Выделите большое количество ресурсов для задания нагрузки.  
  
-   Укажите дополнительные ресурсы для построения индекса columnstore.  
  
-   Устранение неполадок замедлить выполнение хэш-соединение для просмотра, если он требуется больше памяти и присвойте ему больше памяти.  
  
## <a name="Basics"></a>Основы управления рабочей нагрузки  
  
### <a name="key-terms"></a>Основные понятия  
Управление рабочими нагрузками  
*Управление рабочими нагрузками* — возможность понять и настроить использование системных ресурсов для достижения наилучшей производительности для параллельных запросов.  
  
Класс ресурсов  
В SQL Server PDW *класс ресурсов* является встроенной ролью сервера, имеет назначенный ограничения для памяти и параллелизма. SQL Server PDW выделяет ресурсы запросы в соответствии с ресурсов класса сервера членство в роли имени входа, которое отправляет запросы.  
  
На вычислительных узлах в реализации классов ресурса используется функция регулятора ресурсов в SQL Server. Дополнительные сведения о регуляторе ресурсов см. в разделе [регулятора ресурсов](http://msdn.microsoft.com/library/bb933866(v=sql.11).aspx) на сайте MSDN.  
  
### <a name="understand-current-resource-utilization"></a>Понять текущий уровень использования ресурсов  
Чтобы понять использование системных ресурсов для текущих запросов, используйте динамические административные представления SQL Server PDW. Например можно использовать динамические административные представления для понимания ли больше памяти, задав преимущество медленного выполнения большого хэш-соединение.  
  
<!-- MISSING LINKS
For examples, see [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md).  
-->
  
### <a name="adjust-resource-allocations"></a>Настройка выделения ресурсов  
Чтобы настроить использование ресурсов, измените членство класс ресурсов входа, которое Отправка запроса. Роли сервера ресурсов класса имеют имена **mediumrc**, **largerc**, и **xlargerc**. Они представляют средней, больших и чрезвычайно больших выделений соответственно.  
  
Например, чтобы выделить большой объем системных ресурсов на запрос, добавить имя входа, которое Отправка запроса на **largerc** роли сервера. Следующая инструкция ALTER SERVER ROLE добавляется имя входа Анна largerc серверной роли.  
  
```sql  
ALTER SERVER ROLE largerc ADD MEMBER Anna;  
```  
  
## <a name="RC"></a>Описание класса ресурсов  
Ниже перечислены классы ресурсов и их распределение ресурсов системы.  
  
|Класс ресурсов|Важность запроса|Максимальный объем памяти использования *|Слоты параллелизма (максимальное = 32)|Описание|  
|------------------|----------------------|--------------------------|---------------------------------------|---------------|  
|значение по умолчанию|Средний|400 МБ|1|По умолчанию каждой учетной записи входа разрешен небольшой объем памяти и ресурсов параллелизма для запросов.<br /><br />Когда имя входа добавляется в класс ресурсов, новый класс имеет больший приоритет. При удалении имени входа из всех классов ресурса, имя входа восстанавливает выделения ресурсов по умолчанию.|  
|MediumRC|Средний|1200 МБ|3|Примеры запросов, которые могут понадобиться класса ресурсов со средним.<br /><br />CTAS операций, которые имеют большой хэш-соединения.<br /><br />ВЫБЕРИТЕ операции, которые требуется больше памяти, чтобы избежать кэширования на диск.<br /><br />Загрузка данных в кластеризованных индексах columnstore.<br /><br />Создание, перестроение и реорганизации кластеризованные индексы columnstore для небольших таблиц со столбцами, 10-15.|  
|largerc|Высокий|2.8 ГБ|7|Примеры запросов, которые могут потребоваться больших ресурсов класса.<br /><br />Очень большой операции CTAS, повышая хэш-соединения, либо содержит больших статистических выражений, таких как большие предложения ORDER BY или GROUP BY.<br /><br />Выбор операций, требующих очень большого объема памяти для операций, таких как хэш-соединения или агрегатов, таких как предложения ORDER BY или GROUP BY<br /><br />Загрузка данных в кластеризованных индексах columnstore.<br /><br />Создание, перестроение и реорганизации кластеризованные индексы columnstore для небольших таблиц со столбцами, 10-15.|  
|xlargerc|Высокий|8.4 ГБ|22|Класс ресурсов долгосрочного предназначен для запросов, которые могут потребовать потребления чрезвычайно больших ресурсов во время выполнения.|  
  
<sup>*</sup>Максимальный объем используемой памяти является приближением.  
  
### <a name="request-importance"></a>Важность запроса  
Важность запрос сопоставляет количество времени ЦП, что SQL Server, на вычислительных узлах даст на запросы.  Запросы с более высоким приоритетом получают больше Процессорного времени.  
  
### <a name="maximum-memory-usage"></a>Максимальный объем используемой памяти  
Максимальный объем используемой памяти является максимальный объем доступной памяти, запрос можно использовать в каждом пространстве обработки. Запрос mediumrc можно использовать, например до 1200 МБ для обработки в пределах каждого распределения. По-прежнему важно убедиться, что данные не синхронизована, чтобы избежать необходимости некоторых дистрибутивов выполняет большую часть работы.  
  
### <a name="concurrency-slots"></a>Слоты параллелизма  
Выделение 1, 3, 7 и 22 слоты параллелизма предназначена для того, чтобы разрешить выполнение одновременно, без блокировки small процесса при выполнении больших процесс больших и малых процессов.  Например SQL Server PDW выделяется не более 32 слоты параллелизма для выполнения 1 долгосрочного запрос (22 слоты), 1 запроса большого объема (7 разъемов) и 1 запрос на средние (3 разъема), в то же время.  
  
Примеры выделения до 32 слоты параллелизма для параллельных запросов.  
  
-   28 слоты = 4 большой  
  
-   30 слоты = 10 средний  
  
-   Слоты 32 = 32 по умолчанию  
  
-   Слоты 32 = долгосрочного 1 + 1-средний больших + 1  
  
-   Слоты 32 = 2-средний больших + 4 + 6 по умолчанию  
  
Предположим, что 6 большие запросы отправляются на SQL Server PDW, а затем отправляет запросы 10 по умолчанию. SQL Server PDW обрабатывал запросы в порядке приоритета:  
  
-   Выделить 28 слоты параллелизма для запуска обработки 4 больших запросов, как память становится доступной, а также защиту 2 больших запросов в очереди.  
  
-   Выделите 4 слоты параллелизма для запуска обработки 4 запроса по умолчанию и сохранить 6 по умолчанию запросы в очереди ожидания.  
  
Завершения запросов и параллелизма слоты станут доступны, SQL Server PDW выделит остальные запросы в соответствии с доступными ресурсами и приоритет. Например есть 7 параллелизма открытых слотов, ожидает больших запросов после более высокий приоритет, чем ожидающих запросов на средний 7 разъемов.  Если открыть 6 слотов, SQL Server PDW выделит 6 Дополнительные запросы с размерами по умолчанию. Тем не менее памяти и параллелизма слоты должны быть доступны все прежде, чем запрос на запуск позволяет SQL Server PDW.  
  
В каждом классе ресурса запросы выполняются в первым в порядке ФИФО (FIFO).  
  
## <a name="GeneralRemarks"></a>Общие замечания  
Если имя входа является членом более чем одним классом ресурсов, в классе с больше всего ресурсов имеет приоритет.  
  
При добавлении имени входа или удалены из класса ресурсов, изменение вступит в силу немедленно для всех будущих запросов. текущих запросов, выполняющихся или ожидания не затрагиваются. Имя входа не требуется отключиться и подключиться в порядке для изменения произойдет.  
  
Для каждого имени входа для отдельных инструкций и операции, а не к сеансу, применяются параметры класса ресурсов.  
  
Перед выполнением инструкции SQL Server PDW пытается получить слоты параллелизма, необходимые для запроса. Если он не может получить достаточно слоты параллелизма, SQL Server PDW переводит запрос в состояние ожидания выполнения. Все системные ресурсы, выделенные для запроса уже возвращаются обратно в систему.  
  
Большинство инструкций SQL, которые всегда требуют распределения ресурсов по умолчанию и таким образом обеспечиваются классов ресурсов. Например, CREATE LOGIN только должен небольшой объем ресурсов и выделяется ресурсов по умолчанию, даже если имя входа, вызов создать имя входа является членом класса ресурсов.  Например если Кирилл является членом класса largerc ресурсов, и она отправляет инструкцию CREATE LOGIN, инструкции CREATE LOGIN будет выполняться с по умолчанию количество ресурсов.  
  
Инструкции SQL и операции, принимается в соответствии с классами ресурсов:  
  
-   ALTER INDEX REBUILD  
  
-   ALTER INDEX REORGANIZE  
  
-   ПЕРЕСТРОЕНИЕ ТАБЛИЦЫ ALTER  
  
-   СОЗДАНИЕ КЛАСТЕРИЗОВАННОГО ИНДЕКСА  
  
-   СОЗДАНИЕ КЛАСТЕРИЗОВАННОГО ИНДЕКСА COLUMNSTORE  
  
-   СОЗДАНИЕ TABLE AS SELECT  
  
-   СОЗДАНИЕ УДАЛЕННОГО TABLE AS SELECT  
  
-   Загрузка данных с помощью **dwloader**.  
  
-   ИНСТРУКЦИЮ INSERT SELECT  
  
-   UPDATE  
  
-   DELETE  
  
-   ВОССТАНОВЛЕНИЕ базы данных при восстановлении в устройство с дополнительные вычислительные узлы.  
  
-   ВЫБЕРИТЕ, за исключением запросов только для динамических административных Представлений  
  
## <a name="Limits"></a>Ограничения  
Классы ресурсов управляют выделения памяти и параллелизма.  Они не определяют операций ввода вывода.  
  
## <a name="Metadata"></a>Метаданные  
Динамические административные представления, содержащие сведения о классах ресурсов и члены класса ресурсов.  
  
-   [sys.server_role_members](../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
-   [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
Динамические административные представления, содержащие сведения о состоянии запросы и ресурсы, которые им требуются:  
  
-   [sys.dm_pdw_lock_waits](../relational-databases/system-dynamic-management-views/sys-dm-pdw-lock-waits-transact-sql.md)  
  
-   [sys.dm_pdw_resource_waits](../relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql.md)  
  
Связанные системные представления в динамических административных представлений SQL Server на вычислительных узлах. В разделе [динамических административных представлений SQL Server](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) ссылки на эти динамические административные представления в библиотеке MSDN.  
  
-   sys.dm_pdw_nodes_resource_governor_resource_pools  
  
-   sys.dm_pdw_nodes_resource_governor_workload_groups  
  
-   sys.dm_pdw_nodes_resource_governor_resource_pools  
  
-   sys.dm_pdw_nodws_resource_governor_workload_groups  
  
-   sys.dm_pdw_nodes_exec_sessions  
  
-   sys.dm_pdw_nodes_exec_requests  
  
-   sys.dm_pdw_nodes_exec_query_memory_grants  
  
-   sys.dm_pdw_nodes_exec_query_resource_semaphores  
  
-   sys.dm_pdw_nodes_os_memory_brokers  
  
-   sys.dm_pdw_nodes_os_memory_cache_entries  
  
-   sys.dm_pdw_nodes_exec_cached_plans  
  
## <a name="RelatedTasks"></a>Связанные задачи  
[Задачи управления рабочей нагрузки](workload-management-tasks.md)  
  
<!-- MISSING LINKS
See the Workload Management section of [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md) for the following tasks:  
  
1.  Find the number of active requests for each resource group.  
  
2.  Determine if my requests need more memory  
  
3.  Determine if there are a high number of query optimizations or suboptimal plan generations.  
  
4.  Determine Average CPU time per request in each resource pool to date  
  
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  
