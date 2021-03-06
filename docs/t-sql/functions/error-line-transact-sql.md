---
title: ERROR_LINE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ERROR_LINE
- ERROR_LINE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], line number
- TRY...CATCH [SQL Server]
- line number of error [SQL Server]
- ERROR_LINE function
- CATCH block
ms.assetid: 47335734-0baf-45a6-8b3b-6c4fd80d2cb8
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 44745c0f27527872abd1b63c3fa5b3954328c1be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="errorline-transact-sql"></a>ERROR_LINE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Возвращает номер строки, в которой возникла ошибка, приведшая к активации блока CATCH конструкции TRY…CATCH.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
ERROR_LINE ( )  
```  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **int**  
  
## <a name="return-value"></a>Возвращаемое значение  
 При вызове в блоке CATCH:  
  
-   Возвращает номер строки, в которой возникла ошибка.  
  
-   Возвращает номер строки в процедуре, если ошибка возникла в хранимой процедуре или триггере.  
  
 Возвращает значение NULL в случае вызова вне блока CATCH.  
  
## <a name="remarks"></a>Remarks  
 Эта функция может быть вызвана в любом месте в пределах блока CATCH.  
  
 Функция ERROR_LINE возвращает номер строки, в которой возникла ошибка, независимо от того, сколько раз или в какой области блока CATCH она была вызвана. В этом ее отличие от таких функций, как @@ERROR, которые возвращают номер ошибки в той инструкции, которая непосредственно следует за инструкцией, вызвавшей ошибку, или же в первой инструкции блока CATCH.  
  
 Во вложенных блоках CATCH функция ERROR_LINE возвращает номер строки ошибки, связанной с тем блоком CATCH, в котором она была вызвана. Например, блок CATCH конструкции TRY…CATCH может содержать вложенную конструкцию TRY…CATCH. Внутри вложенного блока CATCH функция ERROR_LINE возвращает номер строки ошибки, вызвавшей вложенный блок CATCH. Если функция ERROR_LINE запущена во внешнем блоке CATCH, она возвращает номер строки ошибки, вызвавшей этот блок CATCH.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-errorline-in-a-catch-block"></a>A. Использование функции ERROR_LINE в блоке CATCH  
 В следующем примере кода приведена инструкция `SELECT`, формирующая ошибку деления на ноль. Возвращается номер строки, в которой возникла ошибка.  
  
```  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
```  
  
### <a name="b-using-errorline-in-a-catch-block-with-a-stored-procedure"></a>Б. Использование функции ERROR_LINE в блоке CATCH с хранимой процедурой  
 В коде следующего примера приведена хранимая процедура, вызывающая ошибку деления на 0. `ERROR_LINE` возвращает номер строки хранимой процедуры, в которой возникла ошибка.  
  
```  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
```  
  
### <a name="c-using-errorline-in-a-catch-block-with-other-error-handling-tools"></a>В. Использование функции ERROR_LINE в блоке CATCH с другими средствами обработки ошибок  
 В следующем примере кода приведена инструкция `SELECT`, формирующая ошибку деления на ноль. Вместе с номером строки, в которой возникла ошибка, возвращаются сведения, касающиеся этой ошибки.  
  
```  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  
