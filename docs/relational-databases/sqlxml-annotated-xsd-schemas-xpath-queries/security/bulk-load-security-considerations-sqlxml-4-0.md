---
title: Массовая загрузка вопросы безопасности (SQLXML 4.0) | Документы Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- bulk load [SQLXML], security
- security [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], security
ms.assetid: 192fc6d4-ecbc-4a4d-a5cb-55e1f64af318
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: dd240ede23b5ff4845531bebf24529bd0219c5ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-load-security-considerations-sqlxml-40"></a>Вопросы безопасности массовой загрузки (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Ниже приведены рекомендации по обеспечению безопасности при использовании массовой загрузки XML.  
  
-   При указании массовой загрузки, операция, которая должна быть выполнена как транзакция, используемая **TempFilePath** свойство, чтобы указать папку, в которой для создания временных файлов.  
  
     Процесс массовой загрузки создает эти временные файлы со следующими разрешениями.  
  
    -   Процессу массовой загрузки предоставляется доступ для чтения, записи или удаления.  
  
    -   Разрешение на чтение предоставляется всем пользователям, так как учетная запись, под которой Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] будет осуществлять доступ к этим файлам, неизвестна. Можно ограничить доступ к этим временным файлам, задав соответствующие разрешения на каталог, содержащий файлы.  
  
-   Массовая загрузка XML сама по себе не имеет настроек разрешений. Предполагается, что база данных настроена верно, а пользовательский контекст (т. е. имя входа, на использование которого настроены средства массовой загрузкой) имеет соответствующий набор разрешений.  
  
-   В нетранзакционном режиме в случае возникновения ошибки в процессе массовой загрузки данные могут остаться в состоянии частичной загрузки. В этом случае массовая загрузка просто останавливается в той точке, где это произошло. Чтобы устранить эту проблему, можно использовать транзакционный режим.  
  
-   В случае возникновения ошибок массовой загрузки в сообщения об ошибках могут быть включены сведения о базе данных. Например, сообщения об ошибках могут содержать имя таблицы или столбца, а также сведения о типе столбца. При использовании массового копирования необходимо обеспечить перехват ошибок процесса массовой загрузки и возврат общего сообщения об ошибке, а не предоставлять пользователям непосредственный доступ ко всем сведениям об ошибках.  
  
-   Массовая загрузка не налагает ограничений на размер обрабатываемых данных. Массовая загрузка не предусматривает какую-либо проверку размера загружаемых данных. За обеспечение в процессе массовой загрузки достаточного объема памяти для обработки указанного файла и достаточного пространства в базе данных для хранения загружаемых данных отвечает пользователь.  
  
-   В ходе массовой загрузки не осуществляются какие-либо попытки использовать полученные данные в качестве кода. Входные данные никогда и никоим образом не вызываются на выполнение. Любой код или команды во входных данных рассматриваются как обычные данные и не выполняются.  
  
-   Массовая загрузка позволяет вносить в полученные данные изменения, связанные с форматированием, исходя из различий между моделями данных XML и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Например, различаются форматы задания времени. В ходе массовой загрузки предпринимаются попытки устранить эти различия. В результате некоторая часть данных о точности может быть потеряна.  
  
-   Массовая загрузка не налагает ограничений на количество времени, которое потребуется на обработку данных. Обработка продолжится до завершения или до тех пор, пока не возникнут ошибки.  
  
-   Массовая загрузка позволяет создавать и удалять временные таблицы в базе данных, для чего должны быть предоставлены разрешения. Разрешения на эти таблицы предоставляются тому пользователю, который создал соединение с базой данных для выполнения процесса массовой загрузки.  
  
-   Массовая загрузка позволяет создавать и удалять временные файлы, используемые при обработке в транзакционном режиме, для чего должны быть предоставлены разрешения. Эти файлы создаются с теми же разрешениями, с которыми текущий пользователь потока осуществляет массовую загрузку.  
  
-   Если пользователь задает для SQLXML файл журнала ошибок для записи ошибок, то каждый раз при выполнении массовой загрузки этот файл будет перезаписываться данными последнего процесса массовой загрузки.  
  
## <a name="see-also"></a>См. также  
 [Выполнение массовой загрузки XML-данных & #40; SQLXML 4.0 & #41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
