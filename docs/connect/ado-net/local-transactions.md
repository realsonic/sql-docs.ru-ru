---
title: Локальные транзакции
description: Демонстрирует выполнение транзакций с базой данных с помощью поставщика данных Microsoft SqlClient для SQL Server.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051368"
---
# <a name="local-transactions"></a>Локальные транзакции

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Транзакции в ADO.NET используются, когда требуется связать несколько задач так, чтобы они выполнялись как одна единица работы. Например, пусть приложение выполняет две задачи. Во-первых, оно заносит в таблицу сведения о заказе. Во-вторых, обновляет таблицу, содержащую список товаров на складе, списывая заказанные элементы. При сбое любой задачи будет выполнен откат обоих изменений.  

## <a name="determining-the-transaction-type"></a>Определение типа транзакции

Транзакция считается локальной, если она состоит из одной фазы и обрабатывается непосредственно базой данных. Транзакция считается распределенной, если она координируется монитором транзакций и использует для разрешения транзакций резервные механизмы (например, двухфазную фиксацию).

Поставщик данных Microsoft SqlClient для SQL Server использует собственный объект <xref:Microsoft.Data.SqlClient.SqlTransaction> для выполнения локальных транзакций в базах данных SQL Server. Другие поставщики данных .NET также имеют собственные объекты `Transaction`. Кроме того, существует класс <xref:System.Data.Common.DbTransaction>, доступный для написания независимого от поставщика кода с использованием транзакций.

> [!NOTE]
> Транзакции наиболее эффективны, когда выполняются на сервере. При работе с базой данных SQL Server, интенсивно использующей явные транзакции, следует рассмотреть возможность их записи в виде хранимых процедур при помощи инструкции Transact-SQL BEGIN TRANSACTION.

## <a name="performing-a-transaction-using-a-single-connection"></a>Выполнение транзакции с помощью одного подключения 

В ADO.NET управление транзакциями осуществляется с помощью объекта `Connection`. Инициировать транзакцию можно с помощью метода `BeginTransaction`. После начала транзакции при помощи свойства `Transaction` объекта `Command` к ней можно прикрепить команду. Затем в зависимости от успеха или ошибки компонентов транзакции можно зафиксировать или откатить изменения, сделанные в источнике данных.

> [!NOTE]
> Метод `EnlistDistributedTransaction` не должен использоваться для локальной транзакции.

Область действия транзакции ограничена соединением. В следующем примере выполняется явная транзакция, состоящая из двух отдельных команд в блоке `try`. Команды выполняют инструкции INSERT для таблицы `Production.ScrapReason` в образце базы данных SQL Server AdventureWorks, которые будут зафиксированы при отсутствии исключений. Код в блоке `catch` откатит транзакцию, если возникнет исключение. При отмене транзакции или обрыве соединения до выполнения транзакции она откатывается автоматически.

## <a name="example"></a>Пример  

 Чтобы осуществить транзакцию, выполните указанные ниже действия.

1. Вызовите метод <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> объекта <xref:Microsoft.Data.SqlClient.SqlConnection> для отметки начала транзакции. Метод <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> возвращает ссылку на транзакцию. Эта ссылка назначается объектам <xref:Microsoft.Data.SqlClient.SqlCommand>, прикрепленным к транзакции.

2. Присвойте объект `Transaction` свойству <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A> объекта <xref:Microsoft.Data.SqlClient.SqlCommand>. Исключение вызывается, если команда выполняется при соединении с активной транзакцией, а объект `Transaction` не был назначен свойству `Transaction` объекта `Command`.

3. Выполните требуемые команды.

4. Для выполнения транзакции вызовите метод <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> объекта <xref:Microsoft.Data.SqlClient.SqlTransaction>, для завершения транзакции вызовите метод <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>. Транзакция откатывается, если соединение закрывается или пропадает до выполнения метода <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> либо <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>.

В следующем примере кода демонстрируется логика транзакций с использованием поставщика данных Microsoft SqlClient для SQL Server.  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>См. также раздел

- [Транзакции и параллельность](transactions-and-concurrency.md)
- [Распределенные транзакции](distributed-transactions.md)
- [Интеграция System.Transactions с SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
