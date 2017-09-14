---
title: "PredictTimeSeries (расширения интеллектуального анализа данных) | Документы Microsoft"
ms.custom: 
ms.date: 03/02/2016
ms.prod: sql-server-2016
ms.reviewer: 
ms.suite: 
ms.technology:
- analysis-services
- analysis-services/data-mining
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- PredictTimeSeries
dev_langs:
- DMX
helpviewer_keywords:
- time series algorithms [Analysis Services]
- time series [Analysis Services]
- EXTEND_MODEL_CASES parameter
- REPLACE_MODEL_CASES parameter
- PredictTimeSeries function
ms.assetid: 85c596be-a7f4-499b-8d36-7e67c2647b6c
caps.latest.revision: 56
author: Minewiskan
ms.author: owend
manager: erikre
ms.translationtype: MT
ms.sourcegitcommit: 1419847dd47435cef775a2c55c0578ff4406cddc
ms.openlocfilehash: d6770961f4e48ca2f6d96aecb4d51e679af396d0
ms.contentlocale: ru-ru
ms.lasthandoff: 08/02/2017

---
# <a name="predicttimeseries-dmx"></a>PredictTimeSeries (расширения интеллектуального анализа данных)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx_md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Возвращает прогнозируемые будущие значения для временного ряда данных. Данные временных рядов являются непрерывными и могут храниться во вложенной таблице или в таблице вариантов. **PredictTimeSeries** функция всегда возвращает вложенную таблицу.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
PredictTimeSeries(<table column reference>)  
PredictTimeSeries(<table column reference>, n)  
PredictTimeSeries(<table column reference>, n-start, n-end)  
PredictTimeSeries(<scalar column reference>)  
PredictTimeSeries(<scalar column reference>, n)  
PredictTimeSeries(<scalar column reference>, n-start, n-end)  
PredictTimeSeries(<table column reference>, n, REPLACE_MODEL_CASES | EXTEND_MODEL_CASES) PREDICTION JOIN <source query>  
PredictTimeSeries(<table column reference>, n-start, n-end, REPLACE_MODEL_CASES | EXTEND_MODEL_CASES) PREDICTION JOIN <source query>  
PredictTimeSeries(<scalar column reference>, n, REPLACE_MODEL_CASES | EXTEND_MODEL_CASES) PREDICTION JOIN <source query>  
PredictTimeSeries(<scalar column reference>, n-start, n-end, REPLACE_MODEL_CASES | EXTEND_MODEL_CASES) PREDICTION JOIN <source query>  
```  
  
## <a name="arguments"></a>Аргументы  
 *\<ссылка на столбец таблицы >*,  *\<ссылку на скалярный столбец >*  
 Указывает имя прогнозируемого столбца. Столбец может содержать скалярные или табличные данные.  
  
 *n*  
 Указывает число следующих прогнозируемых шагов. Если значение не указано для  *n* , значение по умолчанию — 1.  
  
 *n*не может быть равно 0. Функция возвращает ошибку, если не сделать хотя бы один прогноз.  
  
 *n-start, n-end*  
 Указывает диапазон шагов временного ряда.  
  
 *n-start* должен быть целым числом и не может быть равно 0.  
  
 *n-end* должно быть целое число больше *n-start*.  
  
 *\<исходный запрос >*  
 Определяет внешние данные, используемые при формировании прогнозов.  
  
 REPLACE_MODEL_CASES | EXTEND_MODEL_CASES  
 Указывает метод обработки новых данных.  
  
 REPLACE_MODEL_CASES указывает, что точки данных в модели должны быть заменены новыми данными. При этом прогнозы основываются на шаблонах в существующей модели интеллектуального анализа данных.  
  
 EXTEND_MODEL_CASES указывает, что новые данные должны быть добавлены к исходному набору данных для обучения. Будущие прогнозы делаются по составному набору данных только после того, как будут полностью использованы новые данные.  
  
 Эти аргументы можно использовать, только если новые данные добавляются с помощью инструкции PREDICTION JOIN. Если в запросе PREDICTION JOIN аргумент не указан, значение по умолчанию — EXTEND_MODEL_CASES.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 Объект \< *таблицы выражение*>.  
  
## <a name="remarks"></a>Замечания  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Алгоритма временных рядов не поддерживает историческое прогнозирование при использовании инструкции PREDICTION JOIN для добавления новых данных.  
  
 В инструкции PREDICTION JOIN процесс прогнозирования всегда начинается на временном шаге сразу после окончания первоначальной обучающей последовательности. Так происходит даже при добавлении новых данных. Таким образом  *n*  параметр и *n-start* значения параметра должны быть целыми числами больше 0.  
  
> [!NOTE]  
>  Длина новых данных не влияет на точку начала прогнозирования. Таким образом, чтобы добавить новые данные и сделать новые прогнозы, убедитесь, что значение, в котором задана точка начала прогнозирования, больше длины новых данных, либо переместите точку окончания прогнозирования на размер длины новых данных.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано прогнозирование для существующей модели временных рядов.  
  
-   В первом примере показано, как сделать указанное число прогнозов на основе текущей модели.  
  
-   Во втором примере показано использование параметра REPLACE_MODEL_CASES для применения шаблонов в заданной модели к новому набору данных.  
  
-   В третьем примере показано использование параметра EXTEND_MODEL_CASES для обновления модели интеллектуального анализа данных с новыми данными.  
  
 Дополнительные сведения о работе с моделями временных рядов см. Учебник по интеллектуальному анализу данных, [Lesson 2: построение сценария прогнозирования &#40; промежуточного учебник по интеллектуальному анализу данных &#41;](http://msdn.microsoft.com/library/9a988156-c900-4c22-97fa-f6b0c1aea9e2) и [учебника расширений интеллектуального анализа данных прогнозирования ряда времени](http://msdn.microsoft.com/library/38ea7c03-4754-4e71-896a-f68cc2c98ce2).  
  
> [!NOTE]  
>  Модель может давать разные результаты; результаты приведенных ниже примеров призваны лишь продемонстрировать формат результатов.  
  
### <a name="example-1-predicting-a-number-of-time-slices"></a>Пример 1: Прогнозирование числа срезов времени  
 В следующем примере используется **PredictTimeSeries** функция возвращает прогноз для следующих трех временных шагов и при этом результаты ограничиваются рядами M200 в европейском и тихоокеанском регионах. В этой конкретной модели прогнозируемым атрибутом является количество, поэтому необходимо использовать `[Quantity]` в качестве первого аргумента функции PredictTimeSeries.  
  
```  
SELECT FLATTENED  
    [Forecasting].[Model Region],  
    PredictTimeSeries([Forecasting].[Quantity],3)AS t   
