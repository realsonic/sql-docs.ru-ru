---
title: Извлечение данных с помощью DataReader
description: Узнайте, как извлекать данные с помощью объекта DataReader в ADO.NET с использованием этого примера кода. DataReader предоставляет небуферизованный поток данных.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: dfbe41fe29a3dadc69a2a428f1bf8b606a2a7050
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771359"
---
# <a name="retrieve-data-by-a-datareader"></a>Извлечение данных с помощью DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Для извлечения данных с помощью объекта **DataReader** создайте экземпляр объекта **Command**, а затем создайте поставщика **DataReader**, вызвав метод **Command.ExecuteReader** для получения строк из источника данных. Объект **DataReader** предоставляет небуферизованный поток данных, позволяющий процедурам последовательно обрабатывать результаты из источника данных.

> [!NOTE]
> Объект **DataReader** хорошо подходит для извлечения больших объемов данных, поскольку данные не кэшируются в памяти.

В следующем примере иллюстрируется использование объекта **DataReader**, где `reader` представляет допустимый DataReader, а `command` представляет допустимый объект Command.  

```csharp
reader = command.ExecuteReader();  
```

Используйте метод **DataReader.Read** для получения строки из результатов запроса. Доступ к отдельным столбцам возвращенной строки осуществляется по имени или порядковому номеру столбца через объект **DataReader**. Однако для максимальной производительности объект **DataReader** предоставляет ряд методов, позволяющих обращаться к значениям столбцов в их собственных типах данных (**GetDateTime**, **GetDouble**, **GetGuid**, **GetInt32** и т. д.). Список типизированных методов доступа в объекте **DataReader** для конкретных поставщиков данных см. в разделе <xref:Microsoft.Data.SqlClient.SqlDataReader>. Использование типизированных методов доступа при известном базовом типе данных сокращает объем преобразований типов, необходимых при извлечении значения столбца.  

В следующем примере просматриваются результаты, возвращенные объектом **DataReader** и возвращаются два столбца для каждой строки.  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="close-the-datareader"></a>Закрытие DataReader  

По окончании использования объекта `DataReader` всегда вызывайте метод `Close()`.

> [!NOTE]
> Если метод **Command** содержит выходные параметры или возвращаемые значения, они будут недоступны до закрытия объекта **DataReader**.  

> [!IMPORTANT]
> Имейте в виду, что, пока объект **DataReader** открыт, соединение **Connection** используется исключительно этим объектом **DataReader**. Невозможно выполнять какие-либо команды для **Connection**, включая создание другого объекта **DataReader**, пока исходный объект **DataReader** не будет закрыт.  

> [!NOTE]
> В методе **Finalize** вашего класса нельзя вызывать методы **Close** или **Dispose** объектов **Connection**, **DataReader** или любого другого управляемого объекта. В методе завершения следует только освобождать неуправляемые ресурсы, которыми ваш класс непосредственно владеет. Если класс не владеет какими-либо неуправляемыми ресурсами, не включайте в его определение метод **Finalize**. Дополнительные сведения см. в статье [Сборка мусора](/dotnet/standard/garbage-collection/index).
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>Извлечение нескольких результирующих наборов при помощи NextResult

Если объект **DataReader** возвращает несколько результирующих наборов, используйте метод **NextResult** для просмотра наборов результатов по порядку. В следующем примере показан объект <xref:Microsoft.Data.SqlClient.SqlDataReader>, обрабатывающий результаты двух инструкций SELECT с помощью метода <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>.  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>Получение данных схемы от DataReader  

Когда открыт объект **DataReader**, с помощью метода **GetSchemaTable** можно получить данные схемы о текущем результирующем наборе. Метод **GetSchemaTable** возвращает объект <xref:System.Data.DataTable>, заполненный строками и столбцами, которые содержат данные схемы для текущего результирующего набора. Объект **DataTable** содержит одну строку для каждого столбца результирующего набора. Каждый столбец строки таблицы схемы соответствует свойству столбца, возвращенного в результирующем наборе, где **ColumnName** — это имя свойства, а значение столбца — это значение свойства. В следующем примере на консоль выводятся данные схемы для объекта **DataReader**.  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>См. также

- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Команды и параметры](commands-parameters.md)
- [Извлечение сведений о схеме базы данных](retrieving-database-schema-information.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
