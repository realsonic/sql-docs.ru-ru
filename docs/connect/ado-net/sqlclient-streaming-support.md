---
title: Поддержка потоковой передачи в SqlClient
description: Описывает, как создавать приложения, которые выполняют потоковую передачу данных из SQL Server без их полной загрузки в память.
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744395"
---
# <a name="sqlclient-streaming-support"></a>Поддержка потоковой передачи в SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Поддержка потоковой передачи данных между SQL Server и приложением предусматривает использование неструктурированных данных на сервере (документы, образы и файлы мультимедиа). База данных SQL Server может хранить большие двоичные объекты (BLOB), но для их извлечения может потребоваться много памяти.

Поддержка потоковой передачи данных в SQL Server и обратно упрощает написание приложений, которые выполняют потоковую передачу данных, что позволяет обойтись без полной загрузки данных в память и, следовательно, снизить количество исключений, связанных с переполнением памяти.

Поддержка потоковой передачи также позволит лучше масштабировать приложения среднего уровня, особенно в сценариях, при которых бизнес-объекты подключаются к Azure SQL для отправки, получения и обработки больших BLOB-объектов.

> [!WARNING]
> Элементы, поддерживающие потоковую передачу, используются для извлечения данных из запросов и передачи параметров запросам и хранимым процедурам. Функция потоковой передачи предназначена для основных сценариев OLTP и миграции данных и применима к локальным и удаленным средам переноса данных.

## <a name="streaming-support-from-sql-server"></a>Поддержка потоковой передачи из SQL Server

Поддержка потоковой передачи из SQL Server привела к появлению новых функциональных возможностей в классах <xref:System.Data.Common.DbDataReader> и <xref:Microsoft.Data.SqlClient.SqlDataReader> для получения объектов <xref:System.IO.Stream>, <xref:System.Xml.XmlReader> и <xref:System.IO.TextReader>, а также реагирования на них. Эти классы используются для получения данных из запросов. В результате поддержка потоковой передачи из SQL Server обращается к сценариям OLTP и применяется к локальной и удаленной средам.

Для активации поддержки потоковой передачи из SQL Server в <xref:Microsoft.Data.SqlClient.SqlDataReader> добавлены следующие элементы:

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

Для активации поддержки потоковой передачи из SQL Server в <xref:System.Data.Common.DbDataReader> добавлены следующие элементы:

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>Поддержка потоковой передачи в SQL Server

Поддержка потоковой передачи для SQL Server находится в классе <xref:Microsoft.Data.SqlClient.SqlParameter>, поэтому он может принимать объекты <xref:System.Xml.XmlReader>, <xref:System.IO.Stream> и <xref:System.IO.TextReader> и реагировать на них. <xref:Microsoft.Data.SqlClient.SqlParameter> используется для передачи параметров в запросы и хранимые процедуры.

> [!NOTE]
> Удаление объекта <xref:Microsoft.Data.SqlClient.SqlCommand> или вызов <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> должны приводить к отмене любой потоковой операции. Если приложение передает <xref:System.Threading.CancellationToken>, отмена не гарантируется.

Следующие типы <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> могут принимать объект <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> из <xref:System.IO.Stream>:

- **Двоичный**

- **VarBinary**

Следующие типы <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> могут принимать объект <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> из <xref:System.IO.TextReader>:

- **Char**

- **NChar**

- **NVarChar**

- **Xml**

Тип **Xml** <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> принимает <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> из <xref:System.Xml.XmlReader>

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> может принимать значения типа <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> и <xref:System.IO.Stream>.

Объекты <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> и <xref:System.IO.Stream> будут перенесены вплоть до значения, определяемого в <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A>.

## <a name="sample----streaming-from-sql-server"></a>Пример. Потоковая передача из SQL Server

Для создания примера базы данных используйте следующий язык Transact-SQL:

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

В образце демонстрируется выполнение следующих действий.

- Избегайте блокирования потока пользовательского интерфейса путем предоставления асинхронного способа извлечения больших файлов.

- Перенос большого текстового файла из SQL Server в .NET.

- Перенос большого XML-файла из SQL Server в .NET.

- Получение данных из SQL Server.

- Перенос файлов больших двоичных объектов из одной базы данных SQL Server в другую, не сталкиваясь с проблемой нехватки памяти.

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>Пример. Потоковая передача в SQL Server

Для создания примера базы данных используйте следующий язык Transact-SQL:

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

В образце демонстрируется выполнение следующих действий.

- Передача большого двоичного объекта в SQL Server в .NET.

- Передача большого текстового файла в SQL Server в .NET.

- Использование новой асинхронной возможности для передачи большого двоичного объекта.

- Использование новой асинхронной функции и ключевого слова await для передачи большого двоичного объекта.

- Отмена передачи большого двоичного объекта.

- Потоковая передача из одного объекта SQL Server в другой с помощью асинхронной возможности.

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>Пример. Потоковая передача из одного объекта SQL Server в другой

В примере показано, как асинхронно передавать большой двоичный объект из одного объекта SQL Server в другой с поддержкой отмены.

> [!NOTE]
> Перед запуском следующего примера убедитесь в том, что базы данных Demo и Demo2 созданы.

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>См. также

- [Извлечение и изменение данных в ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
