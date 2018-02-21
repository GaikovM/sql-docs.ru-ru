---
title: "Объект SqlXmlCommand (управляемые классы SQLXML) | Документы Microsoft"
ms.custom: 
ms.date: 03/16/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: sqlxml
ms.reviewer: 
ms.suite: sql
ms.technology:
- dbe-xml
ms.tgt_pltfrm: 
ms.topic: reference
helpviewer_keywords:
- void ExecuteNonQuery() method
- namespaces property
- Stream ExecuteStream() method
- CommandType property
- RootTag property
- OutputEncoding property
- XmlReader ExecuteXmlReader() method
- Managed Classes [SQLXML], SqlXmlCommand object
- SQLXML Managed Classes, SqlXmlCommand object
- Base Path property
- void ClearParameters() method
- public void ExecuteToStream(Stream outputStream) method
- SqlXmlCommand object
- CommandText property
- XslPath property
- SchemaPath property
- SqlXmlParameter CreateParameter() method
- ClientSideXML property
- CommandStream property
ms.assetid: c1f9e0bb-a89d-4d6a-a96e-289ef516a3a6
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 8b2621c44e57dc83f21db4574e86b0ea16b22041
ms.sourcegitcommit: 37f0b59e648251be673389fa486b0a984ce22c81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
# <a name="sqlxml-managed-classes---sqlxmlcommand-object"></a>Управляемые классы - SQLXML SqlXmlCommand, объект
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Это конструктор для объекта SqlXmlCommand:  
  
