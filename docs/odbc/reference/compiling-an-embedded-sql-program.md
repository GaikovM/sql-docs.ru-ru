---
title: Компиляция программы Embedded SQL | Документы Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL [ODBC], embedded SQL
- SQL statements [ODBC], embedded SQL
- compiling embedded SQL programs [ODBC]
- embedded SQL [ODBC]
ms.assetid: 9e94146a-5b80-4a01-b586-1e03ff05b9ac
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4e4b89a65475b35b50a968b9497c6d90574c6738
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="compiling-an-embedded-sql-program"></a>Компиляция программы Embedded SQL
Так как встроенной программы SQL содержит сочетание инструкций языка SQL и узлом, его невозможно отправить непосредственно на компилятор для основного языка. Вместо этого он компилируется через многоэтапным процессом. Несмотря на то, что этот процесс отличается от продукта для продукта, шаги примерно, одинаковы для всех продуктов.  
  
 На этом рисунке показаны действия, необходимые для компиляции встроенной программы SQL.  
  
 ![Шаги для компиляции встроенной программы SQL](../../odbc/reference/media/pr02.gif "pr02")  
  
 В компиляции встроенной программы SQL участвуют пять шагов.  
  
1.  Передан встроенной программы SQL предкомпилированного SQL, средство программирования. Предварительной компиляции программа сканирует, находит внедренные инструкции SQL и обрабатывает их. Другой предкомпилированного является обязательным для каждого языка программирования, поддерживаемых в СУБД. СУБД обычно обеспечивают precompilers для одного или нескольких языков, включая C, Pascal, COBOL, Fortran, Ada, PL / I и различных языков сборки.  
  
2.  Компиляции создает два выходных файлов. Первый файл является исходным файлом, удаляются внедренные инструкции SQL. Вместо них предварительной компиляции замещает вызовы собственных СУБД подпрограммы, которые предоставляют ссылку между программой и СУБД во время выполнения. Как правило имена и последовательности вызова из этих подпрограмм известны только для предварительной компиляции и СУБД; они не являются на общедоступном интерфейсе СУБД. Второй файл является копией все внедренные инструкции SQL, используемые в программе. Этот файл называется модуля запрос базы данных или DBRM.  
  
3.  Стандартный компилятор передан источника файла, получаемый из предварительной компиляции для программирования языка (например, компилятор C или COBOL) узла. Компилятор обрабатывает исходный код и формирует код объекта в качестве результата. Обратите внимание, что это никак не связано с СУБД или с помощью SQL.  
  
4.  Компоновщик принимает модули объектов, созданных компилятором, связывает их с различными библиотечные процедуры и создает исполняемую программу. Подпрограммы библиотеки, входящие в программу включают собственные подпрограммы СУБД, описано в шаге 2.  
  
5.  Модуль запросов базы данных, созданный средством предварительной компиляции отправляется к служебной программе специальные привязки. Эта программа проверяет инструкций SQL, выполняет синтаксический анализ, проверяет и позволяет оптимизировать их и затем создает план доступа для каждой инструкции. Результатом является объединенный доступа план для всей программы, представляющий исполняемой версии внедренные инструкции SQL. Служебную программу привязки хранит плана в базе данных, обычно назначение имя прикладной программы, которая будет использовать его. Является ли этот шаг выполняется во время компиляции и время выполнения зависит от СУБД.  
  
 Обратите внимание, что действия, используемые для компиляции встроенной программы SQL тесную корреляции с действия, описанные ранее в [обработки инструкции SQL](../../odbc/reference/processing-a-sql-statement.md). В частности Обратите внимание, предварительной компиляции отделяет инструкции SQL из кода основного языка и привязки программа выполняет синтаксический анализ и проверяет инструкций SQL и создает планы доступа. В СУБД, где шаг 5 выполняется во время компиляции первые четыре шага обработки инструкции SQL выполняются во время компиляции, время последнего шага (выполнение) во время выполнения. Это приводит к при выполнения запроса из таких СУБД очень быстро.
