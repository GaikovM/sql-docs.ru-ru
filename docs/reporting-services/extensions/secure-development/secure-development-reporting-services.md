---
title: Разработка безопасных приложений (Reporting Services) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- development security [Reporting Services]
- security [Reporting Services], development
- security [Reporting Services], strategies
ms.assetid: 12161a6c-b93b-4312-9d27-0c922561eb9b
caps.latest.revision: 15
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 00bd472c52b756f3b54271a0046e0eb19b400c51
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="secure-development-reporting-services"></a>Разработка безопасных приложений (службы Reporting Services)
  Платформа [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] обеспечивает надежную систему безопасности, которая может выполнять код в строго определенных рамках заданного администратором контекста безопасности. Службы [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] используют систему безопасности платформы [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], известную как защита доступа к коду (или защита доступа на основе признаков). Если в защите доступа доступа к коду пользователю предоставлен доверенный доступ к ресурсу, но выполняемый пользователем код не является доверенным, то доступ к ресурсу будет запрещен.  
  
 Система безопасности, основанная на коде, в противоположность отдельным пользователям, позволяет распространять правила безопасности на пользовательские сборки или модули данных, доставки, подготовки к просмотру и безопасности, создаваемые разработчиками для служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Модуль может выполняться любым количеством пользователей служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], которые были неизвестны на этапе разработки. Пользовательским сборкам или модулям необходимы конкретные политики безопасности служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Эти политики безопасности представлены в платформе [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] как типы. Дополнительные сведения о защите доступа к коду см. в разделе «Защита доступа к коду» в документации по платформе [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="in-this-section"></a>в этом разделе  
 [Управление доступом для кода в службах Reporting Services](../../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)  
 Основы защиты доступа к коду и настройка политики для пользовательских сборок и модулей в службах [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Основные сведения о политиках безопасности](../../../reporting-services/extensions/secure-development/understanding-security-policies.md)  
 Описание различных типов сборок в службах [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] и влияние защиты доступа к коду на разрешения кода.  
  
 [Использование файлов политики безопасности служб Reporting Services](../../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md)  
 Описание различных компонент служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] и соответствующих файлов конфигурации политик.  
  
  
