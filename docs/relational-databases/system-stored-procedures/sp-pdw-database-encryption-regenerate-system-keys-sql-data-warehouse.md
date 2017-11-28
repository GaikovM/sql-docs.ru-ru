---
title: "sp_pdw_database_encryption_regenerate_system_keys (хранилище данных SQL) | Документы Microsoft"
ms.custom: 
ms.date: 03/03/2017
ms.prod: 
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: 
ms.service: sql-data-warehouse
ms.component: system-stored-procedures
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
dev_langs: TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
caps.latest.revision: "8"
author: barbkess
ms.author: barbkess
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 94f45c520397c608845e9bd07ec4aed173d1fd91
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
---
# <a name="sppdwdatabaseencryptionregeneratesystemkeys-sql-data-warehouse"></a>sp_pdw_database_encryption_regenerate_system_keys (хранилище данных SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Используйте **sp_pdw_database_encryption_regenerate_system_keys** для смены ключа шифрования сертификата и базы данных для внутренней базы данных, которые шифруются при включении прозрачного шифрования данных на устройстве. в том числе и `tempdb`. Это будет успешным, только в том случае, если включено прозрачное шифрование данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```tsql  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 Процедура не имеет параметров.  
  
 Эту процедуру можно использовать при низкой трафика в устройстве.  
  
## <a name="permissions"></a>Permissions  
 Требуется членство в **sysadmin** предопределенной роли базы данных или **CONTROL SERVER** разрешение.  
  
## <a name="example"></a>Пример  
 Следующий пример обновляет ключи шифрования базы данных.  
  
```tsql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_pdw_database_encryption &#40; Хранилище данных SQL &#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40; Хранилище данных SQL &#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
  