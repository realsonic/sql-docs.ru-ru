---
title: Транзакции и параллельность
description: Узнайте, как использовать поставщик данных Microsoft SqlClient для SQL Server с возможностями транзакций и параллельности.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a30a3d3184c411fc0b54c0e26330a8fb138ffdae
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051365"
---
# <a name="transactions-and-concurrency"></a>Транзакции и параллельность

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Транзакция состоит из одной команды или группы команд, которые выполняются как пакет. Транзакции позволяют объединить несколько операций в одну единицу работы. Если в какой-либо точке транзакции возникает ошибка, может быть выполнен откат всех обновлений к их состоянию до начала транзакции.

Для обеспечения согласованности данных транзакция должна соответствовать свойствам ACID (атомарность, согласованность, изоляция и устойчивость). Большая часть систем реляционных баз данных, таких как Microsoft SQL Server, поддерживает транзакции, предоставляя блокировку, ведение журнала и средства управления транзакцией при выполнении клиентским приложением операций обновления, вставки или удаления.

> [!NOTE]
> Транзакции, которые задействуют множество ресурсов, могут снизить параллелизм, если слишком долго удерживают блокировки. Поэтому транзакции следует делать как можно короче.  

В том случае, если в транзакции участвует несколько таблиц одной базы данных или одного сервера, явные транзакции в хранимых процедурах часто выполняются лучше. Транзакции можно создавать в хранимых процедурах SQL Server с использованием инструкций Transact-SQL `BEGIN TRANSACTION`, `COMMIT TRANSACTION` и `ROLLBACK TRANSACTION`. Дополнительные сведения см. в электронной документации по SQL Server.

Если для выполнения транзакций требуются различные диспетчеры ресурсов, например для транзакций между SQL Server и Oracle, необходимо использовать распределенную транзакцию.

## <a name="in-this-section"></a>В этом разделе

 [Локальные транзакции](local-transactions.md)  
 Демонстрирует выполнение транзакций на базе данных.  
  
 [Распределенные транзакции](distributed-transactions.md)  
 Описывает выполнение распределенных транзакций в ADO.NET.  
  
 [Интеграция System.Transactions с SQL Server](system-transactions-integration-with-sql-server.md)  
 Описывает интеграцию <xref:System.Transactions> с SQL Server для работы с распределенными транзакциями.  
  
 [Оптимистический параллелизм](optimistic-concurrency.md) описывает оптимистический и пессимистический параллелизм, а также способ проверки нарушений параллелизма.  

## <a name="see-also"></a>См. также раздел

- [Основные сведения о транзакциях](/dotnet/framework/data/transactions/transaction-fundamentals)
- [Подключение к источнику данных](connecting-to-data-source.md)
- [Команды и параметры](commands-parameters.md)
- [Объекты DataAdapter и DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