FROM  
    [Forecasting]  
WHERE [Model Region] = 'M200 Europe'  
OR [Model Region] = 'M200 Pacific'  
```  
  
 Ожидаемый результат:  
  
|Model Region|t.$TIME|t.Quantity|  
|------------------|-------------|----------------|  
|M200 Europe|7/25/2008 12:00:00 AM|121|  
|M200 Europe|8/25/2008 12:00:00 AM|142|  
|M200 Europe|9/25/2008 12:00:00 AM|152|  
|M200 Pacific|7/25/2008 12:00:00 AM|46|  
|M200 Pacific|8/25/2008 12:00:00 AM|44|  
|M200 Pacific|9/25/2008 12:00:00 AM|42|  
  
 В этом примере для облегчения чтения результатов было использовано ключевое слово FLATTENED.  Если ключевое слово FLATTENED не используется и возвращается иерархический набор строк, запрос возвращает два столбца. Первый столбец содержит значение для [ModelRegion], а второй столбец содержит вложенную таблицу с двумя столбцами: $TIME, показывающий прогнозируемые срезы времени и столбец Quantity, содержащий прогнозируемые значения.  
  
### <a name="example-2-adding-new-data-and-using-replacemodelcases"></a>Пример 2: Добавление новых данных и использование параметра REPLACE_MODEL_CASES  
 Предположим, были обнаружены неверные данные для определенного региона и требуется использовать шаблоны в модели, но необходимо скорректировать прогнозы для соответствия новым данным. Или может оказаться, что другой регион демонстрирует более привлекательные тенденции и к данным такого региона необходимо применить более надежную модель.  
  
 В таких случаях можно использовать параметр REPLACE_MODEL_CASES и задать новый набор данных для использования в качестве исторических данных. При этом прогнозы будут основываться на шаблонах в заданной модели, но плавно продолжаться с конца новых точек данных. Полное Пошаговое руководство этого сценария см. в разделе [дополнительные прогнозы временных рядов &#40; промежуточного Data Mining Tutorial &#41;](http://msdn.microsoft.com/library/b614ebdb-07ca-44af-a0ff-893364bd4b71).  
  
 Запрос PREDICTION JOIN демонстрирует синтаксис замены данных и выполнения новых прогнозов. Для заменяемых данных в примере извлекается значение столбцов «Amount» и «Quantity», каждое из которых умножается на два.  
  
```  
SELECT [Forecasting].[Model Region],  
    PredictTimeSeries([Forecasting].[Quantity], 3, REPLACE_MODEL_CASES)   
