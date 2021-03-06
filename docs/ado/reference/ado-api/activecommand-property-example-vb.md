---
title: Пример свойства ActiveCommand (VB) | Документы Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ActiveCommand property [ADO], Visual Basic example
ms.assetid: 23b06499-62df-4f46-88eb-6da392f9b456
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee1b4261eab4e702a8d99509aac2d9da8b234a77
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="activecommand-property-example-vb"></a>Пример свойства ActiveCommand (Visual Basic)
В этом примере демонстрируется [ActiveCommand](../../../ado/reference/ado-api/activecommand-property-ado.md) свойство.  
  
 Получает подпрограмме [записей](../../../ado/reference/ado-api/recordset-object-ado.md) которого **ActiveCommand** свойство используется для отображения текста команды и параметра, который создан **записей**.  
  
```  
'BeginActiveCommandVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
        'recordset and connection variables  
    Dim cmd As ADODB.Command  
    Dim rst As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
        'record variables  
    Dim strPrompt As String  
    Dim strName As String  
  
    Set Cnxn = New ADODB.Connection  
    Set cmd = New ADODB.Command  
  
    strPrompt = "Enter an author's name (e.g., Ringer): "  
    strName = Trim(InputBox(strPrompt, "ActiveCommandX Example"))  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
  
        'create SQL command string  
    cmd.CommandText = "SELECT * FROM Authors WHERE au_lname = ?"  
    cmd.Parameters.Append cmd.CreateParameter("LastName", adChar, adParamInput, 20, strName)  
  
    Cnxn.Open strCnxn  
    cmd.ActiveConnection = Cnxn  
  
        'create the recordset by executing command string  
    Set rst = cmd.Execute(, , adCmdText)  
        'see the results  
    Call ActiveCommandXprint(rst)  
  
    ' clean up  
    Cnxn.Close  
    Set rst = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rst Is Nothing Then  
        If rst.State = adStateOpen Then rst.Close  
    End If  
    Set rst = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndActiveCommandVB  
```  
  
 **ActiveCommandXprint** процедуры предоставляется только **записей** объекта, но его необходимо распечатать, текст команды и параметра, который создан **записей**. Это можно сделать, поскольку **записей** объекта **ActiveCommand** свойство дает связанного [команда](../../../ado/reference/ado-api/command-object-ado.md) объекта.  
  
 **Команда** объекта [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) свойство дает параметризованной команды, которая создана **записей**. **Команда** объекта [параметры](../../../ado/reference/ado-api/parameters-collection-ado.md) коллекции дает значение, которое был замещен заполнителя параметра команды («**?**»).  
  
 Наконец выводится сообщение об ошибке или имя автора имя и идентификатор.  
  
```  
'BeginActiveCommandPrintVB  
Public Sub ActiveCommandXprint(rstp As ADODB.Recordset)  
  
    Dim strName As String  
  
    strName = rstp.ActiveCommand.Parameters.Item("LastName").Value  
  
    Debug.Print "Command text = '"; rstp.ActiveCommand.CommandText; "'"  
    Debug.Print "Parameter = '"; strName; "'"  
  
    If rstp.BOF = True Then  
       Debug.Print "Name = '"; strName; "', not found."  
    Else  
       Debug.Print "Name = '"; rstp!au_fname; " "; rstp!au_lname; _  
             "', author ID = '"; rstp!au_id; "'"  
    End If  
  
    rstp.Close  
    Set rstp = Nothing  
End Sub  
'EndActiveCommandPrintVB  
```  
  
## <a name="see-also"></a>См. также  
 [Свойство ActiveCommand (ADO)](../../../ado/reference/ado-api/activecommand-property-ado.md)   
 [Объект команды (ADO)](../../../ado/reference/ado-api/command-object-ado.md)   
 [Объект Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
