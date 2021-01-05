---
title: Выполнение операций с каталогами
description: Описывает выполнение команд, которые изменяют схему базы данных.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: e60f542f-6271-495b-a9e4-48553481c2a3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 79eef694b3c6294eb12630f27c4b3688823581c7
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771563"
---
# <a name="performing-catalog-operations"></a>Выполнение операций с каталогами

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Чтобы выполнить команду изменения базы данных или каталога, такую как инструкция CREATE TABLE или CREATE PROCEDURE, создайте объект **Command** с помощью соответствующих инструкций SQL и объекта **Connection**. Выполните команду с помощью метода <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> объекта <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="example"></a>Пример

В следующем примере кода создается хранимая процедура в базе данных Microsoft SQL Server.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#3](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#3)]

## <a name="see-also"></a>См. также

- [Использование команд для изменения данных](use-commands-to-modify-data.md)
- [Команды и параметры](commands-parameters.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
