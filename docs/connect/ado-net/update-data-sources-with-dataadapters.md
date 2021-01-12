---
title: Обновление источников данных с помощью DataAdapter
description: Узнайте, как метод Update объекта DataAdapter разрешает изменения из набора данных обратно в источник данных в приложениях ADO.NET.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771388"
---
# <a name="update-data-sources-with-dataadapters"></a>Обновление источников данных с помощью DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Метод `Update` объекта <xref:System.Data.Common.DataAdapter> вызывается для решения задачи по передаче изменений из <xref:System.Data.DataSet> обратно в источник данных. Метод `Update`, как и метод `Fill`, принимает в качестве аргументов экземпляр `DataSet`, а также (необязательно) объект <xref:System.Data.DataTable> или имя `DataTable`. Экземпляр `DataSet` представляет собой объект `DataSet`, который содержит выполненные изменения, а `DataTable` указывает на таблицу, из которой должны быть получены эти изменения. Если ни один объект `DataTable` не задан, используется первый объект `DataTable` в `DataSet`.

При вызове метода `Update` в `DataAdapter` анализируются внесенные изменения и выполняется соответствующая команда (INSERT, UPDATE или DELETE). Если в `DataAdapter` обнаруживается изменение в <xref:System.Data.DataRow>, то в этом объекте используется команда <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> или <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> для обработки этого изменения.

Эти свойства позволяют максимально увеличить производительность приложений ADO.NET за счет указания синтаксиса команд во время разработки и, где это возможно, за счет использования хранимых процедур. Необходимо явно задавать команды перед вызовом `Update`. Если вызывается `Update`, и не существует подходящая команда для конкретного обновления (например, отсутствует `DeleteCommand` для удаленных строк), то активизируется исключение.

> [!IMPORTANT]
> Если для изменения или удаления данных с помощью `DataAdapter` используются хранимые процедуры SQL Server, убедитесь, что в определении хранимой процедуры не используется `SET NOCOUNT ON`. В таком случае возвращается число затронутых строк, равное нулю, что `DataAdapter` интерпретирует как конфликт параллелизма. Это событие вызовет исключение <xref:System.Data.DBConcurrencyException>.

Параметры команды могут использоваться в целях указания входных и выходных значений для инструкции SQL или хранимой процедуры применительно к каждой модифицированной строке в `DataSet`. Дополнительные сведения см. в разделе [Параметры DataAdapter](dataadapter-parameters.md).

> [!NOTE]
> Важно учитывать различие между обозначением строки как удаленной в <xref:System.Data.DataTable> и удалением этой строки. Если вызывается метод `Remove` или `RemoveAt`, строка немедленно удаляется. Все соответствующие строки в источнике данных серверной части **не будут затронуты**, если вы затем передадите `DataTable` или `DataSet` в `DataAdapter` и вызовите `Update`. Если же используется метод `Delete`, то строка остается в `DataTable` и отмечается как предназначенная для удаления. Если затем передать `DataTable` или `DataSet` в `DataAdapter` и вызвать `Update`, соответствующая строка в источнике данных серверной части **удаляется**.

Если значение `DataTable` сопоставляется или создается на основе одной таблицы базы данных, то можно воспользоваться тем, что объект <xref:System.Data.Common.DbCommandBuilder> автоматически создает объекты `DeleteCommand`, `InsertCommand` и `UpdateCommand` для `DataAdapter`. Дополнительные сведения см. в статье [Создание команд с помощью классов CommandBuilder](generate-commands-with-commandbuilders.md).

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>Использование UpdatedRowSource для сопоставления значений в наборе данных

Можно управлять тем, как значения, возвращаемые из источника данных, сопоставляются с `DataTable` после вызова метода <xref:System.Data.Common.DbDataAdapter.Update%2A> объекта `DataAdapter`, используя свойство <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> объекта <xref:Microsoft.Data.SqlClient.SqlCommand>. Задавая значение свойства `UpdatedRowSource` равным одному из значений перечисления <xref:System.Data.UpdateRowSource>, можно управлять тем, должны ли пропускаться выходные параметры, возвращаемые командами `DataAdapter`, или применяться к изменившейся строке в `DataSet`. Можно также указать, применяется ли первая возвращенная строка (если она существует) к изменившейся строке в `DataTable`.

В следующей таблице приведено описание различных значений перечисления `UpdateRowSource` и показано, как они влияют на поведение команды, используемой в сочетании с `DataAdapter`.

|Перечисление UpdatedRowSource|Описание|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|И выходные параметры, и первая строка возвращенного результирующего набора могут быть сопоставлены с модифицированной строкой в `DataSet`.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Только данные из первой строки возвращенного результирующего набора могут быть сопоставлены с модифицированной строкой в `DataSet`.|
|<xref:System.Data.UpdateRowSource.None>|Любые выходные параметры или строки возвращенного результирующего набора пропускаются.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Только выходные параметры могут быть сопоставлены с модифицированной строкой в `DataSet`.|

Метод `Update` позволяет решить задачу по передаче внесенных изменений обратно в источник данных; но может оказаться так, что другие клиенты уже внесли изменения в данные источника данных с того момента, как последний раз было осуществлено заполнение `DataSet`. Чтобы обновить применяемый объект `DataSet` с использованием текущих данных, воспользуйтесь `DataAdapter` и методом `Fill`. Произойдет добавление новых строк к таблице, а обновленная информация будет включена в существующие строки.

