---
title: Функция SQLSetConnectInfo | Документы Microsoft
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
- SQLSetConnectInfo function [ODBC]
ms.assetid: 0782a1c3-c5d1-499b-a8ba-134162db9990
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3418d3993ef59722554956204ff48539d6ee3809
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetconnectinfo-function"></a>Функция SQLSetConnectInfo
**Соответствия**  
 Появился в версии: Полное соответствие стандартам 3,81 ODBC: ODBC  
  
 **Сводка**  
 **SQLSetConnectInfo** используется для задания источника данных, идентификатор пользователя и пароль в info маркер подключения для приложения [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) вызова.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
SQLRETURN  SQLSetConnectInfo(  
                SQLHDBC_INFO_TOKEN   TokenHandle,  
                WCHAR *              ServerName,  
                SQLSMALLINT          NameLength1,  
                WCHAR *              UserName,  
                SQLSMALLINT          NameLength2,  
                WCHAR *              Authentication,  
                SQLSMALLINT          NameLength3 );  
```  
  
## <a name="arguments"></a>Аргументы  
 *TokenHandle*  
 [Вход] Дескриптор токена.  
  
 *ServerName*  
 [Вход] Имя источника данных. Данные могут быть расположены на том же компьютере, что и программа или на другом компьютере, где-нибудь в сети. Сведения о выборе источника данных в приложении см. в разделе [Выбор источника данных или драйвер](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 *NameLength1*  
 [Вход] Длина **ServerName* в символах.  
  
 *UserName*  
 [Вход] Идентификатор пользователя.  
  
 *NameLength2*  
 [Вход] Длина **UserName* в символах.  
  
 *Проверка подлинности*  
 [Вход] Строка для проверки подлинности (обычно пароль).  
  
 *NameLength3*  
 [Вход] Длина **проверки подлинности* в символах.  
  
## <a name="returns"></a>Возвращает  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, значение SQL_ERROR или SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Диагностика  
 То же, что [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) для ввода ошибок проверки, за исключением того, что будет использоваться диспетчер драйверов **HandleType** из SQL_HANDLE_DBC_INFO_TOKEN и **обработки** из *hDbcInfoToken*.  
  
## <a name="remarks"></a>Замечания  
 Всякий раз, когда драйвер возвращает значение SQL_ERROR или SQL_INVALID_HANDLE, диспетчер драйверов возвращает приложению ошибку (в [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) или [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Каждый раз, когда драйвер возвращает SQL_SUCCESS_WITH_INFO, диспетчер драйверов будет получать диагностические данные из *hDbcInfoToken*и возвращают значение SQL_SUCCESS_WITH_INFO к приложению в [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)и [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Приложения не должны напрямую вызывать эту функцию. Драйвер ODBC, который поддерживает пулы соединений с учетом драйвера необходимо реализовать эту функцию.  
  
 Включить sqlspi.h для разработки драйвера ODBC.  
  
## <a name="see-also"></a>См. также  
 [Разработка драйвера ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Организация пулов соединений с учетом драйвера](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Разработка драйвера ODBC с поддержкой пула подключений](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
