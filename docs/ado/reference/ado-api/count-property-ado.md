---
title: Свойство Count (ADO) | Документы Microsoft
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
- _Collection::Count
helpviewer_keywords:
- Count property [ADO]
ms.assetid: da9ccd1f-d402-41a2-940c-45556fc5340d
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: edda8415ad83ee3aa874ec1e3e6dd6f8e20b41b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="count-property-ado"></a>Свойство Count (ADO)
Указывает количество объектов в коллекции.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает **длинные** значение.  
  
## <a name="remarks"></a>Замечания  
 Используйте **число** свойство, чтобы определить, сколько объектов в данной коллекции.  
  
 Поскольку нумерация для членов коллекции начинается с нуля, всегда следует создавать циклы, начиная с нуля элемента и заканчивая значение **число** свойство минус 1. Если используется Microsoft Visual Basic для перебора элементов коллекции без проверки в **число** свойства, используйте **For Each... Далее** команды.  
  
 Если **число** свойство имеет значение 0, нет ни одного объекта в коллекции.  
  
## <a name="applies-to"></a>Объект применения  
  
||||  
|-|-|-|  
|[Коллекция Axes (многомерные объекты ADO)](../../../ado/reference/ado-md-api/axes-collection-ado-md.md)|[Коллекция Columns (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md)|[Коллекция CubeDefs (многомерные объекты ADO)](../../../ado/reference/ado-md-api/cubedefs-collection-ado-md.md)|  
|[Коллекция Dimensions (многомерные объекты ADO)](../../../ado/reference/ado-md-api/dimensions-collection-ado-md.md)|[Коллекция Errors (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)|[Коллекция Fields (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)|  
|[Коллекция Groups (ADOX)](../../../ado/reference/adox-api/groups-collection-adox.md)|[Коллекция Hierarchies (многомерные объекты ADO)](../../../ado/reference/ado-md-api/hierarchies-collection-ado-md.md)|[Коллекция Indexes (ADOX)](../../../ado/reference/adox-api/indexes-collection-adox.md)|  
|[Коллекция Keys (ADOX)](../../../ado/reference/adox-api/keys-collection-adox.md)|[Коллекция Levels (многомерные объекты ADO)](../../../ado/reference/ado-md-api/levels-collection-ado-md.md)|[Коллекция Members (многомерные объекты ADO)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)|  
|[Коллекция Parameters (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)|[Коллекция Positions (многомерные объекты ADO)](../../../ado/reference/ado-md-api/positions-collection-ado-md.md)|[Коллекция Procedures (ADOX)](../../../ado/reference/adox-api/procedures-collection-adox.md)|  
|[Коллекция Properties (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)|[Коллекция Tables (ADOX)](../../../ado/reference/adox-api/tables-collection-adox.md)|[Коллекция Users (ADOX)](../../../ado/reference/adox-api/users-collection-adox.md)|  
|[Коллекция Views (ADOX)](../../../ado/reference/adox-api/views-collection-adox.md)|||  
  
## <a name="see-also"></a>См. также  
 [Пример свойства Count (Visual Basic)](../../../ado/reference/ado-api/count-property-example-vb.md)   
 [Пример свойства Count (VC ++)](../../../ado/reference/ado-api/count-property-example-vc.md)   
 [Метод Refresh (ADO)](../../../ado/reference/ado-api/refresh-method-ado.md)