```  
public SqlXmlCommand(string cnString)  
```  
  
 , где `cnString` — строка подключения ADO или OLEDB, задающая сервер, базу данных и сведения для входа в систему. Например, `Provider=SQLOLEDB; Server=(local); database=AdventureWorks; Integrated Security=SSPI"`.  
  
 В строке соединения параметр `Provider` должен иметь значение SQLOLEDB, а параметр `Data Provider` в строку поставщика не включается).  
  
 Работающий пример см. в разделе [выполнение запросов SQL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
## <a name="methods"></a>Методы  
 TheSqlXmlCommand объект поддерживает несколько методов, включая следующие методы для выполнения команды:  
  
 void ExecuteNonQuery()  
 Выполняет команду, но не возвращает никакого результата. Этот метод полезен для выполнения команд, которые не являются запросами (то есть команд, которые ничего не возвращают). Например, выполнение диаграммы обновления или дельты, которая изменяет записи, но ничего не возвращает.  
  
 Stream ExecuteStream()  
 Возвращает новый объект потока. Этот метод полезен, если нужно вернуть результаты запроса в новом потоке. Работающий пример см. в разделе [выполнение запросов SQL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 открытые void ExecuteToStream (Stream outputStream)  
 Записывает результаты запроса в существующий поток. Этот метод полезен при наличии потока, к которому нужно добавить (например, результаты запроса, записываемый System.Web.HttpResponse.OutputStream) результаты. Работающий пример см. в разделе [выполнение запросов SQL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 XmlReader ExecuteXmlReader()  
 Возвращает объект XmlReader. Этот метод можно использовать напрямую манипулировать данными в объект XmlReader или подключите к цепочкам архитектуры System.Xml. Дополнительные сведения см. в документации по платформе [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. Работающий пример см. в разделе [выполнение запросов SQL с использованием метода ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md).  
  
 Объект TheSqlXmlCommand также поддерживает эти дополнительные методы:  
  
 SqlXmlParameter CreateParameter()  
 Создает объект SqlXmlParameter. Можно задать значения для *имя* и *значение* параметров этого объекта. Этот метод полезен для передачи параметров команды. Работающий пример см. в разделе [выполнение запросов SQL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 void ClearParameters()  
 Очищает параметры, созданные для данного командного объекта. Это удобно, если нужно выполнить несколько запросов, используя один и тот же командный объект.  
  
## <a name="properties"></a>Свойства  
 Объект SqlXmlCommand также поддерживает следующие свойства:  
  
 ClientSideXml  
 Если для этого свойства задать значение True, преобразование набора строк в XML будет происходить на клиенте, а не на сервере. Это свойство полезно, если нужно переместить вычислительную нагрузку на средний уровень архитектуры. Это свойство также позволяет упаковывать существующие хранимые процедуры в инструкции FOR XML для получения вывода в формате XML.  
  
 SchemaPath  
 Название схемы сопоставления вместе с путем к каталогу (например, C:\x\y\MySchema.xml). Это свойство полезно для задания схемы сопоставления в запросах XPath. Может быть указан абсолютный или относительный путь. Если путь является относительным, для разрешения относительного пути используется базовый путь, указанный в пути к базовой папке. Если базовый путь не указан, то относительный путь разрешается относительно текущего каталога. Работающий пример см. в разделе [доступ к функциональным возможностям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
 XslPath  
 Имя XSL-файла, включая путь к каталогу. Может быть указан абсолютный или относительный путь. Если путь является относительным, для разрешения относительного пути используется базовый путь, указанный в пути к базовой папке. Если базовый путь не указан, то относительный путь разрешается относительно текущего каталога. Работающий пример см. в разделе [применение преобразования XSL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 Базовый путь  
 Базовый путь (путь к каталогу). Это свойство можно использовать для разрешения относительного пути, заданного для XSL-файл (с помощью свойства XslPath), файл схемы сопоставления (с помощью свойства SchemaPath) или ссылка на внешнюю схему в XML-шаблон (указанный с помощью  **Схема сопоставления** атрибута).  
  
 OutputEncoding  
 Указывает кодировку потока, который возвращается при выполнении программы. С помощью этого свойства удобно задавать конкретную кодировку для возвращаемого потока. Примеры распространенных кодировок: UTF-8, ANSI и Unicode. По умолчанию используется UTF-8.  
  
 Пространства имен  
 Разрешает выполнение запросов XPath, использующих пространства имен. Дополнительные сведения о запросах XPath с пространствами имен см. в разделе [выполнение запросов XPath с пространствами имен &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md). Работающий пример см. в разделе [выполнение запросов XPath &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md).  
  
 RootTag  
 Задает единственный корневой элемент для XML-документа, сформированного при выполнении команды. У допустимого XML-документа может быть только один тег на корневом уровне. Если выполненная команда формирует фрагмент XML (у которого нет единственного элемента верхнего уровня), можно задать корневой элемент для возвращаемого фрагмента XML. Работающий пример см. в разделе [применение преобразования XSL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 CommandText  
 Текст команды. Это свойство используется для задания текста команды, которую нужно выполнить. Работающий пример см. в разделе [выполнение запросов SQL &#40; SQLXML управляемые классы &#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 CommandStream  
 Командный поток. Это свойство служит для выполнения команды из файла (например, XML-шаблона). При использовании CommandStream, только **«Шаблон»**, **«UpdateGram»** и **CommandType «DiffGram»** значения поддерживаются. Работающий пример см. в разделе [выполнение файлов шаблонов, используя свойство CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md).  
  
 CommandType  
 Идентифицирует тип команды. Это свойство используется для задания типа команды, которую нужно выполнить. Значения в следующей таблице задают тип команды. Работающий пример см. в разделе [доступ к функциональным возможностям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
|Значение|Description|  
|-----------|-----------------|  
|SqlXmlCommandType.Sql|Выполняет команду SQL (например, `SELECT * FROM Employees FOR XML AUTO`).|  
|SqlXmlCommandType.XPath|Выполняет команду XPath (например, `Employees[@EmployeeID=1]`).|  
|SqlXmlCommandType.Template|Выполняет шаблон XML.|  
|SqlXmlCommandType.TemplateFile|Выполняет файл шаблона, расположенный по указанному пути.|  
|SqlXmlCommandType.UpdateGram|Выполняет диаграмму обновления.|  
|SqlXmlCommandType.Diffgram|Выполняет дельту.|  
  
## <a name="see-also"></a>См. также:  
 [Объект SqlXmlParameter &#40; SQLXML управляемые классы &#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)   
 [Объект SqlXmlAdapter &#40; SQLXML управляемые классы &#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmladapter-object.md)  
  
  