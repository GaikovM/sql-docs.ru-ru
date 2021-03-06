---
title: Отчет о консолидированных оценки с помощью Power BI (данных помощник миграции SQL Server) | Документы Microsoft
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 0c7479a1e55d90d59fbcc289978b943a8cd3c195
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="report-on-your-consolidated-assessments-by-using-power-bi-data-migration-assistant"></a>Отчет о консолидированных оценки с помощью Power BI (данных помощник по миграции)

В этой статье описывается создание отчета Power BI для консолидированных миграции оценок.

Сведения о консолидации оценки миграции с помощью помощника по миграции данных см. [консолидировать оценки отчетов](../dma/dma-consolidatereports.md).

## <a name="sample-power-bi-reports"></a>Образцы отчетов Power BI

Примеры отчетов Power BI для оценки консолидированные миграции можно загрузить из этого [репозитории Github](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/data-migration-assistant).

Доступны следующие отчеты: 

- [Панель мониторинга](#dashboard--details)

  Включает статистику моментального снимка и детализации отчета.

- [Готовность к обновлению в локальной среде](#on-premises-upgrade-readiness--details)

  Источник данных является представление UpgradeSuccessRanking DMAReporting базы данных.  Этот отчет показывает успешность обновления процент имущество баз данных.

- [Сходные функции в локальной среде](#on-premise-feature-parity--details)

  Показывает возможность рекомендаций для целевой версии SQL Server.

- [Azure SQL DB готовности к обновлению](#azure-sql-db-upgrade-readiness--details)

  Источник данных является представление UpgradeSuccessRanking DMAReporting базы данных.  Этот отчет показывает процент успешность обновления для баз данных, оцениваемого для миграции базы данных SQL Azure.

- [Компоненты Azure баз данных SQL Server не поддерживается](#azure-sql-db-unsupported-features--details)

  Показывает возможности в существующих баз данных, не поддерживаются в базе данных SQL Azure (V12).

Вы можете изменить эти отчеты для работы со средой путем изменения источника данных в Power BI. 

1. Щелкните стрелку вниз рядом с **изменить запросы**и выберите **настройки источника данных**.

   ![Изменение меню запросов, параметры источника данных](../dma/media/DataSourceSettings.png)

1. Выберите **изменение источника...** и введите значения параметров сервера и базы данных.

   ![Изменение источника, сервера и базы данных](../dma/media/ChangeSource.png)

1. Выберите **ОК**, а затем выберите **закрыть**.

1. Обновление отчетов.

   ![Обновление отчета Power BI](../dma/media/RefreshReport.png)

### <a name="dashboard-report"></a>Панель мониторинга отчета

![Панель мониторинга отчета](../dma/media/DashboardReport.png)

На панели мониторинга отображаются сведения обо всех оценки. Срезы в левой области можно использовать для фильтрации по экземпляра или базы данных. На линейчатой диаграмме можно использовать для детализации углублением отдельные категории, чтобы увидеть, где причины проблем.

Для детализации, выберите круг с направленную вниз стрелку в правом верхнем углу диаграммы.

![Категория углубленной детализации](../dma/media/CategoryDrillDown.png)

Углубленная детализация последовательности имеет значение, как показано на следующем рисунке (под **оси**). Для изменения последовательности, перетащите столбцы в требуемом порядке.

![Визуализации оси линейчатой диаграммы](../dma/media/VisualizationsAxis.png)

В этом представлении становится еще больше возможностей, если сначала фильтрации по определенной базы данных и затем перейти к определенной категории проблемы. В следующем примере база данных HR выбран для экземпляра **SQL01** посмотреть все объекты, которые препятствуют миграции (критические изменения).

![Критические изменения в базе данных HR](../dma/media/BreakingChanges.png)

### <a name="on-premises-upgrade-readiness-report"></a>Локального обновления отчета о готовности

![Локального обновления отчета о готовности](../dma/media/OnPremisesUpgradeReadinessReport.png)

В этом отчете содержится информация о готовности базы данных находятся перейти на более поздней версии SQL Server. Данные в этом отчете поступают из dbo. UpgradeSuccessFactor\_представление локальное развертывание в базе данных DMAReporting.

Фильтрация по экземпляра и имя базы данных и использование карт оценка вверху, можно просмотреть, какие версии базы данных могут быть перенесены слишком. Например при фильтрации по базе данных AdventureWorks 2012 видно, что база данных готова для перемещения всех версий SQL Server, перечисленных в отчете. Это определяется убедитесь, что нет критических изменений для этой базы данных и уровень совместимости.

![Фактор успеха обновления для базы данных AdventureWorks](../dma/media/UpgradeSuccessFactor.png)

### <a name="on-premises-feature-parity-report"></a>Локальный компонент четности отчета

![Локальный компонент четности отчета](../dma/media/OnPremisesFeatureParityReport.png)

Этот отчет используется для выделения новых функций, которые можно использовать для базы данных в целевой версии SQL Server.

При выборе компонента в воронкообразной диаграммы данные в нижней выделяет объектов, которые зависят от компонента. В следующем примере **базы данных Stretch для экономии пространства хранения** включена функция таблицы в списке, а лучше из данного компонента.

![Функция рекомендации для базы данных Stretch](../dma/media/FeatureRecommend_StretchDatabase.png)

### <a name="azure-sql-db-upgrade-readiness-report"></a>Azure отчетов готовности к обновлению баз данных SQL Server

![Azure отчетов готовности к обновлению баз данных SQL Server](../dma/media/AzureSQLDBUpgradeReadinessReport.png)

Этот отчет показывает готовности базы данных для миграции базы данных SQL Azure V12. Данные из этого отчета поступают из dbo. Представление UpgradeSuccessRanking DMAReporting базы данных.

### <a name="azure-features-parity-report"></a>Компоненты Azure четности отчета

![Компоненты Azure четности отчета](../dma/media/AzureFeaturesParityReport.png)

Этот отчет используется для выделения *компоненты уровня экземпляра* , которые не поддерживаются в версии 12 базы данных SQL Azure.

При выборе компонента в воронкообразной диаграммы данных внизу перечислены экземпляры и компоненты базы данных, которые не поддерживаются. В следующем примере, эта функция включена: **постоянного доступа к конфигурации группы доступности не поддерживается в базе данных SQL Azure**.  

![Всегда на функции группы доступности](../dma/media/Feature_AlwaysOnAvailability.png)

 
### <a name="azure-sql-db-unsupported-features-report"></a>База данных SQL Azure не поддерживается отчет компонентов

![База данных SQL Azure не поддерживается отчет компонентов](../dma/media/AzureSQLDBUnsupportedFeaturesReport.png)

В этом отчете представлены компоненты, которые не поддерживаются для данного **базы данных** Если целевым объектом является база данных SQL Azure (V12).

С помощью фильтрации по значению имен и характеристик базы данных в воронкообразной диаграммы, можно просмотреть подробные сведения на неподдерживаемую функцию. Сведения включают данные о затронутых объектов и рекомендаций по устранению проблемы.

Например, фильтрация по базе данных DTC и **баз данных только для чтения, не могут быть обновлены**, можно просмотреть список объектов, которые затрагиваются.

![Только для чтения базы данных не может быть обновлен проблемы](../dma/media/ReadOnlyDatabases.png)

## <a name="see-also"></a>См. также:

[Общие сведения о данных помощник по миграции](../dma/dma-overview.md)

[Помощник по миграции загрузки данных](https://www.microsoft.com/download/details.aspx?id=53595)

[Загрузки Power BI](https://powerbi.microsoft.com/)
