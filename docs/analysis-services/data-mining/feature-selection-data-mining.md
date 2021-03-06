---
title: Выбор (интеллектуальный анализ данных) компонентов | Документы Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a93e503978779e56250ddf190c61b1b2411050b9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="feature-selection-data-mining"></a>Выбор компонентов (интеллектуальный анализ данных)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  *Выбор компонентов* является важной частью машинного обучения. Выбор компонентов — это процесс уменьшения объема входных данных для обработки и анализа либо поиска наиболее значимых входных данных. Близкий термин *реконструирование компонентов* (или *извлечение компонентов*) обозначает процесс извлечения полезной информации или функций из существующих данных.  
  
## <a name="why-do-feature-selection"></a>Зачем выполнять выбор компонентов?  
 Выбор компонентов крайне важен для создания эффективной модели по нескольким причинам. Одна из них заключается в том, что выбор компонентов предполагает некоторое *снижение числа элементов*, чтобы сократить количество атрибутов, учитываемых при построении модели. Данные почти всегда содержат больше сведений, чем необходимо для построения модели, часть которых неверна. Например, у вас может быть набор данных с 500 столбцами, описывающими характеристики клиентов, но если данные в некоторых столбцах очень разрежены, то пользы от их добавления в модель будет немного. Если же некоторые столбцы дублируются, использование их обоих может негативно сказаться на модели.  
  
 Выбор компонентов не только повышает качество модели, но и делает процесс моделирования более эффективным. Если использовать ненужные столбцы при построении модели, для ее обучения потребуется больше ресурсов ЦП и памяти, а для готовой модели — больше пространства хранения. Выбор компонентов и определение наилучших столбцов следует осуществлять даже при наличии достаточного количества ресурсов, так как ненужные столбцы могут снизить качество модели по разным причинам:  
  
-   Зашумленные и избыточные данные затрудняют обнаружение значимых закономерностей.  
  
-   Если набор данных относится к высокоразмерным, большинству алгоритмов интеллектуального анализа данных требуется намного более крупный набор данных для обучения.  
  
 В процессе выбора компонентов средство или алгоритм анализа или моделирования активно выбирает или отбрасывает атрибуты в зависимости от их полезности для анализа.  Аналитик может выполнять реконструирование компонентов, чтобы добавить компоненты и удалить или изменить существующие данные, а алгоритм машинного обучения обычно оценивает столбцы и проверяет их полезность для модели.  
  
 ![Выбор программы, и процесса](../../analysis-services/data-mining/media/ssdm-featureselectionprocess.png "функции выделения и процесса")  
  
 Проще говоря, выбор компонентов помогает решить дилемму: либо слишком много данных, имеющих небольшую ценность, либо слишком мало данных, имеющих высокую ценность. Ваша цель при выборе компонентов заключается в том, чтобы определить минимальное количество столбцов из источника данных, которые важны для построения модели.  
  
## <a name="how-feature-selection-works-in-sql-server-data-mining"></a>Работа выбора компонентов при интеллектуальном анализе данных SQL Server  
 Выбор компонентов всегда выполняется до обучения модели. Имеется некоторый набор встроенных алгоритмов и методик выбора компонентов, позволяющих автоматически исключать нерелевантные столбцы и находить лучшие компоненты. Каждый алгоритм включает набор методик по умолчанию для интеллектуального сокращения количества компонентов.  Однако можно также вручную задать параметры, которые повлияют на порядок выбора компонентов.  
  
 Во время автоматического выбора компонентов для каждого атрибута вычисляется оценка, после чего осуществляется выбор только тех атрибутов, которые имеют наилучшие оценки. Предусмотрена также возможность коррекции пороговых значений для верхних оценок. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] В интеллектуальном анализе данных предусмотрено множество методов для вычисления этих оценок. Конкретный метод, который будет применяться к модели, зависит от следующих факторов:  
  
-   Алгоритм, используемый в модели  
  
-   Тип данных атрибута  
  
