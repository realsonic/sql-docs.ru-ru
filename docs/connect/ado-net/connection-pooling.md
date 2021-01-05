---
title: Организация пулов соединений
description: Сведения о объединении подключений в пул, которое представляет собой технику оптимизации, используемую ADO.NET для максимально экономичного открытия подключений к источникам данных.
ms.date: 11/13/2020
ms.assetid: 955c057f-aea8-4ba8-aa6d-e3dfa18ba8d5
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 783fad79522c52685349defca93360c4ea8c80c9
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771653"
---
# <a name="connection-pooling"></a>Организация пулов соединений

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

На соединение с источником данных может требоваться значительное время. Чтобы свести к минимуму затраты на открытие подключений, в ADO.NET используется техника *объединения подключений в пул*, которая позволяет минимизировать затраты на часто открываемые и закрываемые подключения.

## <a name="in-this-section"></a>В этом разделе  

[Объединение подключений в пул в SQL Server (ADO.NET)](sql-server-connection-pooling.md)  
Общие сведения о пулах соединений и описание принципов работы пулов соединений в SQL Server.

## <a name="see-also"></a>См. также

- [Извлечение и изменение данных в ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
