---
title: Сценарии обработки и анализа данных и шаблоны решений (SQL Server машинного обучения) | Документы Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d67fd15c44d188870989f2ad6498733c5901fb9d
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="data-science-scenarios-and-solution-templates"></a>Сценарии обработки и анализа данных и шаблоны решений
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Шаблоны — это образцы решений, которые демонстрируют практические приемы и предоставляют стандартные блоки для быстрой реализации собственного решения. Каждый шаблон предназначен для решения конкретной проблемы, для конкретных по вертикали или отрасли. Каждый шаблон охватывает различные задачи: от подготовки данных и формирования характеристик до обучения модели и оценки. Использовать эти шаблоны, чтобы узнать, как [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] работает. Затем вы можете настроить шаблон для соответствия собственные сценарии и создать пользовательское решение. 

Каждое решение включает образцы данных, код R или кода Python, а хранимые процедуры SQL, если это применимо. Код может выполняться в основной R или Python среды разработки, с вычислений в SQL Server. В некоторых случаях можно запустить код, непосредственно с помощью T-SQL и любые средства клиента SQL, таких как SQL Server Management Studio.

> [!TIP]
> 
> Большинство шаблонов поставляются в нескольких версиях, поддерживающих как в локальных и облачных платформах для машинного обучения. Например можно построить решение, используя только SQL Server, или можно построить решение в Microsoft R Server или в машинном обучении Azure.

