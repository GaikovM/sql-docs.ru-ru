---
title: Шаг 4. Проверка учебного пакета, созданного на занятии 5 | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 5215b77d-c2ec-4b25-a3de-ca49ea197d74
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 16b703b9d9c0d30d9d355735813b0934c24e9c00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-5-4---testing-the-lesson-5-tutorial-package"></a>Занятие 5–4. Проверка учебного пакета, созданного на занятии 5
В ходе выполнения пакету будет присвоено значение свойства **Directory** из переменной, обновленной во время выполнения, а не имя исходного каталога, которое было указано при создании пакета. Значение этой переменной заполняется из файла SSISTutorial.dtsConfig.  
  
Чтобы убедиться, что пакет обновляет свойство Directory новым значением во время выполнения, выполните пакет. Поскольку в новый каталог были скопированы только три файла образцов данных, поток данных будет запущен только три раза, а не 14 раз в соответствии с количеством файлов в исходном каталоге.  
  
## <a name="checking-the-package-layout"></a>Проверка макета пакета  
Прежде чем тестировать пакет, следует убедиться, что поток управления и поток данных пакета, созданного на занятии 5, содержит объекты, показанные на следующих диаграммах. Поток управления должен быть таким же, как и поток управления в занятии 4. Поток данных должен быть идентичен потоку данных в занятии 4.  
  
**Поток управления**  
  
![Поток управления в пакете](../integration-services/media/task4lesson2control.gif "Поток управления в пакете")  
  
**Поток данных**  
  
![Поток данных в пакете](../integration-services/media/task9lesson1data.gif "Поток данных в пакете")  
  
### <a name="to-test-the-lesson-5-tutorial-package"></a>Проверка пакета занятия 5 учебника  
  
1.  В меню **Отладка** выберите команду **Начать отладку**.  
  
2.  По окончании работы пакета в меню **Отладка** выберите команду **Прекратить отладку**.  
  
## <a name="next-lesson"></a>Следующее занятие  
[Занятие 6. Использование параметров в модели развертывания проекта в службах SSIS](../integration-services/lesson-6-using-parameters-with-the-project-deployment-model-in-ssis.md)  
  
  
  
