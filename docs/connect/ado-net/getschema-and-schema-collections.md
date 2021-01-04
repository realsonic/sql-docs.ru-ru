---
title: Метод GetSchema и коллекции схем
description: Описание применения метода GetSchema для получения и ограничения сведений о схеме из базы данных.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051399"
---
# <a name="get-schema-and-schema-collections"></a>Метод GetSchema и коллекции схем

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Классы **SqlConnection** в поставщике данных Microsoft SqlClient для SQL Server реализуют метод **GetSchema**, который используется для получения сведений о схеме базы данных, с которой в данный момент установлено соединение, и метод **GetSchema** возвращает сведения о схеме в виде объекта <xref:System.Data.DataTable>. **GetSchema** — перегружаемый метод, содержащий необязательные параметры для указания возвращаемой коллекции схем и ограничения объема возвращаемых сведений.

## <a name="specifying-the-schema-collections"></a>Указание коллекций схем

Первым необязательным параметром метода **GetSchema** является имя коллекции, указанное в виде строки. Существует два типа коллекций схем: стандартные (общие для всех поставщиков) и специальные (определенные для каждого поставщика).  

Можно запросить поставщик данных Microsoft SqlClient для SQL Server для определения списка поддерживаемых коллекций схем, вызвав метод **GetSchema** без аргументов или с именем коллекции схем "MetaDataCollections". При этом будет возвращена <xref:System.Data.DataTable> со списком поддерживаемых коллекций схем, число ограничений, которые каждая из них поддерживает, и число идентификационных частей, которые в них используются.  

### <a name="retrieving-schema-collections-example"></a>Пример извлечения коллекций схем

В следующем примере демонстрируется использование метода <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> класса <xref:Microsoft.Data.SqlClient.SqlConnection> поставщика данных Microsoft SqlClient для SQL Server для получения сведений схем обо всех таблицах, содержащихся в образце базы данных **AdventureWorks**:  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>См. также раздел

- [Извлечение сведений о схеме базы данных](retrieving-database-schema-information.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
