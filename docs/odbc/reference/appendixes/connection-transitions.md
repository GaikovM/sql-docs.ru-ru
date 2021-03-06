---
title: Переходы подключения | Документы Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b59fe86e0fb16dca51d24098c98694e77fa7e82b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="connection-transitions"></a>Переходы подключения
Подключения ODBC имеют следующие состояния.  
  
|Состояние|Описание|  
|-----------|-----------------|  
|C0|Незанятое среды, нераспределенное подключения|  
|C1|Выделенный среды, нераспределенное подключения|  
|C2|Выделенные среды, выделенных соединений|  
|C3|Функция подключения требуются данные|  
|C4|Подключенный подключения|  
|C5|Подключение соединения, выделенной инструкции|  
|C6|Подключенный подключения во время выполнения транзакции. Существует возможность подключения находится в состоянии C6 без операторов, выделенные при соединении. Предположим, например, соединение находится в режиме ручной фиксации и находится в состоянии C4. Если инструкция выделенной, выполняется (запуск транзакции) и затем освобождается, транзакция остается активной, но нет инструкций по соединению.|  
  
 В следующих таблицах показано, как каждая функция ODBC влияет на состояние соединения.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> Нет Env.|Нераспределенное C1|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(СИСТЕМЫ) [2]|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(СИСТЕМЫ) [3]|(СИСТЕМЫ)|(08003)|(08003)|C5|--[5]|--[5]|  
