---
title: Изменение данных с помощью хранимых процедур
description: Описывается использование входных и выходных параметров хранимой процедуры для вставки строки в базу данных с возвратом нового значения идентификатора.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744410"
---
# <a name="modify-data-with-stored-procedures"></a>Изменение данных с помощью хранимых процедур

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Хранимые процедуры могут принимать данные в виде входных параметров и возвращать их в виде выходных параметров, результирующих наборов или возвращаемых значений. В примере ниже показано, как поставщик данных Microsoft SqlClient для SQL Server отправляет и получает входные параметры, выходные параметры и возвращаемые значения. В этом примере новая запись вставляется в таблицу, где столбец первичного ключа является столбцом идентификаторов.

> [!NOTE]
> Если для изменения или удаления данных с помощью <xref:Microsoft.Data.SqlClient.SqlDataAdapter> используются хранимые процедуры, убедитесь, что в определении хранимой процедуры не используется **SET NOCOUNT ON**. В таком случае возвращается число затронутых строк, равное нулю, что `DataAdapter` интерпретирует как конфликт параллелизма. Это событие вызовет исключение <xref:System.Data.DBConcurrencyException>.

## <a name="example"></a>Пример

Пример использует следующую хранимую процедуру для вставки новой категории в таблицу **Northwind** **Categories**. Хранимая процедура принимает значение столбца **CategoryName** в качестве входного параметра и с помощью функции **SCOPE_IDENTITY()** получает новое значения поля идентификатора **CategoryID** и возвращает его в выходном параметре. Инструкция RETURN использует функцию **\@\@ROWCOUNT**, чтобы вернуть количество вставленных строк.

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

Следующий пример кода использует хранимую процедуру `InsertCategory`, показанную выше, в качестве источника для свойства <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> класса <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Выходной параметр `@Identity` будет отражен в наборе данных <xref:System.Data.DataSet> после вставки записи в базу данных при вызове метода `Update` объекта <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Код также получает возвращаемое значение.

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>См. также

- [Извлечение и изменение данных в ADO.NET](retrieving-modifying-data.md)
- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Выполнение команды](execute-command.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
