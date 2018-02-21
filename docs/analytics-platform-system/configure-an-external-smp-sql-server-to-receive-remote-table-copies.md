---
title: "Настройка внешних SMP SQL Server для получения копии удаленной таблицы (PDW)"
author: barbkess
ms.author: barbkess
manager: jhubbard
ms.prod: analytics-platform-system
ms.prod_service: mpp-data-warehouse
ms.service: 
ms.component: 
ms.technology: mpp-data-warehouse
ms.custom: 
ms.date: 01/13/2017
ms.reviewer: na
ms.suite: sql
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bbd2ed6-064e-4b45-b67b-608dc0f2b2bc
caps.latest.revision: "13"
ms.openlocfilehash: 18b61d60e8ca771feab84b24a9ff53cc7bdfc193
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-external-smp-sql-server-to-receive-remote-table-copies"></a>Настройка внешних SMP SQL Server для получения копии удаленной таблицы
Описывает, как настроить внешние экземпляр SQL Server для получения копии удаленной таблицы из SQL Server PDW.  
  
В этом разделе описывается один из шагов конфигурации для настройки копирования удаленной таблицы. Список всех шагов см. в разделе [удаленной копии таблицы](remote-table-copy.md).  
  
## <a name="before-you-begin"></a>Перед началом  
Перед тем как настроить внешние SQL Server, необходимо выполнить следующее:  
  
-   С SQL Server 2008 Enterprise Edition или более поздней версии, все готово для установки или уже установлена система Windows. Система Windows должна быть настроена согласно инструкциям в разделе [настроить внешние Windows системы для получения удаленной таблицы копии с помощью InfiniBand](configure-an-external-windows-system-to-receive-remote-table-copies-using-infiniband.md).  
  
-   Администратор учетной записи Windows с возможностью настройки в экземпляре SQL Server и системы Windows.  
  
-   SQL Server учетная запись входа (если он еще не установлен SQL Server) с возможностью создания имен входа и предоставить разрешения на базы данных назначения.  
  
## <a name="HowToSQLServer"></a>Настройка внешних SMP SQL Server для получения копии удаленной таблицы  
Функция копирования удаленной таблицы копирует таблицы из SQL Server PDW appliance к внешней базе данных SMP SQL Server, на котором выполняется в системе Windows. После настройки внешней системы Windows для получения копии удаленной таблицы, следующий шаг — Установка и настройка SQL Server в системе Windows.  
  
Чтобы настроить SQL Server, выполните следующие действия:  
  
1.  Установите SQL Server 2008 Enterprise Edition или более поздней версии в системе Windows. Это называется SMP SQL Server.  
  
2.  Настройка SQL Server на прием подключений TCP/IP фиксированного порта TCP. Эта конфигурация по умолчанию отключен и необходимо включить, чтобы разрешить SQL Server PDW для подключения к SMP SQL Server.  
  
3.  Отключите брандмауэр Windows или настройте SMP SQL Server, TCP-порт, чтобы она будет работать при включенном брандмауэре Windows.  
  
4.  Настройка SQL Server, чтобы разрешить режим проверки подлинности SQL Server. Параллельных данных экспортировать всегда использует учетные записи SQL Server для проверки подлинности.  
  
5.  Определите учетную запись SQL Server на сервере SQL Server SMP, который будет использоваться для проверки подлинности. Предоставьте этой учетной записи право доступа для создания, удаления и вставки данных в таблицы в целевой базе данных для операции экспорта параллельных данных.  
  
## <a name="BPSQLConfig"></a>Советы и рекомендации по конфигурации SMP SQL Server для копирования удаленной таблицы  
При настройке SMP SQL Server для получения копии удаленной таблицы, используйте следующие рекомендации для повышения производительности.  
  
1.  Следуйте рекомендациям, как описано в документации по продукту SQL Server. Например включите шифрование данных. Дополнительные сведения об обеспечении безопасности SQL Server см. в разделе [обеспечение безопасности SQL Server](../relational-databases/security/securing-sql-server.md) на сайте MSDN.  
  
2.  Используйте модель восстановления с неполным протоколированием или простая.  
  
    Во время параллельной данных операций экспорта, данные массовой вставке в только что созданную целевую таблицу. Для достижения максимальной производительности во время операции массовой вставки значение целевой базы данных для использования с неполным протоколированием или простая модель восстановления.  
  
3.  Используйте параметр batch_size для освобождения места в журнале.  
  
    Несмотря на то, что восстановления с неполным протоколированием или простая модели использующие минимального протоколирования для массовой вставки данных, по-прежнему происходит некоторые ведения журнала. Чтобы предотвратить слишком большого файлы журнала, используйте параметр batch_size SQL Server периодически освободить место в журнале.  
  
<!-- MISSING LINKS 
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  