|(СИСТЕМЫ) [4]|(СИСТЕМЫ)|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_ENV.  
  
 [2] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DBC.  
  
 [3] в этой строке показаны переходы при *HandleType* был значение SQL_HANDLE_STMT.  
  
 [4] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DESC.  
  
 [5] вызова **SQLAllocHandle** с *OutputHandlePtr* указав допустимый дескриптор перезаписывает этот дескриптор, без учета предыдущих дескриптор ofthat содержимое и могут вызвать проблемы для драйверов ODBC. Это неправильное программирования приложений ODBC для вызова **SQLAllocHandle** дважды с той же переменной приложения, определенные для  *\*OutputHandlePtr* без вызова  **SQLFreeHandle** для освобождения дескриптора перед ее перераспределения. Перезапись ODBC дескрипторов таким образом, может привести к непредсказуемому поведению или ошибки со стороны драйверов ODBC.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|C4 C3 [d] [s]|--C2 [d] [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|— [1] C5 [2]|  
  
 [1] соединение было в режиме ручной фиксации.  
  
 [2 в режиме автоматической фиксации удалось подключиться к.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges и SQLTables  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|— [1] C6 [2]|--|  
  
 [1] соединение находилась в режиме автоматической фиксации, или источник данных не удалось запустить транзакцию.  
  
 [2] соединение находилась в режиме ручной фиксации, а источник данных начала транзакции.  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc, SQLGetDescField, SQLGetDescRec, SQLSetDescField и SQLSetDescRec  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--[1]|--|--|  
  
 [1] в этом состоянии только дескрипторов, доступных приложению явным образом распределяются дескрипторов.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources и SQLDrivers  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|S C4 - n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ) [1]|--[3]|--[3]|--[3]|--|--|--[4] или ([5], [6] и [8]) C4 [5] и [7] C5 [5], [6] и [9]|  
|(СИСТЕМЫ) [2]|(СИСТЕМЫ)|(08003)|(08003)|--|--|C5|  
  
 [1] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_ENV.  
  
 [2] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DBC.  
  
 [3], так как подключение не находится в подключенном состоянии, не оказывают влияния операции.  
  
 [4 commit или rollback сбой соединения. Эта функция возвращает значение SQL_ERROR в этом случае.  
  
 [5 commit или rollback успешно выполнен для соединения. При фиксации или отката не удалось выполнить на другое соединение или функция возвращает значение SQL_SUCCESS успешности commit или rollback для всех подключений, функция возвращает значение SQL_ERROR.  
  
 [6] было по крайней мере один оператор, выделенные при соединении.  
  
 [7] нет инструкции, не выделенные при соединении.  
  
 [8] соединение было по крайней мере один оператор, для которого существует был открытого курсора, и источник данных сохраняет курсоры при фиксации или откате транзакции, какое из них применяется (в зависимости от того, следует ли *CompletionType* был SQL_ ФИКСАЦИЯ или SQL_ROLLBACK). Дополнительные сведения см. в разделе атрибуты SQL_CURSOR_COMMIT_BEHAVIOR и SQL_CURSOR_ROLLBACK_BEHAVIOR [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 [9] Если соединение содержит все инструкции, для которых были открытые курсоры, курсоры не были сохранены, если транзакция была зафиксирована или выполнен откат.  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect и SQLExecute  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--C6 C6 [1] [2] [3]|--|  
  
 [1] соединение находилась в режиме автоматической фиксации и не была выполнена инструкция *курсор* *спецификации* (например, SELECT); в режим ручной фиксации, а оператор или соединения выполнить не удалось запустить транзакцию.  
  
 [2] соединение находилась в режиме автоматической фиксации и была выполнена инструкция *курсор* *спецификации* (например, SELECT).  
  
 [3] соединение находилась в режиме ручной фиксации, а источник данных начала транзакции.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ) [1]|C0|(HY010)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(СИСТЕМЫ) [2]|(СИСТЕМЫ)|(C1)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(СИСТЕМЫ) [3]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|C4 [5], [6]|--C4 [7] [5] и [8] C5 [6] и [8]|  
|(СИСТЕМЫ) [4]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|--|--|  
  
 [1] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_ENV.  
  
 [2] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DBC.  
  
 [3] в этой строке показаны переходы при *HandleType* был значение SQL_HANDLE_STMT.  
  
 [4] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DESC.  
  
 [5] была только одна инструкция, выделенные при соединении.  
  
 [6] было несколько инструкций, выделенные при соединении.  
  
 [7] соединение было в режиме ручной фиксации.  
  
 [8] соединение было в режиме автоматической фиксации.  
  
## <a name="sqlfreestmt"></a>Функция SQLFreeStmt  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ) [1]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|C5 [3] — [4]|  
|(СИСТЕМЫ) [2]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|--|  
  
 [1] в этой строке показаны транзакций при *параметр* аргумент является SQL_CLOSE.  
  
 [2] в этой строке показаны транзакций при *параметр* аргумент является SQL_UNBIND или SQL_RESET_PARAMS.  
  
 [3] соединение находилась в режиме автоматической фиксации и отсутствуют курсоры были открыты на инструкции, за исключением данного объекта.  
  
 [4] соединение находилась в режиме ручной фиксации, или она находилась в режиме автоматической фиксации и курсор был открыт на хотя бы один оператор.  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|СИСТЕМЫ|СИСТЕМЫ|--[1] 08003[2]|HY010|--|--|--|  
  
 [1] *атрибута* аргумент был SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, параметре, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE или SQL_ATTR_TRACEFILE или ранее было задано значение для атрибута соединения.  
  
 [2] *атрибута* аргумент не SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, параметре, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE или SQL_ATTR_TRACEFILE и для соединения не задано значение атрибут.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>Функция SQLGetDiagField и SQLGetDiagRec  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ) [1]|--|--|--|--|--|--|  
|(СИСТЕМЫ) [2]|(СИСТЕМЫ)|--|--|--|--|--|  
|(СИСТЕМЫ) [3]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|--|  
|(СИСТЕМЫ) [4]|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|--|--|  
  
 [1] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_ENV.  
  
 [2] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DBC.  
  
 [3] в этой строке показаны переходы при *HandleType* был значение SQL_HANDLE_STMT.  
  
 [4] в этой строке показаны переходы при *HandleType* был SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|СИСТЕМЫ|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|СИСТЕМЫ|СИСТЕМЫ|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|СИСТЕМЫ|СИСТЕМЫ|--[1] 08003[2]|08003|--|--|--|  
  
 [1] *свойство* аргумент был SQL_ODBC_VER.  
  
 [2] *свойство* аргумент не SQL_ODBC_VER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|— [1] C6 [2]|--C5 [3] [1]|  
  
 [1] подключение было в режиме автоматической фиксации, а вызов **SQLMoreResults** обработки результирующего набора курсора спецификации не инициализирован.  
  
 [2] подключение было в режиме автоматической фиксации, а вызов **SQLMoreResults** инициализирована обработки результирующего набора курсора спецификации.  
  
 [3 в режиме ручной фиксации удалось подключиться к.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|— [1] C6 [2]|--|  
  
 [1] соединение находилась в режиме автоматической фиксации, или источник данных не удалось запустить транзакцию.  
  
 [2] подключение было в режиме ручной — фиксации и источник данных начала транзакции.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|СИСТЕМЫ|СИСТЕМЫ|--[1] 08003[2]|HY010|--08002 [3] [4] HY011 [5]|--08002 [3] [4] HY011 [5]|--[3] и [6] C5 [8] [4] HY011 08002 [5] или [7]|  
  
 [1] *атрибута* аргумент не помощью настройки SQL_ATTR_TRANSLATE_LIB или SQL_ATTR_TRANSLATE_OPTION.  
  
 [2] *атрибута* аргумент имеет помощью настройки SQL_ATTR_TRANSLATE_LIB или SQL_ATTR_TRANSLATE_OPTION.  
  
 [3] *атрибута* аргумент не SQL_ATTR_ODBC_CURSORS или SQL_ATTR_PACKET_SIZE.  
  
 [4] *атрибута* аргумент был SQL_ATTR_ODBC_CURSORS.  
  
 [5] *атрибута* аргумент был SQL_ATTR_PACKET_SIZE.  
  
 [6] *атрибута* аргумент не SQL_ATTR_AUTOCOMMIT, или *атрибута* аргумент был SQL_ATTR_AUTOCOMMIT и установка этого атрибута не удалось зафиксировать транзакцию.  
  
 [7] *атрибута* аргумент был SQL_ATTR_TXN_ISOLATION.  
  
 [8] *атрибута* аргумент был SQL_ATTR_AUTOCOMMIT и транзакция завершена установка этого атрибута.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|--|--|(HY010)|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>Все остальные функции ODBC  
  
|C0<br /><br /> Нет Env.|C1<br /><br /> Не выделено|C2<br /><br /> Выделенные|C3<br /><br /> Требуются данные|C4<br /><br /> Подключен|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|(СИСТЕМЫ)|--|--|
