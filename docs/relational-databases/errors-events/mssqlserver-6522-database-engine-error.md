---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 836ad79e049d7c5613755e64ca441f9ed5d73048
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797821"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|6522|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|SQLCLR_UDF_EXEC_FAILED|
|Текст сообщения|Произошла ошибка .NET Framework во время выполнения определяемой пользователем подпрограммы или статистической функции "%.*ls": %ls.|
||

## <a name="explanation"></a>Пояснение

Рассмотрим следующие сценарии.

### <a name="scenario-1"></a>Сценарий 1

Вы создаете подпрограмму среды CLR, которая ссылается на сборку Microsoft .NET Framework. Эта сборка .NET Framework не задокументирована в [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). После этого вы устанавливаете исправление на основе .NET Framework 3.5 или .NET Framework 2.0.

### <a name="scenario-2"></a>Сценарий 2

Вы создаете сборку, а затем регистрируете ее в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. После этого вы устанавливаете другую версию сборки в глобальный кэш сборок.

В любом из этих сценариев при выполнении подпрограммы CLR или вызове сборки из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поступает сообщение об ошибке следующего вида:

> Сервер:  Сообщение 6522, уровень 16, состояние 2, строка 1  
Произошла ошибка .NET Framework во время выполнения определяемой пользователем подпрограммы или статистической функции "getsid".
>
> System.IO.FileLoadException: Не удалось загрузить файл или сборку "System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" либо одну из их зависимостей. Сигнатура сборки в хранилище узлов отличается от сигнатуры в глобальном кэше сборок. (Exception from HRESULT: 0x80131050)

## <a name="possible-cause"></a>Возможные причины

