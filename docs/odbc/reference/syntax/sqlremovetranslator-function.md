---
title: Функция SQLRemoveTranslator | Документы Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 495348c07ea707907f664358daea510951e7b208
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="sqlremovetranslator-function"></a>Функция SQLRemoveTranslator
**Соответствия**  
 Появился в версии: ODBC 3.0  
  
 **Сводка**  
 **SQLRemoveTranslator** удаляет сведения о трансляции из раздела Odbcinst.ini сведения о системе и уменьшает счетчик использования компонента транслятора на 1.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Аргументы  
 *lpszTranslator*  
 [Вход] Имя преобразователя, зарегистрированным в ключе Odbcinst.ini информация о системе.  
  
 *lpdwUsageCount*  
 [Выход] Загруженность переводчик после вызова этой функции.  
  
## <a name="returns"></a>Возвращает  
 Функция возвращает значение TRUE, если он прошел успешно, FALSE в случае неудачи. Если не существует запись сведений о системе при вызове этой функцией, функция возвращает значение FALSE.  
  
## <a name="diagnostics"></a>Диагностика  
 Когда **SQLRemoveTranslator** возвращает значение FALSE, связанный с ним  *\*pfErrorCode* значение можно получить путем вызова **SQLInstallerError**. В следующей таблице перечислены  *\*pfErrorCode* значения, которые могут быть возвращены **SQLInstallerError** и описание каждого из них в контексте этой функции.  
  
|*\*pfErrorCode*|Ошибка|Описание|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Установщик Общие ошибки|Произошла ошибка для которого нет ошибок определенного установщика.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Компонент не найден в реестре|Программа установки не удалось удалить сведения транслятор, поскольку не существует в реестре или не найден в реестре.|  
|ODBC_ERROR_INVALID_NAME|Недопустимое имя драйвера или преобразователь|*LpszTranslator* недопустимый аргумент.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Не удалось увеличить или уменьшить загруженность в компонент|Программа установки не удалось уменьшить значение счетчика использования драйвера.|  
|ODBC_ERROR_OUT_OF_MEM|Недостаточно памяти|Программе установки не удалось выполнить функцию из-за нехватки памяти.|  
  
## <a name="comments"></a>Комментарии  
 **SQLRemoveTranslator** дополняет [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) функции и обновления использование компонентов количество сведений о системе. Эта функция должна вызываться только из программы установки приложения.  
  
 **SQLRemoveTranslator** уменьшит счетчик использования компонента на 1. Если счетчик использования компонента становится равным 0, будут удалены записи переводчик сведений о системе. Транслятор запись находится в папке сведения о системе, под именем переводчик:  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** фактически не удалять файлы. Вызывающая программа отвечает за удаление файлов и обслуживание счетчик использования файла. Только после достижения счетчик использования компонента и число файлов использования ноль — это файл, в физически удаляются. Можно удалить некоторые файлы, в компоненте и другие не удален, в зависимости от того, используются ли файлы другим приложениям, которые увеличивается на единицу счетчик использования файла.  
  
 **SQLRemoveTranslator** также вызывается как часть процесса обновления. Если приложение обнаруживает, что он должен выполнить обновление, и ранее установил драйвера, драйвер следует удалить и переустановить. **SQLRemoveTranslator** сначала должен вызываться для уменьшения числа использования компонента, а затем **SQLInstallTranslatorEx** должен вызываться увеличивается счетчик использования компонента. Программа установки должна физически замените старые файлы новых файлов. Счетчик использования файл остается без изменений и других приложений, использующих более старые версии файлов теперь будут использовать новую версию.  
  
## <a name="related-functions"></a>Связанные функции  
  
|Сведения о|См.|  
|---------------------------|---------|  
|Установка переводчика|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