Метод `Fill` определяет, должна ли быть добавлена новая строка или обновлена существующая строка, путем проверки значений первичного ключа в строках объекта `DataSet` и в строках, возвращенных `SelectCommand`. Если в методе `Fill` обнаруживается значение первичного ключа какой-то строки в `DataSet`, которое совпадает со значением первичного ключа строки в результатах, возвращенных `SelectCommand`, то метод обновляет существующую строку на основании данных из строки, возвращенной `SelectCommand`, и задает значение <xref:System.Data.DataRow.RowState%2A> существующей строки, равное `Unchanged`. Если строка, возвращенная `SelectCommand`, имеет значение первичного ключа, не совпадающее ни с одним из значений первичного ключа в строках в `DataSet`, то метод `Fill` добавляет новую строку со значением `RowState`, равным `Unchanged`.

> [!NOTE]
> Если `SelectCommand` возвращает результаты **OUTER JOIN**, `DataAdapter` не будет устанавливать значение `PrimaryKey` для результирующей `DataTable`. Необходимо непосредственно определить значение `PrimaryKey` для обеспечения того, чтобы решение по обработке повторяющихся строк было принято правильно.

Чтобы обеспечить обработку исключений, которые могут возникнуть при вызове метода `Update`, можно воспользоваться событием `RowUpdated` для осуществления ответных действий на ошибки обновления строк по мере их возникновения (см. раздел об [обработке событий DataAdapter](handle-dataadapter-events.md)) или задать значение <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> равным `true` перед вызовом `Update` и осуществлять ответные действия на основании сведений об ошибке, хранящихся в свойстве `RowError` конкретной строки, после завершения обновления.

> [!NOTE]
> Вызов `AcceptChanges` применительно к `DataSet`, `DataTable` или `DataRow` приводит к тому, что все значения `Original`, относящиеся к `DataRow`, перезаписываются значениями `Current`, относящимися к `DataRow`. Если были изменены значения полей, уникальным образом идентифицирующих строку, то после вызова метода `AcceptChanges` значения `Original` больше не будут соответствовать этим значениям в источнике данных. `AcceptChanges` вызывается автоматически для каждой строки во время вызова метода `Update` объекта `DataAdapter`. Можно сохранить первоначальные значения во время вызова метода Update, для чего вначале следует задать значение свойства `AcceptChangesDuringUpdate` объекта `DataAdapter` равным false, или создать обработчик событий для события `RowUpdated` и задать значение <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A>, равное <xref:System.Data.UpdateStatus.SkipCurrentRow>. Дополнительные сведения см. в разделе [Обработка событий DataAdapter](handle-dataadapter-events.md).

В следующих примерах показано, как выполнить обновления применительно к модифицированным строкам путем явного задания значения `UpdateCommand` объекта `DataAdapter` и вызова его метода `Update`.

> [!NOTE]
> Параметр, указанный в `WHERE clause` выражения `UPDATE statement`, настроен на использование значения `Original` объекта `SourceColumn`. Это важно, поскольку значение `Current` могло быть изменено, поэтому может не соответствовать этому значению в источнике данных. Значение `Original` представляет собой значение, которое использовалось для заполнения `DataTable` из источника данных.

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>AutoIncrement - столбцы

Если в таблицах из применяемого источника данных имеются столбцы с автоматическим увеличением значений, то можно обеспечить заполнение столбцов в применяемом `DataSet` путем возврата автоматически увеличивающегося значения как выходного параметра хранимой процедуры и сопоставления его со столбцом таблицы; возврата автоматически увеличивающегося значения в первой строке результирующего набора, возвращенного хранимой процедурой или инструкцией SQL; а также использование события `RowUpdated` объекта `DataAdapter` для выполнения дополнительной инструкции SELECT. Дополнительные сведения и пример см. в статье [Извлечение значений идентификаторов или автонумерации](retrieve-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Порядок в операциях вставки, обновления и удаления

Во многих обстоятельствах имеет значение последовательность передачи изменений, внесенных с помощью `DataSet`, в источник данных. Например, если происходит обновление значения первичного ключа для существующей строки и добавляется новая строка с новым значением первичного ключа в качестве внешнего ключа, то важно вначале осуществить обновление, а затем вставку.

Можно использовать метод `Select` объекта `DataTable` для возврата массива `DataRow`, который ссылается только на строки с конкретным значением `RowState`. После этого можно передать возвращенный массив `DataRow` в метод `Update` объекта `DataAdapter` для обработки измененных строк. Задавая подмножество строк, подлежащих обновлению, можно управлять тем, в какой последовательности обрабатываются вставки, обновления и удаления.

## <a name="example"></a>Пример

Например, в следующем коде обеспечивается то, что удаленные строки таблицы обрабатываются в первую очередь, затем происходит обработка обновленных строк, после чего обрабатываются вставленные строки.

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Использование объекта DataAdapter для получения и обновления данных

DataAdapter можно использовать для извлечения и обновления данных.

- В примере используется метод `DataAdapter.AcceptChangesDuringFill` для клонирования данных в базе данных. Если свойство имеет значение **false**, метод **AcceptChanges** не вызывается при заполнении таблицы, а вновь добавленные строки рассматриваются как вставленные строки. Таким образом, в примере эти строки используются для вставки новых строк в базу данных.

- В примерах используется метод `DataAdapter.TableMappings` для определения сопоставления между исходной таблицей и DataTable.

- В примере используется метод `DataAdapter.FillLoadOption`, чтобы определить, как адаптер заполняет объект **DataTable** из **DbDataReader**. При создании объекта DataTable можно записать данные из базы данных только в текущую или исходную версию, задав в качестве значения свойства **LoadOption.Upsert** или **LoadOption.PreserveChanges**.

- Пример также обновит таблицу с помощью метода `DbDataAdapter.UpdateBatchSize` для выполнения пакетных операций.

Перед компиляцией и выполнением примера необходимо создать образец базы данных:

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>См. также

- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Извлечение значений идентификаторов или автонумерации](retrieve-identity-or-autonumber-values.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
