---
title: sp_clean_db_file_free_space (Transact-SQL) | Документы Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_clean_db_file_free_space
- sp_clean_db_file_free_space_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ghost records
- sp_clean_db_file_free_space
ms.assetid: 3eb53a67-969d-4cb8-9681-b1c8e6fd55b6
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 52f5fb5b32a49ef6bcb4922069dc7f5250c771c9
ms.sourcegitcommit: e37f017cbebb22ad9d12e4daf863190933a4d8a1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34689132"
---
# <a name="spcleandbfilefreespace-transact-sql"></a>sp_clean_db_file_free_space (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Удаляет остаточные данные, оставляемые на страницах базы данных процедурами изменения данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Хранимая процедура sp_clean_db_file_free_space очищает все страницы в одном файле базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_clean_db_file_free_space   
[ @dbname ] = 'database_name'   
, @fileid = 'file_number'   
 [ , [ @cleaning_delay = ] 'delay_in_seconds' ] [;]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @dbname=] '*имя_базы_данных*"  
 Имя очищаемой базы данных. *DBName* — **sysname** и не может иметь значение NULL.  
  
 [ @fileid=] '*file_number*"  
 Идентификатор очищаемого файла данных. *file_number* — **int** и не может иметь значение NULL.  
  
 [ @cleaning_delay=] '*delay_in_seconds*"  
 Интервал задержки между операциями очистки страниц. Применение задержки помогает уменьшить нагрузку на систему ввода-вывода. *delay_in_seconds* — **int** значение по умолчанию 0.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Примечания  
 Удаляет операции из таблицы или обновляет операции, которые приводят к перемещению строки, что может немедленно освободить пространство на странице путем удаления ссылок на строку. Но при определенных обстоятельствах строка может физически оставаться на странице данных как фантомная запись. Фантомные записи периодически удаляются фоновым процессом. Эти остаточные данные не возвращаются [!INCLUDE[ssDE](../../includes/ssde-md.md)] в ответ на запросы. Но если физическая безопасность данных или файлов резервной копии в рабочей среде подвергается угрозам, то можно использовать процедуру sp_clean_db_file_free_space для очистки этих фантомных записей.  
  
 Продолжительность времени, необходимого для выполнения процедуры sp_clean_db_file_free_space, зависит от размера файла, доступного свободного пространства и емкости диска. Тем не менее выполнение процедуры sp_clean_db_file_free_space может оказать существенное отрицательное влияние на работу подсистемы ввода-вывода, поэтому рекомендуется выполнять эту процедуру не в обычное рабочее время.  
  
 Перед выполнением хранимой процедуры sp_clean_db_file_free_space рекомендуется создать полную резервную копию базы данных.  
  
 Связанный [sp_clean_db_free_space](../../relational-databases/system-stored-procedures/sp-clean-db-free-space-transact-sql.md) хранимая процедура удаляет все файлы в базе данных.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных db_owner.  
  
## <a name="examples"></a>Примеры  
 В следующем примере выполняется очистка всех остаточных данных в первичном файле данных базы данных `AdventureWorks2012`.  
  
```  
USE master;  
GO  
EXEC sp_clean_db_file_free_space   
@dbname = N'AdventureWorks2012', @fileid = 1 ;  
```  
  
## <a name="see-also"></a>См. также  
 [Компонент Database Engine хранимой процедуры &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)
 <br>[Руководство по процессу очистки фантомных записей](../ghost-record-cleanup-process-guide.md) 
  
  
