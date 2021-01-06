---
title: Сопоставление типов данных
description: Сведения о типах данных, используемых поставщиком данных Microsoft SqlClient для SQL Server.
ms.date: 11/13/2020
ms.assetid: d4afab94-ada6-4c77-a73c-41f17bae6b5a
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 808acdf89350f539f03f6fdc75f2f7a5ed5b7707
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771622"
---
# <a name="data-type-mappings-in-adonet"></a>Сопоставления типов данных в ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET основывается на системе общих типов, которая определяет правила объявления, использования типов в среде выполнения и управления ими. Система типов состоит из типов-значений и типов-ссылок, производных от базового типа <xref:System.Object>. При работе с источником данных не указанный явным образом тип данных выводится из поставщика данных. Например, объект <xref:System.Data.DataSet> не зависит ни от одного конкретного источника данных. Получение данных в `DataSet` осуществляется из источника данных, а изменения передаются для сохранения в источнике данных с использованием `DataAdapter`. Такой поток программы означает, что когда `DataAdapter` заполняет <xref:System.Data.DataTable> в `DataSet` значениями из источника данных, результирующие типы данных столбцов в `DataTable` будут типами .NET Framework, а не поставщика данных Microsoft SqlClient для SQL Server, который используется для подключения к источнику данных.

Аналогично, когда `DataReader` возвращает значение из источника данных, результирующее значение сохраняется в локальной переменной с типом .NET Framework. Для `Fill` операций методов `DataAdapter` и `Get` в `DataReader` тип .NET Framework выводится из значения, возвращенного поставщиком данных Microsoft SqlClient для SQL Server.

Если известен тип возвращаемого значения, то вместо выводимого типа данных можно воспользоваться типизированными методами доступа объекта `DataReader`. Типизированные методы доступа обеспечивают оптимальную производительность, возвращая значение с конкретным типом .NET Framework, что устраняет необходимость в дополнительном преобразовании типов.

> [!NOTE]
> Значения NULL в типах данных поставщика данных Microsoft SqlClient для SQL Server представляются как `DBNull.Value`.

## <a name="in-this-section"></a>в этом разделе

[Сопоставления типов данных SQL Server](sql-server-data-type-mappings.md) — список выводимых сопоставлений для типов данных и методов доступа к данным для <xref:Microsoft.Data.SqlClient>.

[Числа с плавающей запятой](floating-point-numbers.md) — описание проблем, с которыми часто сталкиваются разработчики при работе с числами с плавающей запятой.

## <a name="see-also"></a>См. также

- [Типы данных SQL Server и ADO.NET](./sql/sql-server-data-types.md)
- [Настройка параметров](configure-parameters.md)
- [Извлечение сведений о схеме базы данных](retrieving-database-schema-information.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
