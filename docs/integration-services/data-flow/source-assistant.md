---
title: Помощник по созданию источника | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sourceassistant.f1
- sql13.dts.designer.addNewSource.f1
ms.assetid: 5ca9d821-7d61-4727-9133-5f9cb485c7f3
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c86b1de022402bf4802da644977cc0e07532c345
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="source-assistant"></a>Помощник источника
  Компонент «Помощник по источнику» позволяет создать компонент источника и диспетчер соединений. Компонент расположен в разделе **Избранное** панели инструментов служб SSIS.  
  
> [!NOTE]  
>  Помощник по источнику заменяет проект соединений служб Integration Services и соответствующий мастер.  
  
## <a name="add-a-source-with-source-assistant"></a>Добавление источника с помощью помощника по созданию источника
В этом разделе приведены шаги по добавлению нового источника с использованием помощника по созданию источника, а также список параметров, доступных в диалоговом окне **Добавление нового источника**, которое открывается при перетаскивании помощника по созданию источника в конструктор служб SSIS.  

1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте пакет служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , в который нужно добавить компонент источника.  
  
2.  Перетащите компонент **Помощник по источнику** из области элементов SSIS на вкладку **Поток данных** . Появится диалоговое окно **Добавление нового источника** . В следующем разделе приведены подробные сведения о параметрах, доступных в этом диалоговом окне.  
  
3.  Выберите тип назначения в списке **Типы** .  
  
4.  Выберите существующий диспетчер подключений в списке **Диспетчеры подключений** или создайте новый, выбрав пункт **\<Создать>**.  
  
5.  Если был выбран существующий диспетчер соединений, нажмите кнопку **ОК** , чтобы закрыть диалоговое окно **Добавление нового назначения** . Должны быть показаны назначение и диспетчеры соединений, добавленные в поток данных.  
  
6.  Если вы выбрали пункт **\<Создать>** для создания диспетчера подключений, должно открыться диалоговое окно **Диспетчер подключений**, где можно указать параметры подключения. После завершения создания нового диспетчера соединений в конструкторе служб SSIS будут показаны назначение и диспетчер соединений.  

## <a name="add-new-source-dialog-box"></a>Диалоговое окно "Добавление нового источника"
В таблице ниже приведен список параметров, доступных в диалоговом окне **Добавление нового источника**.  
  
|Параметр|Description|  
|------------|-----------------|  
|Типы|Выберите тип источника, к которому нужно подключиться.|  
|Диспетчеры соединений|Выберите существующий диспетчер подключений или щелкните **\<Создать>**, чтобы создать его.|  
|Показывать только установленные|Укажите, следует ли просматривать только установленные источники.|  
|OK|Нажмите, чтобы сохранить изменения и открыть все последующие диалоговые окна для настройки дополнительных параметров.| 
  
