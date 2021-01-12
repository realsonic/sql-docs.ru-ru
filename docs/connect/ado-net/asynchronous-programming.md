---
title: Асинхронное программирование
description: Дополнительные сведения об асинхронном программировании в поставщике данных Microsoft SqlClient для SQL Server.
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804299"
---
# <a name="asynchronous-programming"></a>Асинхронное программирование

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

В этом разделе рассматривается поддержка асинхронного программирования в поставщике данных Microsoft SqlClient для SQL Server (SqlClient).

## <a name="legacy-asynchronous-programming"></a>Устаревшее асинхронное программирование

Поставщик данных Microsoft SqlClient для SQL Server содержит методы из **System.Data.SqlClient** для обеспечения обратной совместимости приложений, переходящих на <xref:Microsoft.Data.SqlClient>. Мы не рекомендуем использовать для новых разработок следующие устаревшие методы асинхронного программирования:

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> В поставщике данных Microsoft SqlClient для SQL Server для этих устаревших методов в строке подключения больше не нужно использовать `Asynchronous Processing=true`.

## <a name="asynchronous-programming-features"></a>Возможности асинхронного программирования

Использование этих возможностей асинхронного программирования упрощает создание асинхронного кода.

Дополнительные сведения об асинхронном программировании в .NET см. в следующих разделах:

