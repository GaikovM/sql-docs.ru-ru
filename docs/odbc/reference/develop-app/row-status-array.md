---
title: "Массив состояния строк | Документы Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- row status array [ODBC]
- cursors [ODBC], block
- result sets [ODBC], row status array
- block cursors [ODBC]
- result sets [ODBC], block cursors
- rowset status [ODBC]
ms.assetid: 4b69f189-2722-4314-8a02-f4ffecd6dabd
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: 34fe599aee975dc0c01fc1fbc36f1bed6cab6b6b
ms.contentlocale: ru-ru
ms.lasthandoff: 09/09/2017

---
# <a name="row-status-array"></a>Массив состояния строк
В дополнение к данным **SQLFetch** и **SQLFetchScroll** может возвращать массив, который предоставляет состояние каждой строки в наборе строк. Этот массив указывается через атрибут значения SQL_ATTR_ROW_STATUS_PTR инструкции. Этот массив выделяется для приложения и должен иметь столько элементов, как указано в атрибуте SQL_ATTR_ROW_ARRAY_SIZE инструкции. Значения в массиве, задаваемыми **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll**, и **SQLSetPos.** Значения описаны состояния строки и изменилось ли состояния с момента последней загрузки.  
  
|Значение массива строки состояния|Description|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|Строка была успешно получен и не изменились с момента последней загрузки.|  
|SQL_ROW_SUCCESS_WITH_INFO|Строка была успешно получен и не изменились с момента последней загрузки. Однако предупреждение был возвращен о строке.|  
|SQL_ROW_ERROR|Произошла ошибка при получении строки.|  
|SQL_ROW_UPDATED|Строка была успешно получен и обновилась с момента последней загрузки. Если снова выборке или обновлении по строке **SQLSetPos**, его состояние изменяется на новое состояние.<br /><br /> Некоторые драйверы не может обнаружить изменения в данных и поэтому не может возвращать это значение. Чтобы определить ли драйвер может обнаружить обновления refetched строк, приложение вызывает **SQLGetInfo** с параметром SQL_ROW_UPDATES.|  
|SQL_ROW_DELETED|Строка была удалена с момента последней загрузки.|  
|SQL_ROW_ADDED|Строка была вставлена **SQLBulkOperations**. Если строка выбирается заново или обновляется в соответствии с **SQLSetPos**, она находится в состоянии SQL_ROW_SUCCESS.<br /><br /> Это значение не задается **SQLFetch** или **SQLFetchScroll**.|  
|SQL_ROW_NOROW|Набор строк перекрывающегося конца результирующего набора и строки не был возвращен, что значение соответствовало этот элемент массив состояния строк.|