---
title: Метод getObject (java.lang.String) | Документы Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerCallableStatement.getObject (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a1e955ce-13db-4828-ad59-d9b6a8b2c6cc
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 82618a47bc45d9d204e7c37305289796e77e5a60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="getobject-method-javalangstring"></a>Метод getObject (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Получает значение заданного параметра в виде объекта на языке программирования Java по заданному имени параметра.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.Object getObject(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Параметры  
 *sCol*  
  
 Объект **строка** , содержащее имя параметра.  
  
## <a name="return-value"></a>Возвращаемое значение  
 **Объекта** значение.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Замечания  
 Этот метод getObject указывается с помощью метода getObject в интерфейсе java.sql.CallableStatement.  
  
 Этот метод вернет значение данного столбца в виде объекта Java. Объект Java будет иметь заданный по умолчанию тип, соответствующий типу SQL Server столбца и сопоставленный встроенным типам, указанным в спецификации JDBC. Если значение SQL NULL, то драйвер вернет значение Java NULL.  
  
 Этот метод также может быть использован для чтения абстрактных типов данных, относящихся к базе данных. В JDBC 2.0 API возможности метода getObject расширены и поддерживают материализацию данных определяемых пользователем типов SQL. Если столбец содержит структурированное или уникальное значение, поведение данного метода является, как если бы он был вызов `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 Начиная с версии [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] драйвера JDBC 3.0:  
  
-   Значение типа **даты** будет возвращаться в виде объекта java.sql.Date.  
  
-   Значение типа **время** будет возвращаться в виде объекта java.sql.Time.  
  
-   Значение типа **datetime2** будет возвращаться в виде объекта java.sql.Timestamp.  
  
-   Значение типа **datetimeoffset** будет возвращаться в виде объекта microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>См. также  
 [Метод getObject &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)   
 [Члены SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
