---
title: Получение данных Long | Документы Microsoft
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
- long data [ODBC]
- fetches [ODBC], long data
- result sets [ODBC], fetching
- SQLGetData function [ODBC], getting long data
- retrieving long data [ODBC]
ms.assetid: 6ccb44bc-8695-4bad-91af-363ef22bdb85
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a465c8200181c87caa49d6e36df50cb5c158cdbf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="getting-long-data"></a>Получение данных Long
Определение СУБД *длинных данных* как любой символьных или двоичных данных через определенный размер, например 255 символов. Эти данные могут быть достаточно мал для хранения в один буфер, таких как часть описания несколько тысяч символов. Однако возможно, слишком длинное для хранения в памяти, например, длинные текстовые документы или растровые изображения. Поскольку такие данные не могут храниться в одиночный буфер, он извлекается из драйвера в части с **SQLGetData** после получило данные из строки.  
  
> [!NOTE]  
>  Приложение может получать фактически любой тип данных с **SQLGetData**, не только длинные данные, несмотря на то, что в частях можно получить только символьных и двоичных данных. Тем не менее, если данные помещаются в один буфер, обычно есть причин использовать **SQLGetData**. Это значительно упрощает привязку к столбцу буфера и разрешить драйверу возвращения данных в буфере.  
  
 Для получения длинных данных из столбца, приложение сначала вызывает **SQLFetchScroll** или **SQLFetch** перейдите к строке и для получения данных для связанных столбцов. Затем приложение вызывает **SQLGetData**. **SQLGetData** те же аргументы, как **SQLBindCol**: дескриптор инструкции; номер столбца; C тип, адрес и байтов длина данных переменной приложения; и адрес буфер длины/индикатора. Обе функции имеют те же аргументы, так как они выполняют по сути ту же задачу: они описывают переменную приложения к драйверу и указать, что данные для определенного столбца должно возвращаться в этой переменной. Основные различия —, **SQLGetData** вызывается после выборке строки (и иногда называется *позднего связывания* по этой причине) и что привязка, указанная **SQLGetData**  остается включенным только в течение всего вызова.  
  
 Один столбец, в отношении **SQLGetData** ведет себя как **SQLFetch**: извлечение данных для столбца, он преобразуется в тип переменной приложения и возвращает его в переменной. Он также возвращает байтовая длина данных в буфер длины/индикатора. Дополнительные сведения о том, как **SQLFetch** возвращает данные в разделе [выборки данных из строк](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 **SQLGetData** отличается от **SQLFetch** в один важный фактор. Если вызывается несколько раз подряд для одного столбца, каждый вызов возвращает последовательные части данных. Каждый вызов функции, за исключением последнего вызова метода возвращает SQL_SUCCESS_WITH_INFO и SQLSTATE 01004 (строковые данные, правый усечение); последний вызов возвращает значение SQL_SUCCESS. Это как **SQLGetData** используется для получения данных long в частях. Если больше нет данных для возврата, **SQLGetData** не вернет значение SQL_NO_DATA. Приложение отвечает за объединения длинных данных, которой может означать объединения фрагменты данных. Каждый компонент является символом null; приложение необходимо удалить знак завершения null, если для объединения частей. Извлечение данных из частей может выполняться для закладок переменной длины, а также другие данные большой длины. Значение, возвращаемое в уменьшается буфер длины/индикатора в каждом вызове число байтов, возвращенных в предыдущем вызове, несмотря на то, что вполне драйвер может быть неспособен обнаружить объем доступных данных и возврата числа SQL_NO_TOTAL байт. Например:  
  
```  
// Declare a binary buffer to retrieve 5000 bytes of data at a time.  
SQLCHAR       BinaryPtr[5000];  
SQLUINTEGER   PartID;  
SQLINTEGER    PartIDInd, BinaryLenOrInd, NumBytes;  
SQLRETURN     rc;   
SQLHSTMT      hstmt;  
  
// Create a result set containing the ID and picture of each part.  
SQLExecDirect(hstmt, "SELECT PartID, Picture FROM Pictures", SQL_NTS);  
  
// Bind PartID to the PartID column.  
SQLBindCol(hstmt, 1, SQL_C_ULONG, &PartID, 0, &PartIDInd);  
  
// Retrieve and display each row of data.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   // Display the part ID and initialize the picture.  
   DisplayID(PartID, PartIDInd);  
   InitPicture();  
  
   // Retrieve the picture data in parts. Send each part and the number   
   // of bytes in each part to a function that displays it. The number   
   // of bytes is always 5000 if there were more than 5000 bytes   
   // available to return (cbBinaryBuffer > 5000). Code to check if   
   // rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
   while ((rc = SQLGetData(hstmt, 2, SQL_C_BINARY, BinaryPtr, sizeof(BinaryPtr),  
                           &BinaryLenOrInd)) != SQL_NO_DATA) {  
      NumBytes = (BinaryLenOrInd > 5000) || (BinaryLenOrInd == SQL_NO_TOTAL) ?  
                  5000 : BinaryLenOrInd;  
      DisplayNextPictPart(BinaryPtr, NumBytes);  
   }  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```  
  
 Существует ряд ограничений на использование **SQLGetData**. Как правило, доступ к столбцам с **SQLGetData**:  
  
-   Должен осуществляться в порядке возрастания номеров столбцов (из-за способа чтения столбцы результирующего набора из источника данных). Например, недопустимо для вызова **SQLGetData** для столбца 5 и затем вызывает его для столбца 4.  
  
-   Невозможно привязать.  
  
-   Необходимо иметь более высокий номер столбца, чем последнего связанного столбца. Например, если столбец 3 последнего связанного столбца, недопустимо для вызова **SQLGetData** для столбца 2. По этой причине приложения следует убедиться, что поместите столбцы данных long в конце списка выбора.  
  
-   Нельзя использовать, если **SQLFetch** или **SQLFetchScroll** был вызван для получения более одной строки. Дополнительные сведения см. в разделе [использование блочных курсоров](../../../odbc/reference/develop-app/using-block-cursors.md).  
  
 Некоторые драйверы не принудительного применения этих ограничений. Взаимодействующие приложения следует предположить, существует, или определить, какие ограничения не применяются путем вызова **SQLGetInfo** с параметром SQL_GETDATA_EXTENSIONS.  
  
 Если приложение не все данные в столбце двоичных данных или символ, он позволяют уменьшить сетевой трафик в драйверов на основе DBMS, установив атрибут инструкции sql_attr_max_length параметра до выполнения инструкции. Ограничивает число байтов данных, которое будет возвращаться для любого символьных и двоичных столбцов. Например предположим, что столбец содержит документы длинный текст. Приложение, применяемое для просмотра таблицы, содержащей этот столбец может иметь для отображения только первая страница каждого документа. Несмотря на то, что этот атрибут инструкции можно моделировать в драйвере, нет причин для этого. В частности, если приложению требуется выполнить усечение символьных или двоичных данных, он должен быть привязан небольшим буфером со столбцом с **SQLBindCol** и разрешить драйверу усечение данных.
