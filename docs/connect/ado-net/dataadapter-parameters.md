---
title: Параметры DataAdapter
description: Узнайте о свойствах DbDataAdapter, которые возвращают данные из источника данных и управляют изменениями в источнике данных.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 90985f155392a78da408ffbda3e1d2c0b12d4648
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771606"
---
# <a name="dataadapter-parameters"></a>Параметры DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Класс <xref:System.Data.Common.DbDataAdapter> имеет четыре свойства, которые служат для получения данных из источника данных и обновления данных в нем: свойство <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> возвращает данные из источника данных, а свойства <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> и <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> используются для управления изменениями в источнике данных.

> [!NOTE]
> Свойство `SelectCommand` должно быть установлено до вызова метода `Fill` объекта `DataAdapter`. Свойства `InsertCommand`, `UpdateCommand` или `DeleteCommand` должны быть установлены до вызова метода `Update` объекта `DataAdapter` в зависимости от того, какие изменения были сделаны в данных в <xref:System.Data.DataTable>. Например, если добавлены строки, свойство `InsertCommand` должно быть установлено перед вызовом метода `Update`. Если метод `Update` обрабатывает вставленную, обновленную или удаленную строку, `DataAdapter` использует соответствующее свойство `Command` для обработки действия. Текущие данные об измененной строке передаются в объект `Command` через коллекцию `Parameters`.

При обновлении строки в источнике данных вызывается инструкция UPDATE, которая использует уникальный идентификатор для идентификации строки в обновляемой таблице. Уникальным идентификатором обычно является значение поля первичного ключа. Инструкция UPDATE использует параметры, содержащие и уникальный идентификатор, и столбцы и обновляемые значения, как показано в следующей инструкции Transact-SQL.

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> Синтаксис местозаполнителей параметров зависит от источника данных. В этом примере показаны местозаполнители для источника данных SQL Server.

В этом примере поле `CompanyName` обновляется значением параметра `@CompanyName` для строки, в которой `CustomerID` равен значению параметра `@CustomerID`. Параметры получают данные из измененной строки, используя свойство <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A> объекта <xref:Microsoft.Data.SqlClient.SqlParameter>. Далее представлены параметры для предыдущего образца инструкции UPDATE. В коде предполагается, что переменная `adapter` представляет действительный объект <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

Метод `Add` коллекции `Parameters` принимает имя параметра, тип данных, размер (если он применим к типу) и имя <xref:System.Data.Common.DbParameter.SourceColumn%2A> из `DataTable`. Обратите внимание, что <xref:System.Data.Common.DbParameter.SourceVersion%2A> параметра `@CustomerID` установлена в `Original`. Это гарантирует, что существующая строка в источнике данных обновляется, если значение идентифицирующих столбцов изменилось в измененном <xref:System.Data.DataRow>. В этом случае значение строки `Original` будет соответствовать текущему значению в источнике данных и значение строки `Current` будет содержать обновленное значение. `SourceVersion` для параметра `@CompanyName` не установлена и использует значение по умолчанию строки `Current`.

> [!NOTE]
> Для операций `Fill` класса `DataAdapter` и методов `Get` класса `DataReader` тип .NET Framework выводится из значения, возвращенного поставщиком данных Microsoft SqlClient для SQL Server. Выводимые типы .NET и методы доступа для типов данных Microsoft SQL Server описываются в разделе [Сопоставление типов данных в ADO.NET](data-type-mappings-ado-net.md).

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn, Parameter.SourceVersion

`SourceColumn` и `SourceVersion` могут быть посланы как аргументы в конструктор `Parameter` или установлены как свойства существующих `Parameter`. `SourceColumn` является именем <xref:System.Data.DataColumn> из <xref:System.Data.DataRow>, где значение `Parameter` будет получено. `SourceVersion` задает версию `DataRow`, которую `DataAdapter` использует для получения значения.

В следующей таблице показаны значения перечисления <xref:System.Data.DataRowVersion>, доступные для использования с `SourceVersion`.

|Перечисление DataRowVersion|Описание|  
|--------------------------------|-----------------|  
|`Current`|Параметр использует текущее значение столбца. Это значение по умолчанию.|  
|`Default`|Параметр использует `DefaultValue` столбца.|  
|`Original`|Параметр использует исходное значение столбца.|  
|`Proposed`|Параметр использует предложенное значение.|  

В примере кода для `SqlClient` в следующем разделе определяется параметр для <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A>, в котором столбец `CustomerID` используется как `SourceColumn` для двух параметров: `@CustomerID` (`SET CustomerID = @CustomerID`) и `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). Параметр `@CustomerID` используется для обновления столбца **CustomerID** текущим значением в `DataRow`. В результате используется `CustomerID` `SourceColumn` с `SourceVersion` для `Current`. Параметр `@OldCustomerID` используется для идентификации текущей строки в источнике данных. Так как в версии `Original` строки найдено значение, совпадающее со значением столбца, используется тот же `SourceColumn` (`CustomerID`) с `SourceVersion` для `Original`.

## <a name="work-with-sqlclient-parameters"></a>Работа с параметрами SqlClient

Следующий пример демонстрирует, как создать <xref:Microsoft.Data.SqlClient.SqlDataAdapter> и установить <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> в <xref:System.Data.MissingSchemaAction.AddWithKey>, чтобы получить из базы данных дополнительные сведения о схеме. Устанавливаются свойства <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> и <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A>, и соответствующие им объекты <xref:Microsoft.Data.SqlClient.SqlParameter> добавляются в коллекцию <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A>. Метод возвращает объект `SqlDataAdapter`.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>См. также

- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Команды и параметры](commands-parameters.md)
- [Обновление источников данных с помощью DataAdapter](update-data-sources-with-dataadapters.md)
- [Изменение данных с помощью хранимых процедур](modify-data-with-stored-procedures.md)
- [Сопоставления типов данных в ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
