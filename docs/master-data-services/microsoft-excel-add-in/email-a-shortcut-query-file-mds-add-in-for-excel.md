---
title: Отправка файла ярлыка запроса по электронной почте (надстройка MDS для Excel) | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: microsoft-excel-add-in
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 5d46f20a-b04a-45c7-82af-02a2baaabbd7
caps.latest.revision: 7
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 22212ddc6cc612eb398be3eba212ddd64426a3c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="email-a-shortcut-query-file-mds-add-in-for-excel"></a>Отправка файла ярлыка запроса по электронной почте (надстройка MDS для Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]можно отправить кому-либо файл ярлыка запроса по электронной почте, если адресат должен иметь возможность работать с теми же данными, что и вы. Вместо сохранения и отправки листа по электронной почте следует предоставлять общий доступ к запросу.  
  
## <a name="prerequisites"></a>предварительные требования  
 Для выполнения этой процедуры:  
  
-   Необходимо иметь установленную программу Outlook 2010 или более позднюю версию.  
  
-   Необходимо наличие данных, управляемых MDS, на активном листе в Excel.  
  
### <a name="to-send-a-shortcut-query-file"></a>Отправка файла ярлыка запроса  
  
1.  Убедитесь, что данные на листе представлены в формате, который должен использоваться совместно. Дополнительные сведения о фильтрации данных и переупорядочении столбцов см. в разделах [Фильтрация данных перед их экспортом (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md) и [Переупорядочение столбцов (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/reorder-columns-mds-add-in-for-excel.md).  
  
2.  В группе **Сохранение и отправка** нажмите **Отправить запрос**. Открывается сообщение электронной почты с файлом ярлыка запроса во вложении.  
  
## <a name="next-steps"></a>Next Steps  
  
-   Чтобы открыть файл ярлыка запроса, получатель электронного письма должен иметь установленный MDS [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] . Получатель должен дважды щелкнуть файл, чтобы открыть его.  
  
## <a name="see-also"></a>См. также:  
 [Файлы ярлыков запросов (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
  
