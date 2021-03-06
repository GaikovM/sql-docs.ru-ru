---
title: Экспорт инвентаризацию доступа (AccessToSQL) | Документы Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Access databases
- Access databases, exporting metadata
- exporting
- exporting Access metadata
- exporting, Access metadata
- exporting, querying exported metadata
- inventories of Access databases
- querying exported metadata
ms.assetid: 7e1941fb-3d14-4265-aff6-c77a4026d0ed
caps.latest.revision: 18
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 7d0877666e13f490463e77ec30bf7792367747d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="exporting-an-access-inventory-accesstosql"></a>Экспорт инвентаризацию доступа (AccessToSQL)
Если у вас есть несколько баз данных Access, и вы не уверены, какие из них для переноса в [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], вы можете экспортировать данные инвентаризации всех баз данных Access в проекте. Можно просмотреть и запросов к метаданным инвентаризации, чтобы определить, какие базы данных и объекты внутри них для переноса. Это инвентаризации позволяет быстро найти ответы на вопросы, например следующие:  
  
-   Что такое крупных баз данных  
  
-   Кто будет владеть большинство баз данных?  
  
-   Какие базы данных содержит те же таблицы?  
  
-   Какие базы данных не были изменены за последние шесть месяцев?  
  
-   Какие базы данных содержат конфиденциальные сведения?  
  
Примеры запросов, что позволяет ответить на эти вопросы приведены в конце этого раздела.  
  
## <a name="exported-metadata"></a>Экспортированные метаданные  
SSMA экспортирует метаданные о доступа баз данных, таблиц, столбцов, индексов, внешние ключи, запросы, отчеты, формы, макросы и модули. Метаданные о каждом из этих категорий элементов экспортируются в отдельной таблице. Схемы этих таблиц в разделе [схемы инвентаризации доступа](http://msdn.microsoft.com/en-us/fdd3cff2-4d62-4395-8acf-71ea8f17f524).  
  
## <a name="exporting-inventory-data"></a>Экспорт данных инвентаризации  
Чтобы экспортировать данные инвентаризации доступа, необходимо сначала откройте или создайте проект SSMA и добавьте базы данных Access, который требуется проанализировать. После добавления базы данных к проекту SSMA, экспортировать метаданные о таких баз данных с заданным [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] базы данных и схемы. При необходимости SSMA создает таблицы для хранения метаданных. SSMA добавляет метаданные базы данных Access для [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] базы данных.  
  
> [!NOTE]  
> В базе данных Access можно разбить на несколько файлов: серверную базу данных, содержащий таблицы и интерфейсные базы данных, содержащие запросы, формы, отчеты, макросы, модули и сочетания клавиш. Если требуется выполнить миграцию разбиение базы данных до [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], добавление SSMA клиентской базы данных.  
  
Следующие инструкции описывают процесс создания проекта, добавьте в проект базы данных, подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], а затем экспортировать данные инвентаризации.  
  
**Создание проекта**  
  
1.  Откройте SSMA для Access.  
  
2.  В меню **Файл** выберите пункт **Создать проект**.  
  
    Откроется диалоговое окно **Создание проекта** .  
  
3.  В поле **Name** введите имя проекта.  
  
4.  В **расположение** введите или выберите папку для проекта.  
  
5.  В **Миграция в** поле со списком выберите целевой версии, к которой вы хотите перенести и нажмите кнопку **ОК**.  
  
