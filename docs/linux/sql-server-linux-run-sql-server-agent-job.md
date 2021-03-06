---
title: Создание и запуск задания для SQL Server в Linux | Документы Microsoft
description: Этот учебник показывает, как для запуска задания агента SQL Server в Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 02/20/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: 162015772bb54023816fcc7d911ca34fbd4a3ac7
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2018
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>Создание и запуск задания агента SQL Server в Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Задания SQL Server используются для выполнения регулярных та же последовательность команд в базе данных SQL Server. Этот учебник пример того, как создать задание агента SQL Server в Linux с помощью Transact-SQL и SQL Server Management Studio (SSMS).

> [!div class="checklist"]
> * Установка агента SQL Server в Linux
> * Создать новое задание для создания ежедневных резервных копий базы данных
> * Планирование и запуск задания
> * Повторите эти действия в среде SSMS (необязательно)

Известные проблемы с агентом SQL Server в Linux см. в разделе [заметки о выпуске](sql-server-linux-release-notes.md).

## <a name="prerequisites"></a>предварительные требования

С этим учебником требуются следующие необходимые компоненты:

* Компьютер Linux со следующими компонентами:
  * 2017 г. SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md), или [Ubuntu](quickstart-install-connect-ubuntu.md)) с помощью средства командной строки.

Следующие компоненты являются необязательными.

* Компьютер Windows с помощью SSMS:
  * [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) для дополнительных шагов SSMS.

## <a name="enable-sql-server-agent"></a>Включить агент SQL Server

Для использования агента SQL Server в Linux, необходимо сначала включить агент SQL Server на компьютере, на котором уже установлен 2017 г. SQL Server.

1. Чтобы включить агент SQL Server, выполните следующие шаги.
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. Перезапустите SQL Server с помощью следующей команды:
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> Начиная с SQL Server CU4 2017 г., агент SQL Server входит в состав **mssql server** пакета и отключена по умолчанию. Для настройки агента до посещения CU4 [установки агента SQL Server в Linux](sql-server-linux-setup-sql-agent.md).

## <a name="create-a-sample-database"></a>Создание образца базы данных

Выполните следующие действия для создания образца базы данных с именем **SampleDB**. Эта база данных используется для ежедневного задания резервного копирования.

1. На компьютере Linux откройте сеанс терминала bash.

1. Используйте **sqlcmd** запустите Transact-SQL **CREATE DATABASE** команды.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. Убедитесь, что база данных создается путем перечисления баз данных на сервере.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>Создание задания с помощью Transact-SQL

Следующие шаги создания задания агента SQL Server в Linux с помощью команды Transact-SQL. Задание выполняется ежедневной резервной копии образца базы данных **SampleDB**.

> [!TIP]
> Можно использовать любой клиент T-SQL для выполнения этих команд. Например, в Linux можно использовать [sqlcmd](sql-server-linux-setup-tools.md) или [кода Visual Studio](sql-server-linux-develop-use-vscode.md). С удаленного сервера Windows можно выполнять запросы в SQL Server Management Studio (SSMS) или использовать пользовательский интерфейс для управления заданиями, как описано в следующем разделе.

1. Используйте [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) Создание задания с именем `Daily SampleDB Backup`.

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. Вызовите [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) Создание шага задания, которое создает резервную копию `SampleDB` базы данных.

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. Затем создайте ежедневного расписания для задания с [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md).

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. Присоединить расписание задания к заданию с [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md).

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. Используйте [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) для назначения задания на целевом сервере. В этом примере целью является локальный сервер.

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. Запустить задание с [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>Создание задания с помощью SSMS

Можно также создать и управлять заданиями удаленно с помощью SQL Server Management Studio (SSMS) в Windows.

1. Запустите SSMS в Windows и подключитесь к экземпляру SQL Server для Linux. Дополнительные сведения см. в разделе [управления SQL Server в Linux с помощью SSMS](sql-server-linux-manage-ssms.md).

1. Убедитесь, что вы создали образец базы данных с именем **SampleDB**.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. Убедитесь, что агент SQL выполнена [установлен](sql-server-linux-setup-sql-agent.md) и правильно настроен. Найдите знак «плюс» рядом с агентом SQL Server в обозревателе объектов. Если агент SQL Server не включена, попробуйте перезагрузить **mssql server** службы в Linux.

   ![Убедитесь, что был установлен агент SQL Server](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. Создайте новое задание.

   ![Создать новое задание](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. Имя задания и создайте вашего шаг задания.

   ![Создание шага задания](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. Укажите, какие подсистемы, которые вы хотите использовать, и действия необходимо выполнить шаг задания.

   ![Задание подсистемы](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![Действие шага задания](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. Создайте новое расписание задания.

   ![расписание заданий;](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![расписание заданий;](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. Запустите задание.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>Следующие шаги

В этом учебнике вы узнали, как:

> [!div class="checklist"]
> * Установка агента SQL Server в Linux
> * Использование Transact-SQL и системные хранимые процедуры для создания заданий
> * Создать задание, выполняющее ежедневное резервное копирование базы данных
> * Использовать пользовательский Интерфейс SSMS для создания и управления заданиями

Далее изучение других возможностей для создания и управления заданиями.

> [!div class="nextstepaction"]
>[Документация по SQL Server, агент](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent)
