---
title: Повторная синхронизация строк | Документы Microsoft
description: Повторная синхронизация строк с помощью драйвера OLE DB для SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- synchronization [OLE DB]
- IRowsetResynch interface
- resynchronizing rows
- data updates [SQL Server], OLE DB
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: cda60f254d200e6e8c23235bdeb9988b2dd80309
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="updating-data-in-rowsets---resynchronizing-rows"></a>Обновление данных в наборах строк - повторная синхронизация строк
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Драйвер OLE DB для SQL Server поддерживает **IRowsetResynch** на [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] курсора поддерживается только наборы строк. **IRowsetResynch** недоступен по требованию. Пользователь должен запросить этот интерфейс перед открытием набора строк.  
  
## <a name="see-also"></a>См. также:  
 [Обновление данных в наборах строк](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
