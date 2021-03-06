---
title: Организация пулов соединений с учетом драйвера | Документы Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 53e7e3f7-edab-4d0b-8943-45442ba3ebc9
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2efa24f11f174c77ebe7f289019b0544f65d0c2d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="driver-aware-connection-pooling"></a>Организация пулов соединений с учетом драйвера
Пул соединений с учетом драйвера — это новая функция из диспетчера драйверов в Windows 8. Пул соединений с учетом драйвера позволяет записи драйвер для настройки поведения в их драйвер ODBC пула подключений.  
  
> [!NOTE]  
>  Пул соединений с учетом драйвера с библиотекой курсоров не поддерживается. Приложение будет получать сообщение об ошибке при попытке включить библиотеку курсоров через SQLSetConnectAttr, когда включено использование пулов соединений с учетом драйвера.  
  
 Пул адресов соединений с учетом драйвера следующие проблемы, связанные с пулами соединений диспетчера драйверов:  
  
 **Фрагментация пула** диспетчера драйверов только возвращает соединение из пула, если точно соответствуют строке подключения новый запрос на соединение.  Одной из причин для диспетчера драйверов Требовать точного соответствия является диспетчер драйверов не понимает каждые ключевое слово строки подключения драйвера и его значение.  Тем не менее некоторые значения ключевое слово строки подключения (например, имя базы данных) не требуется точное соответствие, так как драйвер может меняться в базу данных в меньше времени, необходимый для открытия нового соединения (различие точное время зависит от источника данных). И различия в некоторых атрибутах соединения (например, SQL_ATTR_CURRENT_CATALOG) может занять больше времени для изменения, чем различия в других атрибутов (например, в параметре). Это, тоже может препятствовать диспетчера драйверов с помощью наименьшую стоимость и повторного использования подключения из пула. Если драйвер для создания многих новых соединений, может привести к снижению производительности приложения и может снизить масштабируемость источника данных. Фрагментация пула может быть снижена на пул соединений с учетом драйвера, так как драйвер лучше оценить свои расходы на повторное использование подключений в пуле для запроса на соединение.  
  
 **Без учета предпочтения приложения** некоторые источники данных могут эффективно открывать новые соединения (в сравнении с Сброс некоторые атрибуты), таким образом, лучше создать новое соединение, вместо того чтобы повторно использовать немного несоответствующие приложения соединение из пула и сброса, некоторые значения (несмотря на то, что это может быть медленнее во время инициализации фразу пула соединений). Однако некоторые приложения могут уменьшению нагрузку на сервер и откройте меньшее количество соединений, несмотря на то, что может быть больше затрат по устранению несоответствий, для правильного поведения. Без организации пулов соединений с учетом драйвера нельзя указать такого рода предпочтения по сути, поскольку диспетчер драйверов не распознает все атрибуты подключения драйвера. Организация пулов соединений с учетом драйвера позволяет драйверу получить настройки пользователя (со значением атрибута драйвера SQLSetConnectAttr), чтобы его можно более точно оценить стоимость повторного использования подключения из пула, в зависимости от предпочтений пользователя.  
  
 Дополнительные сведения об организации пулов соединений с учетом драйвера см. в разделе [разработке пула соединений, поддерживающие драйвер ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
## <a name="determining-driver-support"></a>Определение поддержка драйверов  
 Организация пулов соединений с учетом драйвера является дополнительным компонентом, драйвер может не поддерживать. Чтобы определить, поддерживает ли драйвер его, используйте SQL_DRIVER_AWARE_POOLING_SUPPORTED *свойство* из [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
## <a name="how-to-enable-driver-aware-connection-pooling"></a>Как включить использование пулов соединений с учетом драйвера  
 Приложение может использовать драйвер осведомленности пулов соединений, задав атрибут SQL_ATTR_CONNECTION_POOLING в SQL_CP_DRIVER_AWARE с [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Если драйвер не поддерживает осведомленности пула соединений, диспетчер драйверов пулов соединений будет использоваться (то же, как если бы SQL_CP_ONE_PER_HENV было задано, а не SQL_CP_DRIVER_AWARE). Приложения ODBC 2.x и 3.x можно включить эту функцию.  
  
## <a name="see-also"></a>См. также  
 [Разработка драйвера ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)