-   Все параметры, которые можно задать в модели  
  
 Выбор компонентов применяется к входным данным, прогнозируемым атрибутам или состояниям в столбце. После того как оценка для выбора компонентов завершена, только те атрибуты и состояния, которые были выбраны алгоритмом, включаются в процесс построения модели и могут использоваться для прогноза. Если выбрать прогнозируемый атрибут, который не проходит порог для выбора компонентов, то он все же может использоваться для прогнозирования, но прогнозы будут основаны только на глобальной статистике, представленной в модели.  
  
> [!NOTE]  
>  Выбор компонентов затрагивает только те столбцы, которые используются в модели, и не влияет на хранилище структуры интеллектуального анализа данных. Столбцы, которые исключены из модели интеллектуального анализа данных, все еще остаются доступными в структуре, а данные в столбцах структуры интеллектуального анализа данных становятся кэшированными.  
  
### <a name="feature-selection-scores"></a>Оценки при выборе компонентов  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Интеллектуальный анализ данных поддерживает несколько популярных и широко известных методов оценки атрибутов. Метод, применяемый в любом конкретном алгоритме или наборе данных, зависит от типов данных и использования столбцов.  
  
-   Для ранжирования и сортировки атрибутов в столбцах, которые содержат недвоичные непрерывные числовые данные, используется оценка *интересность* .  
  
-   Алгоритмы*Энтропия Шеннона* и оценки *Байеса* доступны для столбцов, содержащих дискретные и дискретизированные данные. Однако если модель содержит непрерывные столбцы, то оценка их полезности будет использоваться для оценки всех входных столбцов в целях обеспечения согласованности.  
  
#### <a name="interestingness-score"></a>Оценка интересности  
 Характеристика представляет интерес, если она предоставляет полезный фрагмент информации. Однако существует множество способов определения *интересности* .  *Новизна* может оказаться ценной при обнаружении выбросов, но способность различать между собой тесно связанные элементы или *распознавать вес*может оказаться в большей степени интересной для классификации.  
  
 Мера интересности, которая используется в интеллектуальном анализе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , *основана на энтропии*, а это означает, что атрибуты со случайными распределениями имеют более высокую энтропию и менее значительный прирост информации, поэтому являются менее интересными. Энтропия, относящаяся к любому конкретному атрибуту, сравнивается с энтропией всех других атрибутов следующим образом:  
  
 Интересность(Атрибут) = - (m - Энтропия(Атрибут)) * (m - Энтропия(Атрибут)).  
  
 Под главной энтропией, или m, подразумевается энтропия всего набора компонентов. Вычитая энтропию целевого атрибута из главной энтропии, можно оценить, сколько информации предоставляет атрибут.  
  
 Эта оценка используется по умолчанию каждый раз, когда столбец содержит недвоичные непрерывные числовые данные.  
  
#### <a name="shannons-entropy"></a>Энтропия Шеннона  
 Энтропия Шеннона используется для измерения неопределенности случайной переменной по отношению к конкретному результату. Например, энтропия броска монеты может быть представлена как функция вероятности выпадения орла.  
  
 В службах Analysis Services используется следующая формула для вычисления энтропии Шеннона:  
  
 H(X) = -∑ P(xi) log(P(xi))  
  
 Этот метод вычисления показателя доступен для дискретных и дискретизированных атрибутов.  
  
