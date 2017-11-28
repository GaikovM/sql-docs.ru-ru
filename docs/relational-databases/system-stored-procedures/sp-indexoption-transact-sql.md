---
title: "процедура sp_indexoption (Transact-SQL) | Документы Microsoft"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sp_indexoption
- sp_indexoption_TSQL
dev_langs: TSQL
helpviewer_keywords: sp_indexoption
ms.assetid: 75f836be-d322-4a53-a45d-25bee6b42a52
caps.latest.revision: "43"
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: b9a7b0b048fa2c2e0e9087463e89d4a4264de20b
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="spindexoption-transact-sql"></a>sp_indexoption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Устанавливает блокирующие значения параметров для определенных пользователем кластеризованных и некластеризованных индексов или таблиц, не имеющих кластеризованного индекса.  
  
 Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] автоматически делает выбор уровня блокировки: страница, строка, таблица. Необязательно задавать эти параметры вручную. **процедура sp_indexoption** предназначена для опытных пользователей, которые знают о конкретных типах блокировки.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]Вместо этого используйте [ALTER INDEX &#40; Transact-SQL &#41; ](../../t-sql/statements/alter-index-transact-sql.md).  
  
||  
|-|  
|**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [текущей версии](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_indexoption [ @IndexNamePattern = ] 'table_or_index_name'   
    , [ @OptionName = ] 'option_name'   
    , [ @OptionValue = ] 'value'  
```  
  
## <a name="arguments"></a>Аргументы  
 [  **@IndexNamePattern=**] **"***table_or_index_name***"**  
 Уточненное или неуточненное имя пользовательской таблицы или индекса. *table_or_index_name* — **nvarchar(1035)**, не имеет значения по умолчанию. Кавычки требуются, только если указан уточненный индекс или таблица. Если указано полное имя таблицы, включая имя базы данных, в качестве последнего должно использоваться имя текущей базы данных. Если имя таблицы указано без индекса, то значение указанного аргумента устанавливается во все индексы этой таблицы и в саму таблицу, если не существует кластеризованных индексов.  
  
 [  **@OptionName =**] **"***option_name***"**  
 Имя параметра индекса. *option_name* — **varchar(35)**, не имеет значения по умолчанию. *option_name* может иметь одно из следующих значений.  
  
|Значение|Description|  
|-----------|-----------------|  
|**AllowRowLocks**|Если TRUE, то допустимы блокировки строк при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки. Если FALSE, то блокировка строк не используется. Значение по умолчанию — TRUE.|  
|**AllowPageLocks**|Если TRUE, то допустимы блокировки страниц при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц. Если FALSE, то блокировка страниц не используется. Значение по умолчанию — TRUE.|  
|**DisAllowRowLocks**|Если TRUE, то блокировка строк не используется. Если FALSE, то допустимы блокировки строк при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.|  
|**DisAllowPageLocks**|Если TRUE, то блокировка страниц не используется. Если FALSE, то допустимы блокировки страниц при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.|  
  
 [  **@OptionValue =**] **"***значение***"**  
 Указывает ли *option_name* параметр будет включен (TRUE, ON, yes или 1) или выключен (FALSE, OFF, no или 0). *значение* — **varchar(12)**, не имеет значения по умолчанию.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или больше чем 0 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 XML-индексы не поддерживаются. Если указаны XML-индексы, или имя таблицы указано без имени индекса, и таблица содержит XML-индекс, то инструкция завершается ошибкой. Чтобы задать эти параметры, используйте [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) вместо него.  
  
 Чтобы отобразить текущую строку и на страницы свойств, используйте [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) или [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) представления каталога.  
  
-   Строки, страницы и блокировки уровня таблицы допустимы при доступе к индексу при **AllowRowLocks** = TRUE или **DisAllowRowLocks** = FALSE, и **AllowPageLocks** = TRUE или  **DisAllowPageLocks** = FALSE. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выбирает соответствующую блокировку и может повышать уровень с блокировки строки или страницы до блокировки таблицы.  
  
 При доступе к индексу, допускается только блокировка на уровне таблицы при **AllowRowLocks** = FALSE или **DisAllowRowLocks** = TRUE и **AllowPageLocks** = FALSE или  **DisAllowPageLocks** = TRUE.  
  
 Если имя таблицы указано без индекса, то настройка применяется ко всем индексам этой таблицы. Если базовая таблица не имеет кластеризованного индекса (т.е. имеется куча), то настройки применяются следующим образом:  
  
-   Когда **AllowRowLocks** или **DisAllowRowLocks** , задано значение TRUE или FALSE, то установка применяется к куче и любым связанным некластеризованным индексам.  
  
-   Когда **AllowPageLocks** параметр имеет значение TRUE или **DisAllowPageLocks** имеет значение false, то установка применяется к куче и любым связанным некластеризованным индексам.  
  
-   Когда **AllowPageLocks** параметр имеет значение FALSE или **DisAllowPageLocks** имеет значение TRUE, установка полностью применяется к некластеризованным индексам. Таким образом, все блокировки страниц не допускаются для некластеризованных индексов. В куче, для страницы недопустимы только совмещаемая (S), обновления (U) и монопольная (X) блокировки. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может запросить намеренную блокировку страницы (IS, IU или IX) для внутренних целей.  
  
## <a name="permissions"></a>Permissions  
 Требуется разрешение ALTER на таблицу.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-setting-an-option-on-a-specific-index"></a>A. Настройка параметра на указанный индекс  
 Следующий пример запрещает блокировки страниц на `IX_Customer_TerritoryID` индекс `Customer` таблицы.  
  
```tsql  
USE AdventureWorks2012;  
GO  
EXEC sp_indexoption N'Sales.Customer.IX_Customer_TerritoryID',  
    N'disallowpagelocks', TRUE;  
```  
  
### <a name="b-setting-an-option-on-all-indexes-on-a-table"></a>Б. Настройка параметра на все индексы таблицы  
 Следующий пример демонстрирует блокировки строк на все индексы, связанные с таблицей `Product`. Представление каталога `sys.indexes` запрашивается до и после выполнения процедуры `sp_indexoption` для демонстрации результата выполнения инструкции.  
  
```tsql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
-- Set the disallowrowlocks option on the Product table.   
EXEC sp_indexoption N'Production.Product',  
    N'disallowrowlocks', TRUE;  
GO  
--Verify the row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
```  
  
### <a name="c-setting-an-option-on-a-table-with-no-clustered-index"></a>В. Настройка параметра на таблицу, не имеющую кластеризованного индекса  
 Следующий пример запрещает блокировки страниц на таблицу, не имеющую кластеризованного индекса (куча). `sys.indexes` Запрос к представлению каталога до и после `sp_indexoption` процедура выполняется для демонстрации результата выполнения инструкции.  
  
```tsql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options of the table.   
SELECT OBJECT_NAME (object_id) AS [Table], type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
-- Set the disallowpagelocks option on the table.   
EXEC sp_indexoption DatabaseLog,  
    N'disallowpagelocks', TRUE;  
GO  
--Verify the row and page lock settings of the table.  
SELECT OBJECT_NAME (object_id) AS [Table], allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  