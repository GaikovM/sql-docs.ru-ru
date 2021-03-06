---
title: Задача скачивания BLOB-объектов Azure | Документы Майкрософт
ms.custom: ''
ms.date: 07/25/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e9c28bd47c2657fad3064329b1aac9ea5c3d01d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="azure-blob-download-task"></a>Задача скачивания BLOB-объектов Azure
Задача скачивания BLOB-объектов Azure позволяет пакету служб SSIS скачивать файлы из хранилища BLOB-объектов Azure.

Чтобы добавить **задачу скачивания BLOB-объектов Azure**, перетащите ее в конструкторе служб SSIS и дважды щелкните или щелкните правой кнопкой мыши и выберите **Изменить** , чтобы вызвать диалоговое окно **Редактор задач скачивания BLOB-объектов Azure** .  
  
 **Задача скачивания BLOB-объектов Azure** входит в состав [пакета дополнительных компонентов SQL Server Integration Services (SSIS) для Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 Следующая таблица содержит описание полей этого диалогового окна.  
  
|||  
|-|-|  
|**Поле**|**Описание**|  
|AzureStorageConnection|Создайте новый или укажите существующий диспетчер подключений службы хранилища Azure, который ссылается на учетную запись хранения Azure, указывающую на место размещения файлов BLOB-объектов.|  
|BlobContainer|Указывает имя контейнера BLOB-объектов, который содержит скачиваемые файлы BLOB-объектов.|  
|BlobDirectory|Указывает каталог больших двоичных объектов, который содержит скачиваемые файлы BLOB-объектов. Каталог больших двоичных объектов — это виртуальная иерархическая структура.|  
|LocalDirectory|Указывает локальный каталог для хранения скачанных файлов BLOB-объектов.|  
|FileName|Указывает фильтр имен для выбора файлов с указанным шаблоном имен. Например, `MySheet*.xls\*` включает такие файлы, как `MySheet001.xls` и `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Указывает фильтр по диапазону времени. Включаются файлы, измененные после **TimeRangeFrom** и до **TimeRangeTo**.|  
  
  
