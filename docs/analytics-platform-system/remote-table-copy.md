---
title: Копирование удаленной таблицы - Parallel Data Warehouse | Документы Microsoft
description: С помощью копирования удаленной таблицы в Analytics платформы системы Parallel Data Warehouse.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 5ed517a471368e4192ad7393a92274424d37f975
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="remote-table-copy"></a>Копирование удаленной таблицы
Описывает, как использовать функцию копирования удаленной таблицы копирование таблиц из базы данных SQL Server PDW для удаленных баз данных SMP SQL Server (отличных от устройства). Используйте копию удаленной таблицы включает звезда сценарии для SQL Server PDW.  
  
## <a name="BasicsPDE"></a>Понимать копирования удаленной таблицы для SQL Server PDW  
Копирование удаленной таблицы — это функция служб SQL Server PDW, позволяет использовать сценарии, звезда, копируя результаты SQL-инструкцию SELECT в таблицу в базе данных SMP. Копирование удаленной таблицы для инициализации [СОЗДАНИЯ УДАЛЕННОГО TABLE AS SELECT](../t-sql/statements/create-remote-table-as-select-parallel-data-warehouse.md) инструкции.  
  
## <a name="BasicsPrerequisites"></a>Требования к использованию копирования удаленной таблицы  
Таблиц копирования для копирования удаленной таблицы из SQL Server PDW в базе данных SQL Server можно использовать при выполнении следующих условий:  
  
-   Целевой базы данных должен быть экземпляр Microsoft® SQL Server®, на котором выполняется в системе Microsoft Windows®, можно подключиться к SQL Server PDW appliance, но не находится на сервере в устройстве. Удаленный экземпляр SQL Server может подключаться к сети InfiniBand с помощью SQL Server PDW или через сеть Ethernet.  
  
-   Данные для копирования должен быть доступным для выбора один допустимый SQL Server PDW [ВЫБЕРИТЕ](../t-sql/queries/select-transact-sql.md) инструкции.  
  
-   Целевой сервер не должен быть устройством. Не удается скопировать данные непосредственно из одного устройства в другую с помощью инструкции в этом разделе.  
  
-   Сервер назначения должны быть доступны для всех узлов в сети Infiniband устройства.  
  
## <a name="ConfigureRemote"></a>Настройка копирования удаленной таблицы  
Использовать копию удаленной таблицы, требуется приобрести и настроить Windows server, настройки SQL Server на сервере Windows server и настроить SQL Server PDW. Используйте следующие ссылки для выполнения этих трех этапов.  
  
1.  [Настройки Windows внешние системы для получения копии удаленной таблицы с помощью InfiniBand](configure-an-external-windows-system-to-receive-remote-table-copies-using-infiniband.md)  
  
2.  [Настройка внешних SMP SQL Server для получения копии удаленной таблицы](configure-an-external-smp-sql-server-to-receive-remote-table-copies.md)  
  
3.  [Настройка для удаленной таблицы копии SQL Server PDW](configure-sql-server-pdw-for-remote-table-copies.md)  
  
## <a name="PerformRemote"></a>Выполнить копирование удаленной таблицы  
Чтобы выполнить копирование удаленной таблицы, используйте [СОЗДАНИЯ УДАЛЕННОГО TABLE AS SELECT](../t-sql/statements/create-remote-table-as-select-parallel-data-warehouse.md) инструкции SQL. Примеры включаются в разделе CREATE REMOTE TABLE.  
  
<!-- MISSING LINKS 
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  
