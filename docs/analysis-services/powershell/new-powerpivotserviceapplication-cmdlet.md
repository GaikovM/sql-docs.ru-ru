---
title: Командлет New-PowerPivotServiceApplication | Документы Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5be9914dfb9361af2067cbef469f2ef6737b4fb7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="new-powerpivotserviceapplication-cmdlet"></a>Командлет «New-PowerPivotServiceApplication»
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Создает новое приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  

>[!NOTE] 
>В этой статье может содержать устаревшие сведения и примеры. С помощью командлета Get-Help для последней версии.
  
 **Применимо для следующих объектов:** SharePoint 2010 и SharePoint 2013.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
New-PowerPivotServiceApplication [-ServiceApplicationName] <string> [-DatabaseServerName] <string> [-DatabaseName] <string> [-AddToDefaultProxyGroup <switch>] [<CommonParameters>]  
```  
  
## <a name="description"></a>Описание  
 Командлет New-PowerPivotServiceApplication создает новое приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в ферме. В ферме следует определить по крайней мере одно приложение службы PowerPivot, которое должно быть членом группы служб-посредников по умолчанию. При необходимости можно создавать дополнительные приложения службы, если требуется изменить свойства или параметры конфигурации. Дополнительные приложения службы должны быть членами пользовательских групп соединений со службами. В группе служб-посредников по умолчанию может быть только одно приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Приложение службы PowerPivot создается с использованием конфигурации по умолчанию. Чтобы настроить свойства конфигурации, используйте командлет Set-PowerPivotServiceApplication.  
  
## <a name="parameters"></a>Параметры  
  
### <a name="-serviceapplicationname-string"></a>-ServiceApplicationName \<строка >  
 Задает отображаемое имя приложения службы.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|0|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-databaseservername-string"></a>-DatabaseServerName \<строка >  
 Определяет экземпляр реляционной базы данных компонента SQL Server, на котором размещается база данных приложений. По умолчанию вы можете использовать сервер баз данных фермы либо выбрать другой сервер баз данных, на котором имеются права на создание базы данных.  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|1|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-databasename-string"></a>Имя базы данных - \<строка >  
 Указывает имя реляционной базы данных SQL Server, которая хранит данные приложений. Укажите имя, соответствующее приложению, чтобы было проще понять его цель. Для вновь создаваемого приложения можно создать новую базу данных или указать существующую базу данных приложений служб [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
|||  
|-|-|  
|Обязательно?|true|  
|Позиция?|2|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="-addtodefaultproxygroup-switch"></a>-AddToDefaultProxyGroup \<переключения >  
 Создает подключение к службе [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в группе подключений по умолчанию. Взаимосвязи между веб-приложениями и приложениями служб определяются членством в этой группе. Все веб-приложения, которые подписаны на группу подключений служб по умолчанию, могут использовать приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , добавленное в группу. Хотя в ферме может быть несколько приложений службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , только одно приложение может быть членом группы подключений служб по умолчанию.  
  
 Если уже имеется приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , которое является членом группы посредников по умолчанию, следует задать значение AddToDefaultProxyGroup:$false для вновь создаваемого приложения. Новое приложение службы нужно добавить в пользовательскую группу соединений служб.  Для этой цели можно использовать встроенные командлеты SharePoint.  Команда Get-SPServiceApplicationProxyGroup возвращает список групп соединений служб, определенных в ферме.  
  
|||  
|-|-|  
|Обязательно?|false|  
|Позиция?|именованный|  
|Значение по умолчанию||  
|Принимать входные данные конвейера?|false|  
|Принимать символы-шаблоны?|false|  
  
### <a name="commonparameters"></a>\<Общие параметры >  
 Этот командлет поддерживает общие параметры: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer и OutVariable. Дополнительные сведения см. в разделе [Об общих параметрах](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Входы и выходы  
 Входной тип — это тип объектов, которые можно направить в командлет. Тип возвращаемого значения — это тип объектов, возвращаемых командлетом.  
  
|||  
|-|-|  
|Входные данные|Нет.|  
|Выходные данные|Нет.|  
  
## <a name="example-1"></a>Пример 1  
  
```  
C:\PS>New-PowerPivotServiceApplication -ServiceApplicationName "PowerPivot Service Application" -DatabaseServerName "AdvWorks-SRV01\PowerPivot" -DatabaseName "PowerPivotServiceApp1" -AddtoDefaultProxyGroup:$true  
```  
  
 В этом примере создается новое приложение службы. База данных приложения службы создается на сервере баз данных с именем AdvWorks-SRV01, который был установлен в качестве именованного экземпляра [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Это является обычной конфигурацией для большинства установок [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint. Для создания базы данных нужно обладать разрешениями dbcreator на соответствующем экземпляре SQL Server. Необходимо также быть членом роли db_owner в базе данных конфигурации SharePoint. Поскольку это первое приложение службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в ферме, оно должно быть членом группы прокси-сервера по умолчанию.  
  
  
