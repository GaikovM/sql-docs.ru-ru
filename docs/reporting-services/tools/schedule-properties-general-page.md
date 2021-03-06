---
title: Свойства расписания (страница "Общие") | Документация Майкрософт
ms.custom: ''
ms.date: 06/11/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.scheduleproperties.general.f1
ms.assetid: 20e43966-6caf-4972-a2e2-0d9131ac8f51
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: fa9be69c55b98396a8f24ef0ebae32625c79c479
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="schedule-properties-general-page"></a>Свойства расписания (страница «Общие»)
  Создать или изменить общее расписание можно с помощью страницы [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] в [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] . Общие расписания могут использоваться вместо расписаний отчетов или подписок. Изменения, внесенные в расписания, применяются после сохранения расписания. Изменение расписания не оказывает воздействия на выполняемые задания. Если какое-то расписание изменяется во время его использования, всем выполняемым в данный момент отчетам и подпискам, запущенным на основе этого расписания, будет разрешено завершение.  
  
 В одном расписании могут поддерживаться не все комбинации частоты событий. Например, если требуется запускать отчет в 12:00 и в 16:00 каждую пятницу, необходимо создать два ежедневных расписания, запускаемых в пятницу, и для одного указать время запуска 12:00, а для другого — 16:00.  
  
 Обработка по расписанию проводится на базе местного времени сервера отчетов, на котором размещается и обрабатывается расписание.  
  
 Чтобы открыть эту страницу:
 1) Запустите среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
 2) Подключитесь к серверу отчетов.
 3) Разверните папку **Общие расписания** .
 4) Щелкните расписание правой кнопкой мыши и выберите параметр **Свойства**.  
  
> [!NOTE]  
>Эта функция доступна не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , и данная страница не открывается при работе с выпуском, в котором отсутствует эта функция. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
## <a name="options"></a>Параметры  
 **Название**  
 Указывает имя общего расписания.  
  
 **Начать выполнение расписания**  
 Задает начальную дату этого расписания.  
  
 **Остановить выполнения расписания**  
 Задает конечную дату этого расписания.  
  
 **Тип**  
 Задает шаблон повторения, основанный главным образом на часах, днях, неделях или месяцах, либо определяет разовый запуск.  
  
 **Час (шаблон повторений)**  
 Указывает параметры запуска запланированной операции через определенные интервалы в часах (например, запускать отчет каждые 6 часов). Интервал можно указывать в часах и минутах.  
  
 **День (шаблон повторений)**  
 Указывает параметры запуска запланированной операции через определенные интервалы в днях (например, запускать отчет каждые 2 дня). Интервал может определяться в днях, с указанием часа и минуты запуска расписания.  
  
 **Неделя (шаблон повторений)**  
 Указывает параметры запуска запланированной операции через определенный интервал в неделях или когда шаблон повторения основан на неделях (например, запускать отчет через неделю). В недельном расписании время запуска расписания может задаваться с указанием дня, часа и минут.  
  
 **Месяц (шаблон повторений)**  
 Указывает параметры запуска запланированной операции через определенный интервал в месяцах или когда шаблон повторения основан на месяцах. В ежемесячном расписании время его запуска может задаваться с указанием дня, часа и минут. Отдельные месяцы могут исключаться из расписания.  
  
 **Однократно**  
 Указывает расписание, выполняемое один раз в заданные дату и время.  
  
## <a name="see-also"></a>См. также:  
 [Справка F1 по использованию сервера отчетов среде Management Studio](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Подключение к серверу отчетов в среде Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Создание, изменение и удаление расписаний](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)   
 [Расписания](../../reporting-services/subscriptions/schedules.md)  
  
  

