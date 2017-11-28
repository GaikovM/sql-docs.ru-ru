---
title: "sys.fn_cdc_has_column_changed (Transact-SQL) | Документы Microsoft"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-functions
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
applies_to: SQL Server (starting with 2008)
f1_keywords:
- sys.fn_cdc_has_column_changed_TSQL
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed_TSQL
- fn_cdc_has_column_changed
dev_langs: TSQL
helpviewer_keywords:
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed
ms.assetid: 2b9e6278-050d-4ffc-8d1a-09606180facc
caps.latest.revision: "16"
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 605d44ed6af4de9abb5e7950954c6e2ade6c60d4
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
---
# <a name="sysfncdchascolumnchanged-transact-sql"></a>sys.fn_cdc_has_column_changed (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Определяет, указывает ли заданная маска обновления на обновление заданного столбца в связанной строке изменений.  
  
||  
|-|  
|**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [текущей версии](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_cdc_has_column_changed ( 'capture_instance','column_name' , update_mask )  
```  
  
## <a name="arguments"></a>Аргументы  
 **"** *capture_instance* **"**  
 Имя экземпляра отслеживания. *capture_instance* — **sysname**.  
  
 **"** *column_name* **"**  
 Отслеживаемый столбец заданного экземпляра отслеживания. *column_name* — **sysname**.  
  
 *update_mask*  
 Маска, идентифицирующая обновленные столбцы во всех связанных строках изменения. *update_mask* — **varbinary(128)**.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **bit**  
  
## <a name="remarks"></a>Замечания  
 Эту функцию можно использовать для получения сведений из маски обновления, которая возвращается в запросе данных изменений. Она особенно полезна во время последующей обработки маски обновления, когда нужно определить, изменился ли конкретный столбец в связанной строке изменений. Дополнительные сведения см. в статье [О системе отслеживания измененных данных (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
 Если эти сведения возвращаются как часть запроса измененных данных, рекомендуется использовать функции [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) и [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) вместо этой функции. Используйте функцию fn_cdc_get_column_ordinal перед запросом информации об изменениях, чтобы номер нужного столбца вычислялся только один раз. Используйте fn_cdc_is_bit_set в запросе для извлечения сведений из маски обновления для каждой возвращаемой строке.  
  
## <a name="permissions"></a>Permissions  
 Необходимо членство в предопределенной роли сервера sysadmin или предопределенной роли базы данных db_owner. Всем остальным пользователям необходимо разрешение SELECT для всех отслеживаемых столбцов в исходной таблице. Кроме того, если для экземпляра отслеживания была определена шлюзовая роль, требуется членство в этой роли базы данных.  
  
## <a name="see-also"></a>См. также:  
 [CDC. &#60; capture_instance &#62; _CT &#40; Transact-SQL &#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)   
 [CDC.captured_columns &#40; Transact-SQL &#41;](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
  
  