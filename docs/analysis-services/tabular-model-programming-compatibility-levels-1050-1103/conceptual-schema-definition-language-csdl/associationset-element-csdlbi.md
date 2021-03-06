---
title: Элемент AssociationSet (CSDLBI) | Документы Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c2ae4f565a2383284421bb433ed5cc98371196d3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="associationset-element-csdlbi"></a>Элемент AssociationSet (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Элемент **AssociationSet** — сложный тип, который определяет взаимосвязь. В модели данных CSDLBI взаимосвязь представляет собой связь между таблицами.  
  
 Элемент **AssociationSet** должен быть задан для каждой связи в модели. Элемент **AssociationSet** определяет конечные точки с помощью элемента **Association** . Элемент **AssociationSet** также определяет метаданные о связи и их использование в модели данных.  
  
## <a name="applicable-attributes"></a>Применимые атрибуты  
 В следующей таблице перечислены элементы и атрибуты, определяющие элемент **AssociationSet** .  
  
|Название|Обязателен|Описание|  
|----------|-----------------|-----------------|  
|Состояние|Да|Строка, которая указывает, активна ассоциация или нет. Значение определяется элементом State.|  
|Скрыта|Нет|Логическое значение, определяющее, является ли связь видимой. По умолчанию значение Hidden — **false**, т. е. все связи видимы в модели.|  
  
## <a name="state-element"></a>Элемент State  
 Элемент **State** — простой тип, который описывает, активна ли связь, должен использоваться в вычислениях или оставаться неактивным, но ссылаться на него в вычислениях следует явно.  
  
 Если имеется несколько наборов ассоциаций между двумя сущностями, не более одного набора ассоциаций может быть отмечено как Active. Другими словами, если между одними и теми же двумя таблицами есть две связи, то только одна связь может быть активной.  
  
 В следующей таблице перечислены значения элемента **State** .  
  
|Значение|Описание|  
|-----------|-----------------|  
|Активен|Взаимосвязь активна.|  
|Неактивный|Взаимосвязь активна.|  
  
## <a name="example"></a>Пример  
 **Табличные**  
  
 В следующем примере показана связь в табличной модели AdventureWorks для CSDLBI версии 1.1. Взаимосвязь помечена как неактивная, так как связь уже существует (между OrderKey и Date).  
  
```  
<AssociationSet   
    Name="InternetSales_Date_Date_Date"  
    Association="Sandbox.InternetSales_Date_Date_Date">  
    <End EntitySet="InternetSales" />  
    <End EntitySet="DimDate" />  
<bi:AssociationSet State="Inactive" />  
</AssociationSet>  
  
```  
  
## <a name="example"></a>Пример  
 **Многомерные**  
  
 В следующем примере показана связь, определенная между таблицами Sales и Currency в кубе операций Contoso.  
  
```  
<AssociationSet   
    Name ="Sales_Currency_Currency_Currency_Name2"  
    Association ="Sandbox.Sales_Currency_Currency_Currency_Name2">  
    <End EntitySet ="Sales" />  
    <End EntitySet ="Currency" />  
<bi:AssociationSet />  
</AssociationSet>  
```  
  
## <a name="see-also"></a>См. также:  
 [Технический справочник по заметки бизнес-Аналитики для языка CSDL](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/technical-reference-for-bi-annotations-to-csdl.md)  
  
  
