---
title: "Состояние готовности свойство (RDS) | Документы Microsoft"
ms.prod: sql-non-specified
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
apitype: COM
helpviewer_keywords:
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: cfcc72e79e90e10885d329208208db2f1a0267f5
ms.contentlocale: ru-ru
ms.lasthandoff: 09/09/2017

---
# <a name="readystate-property-rds"></a>Состояние готовности свойство (RDS)
Индикатор выполнения [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) объекта, поскольку он получает данные в его [записей](../../../ado/reference/ado-api/recordset-object-ado.md) объекта.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, серверные компоненты служб удаленных рабочих СТОЛОВ больше не включаются в операционной системе Windows (в разделе Windows 8 и [руководство по Windows Server 2012 совместимости](https://www.microsoft.com/en-us/download/details.aspx?id=27416) для получения дополнительных сведений). Клиентские компоненты служб удаленных рабочих СТОЛОВ будут удалены в будущих версиях Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие служб удаленных рабочих СТОЛОВ необходимо перенести в [службы данных WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает одно из следующих значений.  
  
|Значение|Description|  
|-----------|-----------------|  
|**adcReadyStateLoaded**|Текущий запрос все еще выполняется, и никакие строки не будут выбраны. **DataControl** объекта **записей** недоступна для использования.|  
|**adcReadyStateInteractive**|Начальный набор строк, полученных с помощью текущего запроса будет сохранено в **DataControl** объекта **записей** и доступны для использования. Оставшиеся строки выбираются все еще выполняется.|  
|**adcReadyStateComplete**|Все строки, извлеченные в текущем запросе сохраненные в **DataControl** объекта **записей** и доступны для использования.<br /><br /> Это состояние также будет существовать, если операция прервана из-за ошибки или **записей** объект не инициализирован.|  
  
> [!NOTE]
>  Каждый клиентский исполняемый файл, который использует эти константы должен предоставить их объявления. Можно вырезать и вставить объявления констант из файла Adcvbs.inc, расположенный в папке установки по умолчанию для библиотеки служб удаленных рабочих СТОЛОВ.  
  
## <a name="remarks"></a>Замечания  
 Используйте [onReadyStateChange](../../../ado/reference/rds-api/onreadystatechange-event-rds.md) событий для отслеживания изменений в **состояние готовности** свойства во время выполнения операции асинхронного запроса. Это более эффективно, чем периодически проверки значения свойства.  
  
 При возникновении ошибки во время асинхронной операции, **состояние готовности** изменения свойств **adcReadyStateComplete**, [состояние](../../../ado/reference/ado-api/state-property-ado.md) свойство изменяется с **adStateExecuting** для **adStateClosed**и **записей** объекта [значение](../../../ado/reference/ado-api/value-property-ado.md) свойство остается *Nothing *.  
  
## <a name="applies-to"></a>Объект применения  
 [Объект DataControl (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойства состояние готовности (VBScript)](../../../ado/reference/rds-api/readystate-property-example-vbscript.md)   
 [Метод Cancel (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [Свойство ExecuteOptions (RDS)](../../../ado/reference/rds-api/executeoptions-property-rds.md)


