---
title: MSSQL_ENG014157 | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014157 error
ms.assetid: 1a0890cf-d977-43e0-a2ba-9c5ff1a8f856
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 531a4df4ea6212090129b4ed8d172c16f2cfc11e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng014157"></a>MSSQL_ENG014157
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Сведения о сообщении  
  
|||  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|14157|  
|Источник события|MSSQLSERVER|  
|Компонент|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Подписка, созданная подписчиком "%s" на издателе "%s", истекла и была удалена.|  
  
## <a name="explanation"></a>Объяснение  
 Подписчик должен синхронизироваться с издателем в течение времени, указанного в сроке хранения публикации. Если подписчик не будет синхронизирован в указанный период, то срок действия подписки истечет. Дополнительные сведения см. в разделе [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="user-action"></a>Действие пользователя  
 Прежде чем подписчик сможет снова получать изменения данных, подписка должна быть создана и инициализирована заново:  
  
-   Дополнительные сведения о создании подписок см. в статье [Подписка на публикации](../../relational-databases/replication/subscribe-to-publications.md).  
  
-   Сведения об инициализации подписок см. в [этой статье](../../relational-databases/replication/initialize-a-subscription.md).  
  
 Если в базе данных подписки содержатся изменения, которые не были синхронизированы с издателем, при помощи [tablediff Utility](../../tools/tablediff-utility.md) можно определить, какие строки в базах данных публикации и подписки отличаются.  
  
 Срок действия подписок можно продлить, увеличив срок хранения публикации. Не рекомендуется задавать слишком большое значение, поскольку это может привести к хранению большего объема данных и метаданных, что оказывает отрицательное влияние на производительность. Дополнительные сведения см. в разделе [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="see-also"></a>См. также:  
 [Справочник по ошибкам и событиям (репликация)](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
