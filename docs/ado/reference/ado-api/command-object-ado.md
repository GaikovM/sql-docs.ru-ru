---
title: Команды объекта (ADO) | Документы Microsoft
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
- Command
helpviewer_keywords:
- Command object [ADO]
ms.assetid: a02c22fb-542d-465e-a629-30fd59dcbebf
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bf4a875b8ffbae9442f415fcf56d72809900da81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="command-object-ado"></a>Объект команды (ADO)
Определяет той или иной команды, которую планируется выполнить в источнике данных.  
  
## <a name="remarks"></a>Замечания  
 Используйте **команда** для запросов к базе данных и возвращать записи в [записей](../../../ado/reference/ado-api/recordset-object-ado.md) объекта, для выполнения операции массового или для работы со структурой базы данных. В зависимости от функции поставщика, некоторые **команда** коллекции, методы или свойства может привести к ошибке, если они ссылаются.  
  
 С коллекциями, методы и свойства **команда** объекта, можно сделать следующее:  
  
-   Определить исполняемый текст команды (например, инструкция SQL) с [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) свойство. Кроме того, для команды или запроса структур, отличные от простой строки (например, шаблон запросов XML) определяют команды с [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) свойство.  
  
-   Дополнительно можно указать диалект, используемый в **CommandText** или **CommandStream** с [диалект](../../../ado/reference/ado-api/dialect-property.md) свойство.  
  
-   Определить запросы с параметрами и аргументами хранимая процедура с [параметр](../../../ado/reference/ado-api/parameter-object.md) объектов и [параметры](../../../ado/reference/ado-api/parameters-collection-ado.md) коллекции.  
  
-   Указывают ли имена параметров должны быть переданы поставщика с [NamedParameters](../../../ado/reference/ado-api/namedparameters-property-ado.md) свойство.  
  
-   Выполнять команды и возвращать **записей** объекта при необходимости с [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) метод.  
  
-   Укажите тип команды с [CommandType](../../../ado/reference/ado-api/commandtype-property-ado.md) свойство перед выполнением для оптимизации производительности.  
  
-   Контролировать, сохраняет ли поставщик подготовленную (скомпилированную) версию команды перед выполнением с [Готово](../../../ado/reference/ado-api/prepared-property-ado.md) свойство.  
  
-   Задать число секунд, поставщик будет ожидать выполнения с помощью команды [CommandTimeout](../../../ado/reference/ado-api/commandtimeout-property-ado.md) свойство.  
  
-   Связать открытое соединение с **команда** , задавая его [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) свойство.  
  
-   Задать [имя](../../../ado/reference/ado-api/name-property-ado.md) свойство для идентификации **команда** объекту в качестве метода для соответствующей [подключения](../../../ado/reference/ado-api/connection-object-ado.md) объекта.  
  
-   Передайте **команда** объект [источника](../../../ado/reference/ado-api/source-property-ado-recordset.md) свойство **записей** для получения данных.  
  
-   Доступ к атрибутам поставщика с [свойства](../../../ado/reference/ado-api/properties-collection-ado.md) коллекции.  
  
> [!NOTE]
>  Для выполнения запроса без использования **команда** , передать строку запроса для [Execute](../../../ado/reference/ado-api/execute-method-ado-connection.md) метод **подключения** объекта или [откройте](../../../ado/reference/ado-api/open-method-ado-recordset.md)метод **записей** объекта. Тем не менее **команда** объекта является обязательным, если вы хотите сохранить текст команды и повторно выполните ее или использовать параметры запроса.  
  
 Для создания **команда** объекта независимо от ранее определенные **подключения** , задать его **ActiveConnection** свойства допустимую строку соединения. По-прежнему создает ADO **подключения** , но не назначает этот объект переменной объекта. Тем не менее если несколько **команда** объектов для того же соединения, необходимо явно создать и открыть **подключения** объекта; этот задает **соединение** объекта переменной объекта. Убедитесь, что **подключения** объект был успешно открыт до назначения его **ActiveConnection** свойство **команда** объекта, так как назначение Закрыто **подключения** объекта приводит к ошибке. Если вы не установите **ActiveConnection** свойство **команда** объекта этой переменной объекта ADO создает новую **подключения** объекта для каждого  **Команда** объекта, даже если используется та же строка подключения.  
  
 Для выполнения **команда**, вызвать, его [имя](../../../ado/reference/ado-api/name-property-ado.md) свойства в связанном **подключения** объекта. **Команда** должен иметь его **ActiveConnection** свойство **подключения** объекта. Если **команда** имеет параметров, передаваемых их значений в качестве аргументов метода.  
  
 Если два или более **команда** объекты выполняются на том же соединении и либо **команда** объект является хранимой процедуры с выходными параметрами, возникает ошибка. Для выполнения каждой **команда** объекта, используйте отдельные подключения или отключения всех остальных **команда** объекты из соединения.  
  
 **Параметры** коллекции является членом по умолчанию **команда** объекта. В результате код следующие две инструкции эквивалентны.  
  
```  
objCmd.Parameters.Item(0)  
objCmd(0)  
```  
  
-   **Команда** объект не является безопасным для создания сценариев.  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства объекта команды, методы и события](../../../ado/reference/ado-api/command-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также  
 [Объект соединения (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Коллекция параметров (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)   
 [Коллекция свойств (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Приложение А. Поставщики](../../../ado/guide/appendixes/appendix-a-providers.md)
