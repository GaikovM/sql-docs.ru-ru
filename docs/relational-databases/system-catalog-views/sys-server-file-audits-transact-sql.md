---
title: sys.server_file_audits (Transact-SQL) | Документы Microsoft
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- server_file_audits_TSQL
- sys.server_file_audits_TSQL
- sys.server_file_audits
- server_file_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_file_audits catalog view
ms.assetid: 553288a0-be57-4d79-ae53-b7cbd065e127
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f4e10ec5dc755f1a8487aecd40b620eb0a3eefe8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
---
# <a name="sysserverfileaudits-transact-sql"></a>sys.server_file_audits (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Содержит расширенные сведения о типах аудита файлов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] аудита на экземпляре сервера. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (компонент Database Engine)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|audit_id|**int**|Идентификатор аудита.|  
|имя|**sysname**|Имя аудита.|  
|audit_guid|**uniqueidentifier**|Идентификатор GUID аудита.|  
|create_date|**datetime**|Дата создания аудита файлов в формате UTC.|  
|modify_date|**значения даты и времени**|Дата последнего изменения аудита файлов в формате UTC.|  
|principal_id|**int**|Идентификатор владельца аудита, зарегистрированного на сервере.|  
|Тип|**char(2)**|Тип аудита:<br /><br /> 0 — журнал событий безопасности NT;<br /><br /> 1 — журнал событий приложений NT;<br /><br /> 2 — файл файловой системы.|  
|type_desc|**nvarchar(60)**|Описание типа аудита.|  
|on_failure|**tinyint**|Действие при ошибке:<br /><br /> 0 — продолжить;<br /><br /> 1 — завершить работу экземпляра сервера;<br /><br /> 2 — ошибка операции.|  
|on_failure_desc|**nvarchar(60)**|В случае ошибки записи данных о действии:<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> ОШИБКА ОПЕРАЦИИ|  
|is_state_enabled|**tinyint**|0 = Отключено<br /><br /> 1 = Включено|  
|queue_delay|**int**|Предполагаемое максимальное время, в миллисекундах, ожидания перед записью на диск. Если значением является 0, аудит гарантирует выполнение записи, прежде чем событие может быть продолжено.|  
|predicate|**nvarchar(8000)**|Выражение предиката, применяемое к событию.|  
|max_file_size|**bigint**|Максимальный размер аудита, в мегабайтах:<br /><br /> 0 = размер неограничен или неприменим к выбранному типу аудита.|  
|max_rollover_files|**int**|Максимальное количество файлов для использования с параметром продолжения.|  
|max_files|**int**|Максимальное количество файлов для использования без параметра продолжения.|  
|reserved_disk_space|**int**|Объем места на диске, резервируемого для файла.|  
|log_file_path|**nvarchar(260)**|Путь к местонахождению аудита. Путь к файлу для аудита файлов, путь к журналу приложений для аудита журнала приложений.|  
|log_file_name|**nvarchar(260)**|Базовое имя для файла журнала, предоставленного в CREATE AUDIT DDL. Добавочное число прибавлено к файлу base_log_name в качестве суффикса, чтобы создать имя файла журнала.|  
  
## <a name="permissions"></a>Разрешения  
 Участники **ALTER ANY SERVER AUDIT** или **VIEW ANY DEFINITION** разрешение имеют доступ к этому представлению каталога. Кроме того, участник не должно быть запрещено **VIEW ANY DEFINITION** разрешение.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Создание аудита сервера и спецификации аудита сервера](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
