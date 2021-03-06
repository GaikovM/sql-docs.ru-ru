---
title: В режиме интерпретации | Документы Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data updates [ADO], immediate mode
- immediate mode [ADO]
- updating data [ADO], immediate mode
ms.assetid: 31fc53d0-97de-4315-a87b-3bf5cdd1f432
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6acd34e06c4660cff3e90c8ef2dd760996d77259
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="immediate-mode"></a>Режима интерпретации
Действует режим немедленного при **LockType** свойству **adLockOptimistic** или **adLockPessimistic**. В режиме интерпретации распространения изменений, внесенных запись в источник данных как можно скорее объявляется работу над строкой завершения путем вызова **обновление** метод.  
  
## <a name="calling-update"></a>Вызов обновления  
 При перемещении из записи при добавлении или изменении перед вызовом **обновление** метода ADO будет автоматически вызывать **обновление** для сохранения изменений. Необходимо вызвать **CancelUpdate** метод перед навигации, если вы хотите отменить изменения, внесенные в текущую запись или отменить вновь добавленной записи.  
  
 Текущая запись остаются актуальными, после вызова метода **обновление** метод.  
  
## <a name="cancelupdate"></a>CancelUpdate  
 Используйте **CancelUpdate** метод отменить все изменения, внесенные в текущей строке или отменить вновь добавленной строкой. Невозможно отменить изменения в текущей строке или новую строку после вызова метода **обновление** метод, если изменения являются частью транзакции, можно выполнить откат с **RollbackTrans** метод или его части пакетное обновление. В случае пакетного обновления, вы можете отменить **обновление** с **CancelUpdate** или **CancelBatch** метод.  
  
 При добавлении новой строки при вызове **CancelUpdate** метод, текущая строка становится строка, которая была текущей до **AddNew** вызова.  
  
 Если не изменялись текущей строки или добавить новую строку, при вызове **CancelUpdate** метод приводит к ошибке.
