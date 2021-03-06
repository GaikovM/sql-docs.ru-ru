---
title: представление sys.system_views (Transact-SQL) | Документы Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.system_views_TSQL
- system_views
- system_views_TSQL
- sys.system_views
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_views catalog view
ms.assetid: a526c410-e7b5-4075-8103-e1f3c6837c3c
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3f2a7029a187c48f83ca82c2f56f6966e155b801
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
---
# <a name="syssystemviews-transact-sql"></a>Представление sys.system_views (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Содержит по одной строке для каждого системного представления, автоматически создаваемого [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Все системные представления хранятся в схемах с именами **sys** или **INFORMATION_SCHEMA**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|\<Унаследованные столбцы >||Список столбцов, наследуемых этим представлением см. в разделе [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_replicated**|**бит**|1 = представление реплицировано.|  
|**has_replication_filter**|**бит**|1 = представление имеет фильтр репликации.|  
|**has_opaque_metadata**|**бит**|1 = для представления указан параметр VIEW_METADATA. Дополнительные сведения см. в статье [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).|  
|**has_unchecked_assembly_data**|**бит**|1 = таблица содержит материализованные данные, зависящие от сборки, определение которых изменилось во время последней операции ALTER ASSEMBLY. Будет сброшено в значение 0 после следующей успешной операции DBCC CHECKDB или DBCC CHECKTABLE.|  
|**with_check_option**|**бит**|1 = в определении представления указано предложение WITH CHECK OPTION.|  
|**is_date_correlation_view**|**бит**|1 = представление было автоматически создано системой для хранения сведений о корреляции между **datetime** столбцов. Создание данного представления стало возможным вследствие установки значения ON для параметра DATE_CORRELATION_OPTIMIZATION.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Инструкция DBCC CHECKDB & #40; Transact-SQL & #41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md)  
  
  
