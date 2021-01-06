---
title: Обновление данных в источнике данных
description: Описывает выполнение команд или хранимых процедур, которые изменяют данные в базе данных.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771486"
---
# <a name="updating-data-in-a-data-source"></a>Обновление данных в источнике данных

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Инструкции SQL, изменяющие данные (например, INSERT, UPDATE, или DELETE), не возвращают строки. Аналогичным образом, многие хранимые процедуры выполняют некоторое действие, но не возвращают строки. Для выполнения команд, которые не возвращают строки, создайте объект **Command** с соответствующей командой SQL и объект **Connection**, включающий требуемую коллекцию **Parameters**. Выполните команду с помощью метода <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> объекта <xref:Microsoft.Data.SqlClient.SqlCommand>.

> [!NOTE]
> Метод **ExecuteNonQuery** возвращает значение типа integer, представляющее число строк, затрагиваемых выполненной инструкцией или хранимой процедурой. Если выполняется несколько инструкций, возвращенное значение является суммой количеств записей, затронутых всеми выполненными инструкциями.

## <a name="example"></a>Пример

В следующем примере кода выполняется инструкция INSERT для вставки записи в базу данных с помощью **ExecuteNonQuery**.
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

В следующем примере кода выполняется хранимая процедура, созданная с помощью примера кода из статьи [Выполнение операций c каталогами](perform-catalog-operations.md). Ни одна строка не возвращается хранимой процедурой, так что при использовании метода **ExecuteNonQuery** хранимая процедура получает входной параметр и возвращает выходной параметр и значение.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>См. также

- [Использование команд для изменения данных](use-commands-to-modify-data.md)
- [Обновление источников данных с помощью DataAdapter](update-data-sources-with-dataadapters.md)
- [Команды и параметры](commands-parameters.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
