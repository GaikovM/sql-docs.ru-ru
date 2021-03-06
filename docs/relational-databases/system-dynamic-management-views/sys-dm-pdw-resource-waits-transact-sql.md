---
title: sys.dm_pdw_resource_waits (Transact-SQL) | Документы Microsoft
ms.custom: ''
ms.date: 03/07/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 4930ac03a9262e73f382ca250fd6d63c233e83b6
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwresourcewaits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Отображает ожидания сведения для всех типов ресурсов в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
|Имя столбца|Тип данных|Описание|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Позиция в списке ожидания запроса.|Отсчитываемый от нуля порядковый номер. Это не является уникальным для всех записей ожидания.|  
|session_id|**nvarchar(32)**|Идентификатор сеанса, в котором произошло состояния ожидания.|В разделе session_id в [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|Тип|**nvarchar(255)**|Тип ожидания, которую представляет эта запись.|Возможные значения:<br /><br /> Соединение<br /><br /> Локальные запросы параллелизма<br /><br /> Распределенные запросы с параллелизмом<br /><br /> DMS параллелизма<br /><br /> Параллелизм резервного копирования|  
|object_type|**nvarchar(255)**|Тип объекта, который распространяется ожидания.|Возможные значения:<br /><br /> **ОБЪЕКТ**<br /><br /> **DATABASE**<br /><br /> **SYSTEM**<br /><br /> **SCHEMA**<br /><br /> **ПРИЛОЖЕНИЯ**|  
|object_name|**nvarchar(386)**|Имя или идентификатор GUID объекта, который повлиял ожидания.|Таблицы и представления отображаются трехкомпонентные имена.<br /><br /> Индексы и статистику отображаются четырехкомпонентные имена.<br /><br /> Имена субъектов и баз данных являются строковых имен.|  
|request_id|**nvarchar(32)**|Идентификатор запроса, в котором произошло состояния ожидания.|QID идентификатор запроса.<br /><br /> Идентификатор GUID для запросов загрузки.|  
|request_time|**datetime**|Время, с которой была запрошена блокировка или ресурс.||  
|acquire_time|**datetime**|Время, с которого была получена блокировка или ресурс.||  
|state|**nvarchar(50)**|Состояние из состояния ожидания.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Приоритет элемента ожидания.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|Число слотов параллелизма (32 max), зарезервированный для данного запроса.|1 — для SmallRC<br /><br /> 3 — для MediumRC<br /><br /> 7 для LargeRC<br /><br /> 22 — для XLargeRC|  
|resource_class|**nvarchar(20)**|Класс ресурсов для данного запроса.|SmallRC<br /><br /> MediumRC<br /><br /> LargeRC<br /><br /> XLargeRC|  
  
## <a name="see-also"></a>См. также  
 [Хранилище данных SQL и параллельные хранилища данных динамических административных представлений &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