+ Подробные сведения и обновлений, см. в этом уведомлении: [интересных новых шаблонов в Azure ML](https://blogs.technet.microsoft.com/machinelearning/2015/04/09/exciting-new-templates-in-azure-ml/)

+ Загрузки и инструкции по установке см. в разделе [как использовать шаблоны](#bkmk_HowTo).

## <a name="fraud-detection"></a>обнаружение мошенничества;

[Шаблон обнаружения мошенничества (SQL Server R Services)](https://github.com/Microsoft/r-server-fraud-detection)

**Что:** возможность обнаружения мошеннических транзакций важна для электронной коммерции. Во избежание потери взимания предприятиям необходимо быстро определить транзакции, которые были внесены с помощью инструментов украденного оплаты или учетные данные. При обнаружении мошеннических операций компании обычно принимают меры, блокируя определенные счета как можно скорее, чтобы предотвратить дальнейшие убытки. В этом сценарии вы узнаете, как используются данные из транзакции покупки по в Интернете для идентификации скорее всего, мошенничество.

**Как:** мошенничества с отрисовкой решена как двоичной классификации. Методику, используемую в этом шаблоне, можно легко применять для обнаружения мошенничества в других сферах.


## <a name="campaign-optimization"></a>Оптимизация кампании

[Предсказать, как и когда для связи с потенциальными клиентами](https://microsoft.github.io/r-server-campaign-optimization/)

**Что:** это решение использует insurance Отраслевые данные для прогнозирования на основе демографических данных, данные предыстории ответа и сведения о продукте интересов.  Рекомендации также создаются для указания наиболее канала и время пользователям подход для оказания влияния на поведение покупки.

**Как:** решение создает и сравниваются несколько моделей. Решение также демонстрирует автоматических интеграцию данных и Подготовка данных с помощью хранимых процедур. Параллельные приведены образцы для SQL Server в базе данных, в Azure, а HDInsight Spark. 

## <a name="health-care-predict-length-of-stay-in-hospital"></a>Здравоохранение: спрогнозировать время нахождения в больнице 

[Прогнозирование время нахождения в больницы](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1)

**Что:** точного прогнозирования пациентов, которые могут потребовать долгосрочной hospitalization является важной частью внимания и на планирование. Администраторы должны иметь возможность определить, какие средства требуется больше ресурсов и caregivers нужно гарантировать, что они могут соответствовать требованиям пациентов.

**Как:** это решение использует виртуальная машина анализа данных и включает экземпляр SQL Server с помощью машинного обучения включена. Он также включает набор отчетов Power BI, которые можно использовать для взаимодействия с развернутой модели.

## <a name="customer-churn"></a>Обработка клиента

[Шаблон прогнозирования обработка клиента (SQL Server R Services)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/Churn/Introduction.md)

**Что:** анализ и прогнозирование, обработка клиента — важный аспект любого отрасли, где необходимо обрабатываются и предотвратить потери клиентов конкурентам: банковских операций, связи и розничной торговли и др. Целью такого анализа является определение клиентов, которые с высокой вероятностью могут уйти к конкурентам, и принятие надлежащих мер для их удержания.

**Как:** этот шаблон создают проблемы обработки как **двоичной классификации** проблему. Для разделения клиентов на склонных и не склонных к уходу в нем используются образцы данных из двух источников: демографии клиентов и транзакций клиентов.
  
## <a name="predictive-maintenance"></a>профилактическое обслуживание.

[Диагностическое обслуживание шаблона (SQL Server 2016)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/PredictiveMaintenance/Introduction.md)

**Что:** превентивного обслуживания предназначен для повышения эффективности задач по обслуживанию путем записи после сбоев и использовать эти данные для прогнозирования, когда и где может произойти сбой устройства. Возможность прогноз устареванием устройства является особенно полезна в приложениях, использующих распределенных данных или датчики. Этот метод также может применяться для отслеживания или прогнозирования ошибки в устройствами IoT (Интернета вещей).

Это объявление для получения дополнительной информации см.: [новый шаблон диагностическое обслуживание](https://blogs.technet.microsoft.com/machinelearning/2015/04/09/exciting-new-templates-in-azure-ml/)

**Как:** этом решении основное внимание уделяется ответить на вопрос «Когда обслуживаемых машины не удастся?» Входные данные представляют собой имитацию показаний датчиков авиационных двигателей. Данные, полученные из мониторинг состояния текущей операции механизма, например текущего рабочего цикла, параметры и значения датчика используются для создания трех типов прогнозных моделей:

-   **Регрессионная модель**служит для прогнозирования того, сколько еще проработает двигатель до отказа. Образец модели в прогнозировании метрики «Оставшегося срока» (RUL), также называемый «Времени для Failure» (TTF).
  
-   **Модели классификации**служат для прогнозирования того, существует ли вероятность отказа двигателя.
  
    **Модель двоичной классификации** прогнозирует, если обработчик завершится ошибкой в течение определенного промежутка времени.

    **Многоклассовый классификатор модели** прогнозирует ли определенное ядро может завершиться ошибкой и предоставляет окно вероятного времени сбоя. Например, можно спрогнозировать, есть ли вероятность отказа одного из двигателей в указанный день или в какой-либо период времени после этого дня.

## <a name="energy-demand-forecasting"></a>Прогноз спроса энергии

[Запросу энергии прогнозирования шаблон с SQL Server R Services](https://gallery.cortanaintelligence.com/Tutorial/Energy-Demand-Forecast-Template-with-SQL-Server-R-Services-1)

**Что:**: прогноз спроса-важных проблем в различных доменах, включая энергии, розничной торговли и службы. Прогноз спроса точные помогает организациям проведения лучше производственного планирования выделения ресурсов и принимать другие важные бизнес-решения. В секторе энергии прогноз спроса крайне важна для уменьшая затраты на хранение энергии и балансировка спрос и предложение.

**Как:** этот шаблон использует SQL Server R Services для прогнозирования потребность электроэнергии. Модель, используемая для прогноза представляет собой модель регрессии случайного леса, на основе **rxDForest**, высокой производительности алгоритма машинного обучения включенных в Microsoft R Server. Решение включает в себя средство моделирования потребления, весь необходимый код R и T-SQL для обучения модели, а также хранимые процедуры, которые можно использовать для создания и вывода прогнозов. 


## <a name="bkmk_HowTo"></a>Использование шаблонов

Чтобы загрузить файлы, входящие в состав каждого шаблона, можно использовать команды GitHub ли перейти по ссылке и щелкнуть **Скачать ZIP-файл** , чтобы сохранить все файлы на компьютере.  Загруженное решение обычно содержит перечисленные ниже папки.
  
-   **Data**: содержит образцы данных для каждого приложения.
  
-   **R**: содержит весь необходимый код на языке R для разработки решения. Для решения требуются библиотеки, предоставляемые Microsoft R Server, но его можно открыть и отредактировать в любой интегрированной среде разработки R. Код на языке R оптимизирован так, чтобы вычисления производились в базе данных. Для этого в качестве контекста вычисления установлен экземпляр SQL Server.
  
-   **SQLR**: содержит несколько файлов SQL, которые можно выполнять в среде SQL, например [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , для создания хранимых процедур, выполняющих связанные задачи, такие как обработка данных, формирование характеристик и развертывание модели.
  
    Эта папка также содержит скрипт PowerShell, запустив который, можно вызвать все необходимые скрипты и создать комплексную среду. Отредактируйте этот скрипт в соответствии с особенностями вашей среды.

## <a name="next-steps"></a>Следующие шаги

+ [Учебники по обучения машины SQL Server](machine-learning-services-tutorials.md)




