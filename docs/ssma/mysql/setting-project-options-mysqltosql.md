---
title: Настройка параметров проекта (MySQLToSQL) | Документы Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
- Setting project options,configuration options
ms.assetid: 08820d88-e157-4d49-9401-38580dd7ec2d
caps.latest.revision: 12
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: dbe36fd3276ae6a5de2f5f41a8694fa9331d75a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="setting-project-options-mysqltosql"></a>Настройка параметров проекта (MySQLToSQL)
Для каждого проекта SSMA можно задать параметры на уровне проекта. Эти параметры задают преобразование объектов, процесс миграции данных и сопоставлении типов данных источника к типам данных целевого объекта.  Перед началом преобразования объектов в SQL Server или SQL Azure или переноса данных в SQL Server или SQL Azure, убедитесь, что параметры конфигурации подходят для проекта.  
  
SSMA можно настроить параметры по умолчанию для всех проектов. Эти параметры применяются к новый созданный проект. Затем можно настроить параметры для каждого проекта.  
  
## <a name="configuration-options-and-modes"></a>Параметры конфигурации и режимы  
SSMA имеет пять наборов параметров проекта.  
  
-   Сведения о проекте  
  
-   Общие (преобразования, миграции и SQL Azure)  
  
-   Синхронизация  
  
-   ГРАФИЧЕСКОГО ПОЛЬЗОВАТЕЛЬСКОГО ИНТЕРФЕЙСА  
  
-   Сопоставление типов  
  
Можно настроить параметры проекта четырьмя способами:  
  
-   По умолчанию  
  
-   Optimistic  
  
-   Полная  
  
-   Другой  
  
По умолчанию режим рекомендуется для большинства пользователей. Режим оптимистичного позволяет более текущего синтаксиса MySQL и удобен для чтения. Однако сохранение текущего синтаксиса может оказаться неточным. Если синтаксис MySQL должны быть преобразованы в эквивалентный синтаксис SQL Server или SQL Azure, полный режим выполняет наиболее полное преобразование. Результирующий код, однако могут быть трудности с чтением. В режиме "Настраиваемый" можно задать параметры.  
  
Дополнительные сведения о параметрах и применение параметров в каждом режиме см. в следующих разделах:  
  
-   [Параметры проекта &#40;преобразования&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
-   [Параметры проекта &#40;миграции&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)  
  
-   [Параметры проекта (GUI) (SSMA общее)](http://msdn.microsoft.com/en-us/cf06baf1-8714-48a3-95dc-781f6ca53693)  
  
-   [Параметры проекта &#40;сопоставление типов&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)  
  
-   [Параметры проекта &#40;синхронизации&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
-   [Параметры проекта &#40;базы данных Azure SQL&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)  
  
## <a name="setting-project-options"></a>Настройка параметров проекта  
В SSMA можно настроить параметры по умолчанию для всех проектов. Эти параметры сохраняются в файл конфигурации SSMA и применены к новый созданный проект.  
  
**Чтобы задать параметры проекта по умолчанию**  
  
1.  На **средства** меню, нажмите кнопку **параметры проекта по умолчанию**.  
  
2.  В **параметры проекта по умолчанию** диалоговое окно, используйте один из следующих процедур:  
  
    1.  Выберите тип проекта миграции, для которого требуются параметры быть просмотреть / изменить из **миграции целевой версии** раскрывающийся список, нажмите кнопку **Общие** в нижней части левой панели, а затем выберите **преобразования или миграции в SQL Azure** параметр.  
  
    2.  Выберите предопределенный режим, выберите **по умолчанию**, **Optimistic**, или **полного** из **режим** раскрывающегося списка.  
  
    3.  Чтобы задать пользовательские параметры, выберите или введите новые параметры или значения.  
  
3.  Нажмите кнопку **ОК** для сохранения настроек.  
  
Можно также настроить параметры для текущего проекта. Параметры, сохраняемых в текущем файле проекта.  
  
**Чтобы настроить параметры для текущего проекта**  
  
1.  На **средства** меню, нажмите кнопку **ProjectSettings**.  
  
2.  В **ProjectSettings** диалоговое окно, используйте один из следующих процедур:  
  
    1.  Выберите предопределенный режим, выберите **по умолчанию**, **Optimistic**, или **полного** из **режим** раскрывающегося списка.  
  
    2.  Чтобы указать настраиваемый режим, выберите **пользовательские** из **режим** раскрывающегося списка. А затем выберите параметры соответствующих проектов.  
  
3.  Нажмите кнопку **ОК** для сохранения настроек.  
  
## <a name="next-step"></a>Следующий шаг  
Следующий шаг в процессе переноса, зависит от требований проекта:  
  
-   Настройка сопоставления исходных и целевых типов данных, в разделе [MySQL сопоставления и типов данных SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   В противном случае можно преобразовать определения объектов базы данных MySQL в SQL Server или SQL Azure определений объектов. Дополнительные сведения см. в разделе [преобразование баз данных MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>См. также  
[Сопоставление типов данных SQL Server, а MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
