---
title: Обработка результатов | Документы Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-provider
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [SQL Server Native Client]
ms.assetid: 20887ac4-f649-4e7f-92e6-f929e2e70952
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8261fed484c457a1245fbce3a0659e495d689fd2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="processing-results"></a>Обработка результатов
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Если объект набора строк создан в результате выполнения команды или его формирования непосредственно из поставщика, то пользователю необходимо иметь возможность доступа к данным набора строк и их получения.  
  
 Наборы строк являются важными объектами, позволяющими поставщику OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обеспечивать доступ к данным в табличной форме. С концептуальной точки зрения объект набора строк — это набор строк, в котором каждая строка содержит данные столбцов. Объект набора строк предоставляет интерфейсы, такие как **IRowset** (содержит методы для последовательной выборки строк из набора строк), **IAccessor** (разрешает определение группы привязок столбцов описывает способ табличных данных привязана к переменным программы потребителей), **IColumnsInfo** (предоставляет сведения о столбцах в наборе строк), и **IRowsetInfo** (предоставляет сведения о наборе строк).  
  
 Пользователь может вызвать **IRowset::GetData** метод для извлечения строки данных из набора строк в буфер. Прежде чем **GetData** является именем, пользователь описывает буфер с помощью набора структур DBBINDING. Каждая привязка описывает способ сохранения столбца набора строк в буфере пользователя и содержит следующее.  
  
-   Порядковый номер столбца (или параметра), к которому относится привязка.  
  
-   Информация о том объекте, для которого выполняется привязка (например, значение данных, длина данных, состояние привязки).  
  
-   Информация о смещении в буфере для каждой из этих частей.  
  
-   Длина и тип значений данных, которые имеются в буфере пользователя.  
  
 При получении данных поставщик использует информацию каждой привязки, чтобы определить, куда и как получить данные из буфера пользователя. При задании значений данных в буфере пользователя поставщик использует информацию каждой привязки, чтобы определить, куда и как вернуть данные в буфер пользователя.  
  
 После задания структур DBBINDING создается метод доступа (**IAccessor::CreateAccessor**). Метод доступа представляет собой коллекцию привязок и используется для получения данных из буфера пользователя или задания значений данных в буфере.  
  
## <a name="see-also"></a>См. также:  
 [Создание приложения поставщика OLE DB для собственного клиента SQL Server](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [Разделы руководства по OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
  
