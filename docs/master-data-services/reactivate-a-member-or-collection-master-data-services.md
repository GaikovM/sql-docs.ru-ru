---
title: Повторная активация элемента или коллекции (службы Master Data Services) | Документы Майкрософт
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], reactivating
- consolidated members [Master Data Services], reactivating
- reactivating members [Master Data Services]
- members [Master Data Services], reactivating
- reactivating collections [Master Data Services]
- leaf members [Master Data Services], reactivating
ms.assetid: bb4884c0-3658-4763-92d1-636804278b1c
caps.latest.revision: 11
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: ccc919dc259eb551cc95fff24cf1f3df3ff55a7f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="reactivate-a-member-or-collection-master-data-services"></a>Повторная активация элемента или коллекции (службы Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  В [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]можно повторно активировать элемент, который был:  
  
-   деактивирован в промежуточном процессе;  
  
-   удален в MDS [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)];  
  
-   удален в веб-приложении [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
 При повторной активации элемента восстанавливаются его атрибуты и членство в иерархиях и коллекциях.  
  
 Также можно повторно активировать коллекции. При этом восстанавливаются атрибуты коллекции и принадлежащие к ней элементы.  
  
 При повторной активации коллекции или элемента восстанавливаются все предыдущие транзакции.  
  
## <a name="prerequisites"></a>предварительные требования  
 Для выполнения этой процедуры:  
  
-   В [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]необходимо иметь разрешение на доступ к функциональной области **Управление версиями** .  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в статье [Администраторы (службы Master Data Services)](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-reactivate-a-member-or-collection"></a>Повторная активация элемента или коллекции  
  
1.  На домашней странице [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] нажмите **Управление версиями**.  
  
2.  В строке меню выберите пункт **Транзакции**.  
  
3.  На странице **Транзакции** выберите модель в списке **Модель** .  
  
4.  Выберите версию из списка **Версия** .  
  
5.  На панели **Транзакции** щелкните строку элемента или коллекции, которые необходимо повторно активировать. Эта строка должна иметь состояние **Активный** в столбце **Прежнее значение** и **Выключен** в столбце **Новое значение** .  
  
6.  Нажмите кнопку **Отменить транзакцию**.  
  
7.  В диалоговом окне подтверждения нажмите кнопку **ОК**. Будет добавлена новая транзакция со значением **Активный** в столбце **Новое значение** .  
  
## <a name="see-also"></a>См. также:  
 [Удаление элемента или коллекции (службы Master Data Services)](../master-data-services/delete-a-member-or-collection-master-data-services.md)   
 [Элементы (службы Master Data Services)](../master-data-services/members-master-data-services.md)   
 [Коллекции (службы Master Data Services)](../master-data-services/collections-master-data-services.md)  
  
  
