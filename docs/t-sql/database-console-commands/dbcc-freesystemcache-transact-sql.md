---
title: DBCC FREESYSTEMCACHE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FREESYSTEMCACHE_TSQL
- DBCC_FREESYSTEMCACHE_TSQL
- DBCC FREESYSTEMCACHE
- FREESYSTEMCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- clearing unused cache entries
- DBCC FREESYSTEMCACHE statement
- unused cache entries
- releasing unused cache entries
- freeing unused cache entries
- cleaning unused cache entries
ms.assetid: 4b5c460b-e4ad-404a-b4ca-d65aba38ebbb
caps.latest.revision: 35
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 75c98aca474af0ebf02fd446cf9645fdb7c20373
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-freesystemcache-transact-sql"></a>DBCC FREESYSTEMCACHE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Удаляет все неиспользуемые элементы из всех кэшей. Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] заранее автоматически очищает неиспользуемые элементы кэша в фоновом режиме, освобождая память для текущих записей. Однако можно использовать эту команду, чтобы вручную удалить неиспользуемые записи из всех кэшей или из указанного кэша пула регулятора ресурсов.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
```sql
DBCC FREESYSTEMCACHE   
    ( 'ALL' [, pool_name ] )   
    [WITH   
    { [ MARK_IN_USE_FOR_REMOVAL ] , [ NO_INFOMSGS ]  }  
    ]  
```  
  
## <a name="arguments"></a>Аргументы  
 ( 'ALL' [,*pool_name* ] )  
 Ключевое слово ALL указывает все поддерживаемые кэши.  
 Аргумент *pool_name* указывает кэш пула Resource Governor. Освобождены будут только записи, связанные с этим пулом.  
  
 MARK_IN_USE_FOR_REMOVAL  
 Асинхронно освобождает текущие используемые элементы из соответствующих кэшей после того, как они перестают использоваться. Не влияет на новые элементы, созданные в кэше после выполнения DBCC FREESYSTEMCACHE WITH MARK_IN_USE_FOR_REMOVAL.  
  
 NO_INFOMSGS  
 Подавляет вывод всех информационных сообщений.  
  
## <a name="remarks"></a>Remarks  
При выполнении инструкции DBCC FREESYSTEMCACHE очищается кэш планов для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обнаружил %d экземпляров, сброшенных на диск хранилищ кэша для хранилища кэша "%s" (части кэша планов) в результате операций DBCC FREEPROCCACHE или DBCC FREESYSTEMCACHE". Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

## <a name="result-sets"></a>Результирующие наборы  
Инструкция DBCC FREESYSTEMCACHE возвращает следующее сообщение: "Выполнение инструкции DBCC завершено. Если инструкция DBCC выдает сообщения об ошибках, обратитесь к системному администратору».
  
## <a name="permissions"></a>Разрешения  
Требует разрешения ALTER SERVER STATE на сервере.
  
## <a name="examples"></a>Примеры  
  
### <a name="a-releasing-unused-cache-entries-from-a-resource-governor-pool-cache"></a>A. Освобождение неиспользуемых записей кэша из кэша пула регулятора ресурсов  
В следующем примере показывается, как очищать кэши, выделенные указанному пулу ресурсов регулятора ресурсов.
  
```sql
-- Clean all the caches with entries specific to the resource pool named "default".  
DBCC FREESYSTEMCACHE ('ALL', default);  
```  
  
### <a name="b-releasing-entries-from-their-respective-caches-after-they-become-unused"></a>Б. Освобождение записей из кэшей после прекращения их использования  
В следующем примере используется предложение MARK_IN_USE_FOR_REMOVAL, чтобы освободить записи из всех текущих кэшей, когда записи становятся неиспользуемыми.
  
```sql
DBCC FREESYSTEMCACHE ('ALL') WITH MARK_IN_USE_FOR_REMOVAL;  
```  
  
## <a name="see-also"></a>См. также:  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[DBCC FREESESSIONCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[Регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)
  
  
