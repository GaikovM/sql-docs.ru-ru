---
title: sp_databases (Transact-SQL) | Документы Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_databases_TSQL
- sp_databases
dev_langs:
- TSQL
helpviewer_keywords:
- sp_databases
ms.assetid: 2a83b92a-9ecc-43c4-8ff4-e91e3a940b5a
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c22415c34f0e25dc1117b6a5f86839c66f0ba53b
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "33238005"
---
# <a name="spdatabases-transact-sql"></a>sp_databases (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Выдает список баз данных, которые размещаются в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или доступны через шлюз базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_databases  
```  
  
## <a name="return-code-values"></a>Значения кода возврата  
 None  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**ИМЯ_БАЗЫ_ДАННЫХ**|**sysname**|Имя базы данных. В [!INCLUDE[ssDE](../../includes/ssde-md.md)], этот столбец представляет имя базы данных, хранящихся в **sys.databases** представления каталога.|  
|**DATABASE_SIZE**|**int**|Размер базы данных в килобайтах.|  
|**ПРИМЕЧАНИЯ**|**varchar(254)**|Для компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] это поле всегда возвращает значение NULL.|  
  
## <a name="remarks"></a>Примечания  
 Возвращаемые имена баз данных могут использоваться в качестве параметров в инструкции USE для изменения текущего контекста базы данных.  
  
 **sp_databases** не имеет эквивалента в Open Database Connectivity (ODBC).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CREATE DATABASE, ALTER ANY DATABASE или VIEW ANY DEFINITION; кроме того, должно быть разрешение на доступ к базе данных. Разрешение VIEW ANY DEFINITION не может быть запрещено.  
  
## <a name="examples"></a>Примеры  
 Следующий пример демонстрирует выполнение процедуры `sp_databases`.  
  
```sql  
USE master;  
GO  
EXEC sp_databases;  
```  
  
## <a name="see-also"></a>См. также  
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Функция HAS_DBACCESS &#40;Transact-SQL&#41;](../../t-sql/functions/has-dbaccess-transact-sql.md)  
  
  
