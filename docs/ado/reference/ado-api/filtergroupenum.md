---
title: FilterGroupEnum | Документы Microsoft
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
apitype: COM
f1_keywords:
- FilterGroupEnum
helpviewer_keywords:
- FilterGroupEnum enumeration [ADO]
ms.assetid: b22e725e-84bd-4286-a070-290c278c3783
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4452dceb121993655b216112a356f05c4267b1b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="filtergroupenum"></a>FilterGroupEnum
Указывает группу записей, которые будут отфильтрованы из [записей](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**adFilterAffectedRecords**|2|Фильтры для просмотра только записей, затронутых последней [удаление](../../../ado/reference/ado-api/delete-method-ado-recordset.md), [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md), или [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) вызова.|  
|**adFilterConflictingRecords**|5|Фильтры для просмотра записей, прошедших с последнего пакета обновления.|  
|**adFilterFetchedRecords**|3|Фильтры для просмотра записей в текущем кэше — то есть, результаты последнего вызова для получения записей из базы данных.|  
|**adFilterNone**|0|Удаляет текущий фильтр и восстанавливает все записи для просмотра.|  
|**adFilterPendingRecords**|1|Фильтры для просмотра только записей, были изменены, но еще не отправлены на сервер. Применимо только для режима обновления пакета.|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Пакет: **com.ms.wfc.data**  
  
|Константа|  
|--------------|  
|AdoEnums.FilterGroup.AFFECTEDRECORDS|  
|AdoEnums.FilterGroup.CONFLICTINGRECORDS|  
|AdoEnums.FilterGroup.FETCHEDRECORDS|  
|AdoEnums.FilterGroup.NONE|  
|AdoEnums.FilterGroup.PENDINGRECORDS|  
  
## <a name="applies-to"></a>Объект применения  
 [Свойство Filter](../../../ado/reference/ado-api/filter-property.md)
