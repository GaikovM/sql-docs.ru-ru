---
title: Lead (многомерные Выражения) | Документы Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0e9faf5d0f7b549887c6aa4a12e4c502c2663ce3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578996"
---
# <a name="lead-mdx"></a>Lead (многомерные выражения)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Возвращает элемент, который следует за заданным элементом через указанное число позиций на уровне элемента.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Member_Expression.Lead( Index )  
```  
  
## <a name="arguments"></a>Аргументы  
 *Member_Expression.*  
 Допустимое многомерное выражение, возвращающее элемент.  
  
 *Index*  
 Допустимое числовое выражение, указывающее число позиций элементов.  
  
## <a name="remarks"></a>Примечания  
 Позиции элементов на уровне определяются естественным порядком иерархии атрибутов. Нумерация позиций начинается с нуля.  
  
 Если заданная позиция равна нулю (0), **привести** функция возвращает указанный элемент.  
  
 Если заданная позиция отрицательна, **привести** функция возвращает предыдущий элемент.  
  
 `Lead(1)` эквивалентно [NextMember](../mdx/nextmember-mdx.md) функции. `Lead(-1)` эквивалентно [PrevMember](../mdx/prevmember-mdx.md) функции.  
  
 **Привести** функция подобна [запаздывания](../mdx/lag-mdx.md) за тем исключением, которое **запаздывания** функция выглядит в направлении, противоположном направлению **привести** функции. Таким образом, вызов `Lead(n)` эквивалентен вызову `Lag(-n)`.  
  
## <a name="example"></a>Пример  
 В следующем примере возвращается значения для декабря 2001 г.:  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(-2) ON 0  
FROM [Adventure Works]  
  
```  
  
 В следующем примере возвращается значения для марта 2002 г.:  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(1) ON 0  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>См. также  
 [Справочник по функциям многомерных Выражений &#40;многомерных Выражений&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
