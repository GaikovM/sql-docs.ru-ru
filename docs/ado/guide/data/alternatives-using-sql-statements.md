---
title: 'Альтернативы: С помощью инструкций SQL | Документы Microsoft'
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ADO]
- editing data [ADO], sql statements
- ADO, SQL statements
ms.assetid: 8b528b23-063d-45ea-8dea-6a90d4060b20
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9a8480e03fe09d640e02bb387e56e2c44556c6d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="alternatives-using-sql-statements"></a>Альтернативы: С помощью инструкций SQL
ADO также позволяет с помощью команд в качестве альтернативы его встроенные свойства и методы для изменения данных. В зависимости от поставщика, все операции, описанные в этом разделе также может выполняться путем передачи команды для источника данных. Например, можно использовать инструкции SQL UPDATE для изменения данных без использования **значение** свойство **поля**. Добавление новых записей в источник данных, а не метод ADO можно использовать инструкции SQL INSERT **AddNew**. Дополнительные сведения о SQL или языка обработки данных поставщика см. в документации источника данных.  
  
 Например можно передать строку SQL, содержащий инструкции DELETE в базе данных, как показано в следующем коде:  
  
```  
'BeginSQLDelete  
strSQL = "DELETE FROM Shippers WHERE ShipperID = " & intId  
objConn1.Execute strSQL, , adCmdText + adExecuteNoRecords  
'EndSQLDelete  
```
