---
title: Уровни изоляции (OLE DB) | Документы Microsoft
description: Уровни изоляции (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-transactions
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- isolation levels [OLE DB]
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 55cc5c0db959ce21a5a4b3ae60d83c2f42a56a09
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="isolation-levels-ole-db"></a>Уровни изоляции (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Клиенты [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] могут управлять уровнями изоляции транзакций для соединения. Чтобы управлять уровнем изоляции транзакций, использует драйвер OLE DB для объекта-получателя SQL Server:  
  
-   Свойство DBPROP_SESS_AUTOCOMMITISOLEVELS драйвера OLE DB для SQL Server режим автоматической фиксации по умолчанию.  
  
     Драйвер OLE DB для SQL Server по умолчанию для уровня — DBPROPVAL_TI_READCOMMITTED.  
  
-   *IsoLevel* параметр **ITransactionLocal::StartTransaction** метод для локальных ручной фиксации транзакций.  
  
-   *IsoLevel* параметр **ITransactionDispenser::BeginTransaction** метод координацией MS DTC распределенные транзакции.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] разрешает доступ только для чтения на уровне изоляции чтения «грязных» данных. Все другие уровни ограничивают параллелизм применением блокировок к объектам [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если клиент требует более высоких уровней параллелизма, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] применяет более строгие ограничения на параллельные обращения к данным. Чтобы обеспечить высокий уровень параллельного доступа к данным, драйвер OLE DB для SQL Server потребитель должен интеллектуально управлять запросами для конкретных уровней параллелизма.  
  
> [!NOTE]  
>  В [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] появился уровень изоляции моментального снимка. Дополнительные сведения см. в разделе [работа с изоляцией моментального снимка](../../oledb/features/working-with-snapshot-isolation.md).  
  
## <a name="see-also"></a>См. также  
 [Транзакции](../../oledb/ole-db-transactions/transactions.md)  
  
  