FROM  
    [Forecasting]  
PREDICTION JOIN  
  OPENQUERY([Adventure Works DW Multidimensional 2012],  
    'SELECT [ModelRegion],   
    ([Quantity] * 2) as Quantity,  
    ([Amount] * 2) as Amount,  
      [ReportingDate]  
    FROM [dbo].vTimeSeries  
    WHERE ModelRegion = N''M200 Pacific''  
    ') AS t  
ON  
  [Forecasting].[Model Region] = t.[ Model Region] AND  
[Forecasting].[Reporting Date] = t.[ReportingDate] AND  
[Forecasting].[Quantity] = t.[Quantity] AND  
[Forecasting].[Amount] = t.[Amount]  
```  
  
 В следующей таблице сравниваются результаты прогноза.  
  
 Исходные прогнозы:  
  
||||  
|-|-|-|  
|M200 Pacific|7/25/2008 12:00:00 AM|46|  
|M200 Pacific|8/25/2008 12:00:00 AM|44|  
|M200 Pacific|9/25/2008 12:00:00 AM|42|  
  
 Обновленные прогнозы:  
  
||||  
|-|-|-|  
|M200 Pacific|7/25/2008 12:00:00 AM|91|  
|M200 Pacific|8/25/2008 12:00:00 AM|89|  
|M200 Pacific|9/25/2008 12:00:00 AM|84|  
  
### <a name="example-3-adding-new-data-and-using-extendmodelcases"></a>Пример 3: Добавление новых данных и использование параметра EXTEND_MODEL_CASES  
 Пример 3 показано использование *EXTEND_MODEL_CASES* возможность предоставления новых данных, который добавляется в конец существующего ряда данных. Новые данные не заменяют существующие точки данных, а добавляются к модели.  
  
 В следующем примере новые данные предоставляются в инструкции SELECT после NATURAL PREDICTION JOIN. С помощью этого синтаксиса можно указать несколько строк нового ввода, но каждая новая строка ввода имеет уникальную метку времени.  
  
```  
SELECT [Model Region],  
    PredictTimeSeries([Forecasting].[Quantity], 5, EXTEND_MODEL_CASES)   
FROM  
    [Forecasting]  
NATURAL PREDICTION JOIN  
    (SELECT  
        1 as [Reporting Date],  
        10 as [Quantity],  
        'M200 Europe' AS [Model Region]  
    UNION SELECT   
        2 as [Reporting Date],  
        15 as [Quantity],  
        'M200 Europe' AS [Model Region]  
) AS T  
WHERE ([Model Region] = 'M200 Europe'  
 OR [Model Region] = 'M200 Pacific')  
```  
  
 Поскольку в запросе используется *EXTEND_MODEL_CASES* параметр [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] выполняет следующие действия для прогнозов:  
  
-   Увеличивают общий размер обучающих вариантов путем добавления к модели данных за два месяца.  
  
-   Начинают прогноз с конца предыдущих данных варианта. Таким образом, первые два прогноза представляют новые фактические данные продаж, только что добавленные к модели.  
  
-   Возвращают новые прогнозы для оставшихся трех срезов времени на основе только что расширенной модели.  
  
 В следующей таблице приведены результаты выполнения запроса из примера 2. Обратите внимание, что первые два значения, возвращенные для столбца M200 Europe, в точности соответствуют новым указанным значениям. Это поведение не является ошибкой; чтобы начать прогнозы с конца новых данных, необходимо указать шаг времени начала и завершения. Пример того, как это сделать см. в разделе [занятия 5: расширение модели временных рядов](http://msdn.microsoft.com/library/7aad4946-c903-4e25-88b9-b087c20cb67d).  
  
 Также обратите внимание, что новые данные для региона Pacific указаны не были. Таким образом, службы [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] возвращают новые прогнозы по всем пяти срезам времени.  
  
 Количество: M200 Europe. EXTEND_MODEL_CASES:  
  
|$TIME|Количество|  
|-----------|--------------|  
|7/25/2008 0:00|10|  
|8/25/2008 0:00|15|  
|9/25/2008 0:00|72|  
|10/25/2008 0:00|69|  
|11/25/2008 0:00|68|  
  
 Количество: M200 Pacific. EXTEND_MODEL_CASES:  
  
|$TIME|Количество|  
|-----------|--------------|  
|7/25/2008 0:00|46|  
|8/25/2008 0:00|44|  
|9/25/2008 0:00|42|  
|10/25/2008 0:00|42|  
|11/25/2008 0:00|38|  
  
## <a name="example-4-returning-statistics-in-a-time-series-prediction"></a>Пример 4: Возврат статистики в прогнозирование временных рядов  
 **PredictTimeSeries** функция не поддерживает *INCLUDE_STATISTICS* как параметр. Однако вернуть статистику прогнозов для запроса по временным рядам можно с помощью приведенного ниже запроса. Кроме того, этот подход можно использовать при работе с моделями, в которых есть столбцы вложенных таблиц.  
  
 В этой конкретной модели прогнозируемым атрибутом является количество, поэтому необходимо использовать `[Quantity]` в качестве первого аргумента функции PredictTimeSeries. Если в модели используется другой прогнозируемый атрибут, можно подставить другое имя столбца.  
  
```  
SELECT FLATTENED [Model Region],  
(SELECT   
     $Time,  
     [Quantity] as [PREDICTION],   
     PredictVariance([Quantity]) AS [VARIANCE],  
     PredictStdev([Quantity]) AS [STDEV]  
FROM  
      PredictTimeSeries([Quantity], 3) AS t  
) AS t  
FROM Forecasting  
WHERE [Model Region] = 'M200 Europe'  
OR [Model Region] = 'M200 North America'  
```  
  
 Образец результатов.  
  
|Model Region|t.$TIME|t.PREDICTION|t.VARIANCE|t.STDEV|  
|------------------|-------------|------------------|----------------|-------------|  
|M200 Europe|7/25/2008 12:00:00 AM|121|11.6050581415597|3.40661975300439|  
|M200 Europe|8/25/2008 12:00:00 AM|142|10.678201866621|3.26775180615374|  
|M200 Europe|9/25/2008 12:00:00 AM|152|9.86897842568614|3.14149302493037|  
|M200 North America|7/25/2008 12:00:00 AM|163|1.20434529288162|1.20434529288162|  
|M200 North America|8/25/2008 12:00:00 AM|178|1.65031343900634|1.65031343900634|  
|M200 North America|9/25/2008 12:00:00 AM|156|1.68969399185442|1.68969399185442|  
  
> [!NOTE]  
>  Ключевое слово FLATTENED использовалось в этом примере, чтобы представить результаты в виде таблицы, но, если поставщик поддерживает иерархические наборы строк, ключевое слово FLATTENED можно опустить. Если опустить ключевое слово FLATTENED, запрос вернет два столбца. Первый столбец будет содержать значение, идентифицирующее ряды данных `[Model Region]`, а второй столбец — вложенную таблицу со статистикой.  
  
## <a name="see-also"></a>См. также:  
 [Расширения интеллектуального анализа данных &#40; расширений интеллектуального анализа данных &#41; Справочник по функциям](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Примеры запросов для модели временных рядов](../analysis-services/data-mining/time-series-model-query-examples.md)   
 [Прогноз &#40; расширений интеллектуального анализа данных &#41;](../dmx/predict-dmx.md)  
  
  
