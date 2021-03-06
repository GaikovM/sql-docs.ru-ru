---
title: Занятие 2 Импорт данных SQL Server с помощью PowerShell | Документы Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 6dbd1af581865d44c1636a6bac5acc02ae68532b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="lesson-2-import-data-to-sql-server-using-powershell"></a>Занятие 2: Импорт данных в SQL Server с помощью PowerShell
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

В этой статье является частью учебника для разработчиков SQL по использованию R в SQL Server.

В этом шаге вы выполните один из скриптов, чтобы создать объекты базы данных, необходимые для работы с этим пошаговым руководством. Скрипт также создает большинство хранимых процедур, которые вы будете использовать, и добавляет образец данных в указанную таблицу базы данных.

## <a name="run-the-scripts-to-create-sql-objects"></a>Выполнить скрипты для создания объектов SQL

Среди загруженные файлы вы увидите сценарий PowerShell, можно выполнить для подготовки среды к выполнению пошагового руководства. Он выполняет следующие действия:

- Устанавливает SQL Native Client и программы командной строки SQL, если они еще не установлены. Эти программы необходимы для массовой загрузки данных в базу данных с помощью **bcp**.

- Создает базу данных и таблицу в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , а также выполняет массовую вставку данных в таблицу.

- Создает несколько функций и хранимых процедур SQL.

### <a name="run-the-script"></a>Запустите сценарий

1.  Откройте командную строку PowerShell с правами администратора и выполните приведенную ниже команду.
  
    ```ps
    .\RunSQL_SQL_Walkthrough.ps1
    ```
  
    Появится запрос на ввод следующих сведений:
  
    - имя или адрес экземпляра [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , где установлены службы [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] ;
  
    - имя пользователя и пароль учетной записи в экземпляре; Учетная запись должна иметь разрешения на создание баз данных, создание таблиц и хранимых процедур и передавать данные в таблицах. Если не указано имя пользователя и пароль для входа в SQL Server используется удостоверения Windows.
  
    - путь к скачанному файлу с образцом данных и его имя. Например:
  
        `C:\tempRSQL\nyctaxi1pct.csv`
  
2.  На этом этапе также необходимо изменить все скрипты [!INCLUDE[tsql](../../includes/tsql-md.md)] , заменив заполнители на соответствующие имена базы данных и пользователя.
  
    Изучите хранимые процедуры и функции, созданные скриптом.
  
    |**Имя файла скрипта SQL**|**Функция**|
    |-|-|
    |create-db-tb-upload-data.sql|Создает базу данных и две таблицы:<br /><br />nyctaxi_sample: содержит основной набор данных по работе такси в Нью-Йорке. К таблице добавляется кластеризованный индекс columnstore для оптимизации хранения данных и производительности запросов. В таблицу будет вставлена выборка, содержащая 1 % от общего объема набора данных по работе такси в Нью-Йорке.<br /><br />nyc_taxi_models: используется для сохранения обученной модели расширенной аналитики.|
    |fnCalculateDistance.sql|Создает скалярную функцию, которая вычисляет прямое расстояние между местами посадки и высадки.|
    |fnEngineerFeatures.sql|Создает табличную функцию, которая формирует новые характеристики данных для обучения модели.|
    |PersistModel.sql|Создает хранимую процедуру, которую можно вызывать для сохранения модели. Хранимая процедура принимает модель, сериализованную как тип данных varbinary, и записывает ее в указанную таблицу.|
    |PredictTipBatchMode.sql|Создает хранимую процедуру, которая вызывает обученную модель для составления прогнозов. Хранимая процедура принимает запрос в качестве входного параметра и возвращает столбец числовых значений, представляющих оценки для входных строк.|
    |PredictTipSingleMode.sql|Создает хранимую процедуру, которая вызывает обученную модель для составления прогнозов. Эта хранимая процедура принимает новое наблюдение в качестве входных данных, причем отдельные значения характеристик передаются как встроенные параметры, и возвращает значение, представляющее прогнозируемый результат для нового наблюдения.|
  
    В последней части этого пошагового руководства вы создадите ряд дополнительных хранимых процедур.
  
    |**Имя файла скрипта SQL**|**Функция**|
    |------|------|
    |PlotHistogram.sql|Создает хранимую процедуру для изучения данных. Эта хранимая процедура вызывает функцию R для построения гистограммы на основе переменной, а затем возвращает диаграмму в виде двоичного объекта.|
    |PlotInOutputFiles.sql|Создает хранимую процедуру для изучения данных. Эта хранимая процедура создает график с помощью функции R, а затем сохраняет результат в виде локального файла PDF.|
    |TrainTipPredictionModel.sql|Создает хранимую процедуру, которая обучает модель логистической регрессии, вызывая пакет R. Модель прогнозирует значение для столбца tipped и обучается на основе случайной выборки, содержащей 70 % данных. Выходными данными хранимой процедуры является обученная модель, которая сохраняется в таблице nyc_taxi_models.|
  
3.  Войдите в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] и указанного вами имени входа, чтобы убедиться в том, что в нем отображаются созданные база данных, таблицы, функции и хранимые процедуры.
  
    ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")
  
    > [!NOTE]
    > Если объекты базы данных уже существуют, их нельзя создать еще раз.
    >   
    > Если таблица уже существует, данные будут добавляться в нее, а не перезаписываться. Поэтому перед выполнением скрипта удалите все существующие объекты.

## <a name="next-lesson"></a>Следующее занятие

[Занятие 3: Анализ и визуализация данных](../tutorials/sqldev-explore-and-visualize-the-data.md)

## <a name="previous-lesson"></a>Предыдущее занятие

[Урок 1: Загрузите образец данных](../tutorials/sqldev-download-the-sample-data.md)