#### <a name="bayesian-with-k2-prior"></a>Алгоритм Байеса с априорной оценкой K2  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] В интеллектуальном анализе данных предусмотрены две оценки выбора компонентов, которые основаны на байесовских сетях. Байесовская сеть представляет собой *ориентированный* или *ациклический* граф состояний и переходов между состояниями; это означает, что некоторые состояния всегда предшествуют текущему состоянию, некоторые состояния следуют за ним, а граф не повторяется и не содержит циклов. По определению, байесовские сети обеспечивают использование априорных знаний. Но остается вопрос о том, какие предыдущие состояния должны использоваться при вычислении вероятностей последующих состояний, который важен с точки зрения проектирования, производительности и точности алгоритма.  
  
 Купером и Херсковицем был разработан алгоритм K2 для обучения на основе байесовской сети, который часто используется в интеллектуальном анализе данных. Он является масштабируемым и позволяет анализировать многочисленные переменные, но требует упорядочения переменных, используемых в качестве входных. Дополнительные сведения см. в статье Чикеринга, Гейгера и Хекермана [Обучаемые байесовские сети](http://research.microsoft.com/en-us/um/people/heckerman/hgc94uai.pdf) .  
  
 Этот метод вычисления показателя доступен для дискретных и дискретизированных атрибутов.  
  
#### <a name="bayesian-dirichlet-equivalent-with-uniform-prior"></a>Эквивалент Дирихле метода Байеса с однородной априорной оценкой  
 В оценке с помощью эквивалента Дирихле метода Байеса (BDE) также используется байесовский анализ для оценки сети на основе заданного набора данных. Метод оценки BDE был разработан Хекерманом и основан на методе BD, разработанном Купером и Херсковицем. Распределение Дирихле представляет собой мультиноминальное распределение, которое описывает условную вероятность каждой переменной в сети и имеет много свойств, полезных для обучения.  
  
 В методе, представляющем собой эквивалент Дирихле метода Байеса с однородной априорной оценкой (BDEU), предполагается наличие частного случая распределения Дирихле, в котором используется математическая константа для создания постоянного или равномерного распределения априорных состояний. В оценке BDE предполагается также эквивалентность правдоподобия, а это означает, что не следует ожидать, будто применяемые данные позволят различать эквивалентные структуры. Иными словами, если оценка для выражения "если А, то Б" аналогична оценке для выражения "если Б, то А", то соответствующие структуры нельзя различить на основе применяемых данных, поэтому не может быть сделан вывод о причинной обусловленности.  
  
 Дополнительные сведения о байесовских сетях и о реализации указанных методов оценки см. в статье [Обучаемые байесовские сети](http://research.microsoft.com/en-us/um/people/heckerman/hgc94uai.pdf).  
  
### <a name="feature-selection-methods-per-algorithm"></a>Методы выбора компонентов для алгоритма  
 В следующей таблице перечислены алгоритмы, которые обеспечивают выбор компонентов, методы выбора компонентов, используемые в алгоритме, а также параметры, задаваемые в целях управления поведением при выборе компонентов.  
  
|Алгоритм|Метод анализа|Комментарии|  
|---------------|------------------------|--------------|  
|упрощенный алгоритм Байеса|Энтропия Шеннона<br /><br /> Алгоритм Байеса с априорной оценкой K2<br /><br /> Эквивалент Дирихле метода Байеса с однородной априорной оценкой (выбор по умолчанию)|В упрощенном алгоритме Байеса (Майкрософт) допускается применение только дискретных или дискретизированных атрибутов, поэтому в нем не может использоваться оценка интересности.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Naive Bayes Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm-technical-reference.md).|  
|Дерево принятия решений|Оценка интересности<br /><br /> Энтропия Шеннона<br /><br /> Алгоритм Байеса с априорной оценкой K2<br /><br /> Эквивалент Дирихле метода Байеса с однородной априорной оценкой (выбор по умолчанию)|Если какие-либо столбцы содержат недвоичные непрерывные значения, то оценка интересности используется для всех столбцов в целях обеспечения согласованности. В противном случае используется метод выбора компонентов по умолчанию или метод, указанный при создании модели.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Decision Trees Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-decision-trees-algorithm-technical-reference.md).|  
|Нейронная сеть|Оценка интересности<br /><br /> Энтропия Шеннона<br /><br /> Алгоритм Байеса с априорной оценкой K2<br /><br /> Эквивалент Дирихле метода Байеса с однородной априорной оценкой (выбор по умолчанию)|В алгоритме нейронных сетей (Майкрософт) могут применяться оба метода: на основе энтропии и байесовских оценок — при условии, что данные содержат непрерывные столбцы.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Neural Network Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md).|  
|Логистическая регрессия|Оценка интересности<br /><br /> Энтропия Шеннона<br /><br /> Алгоритм Байеса с априорной оценкой K2<br /><br /> Эквивалент Дирихле метода Байеса с однородной априорной оценкой (выбор по умолчанию)|Хотя алгоритм логистической регрессии (Майкрософт) основан на алгоритме нейронной сети (Майкрософт), нельзя настроить модели логистической регрессии для управления поведением при выборе компонентов; поэтому по умолчанию выбор компонентов всегда выполняется методом, наиболее подходящим для атрибута.<br /><br /> Если все атрибуты являются дискретными или дискретизированными, то по умолчанию используется эквивалент Дирихле метода Байеса с однородной априорной оценкой (BDEU).<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Logistic Regression Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-logistic-regression-algorithm-technical-reference.md).|  
|Кластеризация|Оценка интересности|Алгоритм кластеризации (Майкрософт) может использовать дискретные или дискретизированные данные. Но поскольку оценка каждого атрибута вычисляется как расстояние и представляется числом из непрерывного ряда чисел, должна использоваться оценка интересности.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Clustering Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-clustering-algorithm-technical-reference.md).|  
|Линейная регрессия|Оценка интересности|В алгоритме линейной регрессии (Майкрософт) применяется только оценка интересности, поскольку этот алгоритм поддерживает лишь непрерывные столбцы.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Linear Regression Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md).|  
|Правила взаимосвязей<br /><br /> Кластеризация последовательностей|Не используется|Выбор компонентов не запускается с этими алгоритмами.<br /><br /> Тем не менее, можно управлять поведением алгоритма и, при необходимости, уменьшить размер входных данных, задавая значения параметров MINIMUM_SUPPORT и MINIMUM_PROBABILIITY.<br /><br /> Дополнительные сведения см. в разделах [Microsoft Association Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-association-algorithm-technical-reference.md) и [Microsoft Sequence Clustering Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm-technical-reference.md).|  
|Временной ряд|Не используется|Выбор компонентов не применяется к моделям временных рядов.<br /><br /> Дополнительные сведения об этом алгоритме см. в разделе [Microsoft Time Series Algorithm Technical Reference](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md).|  
  