Когда среда CLR загружает сборку, она проверяет наличие той же сборки в глобальном кэше сборок. Если та же сборка уже есть в глобальном кэше сборок, среда CLR проверяет, совпадают ли у них идентификаторы версий модулей (MVID). Если идентификаторы MVID у сборок не совпадают, появляется сообщение об ошибке, которое описано в разделе [Пояснение](#explanation).

При повторной компиляции сборки у нее изменяется значение MVID. Это означает, что после обновления .NET Framework все сборки .NET Framework получат новые идентификаторы MVID, так как эти сборки компилируются заново. Кроме того, сборка компилируется заново при любом обновлении самой сборки. Это означает, что при обновлении она также получит новый MVID.

## <a name="user-action"></a>Рекомендуемые действия

### <a name="action-1"></a>Action1

Чтобы обойти проблему, описанную в сценарии 1 раздела [Пояснение](#explanation), необходимо вручную обновить сборки .NET Framework в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для этого выполните инструкцию `ALTER ASSEMBLY` с указанием на новую версию сборки для платформы .NET Framework в следующей папке:

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> Здесь **Version** обозначает версию установленной или обновленной платформы .NET Framework.

### <a name="action-2"></a>Действие 2

Чтобы обойти проблему, описанную в сценарии 2 раздела [Пояснение](#explanation), выполните инструкцию `ALTER ASSEMBLY` для обновления сборки в базе данных.

Если с помощью этих действий не удается устранить ошибку, удалите сборку из базы данных и зарегистрируйте новую версию сборки.

## <a name="more-information"></a>Дополнительные сведения

Мы не рекомендуем использовать сборки .NET Framework, которые не описаны в статье [Политика поддержки нетестируемых сборок .NET Framework в среде SQL Server на основе среды CLR](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). В статье перечислены все сборки, протестированные в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на основе среды CLR.

### <a name="description-of-clr-routines"></a>Описание подпрограмм среды CLR

Подпрограммы среды CLR включают следующие объекты, которые реализуются путем интеграции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] со средой CLR для .NET Framework:

- определяемые пользователем функции, возвращающие скалярное значение (скалярные определяемые пользователем функции);
- определяемые пользователем функции, возвращающие табличные значения (возвращающие табличное значение функции);
- определяемые пользователем процедуры (определяемые пользователем процедуры);
- определяемые пользователем триггеры.
- Определяемые пользователем типы данных
- определяемые пользователем агрегатные функции;

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>Сборки, которые нужно обновить после установки .NET Framework 3.5

После установки .NET Framework 3.5 необходимо обновить следующие сборки с помощью инструкции ALTER ASSEMBLY:

- Accessibility.dll;
- AspNetMMCExt.dll;
- Cscompmgd.dll;
- IEExecRemote.dll;
- IEHost.dll;
- IIEHost.dll;
- Microsoft.Build.Conversion.dll;
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll; 
- Microsoft.Build.Utilities.dll; 
- Microsoft.CompactFramework.Build.Tasks.dll; 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll; 
- Microsoft.Vsa.dll; 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll; 
- Microsoft_VsaVb.dll; 
- Sysglobl.dll; 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll; 
- System.Drawing.dll 
- System.Drawing.Design.dll; 
- System.EnterpriseServices.dll 
- System.Management.dll; 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll. 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll. 

Эти сборки расположены в следующей папке:

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>Сохранение данных из определяемых пользователем типов данных после удаления сборки

Если вы удаляете сборку, которая используется в пользовательском типе данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], данные из нее можно сохранить с помощью любого из следующих методов.

Предположим, что используется следующий сценарий:

- Вы создали сборку с именем *MyAssembly.dll*.
- Сборка MyAssembly ссылается на сборку `System.DirectoryServices.dll`.
- У вас есть определяемый пользователем тип данных с именем *MyDateTime*.
- Тип данных *MyDateTime* использует сборку *MyAssembly.dll*.
- Вы создали таблицу с именем *MyTable*.
- Таблица *MyTable* содержит данные с типом *MyDateTime*.

#### <a name="method-1-use-the-bcpexe-utility"></a>Метод 1. Использование служебной программы bcp.exe

1. Используйте служебную программу bcp.exe с аргументом -n, чтобы скопировать данные из таблицы MyTable в файл. Например, выполните в командной строке следующую команду:

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio выполните следующие действия:
   1. Удалите таблицу MyTable.
   2. Удалите тип данных MyDateTime.
   3. Удалите сборку `System.DirectoryServices.dll`.
   4. Удалите сборку MyAssembly.
3. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio выполните следующие действия:

   1. Зарегистрируйте сборку `System.DirectoryServices.dll`.
   2. Зарегистрируйте сборку MyAssembly.
   3. Создайте тип данных MyDateTime.
   4. Создайте новую таблицу с такой же структурой, как у таблицы MyTable.
4. Импортируйте данные из файла в таблицу MyTable с помощью служебной программы bcp.exe с аргументом -n. Например, выполните в командной строке следующую команду:
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>Метод 2. Использование инструкции INSERT ... Инструкция SELECT

Предположим, что тип данных MyDateTime занимает 9 байт в хранилище.

1. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio создайте новую таблицу, содержащую столбец с типом данных `VARBINARY(9)`. Для этого выполните следующую инструкцию:

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. Выполните следующую инструкцию INSERT ... SELECT для заполнения таблицы TempTable:

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio выполните следующие действия:
   1. Удалите таблицу MyTable.
   2. Удалите тип данных MyDateTime.
   3. Удалите сборку System.DirectoryServices.dll.
   4. Удалите сборку MyAssembly.
4. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio выполните следующие действия:
   1. Зарегистрируйте сборку System.DirectoryServices.dll.
   2. Зарегистрируйте сборку MyAssembly.
   3. Создайте тип данных MyDateTime.
   4. Создайте новую таблицу с такой же структурой, как у таблицы MyTable.
5. Выполните следующую инструкцию INSERT ... SELECT для заполнения таблицы MyTable:

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>Ссылки

- Дополнительные сведения о версиях сборки см. в [документации по Visual Studio 2005](https://www.microsoft.com/download/details.aspx?id=55984).
- Дополнительные сведения об обновлении сборки см. в статье [ALTER ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/alter-assembly-transact-sql).
- Дополнительные сведения об удалении сборки см. в статье [DROP ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/drop-assembly-transact-sql).
- Дополнительные сведения о регистрации сборки в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в статье [CREATE ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/create-assembly-transact-sql).
- Дополнительные сведения о служебной программе Bcp.exe см. по ссылке [https://msdn2.microsoft.com/library/ms162802.aspx](/sql/tools/bcp-utility).
