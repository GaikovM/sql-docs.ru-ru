---
title: Расширенный выбор объектов (MySQLToSQL) | Документы Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 390ef0c2-107c-4443-9495-80f35f22d168
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ff84e14790640ccd117412ec734867d4d3837983
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="advanced-object-selection--mysqltosql"></a>Выбор дополнительных объектов (MySQLToSQL)
**Расширенный объект раздела** диалоговое окно позволяет отфильтровать объекты базы данных с помощью строки и подстроки в имени объекта, а затем выберите или отмените выделение этих объектов. SSMA выполняет преобразование и миграцию операций на выбранных объектов.  
  
Чтобы открыть это диалоговое окно, щелкните правой кнопкой мыши в обозревателе метаданных и выберите **Расширенный выбор объекта**.  
  
При первом открытии диалоговое окно, нажмите кнопку **Показать элементы подкатегорий** для отображения всех объектов, имеющих метаданные, загруженный в проект. После этого можно вводить строки для фильтрации элементов. Например введите строку «company», чтобы отобразить все элементы, имена которых содержат эту строку.  
  
Прежде чем использовать это диалоговое окно, можно принудительно SSMA для загрузки всех метаданных, преобразование схем или сохранении проекта.  
  
## <a name="options"></a>Параметры  
**Проверьте все элементы**  
Добавляет флажок рядом с пунктами. Эти элементы будут выбираться сразу в обозревателе метаданных.  
  
**Снимите флажки всех элементов**  
Снимает флажок рядом с пунктами. В обозревателе метаданных будет немедленно очистить эти элементы.  
  
**Режим списка**  
Отображение отфильтрованных элементов в списке.  
  
**Режим просмотра таблицы**  
Отображение отфильтрованных элементов в таблице.  
  
**Отображаются только загруженных элементов.**  
Переключение отображения категорий или элементов. При выборе этой кнопки SSMA показывает все элементы, которые соответствуют критериям фильтра и те, которые были ранее загружены. Если эта кнопка не установлен, SSMA показаны папки категории.  
  
**Фильтр**  
Введите строку, которую вы хотите использовать для фильтрации элементов. Например, чтобы найти все доступные элементы, которые содержат строку «ID», в имя элемента, введите строку «ID» в **фильтра** поле.  
  
Если элементы условия фильтра, категории или элементы будут отображаться при вводе строки. Чтобы увидеть совпадающие элементы, рекомендуется нажать кнопку **отображаются только загрузить элементы** кнопки.  
  
**Очистить фильтр**  
Очищает **фильтра** поле.  
  