Дополнительные сведения о создании проектов см. в разделе [Создание и управление проектами](http://msdn.microsoft.com/en-us/f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7).  
  
**Чтобы найти и добавить базы данных**  
  
1.  На **файл** меню, нажмите кнопку **найти баз данных**.  
  
2.  В мастере найти базы данных введите диска, путь к файлу или UNC-путь, который требуется найти. Можно также щелкнуть **Обзор** выберите диск или в сетевую папку.  
  
3.  Нажмите кнопку **добавить** добавьте каталог в список.  
  
    Повторите предыдущие два шага, чтобы добавить дополнительные размещения.  
  
4.  При необходимости добавьте условия поиска для уточнения списка баз данных, которые возвращаются.  
  
    > [!IMPORTANT]  
    > **Полностью или частично имя файла** текстовое поле не поддерживает подстановочные знаки.  
  
5.  Нажмите кнопку **сканирования**.  
  
    Появится страница сканирования. Это показывает, что базы данных, которые были найдены и выполняется поиск. Чтобы остановить поиск, нажмите кнопку **остановить**.  
  
6.  На странице «Выбор файлов» выберите каждой базы данных, который требуется добавить в проект.  
  
    Можно использовать **выделить все** и **очистить все** кнопок в верхней части списка, чтобы выбрать или очистить все базы данных. Можно также удерживая нажатой клавишу CTRL, чтобы выбрать несколько строк или удерживайте клавишу SHIFT, вниз, чтобы выбрать диапазон строк.  
  
7.  Нажмите кнопку **Далее**.  
  
8.  На странице «Проверка» щелкните **Готово**.  
  
Дополнительные сведения о добавлении базы данных в проектах см. в разделе [Добавление и удаление файлов баз данных Access](http://msdn.microsoft.com/en-us/e944c740-4c8a-4bc1-b0ed-be57bc06dced).  
  
**Для подключения к SQL Server**  
  
1.  На **файл** последовательно выберите пункты **подключение к SQL Server**.  
  
2.  В диалоговом окне соединения, введите или выберите имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
    -   При подключении к экземпляру по умолчанию на локальном компьютере, можно ввести **localhost** или точку (**.**).  
  
    -   При подключении к экземпляру по умолчанию на другом компьютере, введите имя компьютера.  
  
    -   При подключении к именованному экземпляру введите имя компьютера, обратную косую черту и имя экземпляра. Например: Сервер\экземпляр.  
  
3.  В **базы данных** введите имя целевой базы данных для экспортированных метаданных.  
  
4.  Если ваш экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] настроен на прием подключений через порт не по умолчанию, введите номер порта, используемый для [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] подключения в **порт сервера** поле. Для экземпляра по умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], по умолчанию используется порт 1433. Для именованных экземпляров SSMA пытается получить номер порта из [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] служба браузера.  
  
5.  В **проверки подлинности** раскрывающееся меню, выберите тип проверки подлинности, используемый для соединения. Чтобы использовать текущую учетную запись Windows, выберите **проверки подлинности Windows**. Для использования [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] входа в систему, выберите **проверки подлинности SQL Server**, а затем укажите имя пользователя и пароль.  
  
Дополнительные сведения о подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], в разделе [подключение к SQL Server &#40;AccessToSQL&#41;](../../ssma/access/connecting-to-sql-server-accesstosql.md).  
  
**Чтобы экспортировать данные инвентаризации**  
  
1.  В обозревателе метаданных доступа разверните **доступа к метабазе**.  
  
2.  Установите флажок рядом с **баз данных**.  
  
    Чтобы исключить отдельные базы данных или объекты базы данных, разверните **баз данных** папки, а затем снимите флажок рядом с именем базы данных или объекта базы данных.  
  
3.  Щелкните правой кнопкой мыши **баз данных** и выберите **Экспорт схемы**.  
  
4.  В **Выбор схемы для экспорта** диалоговое окно, выберите Конечная схема для экспортированных метаданных и нажмите кнопку **ОК**.  
  
Каждый раз при экспорте метаданных, SSMA добавляет данные с данными инвентаризации. Существующие данные инвентаризации не обновляется или удаляется.  
  
## <a name="querying-the-exported-metadata"></a>Запрос экспортированных метаданных  
После экспорта метаданные базы данных Access, можно запросить метаданные. Следующие инструкции описывают использование окна редактора запросов в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] для выполнения запросов.  
  
**Для запроса метаданных**  
  
1.  Из **запустить** последовательно выберите пункты **все программы**, пункты **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005** или **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008** или **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012**и нажмите кнопку **SQL Server Management Studio**.  
  
2.  В **соединение с сервером** диалоговое окно, проверьте параметры и нажмите кнопку **Connect**.  
  
3.  На панели инструментов среды Management Studio щелкните **новый запрос** для открытия редактора запросов.  
  
4.  В окне редактора запросов введите запрос. В следующем разделе приведены некоторые примеры.  
  
