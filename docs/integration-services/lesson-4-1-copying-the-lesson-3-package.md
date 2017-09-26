---
title: "Шаг 1: Копирование пакета занятия 3 | Документы Microsoft"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-server-2016
ms.reviewer: 
ms.suite: 
ms.technology:
- integration-services
ms.tgt_pltfrm: 
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 0d053786-5203-43f3-a613-27a8dd2bc44a
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 2edcce51c6822a89151c3c3c76fbaacb5edd54f4
ms.openlocfilehash: 8c578fc6154a2a78e223bc1a920669228776e689
ms.contentlocale: ru-ru
ms.lasthandoff: 09/26/2017

---
# <a name="lesson-4-1---copying-the-lesson-3-package"></a>Занятие 4-1-копирование пакета занятия 3
В этой задаче будет создана копия пакета Lesson 3.dtsx, созданного на занятии 3. Если вы не прошли занятие 3, можно добавить пакет задания 3, прилагаемый к учебнику по проекту, скопировать его и работать с копией. Полученная копия впоследствии будет использоваться на протяжении всего занятия 4.  
  
### <a name="to-create-the-lesson-4-package"></a>Создание пакета занятия 4  
  
1.  Если средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools еще не открыты, нажмите кнопку **Пуск**, наведите указатель на пункт **Все программы**, затем на пункт **Microsoft SQL Server**и выберите пункт **SQL Server Data Tools**.  
  
2.  В меню **Файл** последовательно выберите пункты **Открыть**, **Проект или решение**, **Учебник по службам SSIS** и нажмите кнопку **Открыть**, а затем дважды щелкните файл **SSIS Tutorial.sln**.  
  
3.  В обозревателе решений правой кнопкой мыши щелкните файл **Lesson 3.dtsx**и выберите команду **Копировать**.  
  
4.  В обозревателе решений правой кнопкой мыши щелкните элемент **Пакеты служб SSIS**и выберите команду **Вставить**.  
  
    По умолчанию копируемому пакету присваивается имя Lesson 4.dtsx.  
  
5.  Чтобы открыть пакет в обозревателе решений, правой кнопкой мыши щелкните файл **Lesson 4.dtsx** .  
  
6.  Щелкните правой копкой мыши в области вкладки **Поток управления** и выберите пункт **Свойства**.  
  
7.  В окне "Свойства" измените свойство **Name** на **Занятие 4**.  
  
8.  Установите флажок для свойства **ID** , а затем в списке щелкните **<Generate New ID>**.  
  
### <a name="to-add-the-completed-lesson-3-package"></a>Добавление готового пакета занятия 3  
  
1.  Откройте среду [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] , затем откройте проект Tutorial служб SSIS.  
  
2.  В обозревателе решений правой кнопкой мыши щелкните **Пакеты служб SSIS**и выберите команду **Добавить существующий пакет**.  
  
3.  В диалоговом окне **Добавление копии существующего пакета** в разделе **Расположение пакета**выберите пункт **Файловая система**.  
  
4.  Нажмите кнопку обзора **(…)** , перейдите в папку с пакетом Lesson 3.dtsx на компьютере и нажмите кнопку **Открыть**.  
  
    Чтобы загрузить все пакеты занятий этого учебника, выполните следующие действия.  
  
    1.  Перейдите к [образцам продуктов служб Integration Services](http://go.microsoft.com/fwlink/?LinkId=275027).  
  
    2.  Перейдите на вкладку **DOWNLOADS** .  
  
    3.  Щелкните файл SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip.  
  
5.  Скопируйте и вставьте пакет занятия 3, как описано в шагах 3–8 предыдущей процедуры.  
  
## <a name="next-task-in-lesson"></a>Следующая задача занятия  
[Этап 2. Создание поврежденного файла](../integration-services/lesson-4-2-creating-a-corrupted-file.md)  
  