## <a name="feature-selection-parameters"></a>Параметры выбора компонентов  
 Алгоритмы, поддерживающие выбор компонентов, позволяют управлять активностью выбора компонентов с помощью следующих параметров. В каждом алгоритме имеется заданное по умолчанию значение допустимого количества входов, но еще предоставляется возможность переопределить это значение по умолчанию и указать количество атрибутов. В этом разделе перечислены параметры для управления выбором компонентов.  
  
#### <a name="maximuminputattributes"></a>MAXIMUM_INPUT_ATTRIBUTES  
 Если в модели содержится больше столбцов, чем задано в параметре *MAXIMUM_INPUT_ATTRIBUTES* , то алгоритм будет пропускать любые столбцы, не представляющие интереса с точки зрения выполненных им вычислений.  
  
#### <a name="maximumoutputattributes"></a>MAXIMUM_OUTPUT_ATTRIBUTES  
 Аналогичным образом, если в модели содержится больше прогнозируемых столбцов, чем задано в параметре *MAXIMUM_OUTPUT_ATTRIBUTES* , то алгоритм будет пропускать любые столбцы, не представляющие интереса с точки зрения выполненных им вычислений.  
  
#### <a name="maximumstates"></a>MAXIMUM_STATES  
 Если в модели содержится больше объектов, чем задано в параметре *MAXIMUM_STATES* , то наименее популярные состояния будут сводиться в одну группу и считаться отсутствующими. Если значение любого из данных параметров равно 0, то выбор компонентов отключается, что влияет на время обработки и производительность.  
  
 Помимо этих методов выбора компонентов, можно повысить эффективность алгоритма по определению или продвижению значимых атрибутов, задав *флаги моделирования* в модели либо *флаги распределения* в структуре. Дополнительные сведения об этих понятиях см. в разделах [Флаги моделирования (интеллектуальный анализ данных)](../../analysis-services/data-mining/modeling-flags-data-mining.md) и [Распределения столбцов (интеллектуальный анализ данных)](../../analysis-services/data-mining/column-distributions-data-mining.md)  
  
## <a name="see-also"></a>См. также  
 [Настройка структуры и моделей интеллектуального анализа данных](../../analysis-services/data-mining/customize-mining-models-and-structure.md)  
  
  