5.  Нажмите клавишу F5, чтобы выполнить запрос.  
  
## <a name="query-examples"></a>Примеры запросов  
Перед запуском следующих запросов необходимо запустить инструкцию USE *имя_базы_данных* запрос, чтобы убедиться в выполнении запросов к базе данных, содержащий экспортированные метаданные. Например, при экспорте метаданных в базе данных с именем MyAccessMetadata, необходимо добавить следующие в начале [!INCLUDE[tsql](../../includes/tsql_md.md)] кода:  
  
```  
USE MyAccessMetadata;  
GO  
```  
В следующих примерах используется **dbo** схемы. При экспорте метаданных в другой схеме, убедитесь, что изменения схемы при выполнении этих запросов.  
  
### <a name="what-tables-and-columns-are-in-these-databases"></a>Какие таблицы и столбцы, в этих базах данных?  
Следующий запрос выполняет соединение таблиц, содержащих метаданных базы данных, таблицы и столбца и затем возвращает имена всех баз данных, таблицы и столбцы, отсортированные по имени столбца:  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D.DatabaseId  
ORDER BY ColumnName;  
```  
  
### <a name="what-are-the-largest-databases"></a>Что такое крупных баз данных  
Следующий запрос возвращает имя базы данных, размер файла и количество таблиц в каждой базе данных Access, отсортированных по размеру файла:  
  
```  
SELECT DatabaseName, FileSize, TablesCount  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileSize DESC;  
```  
  
### <a name="who-is-the-owner-of-most-of-the-databases"></a>Кто является владельцем большинство баз данных?  
Следующий запрос возвращает имя базы данных и владельца каждой базы данных Access, отсортированных по владельцу.  
  
```  
SELECT DatabaseName, FileOwner  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileOwner;  
```  
  
### <a name="which-databases-contain-the-same-tables"></a>Какие базы данных содержит те же таблицы?  
Следующий запрос использует вложенный запрос, чтобы найти все имена таблиц, которые появляются более одного раза в списке таблиц, а затем использует этот список таблиц для получения имени базы данных. Результаты возвращаются в виде имени базы данных, а затем имя таблицы и сортируются по имени таблицы.  
  
```  
SELECT DatabaseName, TableName   
FROM dbo.SSMA_Access_InventoryTables T  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON D.DatabaseId = T.DatabaseId  
WHERE TableName IN (  
SELECT TableName   
FROM dbo.SSMA_Access_InventoryTables  
GROUP BY TableName   
HAVING count(*)>1  
)  
ORDER BY TableName;  
```  
  
### <a name="which-databases-were-not-modified-in-the-last-six-months"></a>Какие базы данных не были изменены за последние шесть месяцев?  
Следующий запрос возвращает текущую дату, возвращает значение месяца для шесть месяцев назад и возвращает список баз данных, у которых дата изменения более чем на 6 месяцев.  
  
```  
SELECT DatabaseName, DateModified  
FROM dbo.SSMA_Access_InventoryDatabases  
WHERE DATEDIFF(month, DateModified, GETDATE()) > 6  
ORDER BY DateModified;  
```  
  
### <a name="which-databases-contain-private-information"></a>Какие базы данных содержат конфиденциальные сведения?  
Базы данных Access может содержать конфиденциальные или личные сведения. Может потребоваться переместить эти базы данных для [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Чтобы воспользоваться функциями безопасности. Если вы знаете, что столбцы, содержащие конфиденциальные данные с определенным именем, или содержат символы, определенные, можно использовать запрос для поиска столбцов, содержащих эти сведения. Например можно найти все столбцы, которые содержат строку «Зарплата».  Запрос возвращает имя базы данных, имя таблицы и имя столбца.  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D. DatabaseId  
WHERE ColumnName LIKE '%salary%';  
```  
Если вы не знаете имя столбца, можно написать запрос, возвращающий все столбцы. Чтобы сделать это, удалите предложение WHERE из предыдущего запроса.  
  
## <a name="see-also"></a>См. также  
[Подготовка к миграции базы данных Access](http://msdn.microsoft.com/en-us/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)  
  
