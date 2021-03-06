---
title: Шаг 3. Изменение диспетчера соединений с неструктурированными файлами | Документы Майкрософт
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
ms.assetid: 459e3995-2116-4f15-aaa2-32f26113869c
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 177ea907a3cefffc33d0dac230b3d2da78aaf5a7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-3---modifying-the-flat-file-connection-manager"></a>Занятие 2-3. Изменение диспетчера соединений с неструктурированными файлами
В этом задании требуется модифицировать диспетчер соединений с неструктурированными файлами, созданный и настроенный на занятии 1. При создании диспетчера соединений неструктурированных файлов он был настроен на статическую загрузку отдельного файла. Чтобы диспетчер соединений с неструктурированными файлами мог последовательно загружать файлы, необходимо изменить свойство ConnectionString диспетчера соединений таким образом, чтобы он принимал пользовательскую переменную `User:varFileName`, содержащую путь к файлу, который должен быть загружен в процессе выполнения.  
  
Изменив диспетчер соединений таким образом, чтобы для заполнения свойства ConnectionString использовалось значение пользовательской переменной `User::varFileName`, можно добиться того, чтобы диспетчер соединений подключался к различным неструктурированным файлам. При выполнении каждой итерации контейнера «цикл по каждому элементу» будет динамически обновляться переменная `User::varFileName` . Обновление переменной в свою очередь вызовет соединение диспетчера со следующим неструктурированным файлом и обработку следующего набора данных задачей потока данных.  
  
### <a name="to-configure-the-flat-file-connection-manager-to-use-a-variable-for-the-connection-string"></a>Настройка диспетчера соединений с неструктурированными файлами для использования переменной в качестве строки соединения  
  
1.  На панели **Диспетчеры соединений** щелкните правой кнопкой мыши **Образец источника данных "неструктурированный файл"** и выберите пункт **Свойства**.  
  
2.  В окне свойств щелкните в пустой ячейке **Выражения**, а затем нажмите кнопку с многоточием **(…)**.  
  
3.  В диалоговом окне **Редактор выражений свойств** в столбце **Свойство** введите или выберите **ConnectionString**.  
  
4.  В столбце **Выражение** нажмите кнопку с многоточием **(…)** , чтобы открыть диалоговое окно **Построитель выражений** .  
  
5.  В диалоговом окне **Построитель выражений** разверните узел **Переменные** .  
  
6.  Перетащите переменную **User::varFileName**в поле **Выражение** .  
  
7.  Нажмите кнопку **ОК** , чтобы закрыть диалоговое окно **Построитель выражений** .  
  
8.  Еще раз нажмите кнопку **ОК** , чтобы закрыть диалоговое окно **Редактор выражений свойств** .  
  
## <a name="next-lesson-task"></a>Следующая задача занятия  
[Шаг 4. Проверка учебного пакета, созданного на занятии 2](../integration-services/lesson-2-4-testing-the-lesson-2-tutorial-package.md)  
  
  
  
