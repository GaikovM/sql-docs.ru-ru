---
title: "Инструкции BULK INSERT (Transact-SQL) | Документы Microsoft"
ms.custom: 
ms.date: 01/04/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- BULK_TSQL
- BULK INSERT
- BULK_INSERT_TSQL
- BULK
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
caps.latest.revision: 153
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: 80b16fd446ff72c6a673a576d9a8deb9514be8b2
ms.contentlocale: ru-ru
ms.lasthandoff: 09/01/2017

---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Выполняет импорт файла данных в таблицу или представление базы данных в формате, указанном пользователем, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
BULK INSERT   
   [ database_name . [ schema_name ] . | schema_name . ] [ table_name | view_name ]   
      FROM 'data_file'   
     [ WITH   
    (   
   [ [ , ] BATCHSIZE = batch_size ]   
   [ [ , ] CHECK_CONSTRAINTS ]   
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]   
   [ [ , ] DATAFILETYPE =   
      { 'char' | 'native'| 'widechar' | 'widenative' } ]   
   [ [ , ] DATASOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATASOURCE = 'data_source_name' ]   
   [ [ , ] FIRSTROW = first_row ]   
   [ [ , ] FIRE_TRIGGERS ]   
   [ [ , ] FORMATFILE_DATASOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]   
   [ [ , ] KEEPNULLS ]   
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]   
   [ [ , ] LASTROW = last_row ]   
   [ [ , ] MAXERRORS = max_errors ]   
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]   
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]   
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]   
   [ [ , ] TABLOCK ]   

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]   
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]   
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]   
    )]   
