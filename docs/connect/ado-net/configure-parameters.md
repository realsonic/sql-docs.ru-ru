---
title: Настройка параметров
description: С помощью параметров объекты команд передают значения в инструкции SQL или хранимые процедуры, обеспечивая проверку типа и проверку в ADO.NET.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 14442892bc59fc733c6a67153832b95c3f368b95
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771464"
---
# <a name="configuring-parameters"></a>Настройка параметров

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Объекты команды используют параметры для передачи значений в выражения SQL или хранимые процедуры, обеспечивая проверку типов и правильности. В отличие от текста команд, входные параметры обрабатываются как буквенные значения, а не как исполняемый код. Это помогает защищаться от атак путем внедрения кода SQL, при которых злоумышленник вставляет в инструкцию SQL команду, ставящую под угрозу безопасность сервера.

Параметризованные команды также позволяют повысить производительность при выполнении запроса, поскольку при их использовании сервер баз данных может точно сопоставить входящей команде правильный кэшированных план запроса. Дополнительные сведения см. в статьях [Кэширование и повторное использование плана выполнения](/sql/relational-databases/query-processing-architecture-guide#execution-plan-caching-and-reuse) и [Повторное использование параметров и плана выполнения](/sql/relational-databases/query-processing-architecture-guide#PlanReuse). Помимо повышения безопасности и производительности параметризованные команды обеспечивают удобный метод организации значений, передающихся в источник данных.

Объект <xref:System.Data.Common.DbParameter> можно создать при помощи конструктора или путем добавления его в коллекцию <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> с помощью метода `Add` коллекции <xref:System.Data.Common.DbParameterCollection> . Метод `Add` принимает в качестве входных данных либо аргументы конструктора, либо существующий объект параметра - в зависимости от поставщика данных.

## <a name="supply-the-parameterdirection-property"></a>Указание свойства ParameterDirection

При добавлении параметров необходимо указать свойство <xref:System.Data.ParameterDirection> для параметров, не являющихся входными. В следующей таблице показаны значения `ParameterDirection` , которые можно использовать с перечислением <xref:System.Data.ParameterDirection> .

|Имя участника|Описание|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|Параметр является входным. Это значение по умолчанию.|
|<xref:System.Data.ParameterDirection.InputOutput>|Параметр можно использовать как для ввода, так и для вывода.|
|<xref:System.Data.ParameterDirection.Output>|Параметр является выходным.|
|<xref:System.Data.ParameterDirection.ReturnValue>|Параметр представляет значение, возвращаемое как результат операции, например хранимой процедуры, встроенной функции или определяемой пользователем функции.|

## <a name="work-with-parameter-placeholders"></a>Работа с заполнителями параметров

Синтаксис местозаполнителей параметров зависит от источника данных. Поставщик данных Microsoft SqlClient для SQL Server обрабатывает именование, а также установку параметров и заполнителей параметров по-разному. Поставщик данных SqlClient использует именованные параметры в формате `@`*имя_параметра*.

## <a name="specify-parameter-data-types"></a>Указание типов данных параметров

Тип данных параметра зависит от поставщика данных Microsoft SqlClient для SQL Server. При указании типа значение `Parameter` преобразуется в поставщик данных типа Microsoft SqlClient для SQL Server перед передачей значения в источник данных. Можно также указать тип `Parameter` универсальным способом, задав свойству `DbType` объекта `Parameter` определенное значение <xref:System.Data.DbType>.

Тип поставщика данных Microsoft SqlClient для SQL Server для объекта `Parameter` выводится из типа .NET Framework `Value` объекта `Parameter` или из `DbType` объекта `Parameter`. Следующая таблица показывает тип `Parameter` , выводимый из объекта, переданного как значение `Parameter` , или указанного значения `DbType`.

|Тип .NET|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|Логическое значение|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|Двоичные данные|VarBinary. Это неявное преобразование не будет выполнено, если размер массива байтов превышает максимальный размер VarBinary, равный 8000 байт. Для массивов байтов размером более 8000 байт необходимо явно указать тип <xref:System.Data.SqlDbType>.|
|<xref:System.Char>| |Вывод типа <xref:System.Data.SqlDbType> из типа char не поддерживается.|
|<xref:System.DateTime>|Дата и время|Дата/время|
|<xref:System.DateTimeOffset>|DateTimeOffset|Тип DateTimeOffset в SQL Server 2008. Вывод типа <xref:System.Data.SqlDbType> из типа DateTimeOffset не поддерживается в версиях SQL Server до SQL Server 2008.|
|<xref:System.Decimal>|Decimal|Decimal|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Один|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|Int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|Объект|Variant|
|<xref:System.String>|Строка|NVarChar. Это неявное преобразование завершится ошибкой, если строка превышает максимальный размер для типа NVarChar (4000 символов). Для строк длиннее 4000 символов явно установите значение <xref:System.Data.SqlDbType>.|
|<xref:System.TimeSpan>|Время|Тип Time в SQL Server 2008. Вывод типа <xref:System.Data.SqlDbType> из типа TimeSpan не поддерживается в версиях SQL Server до SQL Server 2008.|
|<xref:System.UInt16>|UInt16|Вывод типа <xref:System.Data.SqlDbType> из типа UInt16 не поддерживается.|
|<xref:System.UInt32>|UInt32|Вывод типа <xref:System.Data.SqlDbType> из типа UInt32 не поддерживается.|
|<xref:System.UInt64>|UInt64|Вывод типа <xref:System.Data.SqlDbType> из типа UInt64 не поддерживается.|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||Валюта|Money|
||Дата|Тип Date в SQL Server 2008. Вывод типа <xref:System.Data.SqlDbType> из типа Date не поддерживается в версиях SQL Server до SQL Server 2008.|
||SByte|Вывод типа <xref:System.Data.SqlDbType> из типа SByte не поддерживается.|
||StringFixedLength|NCHAR|
||Время|Тип Time в SQL Server 2008. Вывод типа <xref:System.Data.SqlDbType> из типа Time не поддерживается в версиях SQL Server до SQL Server 2008.|
||VarNumeric|Вывод типа <xref:System.Data.SqlDbType> из типа VarNumeric не поддерживается.|
|определяемый пользователем тип (объект с <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>|SqlClient всегда возвращает объект|`SqlDbType.Udt` при наличии <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute>. В противном случае — `Variant`.|

> [!NOTE]
> Преобразования из типа decimal в другие типы являются сужающими. Они округляют десятичное значение до ближайшего целого в направлении нуля. Если результат преобразования нельзя представить в целевом типе, возникает исключение <xref:System.OverflowException> .

> [!NOTE]
> Во время отправки значения параметра NULL на сервер нужно указать <xref:System.DBNull>, а не `null` (`Nothing` в Visual Basic). В системе значение null - это пустой объект, не имеющий значения. Для представления значений null используется тип<xref:System.DBNull> .

## <a name="derive-parameter-information"></a>Получение сведений о параметрах

Информацию о параметрах можно вывести из хранимой процедуры с помощью класса `DbCommandBuilder` . Класс `SqlCommandBuilder` предоставляет статический метод `DeriveParameters`, который обеспечивает автоматическое заполнение коллекции параметров объекта команды, использующего сведения о параметрах из хранимой процедуры. Обратите внимание, что метод `DeriveParameters` перезаписывает существующую информацию о параметрах для команды.

> [!NOTE]
> Выведение информации о параметрах снижает производительность, так как для этого требуется дополнительный обмен данных с источником данных. Если информация о параметрах известна во время разработки, можно увеличить производительность приложения, задав параметры явным образом.

Дополнительные сведения см. в статье [Создание команд с помощью классов CommandBuilder](generate-commands-with-commandbuilders.md).

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>Использование параметров с SqlCommand и хранимой процедурой

Хранимые процедуры дают множество преимуществ в приложениях, управляемых данными. С помощью хранимых процедур операции с базой данных можно инкапсулировать в одну команду, оптимизированную для производительности и обладающую повышенной безопасностью. Хотя хранимые процедуры можно вызывать и с помощью инструкции SQL, указывая в ней имя процедуры и ее аргументы, использование коллекции <xref:System.Data.Common.DbCommand.Parameters%2A> объекта ADO.NET <xref:System.Data.Common.DbCommand> позволяет более явно задавать параметры процедуры, а также обращаться к выходным параметрам и возвращаемым значениям.

> [!NOTE]
> Параметризованные инструкции выполняются на сервере с помощью хранимой процедуры `sp_executesql,` которая позволяет повторно использовать планы запросов. Локальные курсоры или переменные в пакете `sp_executesql` недоступны пакету, вызвавшему `sp_executesql`. Изменения в контексте базы данных длятся только до завершения выполнения инструкции `sp_executesql` . Дополнительные сведения см. в статье [sp_executesql (Transact-SQL)](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql).

Если параметры используются с объектом <xref:Microsoft.Data.SqlClient.SqlCommand> для выполнения хранимой процедуры SQL Server, то имена параметров, добавляемых в коллекцию <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> , должны соответствовать именам маркеров параметров в хранимой процедуре. Поставщик данных Microsoft SqlClient для SQL Server не поддерживает заполнитель в виде вопросительного знака (?) для передачи параметров в инструкцию SQL или хранимую процедуру. Он обрабатывает параметры в хранимой процедуре как именованные параметры и ищет соответствующие маркеры параметров. Например, хранимая процедура `CustOrderHist` определяется с использованием параметра `@CustomerID`. Когда программа выполняет эта хранимую процедуру, она также должна использовать параметр `@CustomerID`.

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>Пример

Этот пример показывает, как вызвать хранимую процедуру SQL Server в образце базы данных `Northwind` . Имя хранимой процедуры – `dbo.SalesByCategory` . Она имеет входной параметр `@CategoryName` с типом данных `nvarchar(15)`. Код создает создает новый объект класса <xref:Microsoft.Data.SqlClient.SqlConnection> в блоке Using, чтобы в конце процедуры соединение удалялось. Создаются объекты <xref:Microsoft.Data.SqlClient.SqlCommand> и <xref:Microsoft.Data.SqlClient.SqlParameter> устанавливаются их свойства. Объект класса <xref:Microsoft.Data.SqlClient.SqlDataReader> выполняет `SqlCommand` и возвращает результирующий набор из хранимой процедуры, отображая выходные данные в окне консоли.

> [!NOTE]
> Вместо того, чтобы создавать объекты `SqlCommand` и `SqlParameter` и затем задавать их свойства в отдельных инструкциях, можно использовать один из перегруженных конструкторов и задать свойства в одной инструкции.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>См. также

- [Команды и параметры](commands-parameters.md)
- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Сопоставления типов данных в ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
