---
title: Пример свойства URL-адреса (VBScript) | Документы Microsoft
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: drivers
ms.component: reference
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- URL property [ADO], VBScript example
ms.assetid: 6ae5ac50-c88c-4262-b7ab-f2b3de382a0b
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9aff5efe693204be71a94c4cae47a64d4ecfba37
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="url-property-example-vbscript"></a>Пример свойства URL-адреса (VBScript)
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, серверные компоненты служб удаленных рабочих СТОЛОВ больше не включаются в операционной системе Windows (в разделе Windows 8 и [руководство по Windows Server 2012 совместимости](https://www.microsoft.com/en-us/download/details.aspx?id=27416) для получения дополнительных сведений). Клиентские компоненты служб удаленных рабочих СТОЛОВ будут удалены в будущих версиях Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие служб удаленных рабочих СТОЛОВ необходимо перенести в [службы данных WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 В следующем коде показано, как задать **URL-адрес** свойство на стороне клиента, чтобы указать ASP-файла, который в свою очередь обрабатывает передачей изменений в источник данных.  
  
```  
<!-- BeginURLClientVBS -->  
<%@ Language=VBScript %>  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>URL Property Example (VBScript)</title>  
<style>  
<!--  
body {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead {  
   background-color: #008080;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</style>  
</head>  
  
<body onload=Getdata()>  
<h1>URL Property Example (VBScript)</h1>  
<OBJECT classid=clsid:BD96C556-65A3-11D0-983A-00C04FC29E33 height=1 id=ADC width=1>  
</OBJECT>  
  
<table datasrc="#ADC" align="center">  
<thead>  
<tr id="ColHeaders" class="thead2">  
   <th>FirstName</th>  
   <th>LastName</th>  
   <th>Extension</th>  
</tr>  
</thead>  
<tbody class="tbody">  
<tr>  
   <td><input datafld="FirstName" size=15> </td>  
   <td><input datafld="LastName" size=25> </td>  
   <td><input datafld="Extension" size=15> </td>  
</tr>  
</tbody>  
</table>  
  
<script Language="VBScript">  
Sub Getdata()  
  
      ADC.URL = "http://MyServer/URLServerVBS.asp"  
      ADC.Refresh  
End Sub  
  
</script>  
  
</body>  
</html>  
<!-- EndURLClientVBS -->  
```  
  
 Серверный код, который существует в **URLServerVBS.asp** отправляет обновленный **записей** к источнику данных.  
  
```  
<!-- BeginURLServerVBS -->  
<%@ Language=VBScript %>  
<%  
  
        ' XML output req's  
    Response.ContentType = "text/xml"  
    const adPersistXML  = 1  
  
        ' recordset vars  
    Dim strSQL, rsEmployees   
    Dim strCnxn, Cnxn  
  
    strCnxn = "Provider='sqloledb';Data Source=" & _  
            Request.ServerVariables("SERVER_NAME") & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    Set Cnxn = Server.CreateObject("ADODB.Connection")  
    Set rsEmployees = Server.CreateObject("ADODB.Recordset")  
  
    strSQL = "SELECT FirstName, LastName, Extension FROM Employees"  
  
    Cnxn.Open strCnxn  
    rsEmployees.Open strSQL, Cnxn  
  
        ' output as XML  
    rsEmployees.Save Response, adPersistXML  
  
        ' Clean up  
    rsEmployees.Close  
    Cnxn.Close  
    Set rsEmployees = Nothing  
    Set Cnxn = Nothing  
%>  
<!-- EndURLServerVBS -->  
```  
  
## <a name="see-also"></a>См. также  
 [Свойство URL (служба удаленных рабочих столов)](../../../ado/reference/rds-api/url-property-rds.md)