- [Асинхронное программирование на C#](/dotnet/csharp/async)

- [Асинхронное программирование с использованием ключевых слов Async и Await (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

Если пользовательский интерфейс не отвечает или не удается осуществить масштабирование сервера, то, скорее всего, необходимо обеспечить большую поддержку асинхронных средств в коде. Согласно традиционному способу, создание асинхронного кода влечет за собой установку обратного вызова (называемого также продолжением) для представления логики, которая выполняется после завершения асинхронной операции. Это осложняет структуру асинхронного кода по сравнению с синхронным.

Вы можете выполнять вызов в асинхронные методы, не используя обратные вызова и не разбивая код на несколько методов или лямбда-выражений.

Модификатор `async` указывает, что метод является асинхронным. При вызове метода `async` происходит возврат задачи. Когда к задаче применяется оператор `await`, текущий метод немедленно завершает работу. После завершения задачи выполнение возобновляется в том же методе.

> [!TIP]
> В поставщике данных Microsoft SqlClient для SQL Server для задания ключевого слова строки подключения `Context Connection` не нужно использовать асинхронные вызовы.

Вызов метода `async` не приводит к выделению каких-либо дополнительных потоков. В конце он может на короткое время использовать существующий поток завершения ввода-вывода.

Следующие методы поставщика данных Microsoft SqlClient для SQL Server поддерживают асинхронное программирование:

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

Другие асинхронные элементы предусматривают [поддержку потоковой передачи в SqlClient](sqlclient-streaming-support.md).

> [!TIP]
> Для асинхронных методов в строке подключения не нужно указывать `Asynchronous Processing=true`. Поэтому это свойство является устаревшим в поставщике данных Microsoft SqlClient для SQL Server.

### <a name="synchronous-to-asynchronous-connection-open"></a>Синхронное подключение к асинхронному открытому соединению

Вы можете обновить существующее приложение для использования асинхронной функции. Например, предположим, что приложение имеет синхронный алгоритм соединения и блокирует поток пользовательского интерфейса при каждом его подключении к базе данных, а после подключения приложение вызывает хранимую процедуру, которая указывает другим пользователям БД, кто подключился.

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

При преобразовании для использования асинхронных функций программа будет выглядеть следующим образом:

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>Добавление асинхронной функции в существующее приложение (смешивание старых и новых шаблонов)

Также асинхронную возможность (SqlConnection::OpenAsync) можно добавлять без изменения существующей асинхронной логики. Например, предположим, что приложение использует в настоящее время следующее:

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

Вы можете приступить к использованию асинхронной модели без существенного изменения существующего алгоритма.

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>Использование базовой модели поставщика и асинхронной функции

Может потребоваться создать инструмент, который может подключаться к разным базам данных и выполнять запросы. Вы можете использовать базовую модель поставщика и асинхронную функцию.

Для использования распределенных транзакций необходимо включить на сервере MSDTC. Сведения о том, как включить MSDTC, см. в разделе [Как включить MSDTC на веб-сервере](/previous-versions/commerce-server/dd327979(v=cs.90)).

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>Использование транзакций SQL и новой асинхронной функции

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>Использование распределенной транзакции и новой асинхронной функции

В корпоративном приложении при некоторых сценариях может потребоваться добавление **распределенных транзакций**, чтобы разрешить транзакции между несколькими серверами баз данных. Можно использовать пространство имен System.Transactions и включить распределенную транзакцию следующим образом:

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>Отмена асинхронной операции

Можно отменить асинхронный запрос с использованием <xref:System.Threading.CancellationToken>.

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>Асинхронные операции с помощью SqlBulkCopy

Асинхронные возможности также можно получить в классе <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> при использовании метода <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>.

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>Асинхронное использование нескольких команд с помощью функции MARS

В примере открывается одно соединение с базой данных **AdventureWorks**. С помощью объекта <xref:Microsoft.Data.SqlClient.SqlCommand> создается <xref:Microsoft.Data.SqlClient.SqlDataReader>. По мере использования модуля чтения открывается второй класс <xref:Microsoft.Data.SqlClient.SqlDataReader>, который использует данные из первого класса <xref:Microsoft.Data.SqlClient.SqlDataReader> в качестве входных данных для второго средства чтения.

> [!NOTE]
> В следующем примере используется образец базы данных [**AdventureWorks**](../../samples/adventureworks-install-configure.md). В строке подключения в примере кода предполагается, что база данных установлена и доступна на локальном компьютере. При необходимости измените строку подключения для вашей среды.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>Асинхронное чтение и обновление данных с помощью функции MARS

Режим MARS позволяет использовать соединение как для операций чтения, так и для операций языка обработки данных (DML) с более чем одной ожидающей операцией. Эта функция устраняет необходимость приложения решать проблемы, связанные с ошибками во время подключения. Кроме того, режим MARS может заменить использование курсоров на стороне сервера, которые обычно потребляют больше ресурсов. Наконец, несколько операций могут осуществляться одновременно в одном соединении, поэтому в них может совместно использоваться один и тот же контекст транзакции, в результате чего отпадает необходимость использования системных хранимых процедур **sp_getbindtoken** и **sp_bindsession**.

В следующем консольном приложении показано, как использовать два объекта <xref:Microsoft.Data.SqlClient.SqlDataReader> с тремя объектами <xref:Microsoft.Data.SqlClient.SqlCommand> и один объект <xref:Microsoft.Data.SqlClient.SqlConnection> с включенным режимом MARS. Первый объект команды получает список поставщиков с кредитоспособностью — 5. Второй объект команды использует идентификатор поставщика из <xref:Microsoft.Data.SqlClient.SqlDataReader> для загрузки второго объекта <xref:Microsoft.Data.SqlClient.SqlDataReader> со всеми продуктами данного поставщика. Каждая запись продукта посещается вторым <xref:Microsoft.Data.SqlClient.SqlDataReader>. Для определения нового состояния **OnOrderQty** выполняется вычисление. Затем используется третий объект команды для занесения в таблицу **ProductVendor** нового значения. Весь процесс выполняется в рамках одной транзакции, которая откатывается в конце.

> [!NOTE]
> В следующем примере используется образец базы данных [**AdventureWorks**](../../samples/adventureworks-install-configure.md). В строке подключения в примере кода предполагается, что база данных установлена и доступна на локальном компьютере. При необходимости измените строку подключения для вашей среды.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>См. также

- [Извлечение и изменение данных в ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
