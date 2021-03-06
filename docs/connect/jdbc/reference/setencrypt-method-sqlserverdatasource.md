---
title: Метод setEncrypt (SQLServerDataSource) | Документы Microsoft
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
- setEncrypt Method (SQLServerDataSource)
apilocation:
- setEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 0c85a9c1-f27c-457e-8461-403cc03e2d17
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 723c5c5402fb32f0ad74bf303dd7c682b88d1c84
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="setencrypt-method-sqlserverdatasource"></a>Метод setEncrypt (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Наборы **логическое** значение, указывающее, включено ли свойство encrypt.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public void setEncypt(boolean encrypt)  
```  
  
#### <a name="parameters"></a>Параметры  
 *Шифрование*  
  
 **значение true,** включено ли шифрование Secure Sockets Layer (SSL) между клиентом и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. В противном случае — **false**.  
  
## <a name="remarks"></a>Замечания  
 Если свойству encrypt присвоено **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] гарантирует, что [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] использует шифрование SSL для всех данных, передаваемых между клиентом и сервером, если сервер имеет установленный сертификат. Значение по умолчанию — **false**.  
  
 При выполнении SSL-подтверждения драйвер JDBC определяет виртуальную машину Java (JVM), на которой он запущен.  
  
 Если свойству encrypt присвоено **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] использует для согласования SSL-шифрования с поставщик безопасности JSSE в JVM по умолчанию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Поставщик безопасности по умолчанию может не поддерживать все функции, необходимые для успешного согласования SSL-шифрования. Например, поставщик безопасности по умолчанию может не поддерживать размер открытого ключа RSA, используемого в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] SSL-сертификат. В этом случае поставщик безопасности по умолчанию может вызвать ошибку, в результате которой драйвер JDBC разорвет соединение. Чтобы устранить эту неполадку, выполните одно из следующих действий.  
  
-   Настройка [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] с меньшим открытым ключом RSA сертификата сервера  
  
-   Настройте в JVM использование другого поставщика безопасности JSSE в «\<java-home > / lib/security/java.security» файл свойств безопасности  
  
-   Используйте другой JVM  
  
 Если свойство шифрования не задано или равно **false**, драйвер не будет применять [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] поддержку SSL-шифрования. Если [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] экземпляр не настроен на принудительное шифрование протокола SSL, то соединение устанавливается без применения шифрования. Если [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] экземпляре настроено принудительное SSL-шифрования, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] будет автоматически включить SSL-шифрование при запуске на правильно настроенной виртуальной машины Java, иначе соединение будет разорвано и драйвер возвращает ошибку.  
  
## <a name="see-also"></a>См. также  
 [Элементы SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Класс SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
