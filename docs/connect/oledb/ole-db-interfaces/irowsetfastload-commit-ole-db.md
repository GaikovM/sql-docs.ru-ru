---
title: IRowsetFastLoad::Commit (OLE DB) | Документы Microsoft
description: IRowsetFastLoad::Commit (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- IRowsetFastLoad::Commit (OLE DB)
apitype: COM
helpviewer_keywords:
- Commit method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d93c7a83880e81331f6322386fa3ccbc983fc4ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="irowsetfastloadcommit-ole-db"></a>IRowsetFastLoad::Commit (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Обозначает конец пакета вставляемых строк и записывает эти строки в таблицу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Примеры см. в разделе [массового копирования данных с помощью IRowsetFastLoad &#40;OLE DB&#41; ](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) и [отправить данные больших двоичных ОБЪЕКТОВ SQL SERVER с помощью IROWSETFASTLOAD и ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT Commit(  
      BOOL fDone);  
```  
  
## <a name="arguments"></a>Аргументы  
 *fDone*[in]  
 Если значение равно FALSE, то набор строк сохраняет достоверность и может использоваться пользователем для дополнительной вставки строк. Если значение равно TRUE, то набор строк теряет достоверность и пользователь не может выполнять дальнейшую вставку.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод завершился успешно, и все добавленные записи были записаны в таблицу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 E_FAIL  
 Произошла ошибка, зависящая от поставщика. Получите сведения об ошибке для конкретного текста ошибки из поставщика.  
  
 E_UNEXPECTED  
 Метод был вызван для набора строк массового копирования, ранее недействительными **IRowsetFastLoad::Commit** метод.  
  
## <a name="remarks"></a>Замечания  
 Драйвер OLE DB для SQL Server набору строк массового копирования, ведет себя как набор строк в режиме отложенного обновления. При добавлении данных строк с помощью набора строк, добавленные строки обрабатываются так же, как ожидающие выполнения вставки на наборе строк, поддерживающем **IRowsetUpdate**.  
  
 Пользователь должен вызвать **зафиксировать** метод набору строк массового копирования, чтобы записать добавленные строки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] таблицу так же, как **IRowsetUpdate::Update** метод используется для отправки ожидающих строк экземпляр SQL Server.  
  
 Если потребитель освобождает ссылку на набор данных массового копирования без вызова **зафиксировать** метод, все добавленные строки не были записаны, теряются.  
  
 Пользователь может сгруппировать добавленные строки, вызвав **зафиксировать** метод с *fDone* аргумента значение FALSE. Когда *fDone*имеет значение TRUE, набор строк становится недействительным. Строк недопустимый массового копирования поддерживаются только **ISupportErrorInfo** интерфейс и **IRowsetFastLoad::Release** метод.  
  
## <a name="see-also"></a>См. также  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  
