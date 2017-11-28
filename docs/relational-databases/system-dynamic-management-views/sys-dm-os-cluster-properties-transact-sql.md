---
title: "sys.dm_os_cluster_properties (Transact-SQL) | Документы Microsoft"
ms.custom: 
ms.date: 08/09/2016
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: dmv's
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs: TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
caps.latest.revision: "17"
author: JennieHubbard
ms.author: jhubbard
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: ace074d1b6a517e92fbbb952f6d07c33cb5e7a18
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
---
# <a name="sysdmosclusterproperties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Возвращает одну строку с текущими настройками для свойств ресурса кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], описанных в этом разделе. При запуске этого представления на изолированном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] никакие данные не возвращаются.  
  
 Данные свойства используются для задания значений, которые влияют на обнаружение отказов, время реакции на отказ и ведение журнала для мониторинга состояния работоспособности экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

|Имя столбца|Свойство|Description|  
|-----------------|--------------|-----------------|  
|VerboseLogging|bigint|Уровень ведения журнала для отказоустойчивого кластера SQL Server. Для записи дополнительных сведений в журналы ошибок с целью последующего устранения неполадок можно включить подробный уровень ведения журнала. Одно из следующих значений:<br /><br /> 0 — ведение журнала отключено (по умолчанию)<br /><br /> 1 — только ошибки<br /><br /> 2 — ошибки и предупреждения<br /><br /> Дополнительные сведения см. в разделе [ALTER SERVER CONFIGURATION &#40; Transact-SQL &#41; ](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|bigint|Флаги дампа SQLDumper определяют тип файлов дампа, созданных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Значение по умолчанию — 0.|  
|SqlDumperDumpPath|nvarchar(260)|Место, в котором программа SQLDumper создает файлы дампа.|  
|SqlDumperDumpTimeOut|bigint|Значение тайм-аута в миллисекундах, отведенное программе SQLDumper для создания дампа в случае возникновения сбоя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Значение по умолчанию — 0.|  
|FailureConditionLevel|bigint|Задает условия, при которых отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен отказать или перезапуститься. Значение по умолчанию равно 3. Подробное описание или изменить значения свойств, см. в [Настройка параметров свойства FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|bigint|Время, в течение которого библиотека ресурсов компонента SQL Server Database Engine будет ждать сведений о состоянии сервера, прежде чем сервер переводится в категорию неотвечающих. Время ожидания указывается в миллисекундах. По умолчанию — 60 000. Дополнительные сведения или изменить значение этого свойства, в разделе [Configure HealthCheckTimeout Property Settings](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Permissions  
 Требует разрешения VIEW SERVER STATE на экземпляре отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано использование sys.dm_os_cluster_properties для возвращения настроек свойств ресурса отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 Ниже приводится образец результирующего набора.  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60000|  
  
  