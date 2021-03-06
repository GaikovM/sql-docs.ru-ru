---
title: Командлет Add-RoleMember | Документы Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8144e76d8d3ba7e0ff588c9f22875af430b68a1b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="add-rolemember-cmdlet"></a>Командлет Add-RoleMember
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]

  Добавляет элемент к указанной роли в табличной или многомерной базе данных служб Analysis Services.  

>[!NOTE] 
>В этой статье может содержать устаревшие сведения и примеры. С помощью командлета Get-Help для последней версии.
  
## <a name="syntax"></a>Синтаксис  
 `Add-RoleMember [-MemberName] <System.String> [-Database] <System.String> [-RoleName] <System.String> [<CommonParameters>]`  
  
 `Add-RoleMember [-MemberName] <System.String> [-DatabaseRole] <Microsoft.AnalysisServices.Role> [<CommonParameters>]`  
  
## <a name="description"></a>Описание  
 Командлет Add-RoleMember добавляет допустимого члена к существующей роли базы данных. Разрешены только роли базы данных. С помощью этого командлета нельзя добавлять членов к роли сервера.  
  
 За один раз можно добавить только одного члена, которым может быть учетная запись пользователя или группы.  
  
## <a name="parameters"></a>Параметры  
  
### <a name="-membername-string"></a>-MemberName \<строка >  
 Указывает пользователя или группу Windows, которые добавляются к роли.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|0|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-database-string"></a>-Базы данных \<строка >  
 Указывает базу данных, к которой относится роль.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|1|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-rolename-string"></a>-RoleName \<строка >  
 Указывает роль, к которой добавляются члены.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|2|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-databaserole-string"></a>-DatabaseRole \<строка >  
 Указывает объект Microsoft.AnalysisServices.Role, к которому будет добавлен член. Используйте этот параметр, как альтернативу параметрам –Database и –RoleName при необходимости указания роли базы данных через конвейер.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|именованный|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|true (ByPropertyName)|  
|Принимать символы-шаблоны?|false|  
  
### <a name="commonparameters"></a>\<Общие параметры >  
 Этот командлет поддерживает общие параметры: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer и -OutVariable. Дополнительные сведения см. в разделе [Об общих параметрах](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Входы и выходы  
 Входной тип — это тип объектов, которые можно направить в командлет. Тип возвращаемого значения — это тип объектов, возвращаемых командлетом.  
  
|||  
|-|-|  
|Входные данные|Нет.|  
|Выходные данные|Нет|  
  
## <a name="example-1"></a>Пример 1  
  
```  
PS SQLSERVER:\sqlas\localhost\default> add-rolemember –membername “adventure-works\bobh” –database “AdventureWorks” –rolename “Reader”  
```  
  
 Эта команда добавляет учетную запись пользователя домена Windows к роли Reader в базе данных AdventureWorks, которая выполняется на локальном экземпляре по умолчанию.  
  
## <a name="example-2"></a>Пример 2  
  
```  
PS SQLSERVER:\sqlas\localhost\default> $roles= dir .\databases\AWTEST\Roles  
PS SQLSERVER:\sqlas\localhost\default> $roles  
PS SQLSERVER:\sqlas\localhost\default> add-rolemember –membername:“adventure-works\bobh” –databaserole:$roles[0]  
```  
  
 Строка 1 добавляет все роли базы данных из базы данных AWTEST в конвейер. Строка 2, в которой вы набираете $roles, отображает массив ролей. Строка 3 добавляет пользователя Windows adventure-works\bobh в качестве члена первой из ролей в массиве.  
  
## <a name="example-3"></a>Пример 3  
  
```  
PS SQLSERVER:\sqlas\localhost\default\Databases\AWTEST\Roles> $roles=dir  
PS SQLSERVER:\sqlas\localhost\default\Databases\AWTEST\Roles> $roles[0] | Add-rolemember –membername “adventure-works\bobh”  
```  
  
 Эта команда добавляет учетную запись пользователя домена Windows в первую роль в массиве, который создан посредством перечисления дочерних элементов папки Roles в контексте конкретной базы данных (AWTEST).  
  

  
  
