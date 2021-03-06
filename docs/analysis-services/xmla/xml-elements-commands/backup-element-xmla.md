---
title: Резервное копирование элемента (XMLA) | Документы Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e00a4991226779a91e16806dbe15973d946ec69
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574216"
---
# <a name="backup-element-xmla"></a>Элемент Backup (XML для аналитики)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Создает резервную копию базы данных служб Analysis Services в файл резервной копии.  
  
## <a name="syntax"></a>Синтаксис  
  
```xml  
  
<Command>  
   <Backup>  
      <Object>...</Object>  
      <File>...</File>  
      <Security>...</Security>  
      <ApplyCompression>...</ApplyCompression>  
      <AllowOverwrite>...</AllowOverwrite>  
      <Password>...</Password>  
      <BackupRemotePartitions>...</BackupRemotePartitions>  
      <Locations>...</Locations>  
   </Backup>  
</Command>  
```  
  
## <a name="element-characteristics"></a>Характеристики элемента  
  
|Характеристика|Описание|  
|--------------------|-----------------|  
|Тип данных и длина|None|  
|Значение по умолчанию|None|  
|Количество элементов|от 0 до n: необязательный элемент, который может встречаться несколько раз.|  
  
## <a name="element-relationships"></a>Связи элемента  
  
|Связь|Элемент|  
|------------------|-------------|  
|Родительские элементы|[Command](../../../analysis-services/xmla/xml-elements-properties/command-element-xmla.md)|  
|Дочерние элементы|[AllowOverwrite](../../../analysis-services/xmla/xml-elements-properties/allowoverwrite-element-xmla.md), [ApplyCompression](../../../analysis-services/xmla/xml-elements-properties/applycompression-element-xmla.md), [BackupRemotePartitions](../../../analysis-services/xmla/xml-elements-properties/backupremotepartitions-element-xmla.md), [файл](../../../analysis-services/xmla/xml-elements-properties/file-element-xmla.md), [расположения](../../../analysis-services/xmla/xml-elements-properties/locations-element-xmla.md), [ Объект](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md), [пароль](../../../analysis-services/xmla/xml-elements-properties/password-element-xmla.md), [безопасности](../../../analysis-services/xmla/xml-elements-properties/security-element-xmla.md)|  
  
## <a name="remarks"></a>Примечания  
 **Резервного копирования** команда создает резервную копию [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] базы данных, указанной в [объекта](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) файл резервной копии и при необходимости выполняет резервное копирование удаленных секций, удаленных файлов резервной копии элемента. Если **объекта** элемент ссылается на объект, отличный от [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] базы данных, возникает ошибка.  
  
 Данные, попадающие в резервную копию, создаваемую командой **Backup** , зависят от режима хранения, используемого объектами базы данных. В следующей таблице показано, резервная копия каких данных создается в каждом режиме хранения.  
  
|Режим хранения|Данные, попадающие в резервную копию|  
|------------------|-----------------------------------|  
|Многомерный OLAP (MOLAP)|Исходные данные, агрегаты и метаданные|  
|Гибридный OLAP (HOLAP)|Агрегаты и метаданные|  
|Реляционный OLAP (ROLAP)|Метаданные|  
  
 Во время **резервного копирования** команды, совмещаемая блокировка на [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] базы данных, указанной в **объекта** элемент. Совмещаемая блокировка снимается после выполнения команды **Backup** .  
  
 Несколько **резервного копирования** команды могут выполняться параллельно, если они включены в [параллельных](../../../analysis-services/xmla/xml-elements-properties/parallel-element-xmla.md) коллекцию [пакета](../../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md) команды. Коллекция **Parallel** позволяет одновременно создавать несколько резервных копий базы данных.  
  
 Дополнительные сведения о резервном копировании и восстановлении баз данных см. в разделе [резервное копирование, восстановление и синхронизация баз данных &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md).  
  
> [!IMPORTANT]  
>  Пользователь, выполняющий команду резервного копирования, должен иметь разрешение на запись в папку резервного копирования, указанную для каждого копируемого файла. Кроме того, пользователь должен быть членом одной из следующих ролей: роль сервера для экземпляра служб [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] или роль базы данных с разрешениями "Полный доступ (администратор)" в базе данных, для которой создается резервная копия.  
  
## <a name="see-also"></a>См. также
 [Элемент RESTORE &#40;XML для Аналитики&#41;](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)   
 [Элемент Synchronize &#40;XML для Аналитики&#41;](../../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)   
 [Команды &#40;XML для Аналитики&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
