---
description: Системные таблицы (Transact-SQL)
title: Системные таблицы (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server]
- tables [SQL Server], system tables
- information retrieval [SQL Server]
- status information [SQL Server], system tables
- system information [SQL Server]
- system tables [SQL Server]
- system tables [SQL Server], about system tables
- system tables [SQL Server], retrieving information from
- retrieving system table information
ms.assetid: 56b8ad51-930c-4e5c-8d99-8c939d5b70ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: a753d453572534373be1b18149a41ea822a3f1f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102683"
---
# <a name="system-tables-transact-sql"></a>Системные таблицы (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В подразделах данного раздела описаны системные таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Системные таблицы не должны изменяться непосредственно ни одним пользователем. Например, не пытайтесь изменять системные таблицы с помощью инструкций DELETE, UPDATE или INSERT либо с помощью пользовательских триггеров.  
  
 Обращение к документированным столбцам системных таблиц разрешено. Однако многие столбцы системных таблиц не документированы. В приложениях непосредственные запросы к недокументированным столбцам применять не следует. Вместо этого для получения данных, хранящихся в системных таблицах, в приложениях можно использовать один из следующих компонентов:  
  
-   Системные хранимые процедуры  
  
-   инструкции и функции языка [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Управляющие объекты (SMO)  
  
-   объекты RMO;  
  
-   функции каталога Database API.  
  
 Эти компоненты составляют опубликованный API-интерфейс для получения системных сведений от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[msCoName](../../includes/msconame-md.md)] поддерживает совместимость этих компонентов от выпуска к выпуску. Формат системных таблиц зависит от внутренней архитектуры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и может изменяться от выпуска к выпуску. Поэтому приложениям, получающим непосредственный доступ к недокументированным столбцам системных таблиц, могут потребоваться изменения, прежде чем они смогут получить доступ к более поздней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>В этом разделе  
 Подразделы о системных таблицах организованы по следующим группам функций.  

:::row:::
    :::column:::
        [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)

        [Таблицы системы отслеживания измененных данных (Transact-SQL)](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)

        [Таблицы плана обслуживания базы данных &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/database-maintenance-plan-tables-transact-sql.md)

        [Таблицы расширенных событий SQL Server (Transact-SQL)](../../relational-databases/extended-events/xevents-references-system-objects.md#system-tables)

        [Integration Servicesные таблицы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/integration-services-tables-transact-sql.md)
    :::column-end:::
    :::column:::
        [Таблицы доставки журналов (Transact-SQL)](../../relational-databases/system-tables/log-shipping-tables-transact-sql.md)

        [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)

        [Агент SQL Serverные таблицы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)

        [sys.sysоледбусерс &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysoledbusers-transact-sql.md)

        [systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также:  
 [Представления совместимости &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
