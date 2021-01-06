---
title: Команды и параметры
description: Узнайте, как использовать объекты команд для поставщика данных SqlClient (Майкрософт) для SQL Server для выполнения команд и получения результатов из источника данных.
ms.date: 11/25/2020
ms.assetid: b623f810-d871-49a5-b0f5-078cc3c34db6
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 07f23aa6545636fd8cbbf1717f6c8ee81ebe9294
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771478"
---
# <a name="commands-and-parameters"></a>Команды и параметры

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

После установки соединения с источником данных при помощи объекта <xref:System.Data.Common.DbCommand> можно выполнять команды и возвращать результаты из источника данных. Вы можете создать команду с помощью одного из конструкторов команд для поставщика данных Microsoft SqlClient для SQL Server. Конструкторы могут принимать необязательные аргументы, например инструкцию SQL для выполнения в источнике данных, объект <xref:System.Data.Common.DbConnection> или объект <xref:System.Data.Common.DbTransaction>.

Эти объекты также можно настроить как свойства команды. При помощи метода <xref:System.Data.Common.DbConnection.CreateCommand%2A> объекта `DbConnection` также можно создать команду для конкретного соединения. Инструкцию SQL, выполняемую командой, можно настроить с помощью свойства <xref:System.Data.Common.DbCommand.CommandText%2A>. Поставщик данных Microsoft SqlClient для SQL Server содержит объект <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="in-this-section"></a>В этом разделе

[Выполнение команды](execute-command.md)  
Описание объекта ADO.NET `Command` и его использования для выполнения запросов и команд в источнике данных.

[Настройка параметров](configure-parameters.md)  
Описание работы с параметрами `Command`, включая направление, типы данных и синтаксис параметров.

[Создание команд с помощью CommandBuilder](generate-commands-with-commandbuilders.md)  
Описание использования построителей команд для автоматического формирования команд INSERT, UPDATE и DELETE для адаптера `DataAdapter`, у которого имеется команда SELECT с одной таблицей.

[Получение одного значения из базы данных](obtain-single-value-from-database.md)  
Описывается использование метода `ExecuteScalar` объекта `Command` для возврата из запроса к базе данных одиночного значения.

[Использование команд для изменения данных](use-commands-to-modify-data.md)  
Описание использования поставщика данных Microsoft SqlClient для SQL Server для выполнения хранимых процедур или инструкций языка описания данных (DDL).

## <a name="see-also"></a>См. также

- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [подключение к источнику данных](connecting-to-data-source.md);
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
