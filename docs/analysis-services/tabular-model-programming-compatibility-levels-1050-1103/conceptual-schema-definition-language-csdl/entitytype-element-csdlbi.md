---
title: Элемент EntityType (CSDLBI) | Документы Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 60d1b91ab417343a9dffe7e83520ebcd868c5d6d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="entitytype-element-csdlbi"></a>Элемент EntityType (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Элемент **EntityType** — это сложный тип, представляющий структуру сущности высокого уровня, например клиента или заказа, в модели данных. Элемент **bi:EntityType** расширяет определение [EntityType](http://msdn.microsoft.com/library/bb399206.aspx) , которое используется в среде [Entity Data Framework](http://msdn.microsoft.com/library/bb399567.aspx).  
  
 Элемент EntityType должен быть указан для каждой из сущностей, включенных в модель данных. Вложенные элементы EntityType описывают столбцы и меры в таблице. Связи между таблицами включены в **EntityContainer**.  
  
## <a name="elements-and-attributes"></a>Элементы и атрибуты  
 В следующей таблице перечислены элементы и атрибуты, определяющие элемент **EntityType** . См. также атрибуты, применимые к элементу [EntityType](http://msdn.microsoft.com/library/bb399206.aspx) .  
  
|Название|Обязателен|Описание|  
|----------|-----------------|-----------------|  
|Содержание|Нет|Строка, содержащая допустимые типы данных для столбцa. Это значение получается из значения DimensionAttributeTypeEnumType в модели данных.<br /><br /> Если значение DimensionAttributeTypeEnumType — «ExtendedType», то значение Contents получается из элемента ExtendedType в DimensionAttribute. От клиента не требуется откликаться на эти значения.|  
|DefaultDetails|Нет|Список ссылок на свойства, которые представляют собой набор столбцов в таблице.<br /><br /> В разделе [элемент DefaultDetails & #40; CSDLBI & #41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/defaultdetails-element-csdlbi.md).|  
|DefaultImage|Нет|Ссылка на столбец, который содержит изображение, иллюстрирующее сущность.<br /><br /> В многомерных моделях этот элемент относится к двоичному атрибуту атрибута измерения. Если этот атрибут присутствует, элемент должен содержать только один элемент MemberRef.<br /><br /> В разделе [элемент MemberRef & #40; CSDLBI & #41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/memberref-element-csdlbi.md).|  
|DefaultMeasure|Нет|Ссылка на меру в сущности, которая должна использоваться по умолчанию для вычислений в сущности. Если значение не задано, то по умолчанию используется SUM.<br /><br /> В разделе [элемент MemberRef & #40; CSDLBI & #41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/memberref-element-csdlbi.md).|  
|DisplayKey|Нет|Список ссылок на столбцы или на окончания роли, служащий жестким идентификатором, с помощью которого пользователь уникально идентифицирует экземпляр сущности.<br /><br /> В разделе [элемент DisplayKey & #40; CSDLBI & #41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/displaykey-element-csdlbi.md).|  
|Иерархия|Нет|Список иерархий в модели.<br /><br /> В разделе [элемент иерархии & #40; CSDLBI & #41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/hierarchy-element-csdlbi.md).|  
|ReferenceName|Да|Идентификатор, который может использоваться для ссылок на эту сущность в запросах выражений анализа данных (DAX).<br /><br /> Если этот атрибут отсутствует, то используется полное имя поля сущности.|  
|SortMembers|Нет|Список свойств, подлежащих сортировке. Атрибут SortDirection задает направление сортировки — по возрастанию или убыванию.|  
  
## <a name="contents-element"></a>Элемент Contents  
 Элемент **Contents** — простой тип, который описывает тип данных в сущности.  
  
 Содержимое сущности (столбец) может иметь любое из следующих значений.  
  
|Значение|Description|  
|-----------|-----------------|  
|Regular|В противном случае не определено.|  
|Time|Атрибуты представляют периоды времени, например годы, семестры, кварталы, месяцы и дни.|  
|Измерение географии|Атрибуты представляют географические сведения, например города или почтовые индексы.|  
|Измерение организаций|Атрибуты представляют сведения об организации, например сотрудниках или филиалах.|  
|Измерение ведомости материалов|Атрибуты представляют опись или производственные данные, например списки деталей для изделий.|  
|Измерение счетов|Атрибуты представляют диаграмму счетов для финансовой отчетности.|  
|Измерение заказчиков|Атрибуты представляют сведения о заказчиках или контактные данные.|  
|Измерение продуктов|Атрибуты представляют сведения о продукте.|  
|Сценарий|Атрибуты представляют сведения о планах или данные о стратегическом анализе.|  
|Количественное измерение|Атрибуты представляют количественную информацию.|  
|Служебная программа|Атрибуты представляют различные сведения.|  
|Измерение валют|Содержит данные и метаданные для валюты.|  
|Измерение курсов|Атрибуты представляют данные о курсе обмена валюты.|  
|Измерение каналов|Атрибуты представляют данные о канале.|  
|Измерение продвижений|Атрибуты представляют сведения об акциях по продвижению.|  
  
## <a name="example"></a>Пример  
 **Табличные**  
  
 В следующем примере показана часть представления CSDLBI версии 1.1 таблицы Geography, используемой в табличной модели AdventureWorks. Столбец RowNumber — скрытый столбец, который создается автоматически в качестве идентификатора строки в табличных моделях и поэтому имеет атрибут Contents ( **RowNumber**).  
  
```  
  
<EntityType   
     Name="DimGeography">  
     <Key>  
        <PropertyRef Name="RowNumber" />  
     </Key>  
     <Property   
        Name="RowNumber"   
        Type="Int64" Nullable="false">  
     <bi:Property   
        Hidden="true"   
        Contents="RowNumber"   
        Stability="RowNumber" />  
     </Property>  
....  
  
```  
  
## <a name="example"></a>Пример  
 **Многомерные**  
  
 В следующем примере показаны элементы EntityType в CSDLBI версии 1.1, представляющие часть измерения времени из куба операций Contoso.  
  
```  
<EntityType   
       Name="CalendarQuarter">  
    <Key>  
       <PropertyRef Name="RowNumber" />  
    </Key>  
  
    <Property Name="RowNumber"   
       Type="Int64"   
       Nullable="false">  
    <bi:Property   
       Hidden="true"   
       Contents="RowNumber"   
       Stability="RowNumber"   
    />  
    </Property>  
  
    <Property Name="CalendarQuarter2"   
       Type="String"   
       MaxLength="Max"   
       Unicode="true"   
       FixedLength="false"   
       Nullable="false">  
    <bi:Property   
       Caption="CalendarQuarter"   
       ReferenceName="CalendarQuarter"   
    />  
    </Property>  
   <bi:EntityType />  
</EntityType>  
```  
  
## <a name="see-also"></a>См. также:  
 [Технический справочник по заметки бизнес-Аналитики для языка CSDL](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/technical-reference-for-bi-annotations-to-csdl.md)  
  
  
