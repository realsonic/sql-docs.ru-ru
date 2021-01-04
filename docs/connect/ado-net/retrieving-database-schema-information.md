---
title: Извлечение сведений о схеме базы данных
description: Сведения об использовании поставщика данных Microsoft SqlClient для SQL Server для извлечения сведений о схеме базы данных.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051391"
---
# <a name="retrieving-database-schema-information"></a>Извлечение сведений о схеме базы данных

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Получение сведений о схеме из базы данных выполняется с помощью процесса обнаружения схемы. Обнаружение схемы позволяет приложениям запрашивать управляемые поставщики для поиска и возвращения сведений о схеме базы данных, также называемых *метаданными*, для данной базы данных. Различные элементы схемы базы данных, например таблицы, столбцы и хранимые процедуры, предоставляются через коллекции схем. Каждая коллекция схемы в зависимости от используемого поставщика содержит различные сведения о схеме.

Поставщик данных Microsoft SqlClient для SQL Server реализует метод **GetSchema** в классе **SqlConnection**, а сведения о схеме, возвращаемые из метода **GetSchema**, представляются в виде объекта <xref:System.Data.DataTable>. **GetSchema** — перегружаемый метод, содержащий необязательные параметры для указания возвращаемой коллекции схем и ограничения объема возвращаемых сведений. Поставщик данных SqlClient также предоставляет метод **GetSchemaTable**, возвращающий объект DataTable с описанием метаданных столбцов объекта **SqlDataReader**.

## <a name="in-this-section"></a>В этом разделе

[Метод GetSchema и коллекции схем](getschema-and-schema-collections.md)  
Описание метода **GetSchema** и его использования для получения и ограничения сведений о схеме из базы данных.

[Ограничения схемы](schema-restrictions.md)  
Описание ограничений схемы, которые можно использовать с методом **GetSchema**. 

[Общие коллекции схем](common-schema-collections.md)  
Описание всех общих коллекций схем, которые поддерживаются всеми управляемыми поставщиками .NET.  
  
[Коллекции схем SQL Server](sql-server-schema-collections.md)  
Описание дополнительных коллекций схем, поддерживаемых поставщиком данных Microsoft SqlClient для SQL Server. 

## <a name="reference"></a>Справочник

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
Описание метода **GetSchema** класса <xref:System.Data.Common.DbConnection>.

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
Описание метода **GetSchema** класса <xref:Microsoft.Data.SqlClient.SqlConnection>.

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
Описание метода **GetSchemaTable** класса <xref:System.Data.Common.DbDataReader>. 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
Описание метода **GetSchemaTable** класса <xref:Microsoft.Data.SqlClient.SqlDataReader>.

## <a name="see-also"></a>См. также

- [Извлечение и изменение данных в ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
