---
title: SELECT DISTINCT FROM &lt;модель &gt; (DMX) | Документы Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DISTINCT
- SELECT
dev_langs:
- DMX
helpviewer_keywords:
- discrete columns [DMX]
- discretized columns [DMX]
- SELECT DISTINCT FROM <model> statement
- continuous columns
ms.assetid: 0ab44ef6-1c3b-4809-a687-4d5d13f343af
caps.latest.revision: 44
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 5754cbeb07789b0d5f7a3f51386f108633cda83e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="select-distinct-from-ltmodel-gt-dmx"></a>SELECT DISTINCT FROM &lt;модель &gt; (расширения интеллектуального анализа данных)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Возвращает все возможные состояния выбранного столбца модели. Возвращаемые значения зависят от того, какие значения содержит указанный столбец — дискретные, дискретизированные числовые или непрерывные числовые значения.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
SELECT [FLATTENED] DISTINCT [TOP <n>] <expression list> FROM <model>   
[WHERE <condition list>][ORDER BY <expression>]  
```  
  
## <a name="arguments"></a>Аргументы  
 *n*  
 Необязательно. Целое число, указывающее количество возвращаемых строк.  
  
 *список выражений*  
 Список связанных идентификаторов столбцов (производных от модели) или выражений.  
  
 *model*  
 Идентификатор модели.  
  
 *Список условий*  
 Условие ограничения значений, возвращаемых из списка столбцов.  
  
 *expression*  
 Необязательно. Выражение, возвращающее скалярное значение.  
  
## <a name="remarks"></a>Замечания  
 **SELECT DISTINCT FROM** инструкции работает только с одним столбцом или же с набором связанных столбцов. С набором несвязанных столбцов это предложение не работает.  
  
 **SELECT DISTINCT FROM** инструкция позволяет напрямую ссылаться на столбец внутри вложенной таблицы. Например:  
  
```  
<model>.<table column reference>.<column reference>  
```  
  
 Результаты **SELECT DISTINCT FROM \<модели >** инструкции, зависит от типа столбца. В следующей таблице описаны поддерживаемые типы столбцов и выводимые инструкцией данные.  
  
|Тип столбца|Вывод|  
|-----------------|------------|  
|Discrete|Уникальные значения в столбце.|  
|Дискретизированный|Средняя точка каждого дискретного сегмента памяти в столбце.|  
|Непрерывный|Средняя точка для значений столбца.|  
  
## <a name="discrete-column-example"></a>Пример дискретного столбца  
 В следующем образце кода основан на `[TM Decision Tree]` модели, создаваемой в [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). Запрос возвращает уникальные значения, существующие в дискретном столбце `Gender`.  
  
```  
SELECT DISTINCT [Gender]  
FROM [TM Decision Tree]  
```  
  
 Пример результатов:  
  
|Пол|  
|------------|  
||  
|Ж|  
|M|  
  
 Для столбцов, содержащих дискретные значения, результаты всегда включают недостающее состояние, показанное как значение NULL.  
  
## <a name="continuous-column-example"></a>Пример непрерывного столбца  
 Следующий образец кода возвращает средний, минимальный и максимальный возраст для всех значений столбца.  
  
```  
SELECT DISTINCT [Age] AS [Midpoint Age],   
    RangeMin([Age]) AS [Minimum Age],   
    RangeMax([Age]) AS [Maximum Age]  
FROM [TM Decision Tree]  
```  
  
 Пример результатов:  
  
|Midpoint Age|Minimum Age|Maximum Age|  
|------------------|-----------------|-----------------|  
||||  
|62|26|97|  
  
 Кроме того, запрос возвращает одну строку значений NULL, которая представляет отсутствующие значения.  
  
## <a name="discretized-column-example"></a>Пример дискретизированного столбца  
 Следующий образец кода возвращает среднее, максимальное и минимальное значения для каждого сегмента, созданного алгоритмом для столбца [`Yearly Income]`. Чтобы воспроизвести результаты этого примера, потребуется создать новую структуру интеллектуального анализа данных, аналогичную `[Targeted Mailing]`. В окне мастера измените тип содержимого `Yearly Income` столбец из **Continuous** для **Discretized**.  
  
> [!NOTE]  
>  Можно также изменить модель интеллектуального анализа данных, созданную в учебнике по основам интеллектуального анализа данных, чтобы дискретизировать столбец структуры интеллектуального анализа данных [`Yearly Income]`. Сведения о том, как это сделать в разделе [изменить дискретизацию столбца в модели интеллектуального анализа данных](../analysis-services/data-mining/change-the-discretization-of-a-column-in-a-mining-model.md). Однако изменение дискретизации столбца повлечет за собой повторную обработку структуры интеллектуального анализа данных; в итоге изменятся результаты других моделей, построенных с использованием этой структуры.  
  
```  
SELECT DISTINCT [Yearly Income] AS [Bucket Average],   
    RangeMin([Yearly Income]) AS [Bucket Minimum],   
    RangeMax([Yearly Income]) AS [Bucket Maximum]  
FROM [TM Decision Tree]  
```  
  
 Пример результатов:  
  
|Bucket Average|Bucket Minimum|Bucket Maximum|  
|--------------------|--------------------|--------------------|  
||||  
|24610.7|10000|39221.41|  
|55115.73|39221.41|71010.05|  
|84821.54|71010.05|98633.04|  
|111633.9|98633.04|124634.7|  
|147317.4|124634.7|170000|  
  
 Видно, что значения столбца [Yearly Income] дискретизированы на пять сегментов. Кроме того, имеется дополнительная строка значений NULL, представляющая недостающие значения.  
  
 Количество десятичных разрядов в результатах зависит от клиента, использованного для выполнения запроса. Здесь они были округлены до двух десятичных разрядов, с одной стороны — для простоты, а с другой — чтобы отразить значения, показанные в среде [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
 К примеру, если при просмотре модели с помощью средства просмотра дерева решений щелкнуть узел, содержащий сгруппированных по показателю дохода клиентов, во всплывающей подсказке отобразятся следующие свойства узла:  
  
 Age >=69 AND Yearly Income < 39 221,41  
  
> [!NOTE]  
>  Минимальное значение минимального сегмента и максимальное значение максимального сегмента представляют собой самое высокое и самое низкое наблюдаемое значение. Предполагается, что все значения, остающиеся за пределами этого наблюдаемого диапазона, принадлежат минимальному и максимальному сегментам.  
  
## <a name="see-also"></a>См. также  
 [ВЫБЕРИТЕ &AMP;#40;РАСШИРЕНИЙ ИНТЕЛЛЕКТУАЛЬНОГО АНАЛИЗА ДАННЫХ&AMP;#41;](../dmx/select-dmx.md)   
 [Расширения интеллектуального анализа данных &#40; расширений интеллектуального анализа данных &#41; Инструкции управления данными](../dmx/dmx-statements-data-manipulation.md)   
 [Расширения интеллектуального анализа данных &#40; расширений интеллектуального анализа данных &#41; Справка по инструкции](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
