---
title: Спецификация VDI резервного копирования — SQL Server для Linux | Документы Microsoft
description: Спецификация интерфейса виртуального устройства резервного копирования SQL Server.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 03/17/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: 1dab0dcc403a7e0f85cd78e69e9461ef0d566b0c
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>SQL Server на клиенте для Linux VDI спецификации SDK

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

В этом документе описываются интерфейсы, предоставляемые SQL Server на клиентский интерфейс (VDI) пакет SDK для Linux виртуального устройства. Независимые поставщики (ISV) можно использовать виртуальный резервной копии устройства приложения программный интерфейс (API) для интеграции SQL Server в свои продукты. В общем случае VDI в Linux ведет себя аналогично VDI в Windows со следующими изменениями:

- Общая память Windows становится POSIX общей памяти.
- Семафоры Windows становятся POSIX семафоров.
- Типы Windows, например HRESULT и DWORD изменяются в эквиваленты целое число со знаком.
- COM-интерфейсы, удаляются и заменяются парой классов C++.
- SQL Server в Linux не поддерживает именованные экземпляры, поэтому будут удалены ссылки на имя экземпляра. 
- Общая библиотека реализована в libsqlvdi.so, установленной на /opt/mssql/lib/libsqlvdi.so

Данный документ является дополнением к **vbackup.chm** , сведения о VDI в Windows спецификации. Загрузить [спецификацию VDI Windows](http://www.microsoft.com/download/details.aspx?id=17282).

Также просмотрите образец решения в среде VDI резервного копирования на [репозитории GitHub образцы SQL Server](http://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux).

## <a name="user-permissions-setup"></a>Настройка разрешений пользователя

В Linux пользователь, создающий их и их группы по умолчанию принадлежат примитивы POSIX. Для объектов, созданных в SQL Server они по умолчанию владельцем будет mssql пользователь и группа mssql. Чтобы разрешить общий доступ к SQL Server и клиент VDI, один из следующих двух методов рекомендуется использовать:

1. Запуск от имени пользователя mssql клиент VDI
   
   Выполните следующую команду, чтобы переключиться на пользователя mssql:
   
   ```bash
   sudo su mssql
   ```

2. Добавьте пользователя mssql vdiuser группы и vdiuser группу mssql.
   
   Выполните следующие команды:

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   Перезапустите сервер, чтобы взять новые группы для SQL Server и vdiuser

## <a name="client-functions"></a>Клиентские функции

В этой главе содержит описания каждого из клиентских функций. Описания содержат следующие сведения:

- Назначение функции
- Синтаксис функции
- Список параметров
- Возвращаемые значения
- Примечания

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**Назначение** эта функция создает набор виртуального устройства.

**Синтаксис**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| | **name** | Это определяет набор виртуального устройства. Необходимо следовать правилам для имен, используемые CreateFileMapping(). Любой символ, за исключением обратной косой черты (\) могут быть использованы. Это строка символов. Рекомендуется использовать префикса строка с именем продукта или компании и имя базы данных пользователя. |
| |**cfg** | Это конфигурация для набора виртуального устройства. Дополнительные сведения см. в разделе «Конфигурация» далее в этом документе.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** | Функция выполнена успешно. |
| |**VD_E_NOTSUPPORTED** |Недопустимый один или несколько полей в конфигурации или в противном случае не поддерживается. |
| |**VD_E_PROTOCOL** | Виртуальное устройство, установите уже существует.

**Примечания** создать метод должен вызываться только один раз для одной операции резервного КОПИРОВАНИЯ или восстановления. После вызова метода Close, клиент может повторно использовать интерфейс для создания другого набора виртуального устройства.

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**Назначение** эта функция используется сервером для настройки набора виртуального устройства.
**Синтаксис**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| | **Время ожидания** | Это время ожидания в миллисекундах. Используйте БЕСКОНЕЧНЫМИ или любое отрицательное целое число для предотвращения тайм-аута.
| | **cfg** | После успешного выполнения содержит конфигурации, выбранной на сервере. Дополнительные сведения см. в разделе «Конфигурация» далее в этом документе.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** | Возвращает конфигурацию.
| |**VD_E_ABORT** |SignalAbort был вызван.
| |**VD_E_TIMEOUT** |Функция, истекло время ожидания.

**Примечания** эта функция блокирует в состоянии минимального. После успешного вызова можно открыть устройства набора виртуального устройства.


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**Назначение** эта функция открывает одно из устройств в наборе виртуального устройства.
**Синтаксис**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| | **name** |Это определяет набор виртуального устройства.
| | **ppVirtualDevice** |Если функция выполняется успешно, возвращается указатель на виртуальном устройстве. Это устройство используется для GetCommand и CompleteCommand.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Функция выполнена успешно.
| |**VD_E_ABORT** | Прерывание выполнения запрашивалось.
| |**VD_E_OPEN** |  Все устройства должны быть открыты.
| |**VD_E_PROTOCOL** |  Набор не находится в состоянии инициализации или данное устройство уже открыт.
| |**VD_E_INVALID** |Недопустимое имя устройства. Он не является одним из имен, известно, составляют набор.

**Примечания** VD_E_OPEN могут быть возвращены без проблем. Клиент может вызвать OpenDevice посредством цикл, пока не будет возвращено этот код.
Если несколько устройств настроен, например *n* устройств, будет возвращать набор виртуального устройства *n* интерфейсы уникальный устройства.

`GetConfiguration` Функция может использоваться подождать, пока эти устройства могут быть открыты.
Если эта функция не завершилась, через ppVirtualDevice возвращается значение null.
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**Назначение** эта функция используется для получения следующей команды в очереди на устройство. При запросе, эта функция ожидает следующей команды.

**Синтаксис**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**Время ожидания** |Это время ожидания в миллисекундах. Используйте INFINTE для неограниченного времени ожидания. Укажите 0 для опроса для команды. VD_E_TIMEOUT возвращается в том случае, если ни одной команды в настоящее время недоступно. Если возникает тайм-аут, клиент решает следующее действие.
| |**Timeout** |Это время ожидания в миллисекундах. Используйте INFINTE или отрицательное значение для неограниченного времени ожидания. Укажите 0 для опроса для команды. VD_E_TIMEOUT возвращается в том случае, если команда не доступна до истечения времени ожидания. Если возникает тайм-аут, клиент решает следующее действие.
| |**ppCmd** |Если команда успешно возвращен, параметр Возвращает адрес команду для выполнения. Память — только для чтения. При выполнении команды подпрограмму CompleteCommand передается этот указатель. Дополнительные сведения о каждой из команд. в разделе «Команды» далее в этом документе.
        
| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Команды, которые были выбраны.
| |**VD_E_CLOSE** |Устройства был закрыт сервером.
| |**VD_E_TIMEOUT** |Команда не была доступна и истечения тайм-аута.
| |**VD_E_ABORT** |Клиент или сервер использовал SignalAbort выключение.

**Примечания** возвращается при VD_E_CLOSE, SQL Server закрыл устройства. Это является частью обычной завершения работы. После закрытия всех устройств, клиент вызывает ClientVirtualDeviceSet::Close, чтобы закрыть виртуальное устройство набора.
Если эта процедура должна заблокировать ожидания команды, поток остается в условие минимального.

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**Назначение** эта функция используется для уведомления SQL Server, команда будет выполнена. Сведения о выполнении для команды должно быть возвращено. Дополнительные сведения см. в разделе «Команды» далее в этом документе.

**Синтаксис** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**pCmd** |Это адрес, ранее возвращенный из ClientVirtualDevice::GetCommand команды.
| |**completionCode** |Это код состояния, указывающий состояние их завершения. Этот параметр должен быть возвращен для всех команд. Возвращенный код должен соответствовать выполняемой команды. ERROR_SUCCESS используется во всех случаях для обозначения успешного выполнения команды. Полный список возможных кодов, см. в файле vdierror.h. Отображается список кодов состояния обычно для каждой команды в «Команды» далее в этом документе.
| |**BytesTransferred** |Это количество успешно переданных байтов. Это значение возвращается только для передачи команд чтения и записи.
| |**position** |Это ответ в GetPosition команду.
        
| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Завершение отмечались правильно.
| |**VD_E_INVALID** |pCmd не активной команды.
| |**VD_E_ABORT** |Прерывание объект получил сигнал.
| |**VD_E_PROTOCOL** |Устройство не открыт.

**Примечания** None

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**Назначение** эта функция используется для обозначения возникновения ненормального завершения.

**Синтаксис** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |Нет | Неприменимо
        
| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR**|Прерывание уведомления успешно отправлено.

**Примечания** в любое время, клиент может прервать операцию резервного КОПИРОВАНИЯ или восстановления. Эта процедура сообщает, что все операции должен прекратить. Состояние набора данных в целом виртуальное устройство переходит в состояние аварийное завершение. Дополнительные команды не возвращаются для любых устройств. Все незавершенные команды автоматически завершены, возвращая ERROR_OPERATION_ABORTED как код завершения. Клиент должен вызывать ClientVirtualDeviceSet::Close после безопасное завершился любое использование необработанных буферов, предоставленные клиенту. Дополнительные сведения см. в разделе «Аварийное завершение» ранее в этом документе.

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Назначение** эта функция закрывает набор виртуального устройства, созданный с ClientVirtualDeviceSet::Create. Это приводит к выпуска все ресурсы, связанные с набором виртуального устройства.

**Синтаксис** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |Нет |Неприменимо
        
| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Это значение возвращается, когда набор виртуального устройства была успешно закрыт.
| |**VD_E_PROTOCOL** |Действие не выполнено, так как набор виртуальное устройство не было открыто.
| |**VD_E_OPEN** |Устройства были по-прежнему открыты.

**Примечания** вызов закрыть является объявлением клиента, что необходимо освободить все ресурсы, используемые набором виртуального устройства. Клиента необходимо убедиться, что все действия, выполняемые с использованием буферов данных и виртуальными устройствами завершается до вызова закрытия. Закройте все интерфейсы виртуального устройства, возвращенные OpenDevice недействительными.
Клиент может выдавать вызова для создания на набор интерфейс виртуальных устройств после завершения вызова Close. Такой вызов будет создания нового виртуального устройства для последующих операций резервного КОПИРОВАНИЯ или восстановления.
Если закрыть вызывается, когда один или несколько виртуальных устройств по-прежнему открыты, возвращается VD_E_OPEN. В этом случае SignalAbort внутренне инициируется, для обеспечения правильного завершения работы, если это возможно. VDI ресурсы освобождаются. Для обозначения VD_E_CLOSE на каждом устройстве перед вызовом ClientVirtualDeviceSet::Close ожидания клиента. Если клиенту известно, что набор виртуальное устройство уже находится в состоянии аварийное завершение, то он не должен ожидать более указанием VD_E_CLOSE из GetCommand и может вызвать ClientVirtualDeviceSet::Close сразу завершения действий для общих буферов.
Дополнительные сведения см. в разделе «Аварийное завершение» ранее в этом документе.

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Назначение** эта функция открывает виртуального устройства, заданные получателей клиенте. Основной клиент должен уже использовали Create and GetConfiguration для настройки набора виртуального устройства.

**Синтаксис** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**setName** |Он определяет набор. Оно учитывает регистр и должно соответствовать имени, используемые основной клиент, при его вызове ClientVirtualDeviceSet::Create.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Функция выполнена успешно.
| |**VD_E_PROTOCOL** |Набор виртуального устройства не был создан, уже открыт на этом клиенте или виртуального устройства набор не готова к принятию открытые запросы от клиентов получателей.
| |**VD_E_ABORT** |Операция прервана.

**Примечания** при использовании нескольких модель процессов, основной клиент отвечает за обнаружение обычного и аномального завершения получателей клиентов.

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Назначение** некоторых приложений может потребоваться несколько процессов для работы с буферами, возвращенных ClientVirtualDevice::GetCommand. В таких случаях процесс, который получает команду можно использовать для получения дескриптора процесса независимо, определяющий размер буфера GetBufferHandle. Этот дескриптор можно передать любой другой процесс, который также имеет то же открытое значение виртуального устройства. Затем этот процесс будет использовать ClientVirtualDeviceSet::MapBufferHandle получить адрес буфера. Адрес будет отличный от адреса в участнику, так как каждый процесс может сопоставление буферов, для которых заданы различные адреса.

**Синтаксис** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**pBuffer** |Это адрес буфера, полученный из команды чтения или записи.
| |**BufferHandle** |Уникальный идентификатор для буфера возвращается.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Функция выполнена успешно.
| |**VD_E_PROTOCOL** |Набор виртуальных устройств не открыт.
| |**VD_E_INVALID** |PBuffer не является допустимым адресом.
Примечания, что процесс, который вызывает функцию GetBufferHandle отвечает за вызов ClientVirtualDevice::CompleteCommand после завершения передачи данных.

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Назначение** эта функция используется для получения адреса допустимый буфер из буфера дескриптора, полученного из другого процесса. 

**Синтаксис** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| Параметры | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**dwBuffer** |Это дескриптор, возвращенный ClientVirtualDeviceSet::GetBufferHandle.
| |**ppBuffer** |Это адрес буфера, который является допустимым в текущем процессе.

| Возвращаемые значения | Аргумент | Объяснение
| ----- | ----- | ------ |
| |**ЗНАЧЕНИЕ NOERROR** |Функция выполнена успешно.
| |**VD_E_PROTOCOL** |Набор виртуальных устройств не открыт.
| |**VD_E_INVALID** |PpBuffer является недопустимым дескриптором.

**Примечания** необходимо соблюдать осторожность должным образом взаимодействовать маркеров. Дескрипторы являются локальными для набора одного виртуального устройства. Партнер процессов, совместное использование дескриптора необходимо убедиться, буфером дескрипторов, используются только в пределах виртуального устройства, от которого еще изначально был получен буфера.


