---
title: Программное обнаружение компонентов потока данных | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: building-packages-programmatically
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- PipelineComponentInfos collection
- data flow task [Integration Services], components
- discovering data flow components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: ff92a96a-8af6-4532-82cc-c0bbff92401b
caps.latest.revision: 44
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f36b4d0d2235c03cee6bfc51c83a03a051e0cebe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="discovering-data-flow-components-programmatically"></a>Программный поиск компонентов потока данных
  После добавления задачи потока данных в пакет может возникнуть необходимость определить, какие компоненты потока данных доступны для использования. Можно программным способом выявить источники, преобразования и назначения потока данных, установленные и доступные на локальном компьютере. Сведения о добавлении задачи потока данных в пакет см. в разделе [Добавление задачи потока данных программным образом](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md).  
  
## <a name="discovering-components"></a>Поиск компонентов  
 Класс <xref:Microsoft.SqlServer.Dts.Runtime.Application> предоставляет коллекцию <xref:Microsoft.SqlServer.Dts.Runtime.Application.PipelineComponentInfos%2A>, содержащую объект <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> для каждого компонента, правильно установленного на локальном компьютере. Каждый объект <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> содержит сведения о компоненте, в частности, его имя, описание и время создания. Можно использовать значение, возвращаемое в свойстве <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A>, чтобы задать свойство <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> при добавлении компонента в пакет.  
  
## <a name="next-step"></a>Следующий шаг  
 Следующим шагом после выявления доступных компонентов является добавление и настройка компонентов, которые рассматриваются в следующем разделе [Добавление компонентов потока данных программным образом](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md).  
  
## <a name="sample"></a>Образец  
 В следующем образце кода показано, как перечислить коллекцию <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfos> объекта <xref:Microsoft.SqlServer.Dts.Runtime.Application>, чтобы программным способом определить компоненты потока данных, доступные на локальном компьютере. В этом примере необходима ссылка на сборку Microsoft.SqlServer.ManagedDTS.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application application = new Application();  
      PipelineComponentInfos componentInfos = application.PipelineComponentInfos;  
  
      foreach (PipelineComponentInfo componentInfo in componentInfos)  
      {  
        Console.WriteLine("Name: " + componentInfo.Name + "\n" +  
          " CreationName: " + componentInfo.CreationName + "\n");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim application As Application = New Application()  
  
    Dim componentInfos As PipelineComponentInfos = application.PipelineComponentInfos  
  
    For Each componentInfo As PipelineComponentInfo In componentInfos  
      Console.WriteLine("Name: " & componentInfo.Name & vbCrLf & _  
        " CreationName: " & componentInfo.CreationName & vbCrLf)  
    Next  
  
    Console.Read()  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>См. также:  
 [Добавление компонентов потока данных программным образом](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
  
  