```  
  
## <a name="arguments"></a>Аргументы  
 *database_name*  
 Имя базы данных, в которой находится указанная таблица или представление. Если не указано, предполагается текущая база данных.  
  
 *schema_name*  
 Имя схемы таблицы или представления. *schema_name* необязательно, если схемой по умолчанию для пользователя, выполняющего операцию массового импорта, является схема указанной таблицы или представления. Если *схемы* не указан и схема по умолчанию пользователя, выполняющего операцию массового импорта, отличается от указанной таблицы или представления, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке и операция массового импорта будет отменена.  
  
 *имя_таблицы*  
 Имя таблицы или представления, куда производится массовый импорт данных. Могут указываться только те представления, в которых все столбцы относятся к одной и той же базовой таблице. Дополнительные сведения об ограничениях при загрузке данных в представления см. в разделе [Вставить &#40; Transact-SQL &#41; ](../../t-sql/statements/insert-transact-sql.md).  
  
 **"** *data_file* **"**  
 Полный путь и имя файла данных, который содержит импортируемые в указанную таблицу или представление данные. Инструкция BULK INSERT может импортировать данные с диска (сетевого, гибкого, жесткого и т. д.).   
 
 *data_file* необходимо указать допустимый путь на сервере, на котором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запущена. Если *data_file* является удаленным файлом, указывайте имя соглашения об универсальных именах (UNC). Имя UNC имеет форму \\ \\ *имя_системы*\\*ShareName*\\*путь* \\ *FileName*. Например, `\\SystemX\DiskZ\Sales\update.txt`.   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.   
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP1.1, data_file могут находиться в хранилище больших двоичных объектов.

**"** *имя_источника_данных* **"**   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.   
Именованный внешний источник данных указывает на место хранения больших двоичных объектов Azure файла, который будет импортирован. Внешний источник данных должен быть создан с помощью `TYPE = BLOB_STORAGE` , который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).    
  
 BATCHSIZE  **=**  *batch_size*  
 Указывает число строк в одном пакете. Каждый пакет копируется на сервер за одну транзакцию. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] фиксирует или откатывает транзакцию для каждого из пакетов. По умолчанию, все данные, содержащиеся в файле, передаются одним пакетом. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.  
  
 CHECK_CONSTRAINTS  
 Указывает, что при выполнении операции массового импорта будет выполняться проверка всех ограничений целевой таблицы или представления. Без параметра CHECK_CONSTRAINTS все ограничения CHECK и FOREIGN KEY пропускаются, и после завершения операции ограничение таблицы помечается как ненадежное.  
  
> [!NOTE]  
>  Ограничения UNIQUE и PRIMARY KEY проверяются в любом случае. При импортировании в символьный столбец, имеющий ограничение NOT NULL, команда BULK INSERT вставляет пустую строку, если в текстовом файле нет значения.  
  
 Рано или поздно необходимо проверять всю таблицу на соответствие ограничениям. Если таблица перед началом операции массового импорта была не пуста, затраты на повторную проверку ограничений могут превысить затраты на применение ограничений CHECK к добавочным данным.  
  
 Отключение проверки ограничений (настройка по умолчанию) может потребоваться в тех ситуациях, когда входные данные содержат строки, нарушающие эти ограничения. Можно выполнить импорт данных при отключенной проверке ограничений CHECK, а затем при помощи инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] удалить недопустимые данные.  
  
> [!NOTE]  
>  Параметр MAXERRORS не влияет на проверку ограничений.  
  
 Кодовая страница  **=**  { **"**ACP**"** | **"**OEM**"**  |  **"**RAW**"** | **"***code_page***"** }  
 Указывает кодовую страницу данных в файле данных. Кодовая страница является применимо только в том случае, если данные содержат **char**, **varchar**, или **текст** столбцов с символьными значениями более **127** или меньше чем **32**.  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)]рекомендует указывать имя параметров сортировки для каждого столбца в [файла форматирования](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  
  
|Значение аргумента CODEPAGE|Description|  
|--------------------|-----------------|  
|ACP|Столбцы **char**, **varchar**, или **текст** данные преобразуются из [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)] / [!INCLUDE[msCoName](../../includes/msconame-md.md)] кодовую страницу Windows (ISO 1252) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] кодовую страницу.|  
|OEM (по умолчанию)|Столбцы **char**, **varchar**, или **текст** преобразуются из кодовой страницы OEM системы для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] кодовую страницу.|  
|RAW|Преобразование кодовой страницы не производится. Это самый быстрый режим.|  
|*code_page*|Номер кодовой страницы, например 850.<br /><br /> **\*\*Важные \* \***  версии, предшествующие [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] не поддерживают кодовую страницу 65001 (кодировка UTF-8).|  
  
 DATAFILETYPE  **=**  { **«char»** | **«native»** | **'widechar'**  |  **«widenative»** }  
 Указывает, что инструкция BULK INSERT выполняет импорт из файла определенного типа.  
  
|Значение DATAFILETYPE|Представление данных|  
|------------------------|------------------------------|  
|**char** (по умолчанию)|В символьном формате.<br /><br /> Дополнительные сведения см. в разделе [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|  
|**собственный**|В собственных типах базы данных. Создайте файл данных собственных типов путем массового импорта данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью **bcp** программы.<br /><br /> Значение собственного типа обеспечивает более высокую производительность по сравнению со значением типа char.<br /><br /> Дополнительные сведения см. в разделе [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|  
|**widechar**|В Юникоде.<br /><br /> Дополнительные сведения см. в разделе [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|  
|**widenative**|Собственные (базы данных) типы данных, за исключением **char**, **varchar**, и **текст** столбцы, в которых данные хранятся в Юникоде. Создание **widenative** файла данных путем массового импорта данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью **bcp** программы.<br /><br /> **Widenative** значение предлагает более высокопроизводительную альтернативу **widechar**. Если файл данных содержит [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)] символы национального алфавита, укажите **widenative**.<br /><br /> Дополнительные сведения см. в разделе [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|  
  
  ERRORFILE **= "***имя_файла***"**  
 Указывает файл, используемый для сбора строк, которые имеют ошибки форматирования и не могут быть преобразованы в набор строк OLE DB. Эти строки без изменений копируются из файла данных в файл ошибок.  
  
 Файл ошибок создается на стадии выполнения команды. Если он уже существует, возникает ошибка. Дополнительно создается управляющий файл с расширением ERROR.txt. в котором содержатся ссылки на каждую из строк в файле ошибок и диагностические сведения. После исправления ошибок эти данные могут быть повторно загружены.   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], `error_file_path` может находиться в хранилище больших двоичных объектов.

«errorfile_data_source_name»   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Именованный внешний источник данных, указывающий на место хранения больших двоичных объектов Azure файла ошибок, который будет содержать ошибки, обнаруженные во время импорта. Внешний источник данных должен быть создан с помощью `TYPE = BLOB_STORAGE` , который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).
 
 FIRSTROW  **=**  *first_row*  
 Указывает номер первой строки для загрузки. Значение по умолчанию — первая строка указанного файла данных. Значения аргумента FIRSTROW начинаются с 1.  
  
> [!NOTE]  
>  Атрибут FIRSTROW не предназначен для пропуска заголовков столбцов. Пропуск заголовков не поддерживается инструкцией BULK INSERT. При пропуске строк компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] выполняет поиск только в признаках конца поля и не проверяет данные в полях пропущенных строк.  
  
 FIRE_TRIGGERS  
 Указывает, что при массовом импорте будут выполняться триггеры типа INSERT, определенные для целевой таблицы. Если для операции INSERT определены триггеры в целевой таблице, они будут срабатывать для каждого загруженного пакета.  
  
 Если параметр FIRE_TRIGGERS не указан, триггеры Insert не выполняются.  

FORMATFILE_DATASOURCE  **=**  «имя_источника_данных»   
**Применяется к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 1.1.   
Именованный внешний источник данных указывает на место хранения больших двоичных объектов Azure файла форматирования, который будет определять схемы импортированных данных. Внешний источник данных должен быть создан с помощью `TYPE = BLOB_STORAGE` , который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).
  
 KEEPIDENTITY  
 Указывает, что значение или значения идентификаторов в файле импортированных данных будут использоваться для столбца идентификаторов. Если параметр KEEPIDENTITY не указан, значения идентификаторов для этого столбца проверяются, но не импортируются, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически назначает уникальные значения на основе начального значения и приращения, указанных при создании таблицы. Если файл данных не содержит значений для столбца идентификаторов, укажите в файле форматирования, что столбец идентификаторов в таблице или представлении при импорте данных следует пропустить. В этом случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически назначит уникальные значения для этого столбца. Дополнительные сведения см. в разделе [DBCC CHECKIDENT (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).  
  
 Дополнительные сведения о хранении значений идентификации см [сохранять удостоверение значения после массового импорта данных &#40; SQL Server &#41; ](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).  
  
 KEEPNULLS  
 Указывает, что пустым столбцам при массовом импорте должны присваиваться значения NULL, а не значения по умолчанию, назначенные для этих столбцов. Дополнительные сведения см. в разделе [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
 KILOBYTES_PER_BATCH  **=**  *kilobytes_per_batch*  
 Указывает приблизительное число килобайт (КБ) данных в пакете как *kilobytes_per_batch*. По умолчанию, значение KILOBYTES_PER_BATCH неизвестно. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.  
  
 LASTROW**=***last_row*  
 Указывает номер последней строки для загрузки. Значение по умолчанию 0, что обозначает последнюю строку в указанном файле данных.  
  
 MAXERRORS  **=**  *max_errors*  
 Указывает максимальное число синтаксических ошибок, допустимых для файла данных, прежде чем операция массового импорта будет отменена. Каждая строка, импорт которой при массовом импорте не может быть выполнен, пропускается и считается за одну ошибку. Если *max_errors* не указан, значение по умолчанию — 10.  
  
> [!NOTE]  
>  Параметр MAX_ERRORS не выполняет проверки ограничения или преобразование **money** и **bigint** типов данных.  
  
 ПОРЯДОК ({ *столбца* [ASC | DESC]} [ **,**... *n* ] )  
 Указывает, каким образом отсортированы данные в файле. Производительность массового импорта увеличивается, если импортируемые данные упорядочены согласно кластеризованному индексу таблицы (при наличии). Если файл данных упорядочен в другом порядке, то есть в порядке отличном от порядка ключа кластеризованного индекса или если в таблице отсутствует кластеризованный индекс, то предложение ORDER не обрабатывается. В целевой таблице должны быть указаны имена столбцов. По умолчанию, операция массовой вставки считает, что файл данных не отсортирован. Для оптимизированного массового импорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , помимо прочего, проверяет сортировку импортируемых данных.  
  
 *n*  
 заполнитель, означающий, что может быть указано несколько столбцов.  
  
 ROWS_PER_BATCH  **=**  *rows_per_batch*  
 Указывает приблизительное число строк в файле данных.  
  
 По умолчанию все данные в файле отправляются на сервер за одну транзакцию, а число строк в пакете оптимизатору запросов неизвестно. Если указать аргумент ROWS_PER_BATCH (со значением > 0), сервер будет использовать это значение для оптимизации операции массового импорта. Значение, указанное в ROWS_PER_BATCH, должно приблизительно совпадать с фактическим числом строк. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.  
  
 
 TABLOCK  
 Указывает необходимость запроса блокировки уровня таблицы на время выполнения массового импорта. Если таблица не имеет индексов и указано ключевое слово TABLOCK, загрузка в таблицу может производиться параллельно несколькими клиентами. По умолчанию работа блокировки определяется параметром таблицы **table lock on bulk load**. Блокировка на время выполнения массового импорта значительно повышает производительность, позволяя снизить состязание блокировок таблицы. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.  
  
 Для индекса columnstore. поведение блокировки будет отличаться, поскольку он является внутренним образом разделен на несколько наборов строк.  Каждый поток загружает данные отдельно в каждый набор строк, блокировка X на наборе строк, позволяя загрузки параллельных данных с сеансами загрузки данных. Использование параметра TABLOCK приведет к потока вступили блокировки (X) в таблице (в отличие от блокировки BU для традиционных наборы строк), который сделает невозможным других параллельных потоков, чтобы загрузить данные одновременно.  

### <a name="input-file-format-options"></a>Параметры формата входных файлов
  
ФОРМАТ  **=**  «CSV»   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.   
Указывает файл разделенный запятыми соответствует условиям [RFC 4180](https://tools.ietf.org/html/rfc4180) standard.

FIELDQUOTE  **=**  «field_quote»   
**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.   
Определяет символ, который будет использоваться в качестве символа кавычки в CSV-файле. Если не указан символ кавычки ("") будет использоваться как символ кавычки, определенный в [RFC 4180](https://tools.ietf.org/html/rfc4180) standard.
  
 FORMATFILE **= "***путь_к_файлу_форматирования***"**  
 Указывает полный путь к файлу форматирования. Файл описание файла данных, содержащий полученные с помощью **bcp** служебной программы на одной и той же таблице или представлении. И предназначен для случаев, когда:  
  
-   файл данных содержит больше или меньше столбцов, чем таблица или представление;  
  
-   столбцы расположены в другом порядке;  
  
-   отличаются разделители столбцов;  
  
-   имеются какие-либо другие изменения в формате данных. Файлы форматирования обычно создаются с помощью **bcp** программы, измененный с помощью текстового редактора, при необходимости. Дополнительные сведения см. в разделе [bcp Utility](../../tools/bcp-utility.md).  

**Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.   
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, может быть путь_к_файлу_форматирования в хранилище больших двоичных объектов.

 FIELDTERMINATOR **='***признак_конца_поля***'**  
 Указывает признак конца поля, используемый для **char** и **widechar** файлов данных. По умолчанию, признаком конца поля является символ табуляции (\t). Дополнительные сведения см. в разделе [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  

 ROWTERMINATOR **='***признак_конца_строки***'**  
 Указывает признак конца строки, используемый для **char** и **widechar** файлов данных. Признаком конца строки по умолчанию является **\r\n** (символ перевода строки).  Дополнительные сведения см. в разделе [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  

  
## <a name="compatibility"></a>Совместимость  
 Инструкция BULK INSERT осуществляет более строгую проверку загружаемых из файла данных, что может вызвать прекращение работы существующих скриптов, которые ранее работали с неправильными данными. В частности, теперь BULK INSERT проверяет следующее:  
  
-   Собственные представления **float** или **реальные** типы данных являются допустимыми.  
  
-   Данные в Юникоде имеют четную длину.  
  
## <a name="data-types"></a>Типы данных  
  
### <a name="string-to-decimal-data-type-conversions"></a>Преобразование символьного типа данных в десятичный  
 Инструкцией BULK INSERT преобразования типов данных символьного в десятичный следуют тем же правилам, что [!INCLUDE[tsql](../../includes/tsql-md.md)] [преобразовать](../../t-sql/functions/cast-and-convert-transact-sql.md) функции, которая отклоняет числовые значения, которые используют экспоненциальный формат. Такие строки инструкция BULK INSERT трактует как недопустимые и создает отчет ошибки преобразования.  
  
 Чтобы обойти это поведение, необходимо использовать файл форматирования для массового импорта экспоненциальном представлении **float** данных в десятичный столбец. В файле форматирования необходимо явно описать столбец с **реальные** или **float** данных. Дополнительные сведения об этих типах данных см. в разделе [float и real &#40; Transact-SQL &#41; ](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
> [!NOTE]  
>  Файл форматирования представляет **реальные** данные в виде **SQLFLT4** тип данных и **float** данные в виде **SQLFLT8** тип данных. Сведения о не XML-файлов форматирования см. в разделе [Указание типа файлового хранилища с помощью программы bcp &#40; SQL Server &#41; ](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).  
  
#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Пример импорта числового значения в экспоненциальном представлении  
 В этом примере используется следующая таблица:  
  
```  
CREATE TABLE t_float(c1 float, c2 decimal (5,4));  
```  
  
 Пользователю необходимо выполнить массовый импорт данных в таблицу `t_float`. Файл данных, C:\t_float-c.dat, содержит экспоненциальное представление чисел **float** данных, например:  
  
```  
8.0000000000000002E-28.0000000000000002E-2  
```  
  
 Однако инструкция BULK INSERT не сможет выполнить импорт этих данных непосредственно в таблицу `t_float`, так как второй столбец `c2` имеет тип данных `decimal`. Поэтому необходим файл форматирования. Файл форматирования должен сопоставить в экспоненциальном представлении **float** данных в десятичный формат столбца `c2`.  
  
 Следующий файл форматирования использует тип данных `SQLFLT8` для сопоставления второго поля данных со вторым столбцом:  
  
 `<?xml version="1.0"?>`  
  
 `<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`  
  
 `<RECORD>`  
  
 `<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>`  
  
 `<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/>  </RECORD>  <ROW>`  
  
 `<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>`  
  
 `<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/>  </ROW> </BCPFORMAT>`  
  
 Чтобы задействовать этот файл форматирования (файл `C:\t_floatformat-c-xml.xml`) при импорте тестовых данных в тестовую таблицу, необходимо выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
```  
BULK INSERT bulktest..t_float  
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');  
GO  
```  
  
### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Типы данных для массового экспорта или импорта документов SQLXML  
 Для массового экспорта или импорта данных SQLXML используется один из следующих типов данных в файле форматирования.  
  
|Тип данных|Действие|  
|---------------|------------|  
|SQLCHAR или SQLVARCHAR|Данные отправляются в кодовой странице клиента или кодовой странице, определенной параметрами сортировки. Действует так же, как указанием параметра DATAFILETYPE **= 'char'** без указания файла форматирования.|  
|SQLNCHAR или SQLNVARCHAR|Данные отправляются в Юникоде. Действует так же, как указанием параметра DATAFILETYPE **= 'widechar'** без указания файла форматирования.|  
|SQLBINARY или SQLVARBIN|Данные отправляются без преобразования.|  
  
## <a name="general-remarks"></a>Общие замечания  
 Сравнение инструкции BULK INSERT, инструкции INSERT ... ВЫБЕРИТЕ \* инструкции FROM OPENROWSET(BULK...) и **bcp** см. в разделе [массовый импорт и экспорт данных &#40; SQL Server &#41; ](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
 Сведения о подготовке данных к массовому импорту см. в разделе [Подготовка данных для массового экспорта или импорта &#40; SQL Server &#41; ](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).  
  
 Инструкция BULK INSERT может выполняться внутри пользовательской транзакции для импорта данных в таблицу или представление. При необходимости использовать несколько соответствий для массового импорта данных, в транзакции можно указать предложение BATCHSIZE в инструкции BULK INSERT. При откате транзакции, состоящей из нескольких пакетов, каждый из пакетов, который в процессе транзакции был отправлен в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], откатывается.  
  
## <a name="interoperability"></a>Совместимость  
  
### <a name="importing-data-from-a-csv-file"></a>Импорт данных из CSV-файла  
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, инструкция BULK INSERT поддерживает формат CSV.  
Прежде чем [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, файлы с разделителями-запятыми (CSV) не поддерживаются [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] операциях массового импорта. Однако в некоторых случаях CSV-файл можно использовать в качестве файла данных для массового импорта данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о требованиях для импорта данных из CSV-файла см. в разделе [Подготовка данных для массового экспорта или импорта &#40; SQL Server &#41; ](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).  
  
## <a name="logging-behavior"></a>Режим ведения журнала  
 Сведения о том, когда в журнале транзакций регистрируются операции вставки строк, выполняемые при массовом импорте, см. в разделе [Предварительные условия для минимального протоколирования массового импорта данных](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
##  <a name="Limitations"></a> Ограничения  
 При использовании файла форматирования с BULK INSERT можно указать не более 1024 полей. Это значение совпадает с максимальным числом столбцов в таблице. При использовании инструкции BULK INSERT с файлом данных, который содержит больше 1024 поля, формируется ошибка 4822. [Служебная программа bcp](../../tools/bcp-utility.md) не имеет этого ограничения, поэтому для файлов данных, которые содержат больше 1024 поля, **bcp** команды.  
  
## <a name="performance-considerations"></a>Вопросы производительности  
 Если число страниц, которые должны быть записаны на диск в едином пакете, превышает внутренний порог, может быть произведен полный просмотр буферного пула для определения страниц, подлежащих записи на диск при фиксации пакета. Такой полный просмотр может повредить производительности массового импорта. Превышение внутреннего порога может возникнуть, если большой буферный пул работает с медленной подсистемой ввода-вывода. Избежать переполнения буфера на больших компьютерах можно либо отказавшись от использования указания TABLOCK (что удалит оптимизацию массовых операций), либо задав меньший размер пакета (что сохранит оптимизацию массовых операций).  
  
 В связи с различиями в компьютерах рекомендуется испытать свою рабочую нагрузку с различными размерами пакетов, чтобы выявить оптимальный вариант.  
  
## <a name="security"></a>безопасность  
  
### <a name="security-account-delegation-impersonation"></a>Делегирование учетных записей безопасности (Олицетворение)  
 Если пользователь использует имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , то применяется профиль безопасности учетной записи процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . За пределами компонента Database Engine невозможно выполнить проверку подлинности имени входа, проходящего проверку подлинности SQL Server. Поэтому, если имя входа, использующее проверку подлинности SQL Server, инициирует команду BULK INSERT, подключение к данным устанавливается с помощью контекста безопасности учетной записи процесса SQL Server (учетной записи, которая используется службой SQL Server Database Engine). Для того чтобы прочитать исходные данные, учетной записи, которая используется службой SQL Server Database Engine, необходимо предоставить доступ к этим исходным данным. Если же пользователь [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] входит в систему с проверкой подлинности Windows, то ему доступны только те файлы, к которым имеет доступ учетная запись пользователя, независимо от профиля безопасности процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 При выполнении инструкции BULK INSERT с помощью **sqlcmd** или **osql**, на одном компьютере, вставка данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на втором компьютере и указав *data_file* на третий компьютер с помощью UNC-путь, может возникнуть ошибка 4861.  
  
 Чтобы устранить эту ошибку, используйте [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] проверки подлинности и укажите [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] входа, которое использует профиль безопасности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учетная запись процесса, либо настройте Windows для делегирования учетных записей безопасности. Дополнительные сведения о том, как сделать учетную запись пользователя доступной для делегирования, см. в справке по Windows.  
  
 Дополнительные сведения об этом и других вопросах безопасности при помощи инструкции BULK INSERT см. в разделе [массовый импорт данных с помощью инструкции BULK INSERT или OPENROWSET &#40; BULK... &#41; &#40; SQL Server &#41; ](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
### <a name="permissions"></a>Permissions  
 Требует разрешений INSERT и ADMINISTER BULK OPERATIONS. Кроме того, необходимо разрешение ALTER TABLE, если выполняется одно из следующих условий.  
  
-   Существуют ограничения, и параметр CHECK_CONSTRAINTS не указан.  
  
    > [!NOTE]  
    >  Ограничения отключены по умолчанию. Чтобы проверить ограничения явно, укажите параметр CHECK_CONSTRAINTS.  
  
-   Триггеры существуют, и параметр FIRE_TRIGGER не указан.  
  
    > [!NOTE]  
    >  По умолчанию, триггеры не срабатывают. Чтобы явно включить триггеры, укажите параметр FIRE_TRIGGER.  
  
-   Для импорта значений идентификаторов из файла данных указан параметр KEEPIDENTITY.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-pipes-to-import-data-from-a-file"></a>A. Применение символа вертикальной черты для импорта данных из файла  
 В следующем примере выполняется импорт подробных сведений о заказах из указанного файла данных в таблицу `AdventureWorks2012.Sales.SalesOrderDetail`, используя символ вертикальной черты (`|`) в качестве признака конца столбца и `|\n` в качестве признака конца строки.  
  
```  
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail  
   FROM 'f:\orders\lineitem.tbl'  
   WITH   
      (  
         FIELDTERMINATOR =' |',  
         ROWTERMINATOR =' |\n'  
      );  
