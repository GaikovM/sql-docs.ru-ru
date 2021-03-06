---
title: sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Документы Microsoft
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- database-engine-imoltp
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c4ad13459024604d748c1dac8a6649c09a53f10f
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbxtpcheckpointfiles-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  Отображает сведения о файлах контрольных точек, включая размер файла, физическое местоположение и идентификатор транзакции.  
  
> **Примечание:** для текущей контрольной точки, которая не была закрыта, столбце "состояние" s`ys.dm_db_xtp_checkpoint_files` будет UNDER CONSTRUCTION для новых файлов. Контрольная точка закрывается автоматически при наличии достаточных рост журнала транзакций с момента последней контрольной точки или выполнении `CHECKPOINT` команда ([контрольной ТОЧКИ &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)).  
  
 Оптимизированные для памяти файловой группе внутренним образом использует только для добавления файлов для хранения вставленные и удаленные строки для таблиц в памяти. Существует два типа файлов. Файл данных содержит вставленные строки, а разностный файл содержит ссылки на удаленные строки. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] существенно отличается от более поздних версий и рассматриваются ниже в разделе, в [SQL Server 2014](#bkmk_2014).  
  
 Дополнительные сведения см. в разделе [Создание и управление хранилищем для оптимизированных для памяти объектов](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздние версии  
 В следующей таблице описаны столбцы для `sys.dm_db_xtp_checkpoint_files`, начиная с версии **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]**.  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|container_id|**int**|Идентификатор контейнера (представленного в виде файла с типом FILESTREAM в таблице sys.database_files), частью которого является файл данных или разностный файл. Соединения с file_id в [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|Идентификатор GUID контейнера, в который входит файл корень, данных или разностных. Соединения с file_guid в таблице sys.database_files.|  
|checkpoint_file_id|**uniqueidentifier**|Идентификатор GUID файла контрольных точек.|  
|relative_file_path|**nvarchar(256)**|Путь к файлу относительно контейнера сопоставлен.|  
|file_type|**smallint**|-1 для FREE<br /><br /> 0 для файла данных.<br /><br /> 1 для РАЗНОСТНОГО файла.<br /><br /> 2 для КОРНЕВОГО файла<br /><br /> 3 для БОЛЬШИХ файлов данных|  
|file_type_desc|**nvarchar(60)**|FREE - все файлы, сохраняется как СВОБОДНЫЕ, доступной для выделения. Бесплатные файлы может иметь разный размер в зависимости от нужд системой. Максимальный размер составляет 1 ГБ.<br /><br /> ТАРИФНЫЙ план — файлы данных содержат строки, которые были вставлены в таблицах, оптимизированных для памяти.<br /><br /> ДЕЛЬТА - разностные файлы содержат ссылки на строки в файлах данных, которые были удалены.<br /><br /> КОРЕНЬ - файлам в корневой содержит системные метаданные для объектов, оптимизированных для памяти и скомпилированными в собственном коде.<br /><br /> Большие ОБЪЕМЫ данных — больших файлов данных содержат значения, вставленные в (n)varchar(max) и varbinary(max) столбцов, а также сегменты столбцов, которые являются частью индексы columnstore таблиц, оптимизированных для памяти.|  
|internal_storage_slot|**int**|Индекс файла в массиве внутреннего хранилища. Значение NULL для КОРНЕВОГО или состояние, отличное от 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Соответствующий файл данных или РАЗНОСТНЫЙ файл. Имеет значение NULL для КОРНЯ.|  
|file_size_in_bytes|**bigint**|Размер файла на диске.|  
|file_size_used_in_bytes|**bigint**|Для пар файлов контрольных точек, заполнение которых все еще выполняется, этот столбец будет обновляться после каждой следующей контрольной точки.|  
|logical_row_count|**bigint**|Для данных, количество вставленных строк.<br /><br /> Для разностных количество строк удалены после учета для удаления таблицы.<br /><br /> Для корневого значение NULL.|  
|state|**smallint**|0 — PRECREATED<br /><br /> 1 — UNDER CONSTRUCTION<br /><br /> 2 — ACTIVE<br /><br /> 3 — MERGE TARGET<br /><br /> 8 – ОЖИДАЮЩИЕ УСЕЧЕНИЕ ЖУРНАЛА|  
|state_desc|**nvarchar(60)**|PRECREATED — несколько файлов контрольных точек, предварительно выделяются, чтобы минимизировать или устранить Ожидание на выделение новых файлов при выполнении транзакций. Эти готовые файлы может иметь разный размер в соответствии с требованиями предполагаемое рабочей нагрузки, но они не содержат данных. Это пространство хранения в базах данных с файловой группой MEMORY_OPTIMIZED_DATA.<br /><br /> В разработке - эти файлы контрольных точек находятся в разработке, это означает, что они заполняются на основе записей журнала, созданных в базе данных и не входят еще контрольной точки.<br /><br /> ACTIVE — они содержат вставленные или удаленные строки из предыдущих закрытых контрольных точек. Они содержат содержимое таблиц, которые считывают область в памяти перед применением активной части журнала транзакций после перезапуска базы данных. Ожидается, что такой размер этих файлов контрольных точек может быть примерно 2 x размер в памяти таблиц, оптимизированных для памяти, при условии, что операция слияния выполняется в настоящий момент с транзакционной рабочей нагрузки.<br /><br /> MERGE TARGET — целевого объекта операции слияния — эти файлы контрольных точек сохранения консолидированные строки данных из исходных файлов, которые были определены политикой слияния. После установки слияния MERGE TARGET переходит в состояние ACTIVE.<br /><br /> ОЖИДАЮЩИЕ усечение ЖУРНАЛА — после установки слияния и пару файлов целевого СЛИЯНИЯ является частью устойчивой контрольной точки, контрольных точек файлы слияния источник переходят в это состояние. Файлы в этом состоянии необходимы для правильности работы базы данных с оптимизированными для памяти таблицы.  Например, чтобы выполнить восстановление от устойчивой контрольной точки, чтобы вернуться назад во времени.|  
|lower_bound_tsn|**bigint**|Нижняя граница транзакции в файл; значение NULL, если не в состоянии (1, 3).|  
|upper_bound_tsn|**bigint**|Верхняя граница транзакции в файл; значение NULL, если не в состоянии (1, 3).|  
|begin_checkpoint_id|**bigint**|Идентификатор начало контрольной точки.|  
|end_checkpoint_id|**bigint**|Идентификатор конечного контрольной точки.|  
|last_updated_checkpoint_id|**bigint**|Идентификатор последней контрольной точки, обновить этот файл.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 = &GT; UNENCRTPTED<br /><br /> 1 = &GT; ШИФРУЕТСЯ КЛЮЧОМ 1<br /><br /> 2 = &GT; ШИФРОВАНИЕ С КЛЮЧОМ 2. Допустимо только для активных файлов.|  
  
##  <a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 В следующей таблице описаны столбцы для `sys.dm_db_xtp_checkpoint_files`, для **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**.  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|container_id|**int**|Идентификатор контейнера (представленного в виде файла с типом FILESTREAM в таблице sys.database_files), частью которого является файл данных или разностный файл. Соединения с file_id в [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|Идентификатор GUID контейнера, частью которого является файл данных или разностный файл.|  
|checkpoint_file_id|**GUID**|Идентификатор файла данных или разностного файла.|  
|relative_file_path|**nvarchar(256)**|Путь к файлу данных или разностному файлу относительно расположения контейнера.|  
|file_type|**tinyint**|0 для файла данных.<br /><br /> 1 для разностного файла.<br /><br /> Значение NULL, если столбцу состояния задано значение 7.|  
|file_type_desc|**nvarchar(60)**|Тип файла: DATA_FILE, DELTA_FILE или значение NULL, если столбец состояния имеет значение 7.|  
|internal_storage_slot|**int**|Индекс файла в массиве внутреннего хранилища. Значение NULL, если столбцу состояния задано значение, отличное от 2 или 3.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Соответствующий файл данных или разностный файл.|  
|file_size_in_bytes|**bigint**|Используемый размер файла. Значение NULL, если столбцу состояния задано значение 5, 6 или 7.|  
|file_size_used_in_bytes|**bigint**|Используемый размер файла задействованного файла. Значение NULL, если столбцу состояния задано значение 5, 6 или 7.<br /><br /> Для пар файлов контрольных точек, заполнение которых все еще выполняется, этот столбец будет обновляться после каждой следующей контрольной точки.|  
|inserted_row_count|**bigint**|Число строк в файле данных.|  
|deleted_row_count|**bigint**|Число удаленных строк в разностном файле.|  
|drop_table_deleted_row_count|**bigint**|Количество строк в файле данных, затронутых операцией удаления таблицы. Относится к файлам данных, когда столбец состояния имеет значение 1.<br /><br /> Показывает количество строк, которые удалены из удаленных таблиц. Статистика drop_table_deleted_row_count компилируется после завершения сборки мусора строк удаленных таблиц из памяти и достижения контрольной точки. Если перезапустить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до того, как статистика удаленных таблиц появится в этом столбце, статистика будет обновлена в рамках восстановления. В процессе восстановления строки из удаленных таблиц не загружаются. Статистика по удаленным таблицам компилируется на стадии загрузки и отображается в этом столбце по завершении восстановления.|  
|state|**int**|0 — PRECREATED<br /><br /> 1 — UNDER CONSTRUCTION<br /><br /> 2 — ACTIVE<br /><br /> 3 — MERGE TARGET<br /><br /> 4 — MERGED SOURCE<br /><br /> 5 — REQUIRED FOR BACKUP/HA<br /><br /> 6 — IN TRANSITION TO TOMBSTONE<br /><br /> 7 — TOMBSTONE|  
|state_desc|**nvarchar(60)**|PRECREATED — небольшой набор пар файлов данных и разностных файлов, которые также называются парами файлов контрольных точек (CFP), выделяются заранее, чтобы свести к минимуму или вообще устранить ожидание на выделение новых файлов при выполнении транзакций. Это полноценные файлы данных размером 128 МБ и разностные файлы размером 8 МБ, но без данных. Число пар файлов соответствует числу логических процессоров или планировщиков (по одному на ядро, без максимального значения), но не менее 8. Это фиксированное пространство хранения в базах данных с таблицами, оптимизированными для памяти.<br /><br /> UNDER CONSTRUCTION — набор пар CFP, в которых записываются вновь вставленные и, возможно, удаленные строки данных с момента последней контрольной точки.<br /><br /> ACTIVE — файлы в этом состоянии содержат вставленные и удаленные строки из ближайших предыдущих закрытых контрольных точек. Эти пары файлов содержат все обаятельные вставленные и удаленные строки, которые необходимы, прежде чем будет применена активная часть журнала транзакций после перезапуска базы данных. Размер этих CFP будет приблизительно в два раза больше размера оптимизированных для памяти таблиц в памяти при условии, что операция объединения выполняется в настоящий момент с транзакционной рабочей нагрузкой.<br /><br /> MERGE TARGET — этот CFP хранит консолидированные строки данных из CFP, которые были определены политикой слияния. После установки слияния MERGE TARGET переходит в состояние ACTIVE.<br /><br /> MERGED SOURCE — после установки операции слияния исходные CFP помечаются как MERGED SOURCE. Обратите внимание, что средство оценки политики слияния может определять несколько слияний, а CFP может участвовать только в одной операции слияния.<br /><br /> REQUIRED FOR BACKUP/HA — после установки слияния, когда CFP в состоянии MERGE TARGET является частью устойчивой контрольной точки, исходные CFP слияния переходят в это состояние. Находящиеся в этом состоянии CFP необходимы для правильности работы базы данных с оптимизированной для памяти таблицы.  Например, чтобы выполнить восстановление от устойчивой контрольной точки, чтобы вернуться назад во времени. Пару файлов можно отметить для сборки мусора после того, как точка усечения журнала выходит из диапазона транзакций.<br /><br /> IN TRANSITION TO TOMBSTONE — эти CFP не нужны ядру In-Memory OLTP, поэтому они могут подпадать под сбор мусора. Это состояние указывает, что эти CFP ожидают, чтобы фоновый поток перевел их в следующее состояние, которым является состояние TOMBSTONE.<br /><br /> TOMBSTONE — эти CFP ожидают, когда их заберет сборщик мусора FILESTREAM. ([sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|Нижняя граница транзакций, содержащихся в файле. Значение NULL, если столбец состояния имеет значение, отличное от 2, 3 или 4.|  
|upper_bound_tsn|**bigint**|Верхняя граница транзакций, содержащихся в файле. Значение NULL, если столбец состояния имеет значение, отличное от 2, 3 или 4.|  
|last_backup_page_count|**int**|Количество логических страниц, которое определяется при последнем резервном копировании. Применяется, когда столбцу состояния задается значение 2, 3, 4 или 5. Значение NULL, если количество страниц неизвестно.|  
|delta_watermark_tsn|**int**|Транзакция последней контрольной точки, которая выполнила запись в этот разностный файл. Это знак разностного файла.|  
|last_checkpoint_recovery_lsn|**nvarchar(23)**|Регистрационный номер транзакции в журнале восстановления последней контрольной точки, которой все еще требуется файл.|  
|tombstone_operation_lsn|**nvarchar(23)**|Файл будет удален после того, как tombstone_operation_lsn оказывается позади регистрационного номера транзакции в журнале усечения журнала.|  
|logical_deletion_log_block_id|**bigint**|Относится только к состоянию 5.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение `VIEW DATABASE STATE` на сервере.  
  
## <a name="use-cases"></a>Способы применения  
 Вы можете оценить хранилища, используемого в памяти OLTP:  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Чтобы увидеть разбивку использования хранилища, состояние и тип файла, выполните следующий запрос:
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>См. также  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
