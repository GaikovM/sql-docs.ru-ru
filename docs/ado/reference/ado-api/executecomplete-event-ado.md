---
title: Событие ExecuteComplete (ADO) | Документы Microsoft
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
- Connection::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- ExecuteComplete event [ADO]
ms.assetid: 62470d42-e511-494c-bec4-ad4591734b7b
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ab8c0b33fe31499999cc73d2ebc03ef0d32ace70
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="executecomplete-event-ado"></a>Событие ExecuteComplete (ADO)
**ExecuteComplete** событие вызывается после завершения выполнения команды.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
ExecuteComplete RecordsAffected, pError, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Параметры  
 *RecordsAffected*  
 Объект **длинные** значение, указывающее количество записей, затронутых этой командой.  
  
 *pError*  
 [Ошибка](../../../ado/reference/ado-api/error-object.md) объекта. Здесь описываются ошибки, возникшей при значение **adStatus** — **adStatusErrorsOccurred**; в противном случае оно не задано.  
  
 *adStatus*  
 [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) значение состояния. Когда это событие вызывается, этот параметр имеет значение **adStatusOK** при успешном выполнении операции, которая вызвала событие или для **adStatusErrorsOccurred** при сбое операции.  
  
 Прежде чем это событие возвращает, присвойте этому параметру значение **adStatusUnwantedEvent** для предотвращения последующих уведомлений.  
  
 *Командной*  
 [Команда](../../../ado/reference/ado-api/command-object-ado.md) объект, который был выполнен. Содержит **команда** даже в том случае, если вызов **Connection.Execute** или **Recordset.Open** без явного создания **команда** , в какие варианты **команда** объект создан внутренним образом ADO.  
  
 *pRecordset*  
 Объект [записей](../../../ado/reference/ado-api/recordset-object-ado.md) объект, являющийся результатом выполнения команды. Это **записей** может быть пустым. Никогда не следует уничтожать этого объекта набора записей в этот обработчик событий. Это приведет к нарушение прав доступа при ADO пытается получить доступ к объекту, который больше не существует.  
  
 *pConnection*  
 Объект [подключения](../../../ado/reference/ado-api/connection-object-ado.md) объекта. Соединение, для которого была выполнена операция.  
  
## <a name="remarks"></a>Замечания  
 **ExecuteComplete** событие может происходить из-за **подключения.** [Выполнение](../../../ado/reference/ado-api/execute-method-ado-connection.md), **команды.** [Выполнение](../../../ado/reference/ado-api/execute-method-ado-command.md), **набор записей.** [Откройте](../../../ado/reference/ado-api/open-method-ado-recordset.md), **набор записей.** [Requery](../../../ado/reference/ado-api/requery-method.md), или **набор записей.** [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md) методы.  
  
## <a name="see-also"></a>См. также  
 [Пример модели событий ADO (VC ++)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Общие сведения об обработчике событий ADO](../../../ado/guide/data/ado-event-handler-summary.md)