```  
  
### <a name="b-using-the-firetriggers-argument"></a>Б. Применение аргумента FIRE_TRIGGERS  
 В следующем примере указывается аргумент `FIRE_TRIGGERS`.  
  
```  
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail  
   FROM 'f:\orders\lineitem.tbl'  
   WITH  
     (  
        FIELDTERMINATOR =' |',  
        ROWTERMINATOR = ':\n',  
        FIRE_TRIGGERS  
      );  
```  
  
### <a name="c-using-line-feed-as-a-row-terminator"></a>В. Применение символа перевода строки в качестве признака конца строки  
 В следующем примере производится импорт файла, в котором в качестве признака конца строки используется символ перевода строки, как в файлах UNIX.  
  
```  
DECLARE @bulk_cmd varchar(1000);  
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail  
FROM ''<drive>:\<path>\<filename>''   
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';  
EXEC(@bulk_cmd);  
```  
  
> [!NOTE]  
>  Из-за как Microsoft Windows обрабатываются текстовые файлы **(\n** автоматически заменяется на **\r\n)**.  
  
### <a name="d-specifying-a-code-page"></a>Г. Указание кодовой страницы  
 В следующем примере показано, как указать кодовую страницу.  
  
```  
BULK INSERT MyTable  
FROM 'D:\data.csv'  
WITH  
( CODEPAGE = '65001',  
    DATAFILETYPE = 'char',  
    FIELDTERMINATOR = ','  
);  
```  
### <a name="e-importing-data-from-a-csv-file"></a>Д. Импорт данных из CSV-файла   
В следующем примере показано, как указать CSV-файла.   
```
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'); 
```

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>Е. Импорт данных из файла в хранилище больших двоичных объектов   
В следующем примере показано, как загрузить данные из CSV-файла в месте хранения BLOB-объектов Azure, которая была настроена в качестве внешнего источника данных. Это требуются учетные данные уровня базы данных с помощью подписанного URL-адреса.    

```tsql
BULK INSERT Sales.Invoices
FROM 'inv-2017-01-19.csv'
WITH (DATA_SOURCE = 'MyAzureInvoices',
     FORMAT = 'CSV'); 
```

Для завершения `BULK INSERT` примеры, включая настройку учетных данных и внешнего источника данных, см. в [примеры массового доступ к данным в хранилище больших двоичных объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).
  
### <a name="additional-examples"></a>Дополнительные примеры  
 Другие `BULK INSERT` примеры приведены в следующих разделах:  
  
-   [Примеры массового импорта и экспорта XML-документов (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
-   [Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)  
  
-   [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
-   [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
-   [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [bcp Utility](../../tools/bcp-utility.md)   
 [Файлы форматирования для импорта или экспорта данных &#40; SQL Server &#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Подготовка данных для массового экспорта или импорта &#40; SQL Server &#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)   
 [sp_tableoption &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)  
  
  
