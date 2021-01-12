---
description: Системные представления каталога (Transact-SQL)
title: Представления каталога (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sql13.SysViewExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- catalog metadata [SQL Server]
- system views [SQL Server], catalog
- metadata [SQL Server], views
- catalogs [SQL Server], metadata
- catalog views [SQL Server]
- Database Engine [SQL Server], metadata
- catalog views [SQL Server], about catalog views
ms.assetid: 13bccc2f-ed3c-4b58-abd0-ca8bf34a66b8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6763194c3a7d12df95caa6e4696c37befe8f6ca8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093232"
---
# <a name="system-catalog-views-transact-sql"></a>Системные представления каталога (Transact-SQL)

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Представления каталога возвращают данные, используемые компонентом [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Рекомендуется, чтобы использовались представления каталога, потому что они имеют наиболее универсальный интерфейс к метаданным каталога и предоставляют наиболее эффективный способ для получения, преобразования и представления настроенных форм этих данных. Все доступные для пользователя метаданные каталога предоставляются через представления каталога.

> [!NOTE]
> Представления каталога не содержат сведений о репликации, резервном копировании, плане обслуживания базы данных и данных каталога агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Некоторые представления каталога наследуют строки других представлений каталога. Например, представление каталога [sys. Tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) наследуется из представления каталога [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) . Представление каталога sys.objects называется базовым представлением, а представление sys.tables называется производным представлением. Представление каталога sys.tables возвращает столбцы, определенные для таблиц, а также все столбцы, которые возвращает представление каталога sys.objects. Представление каталога sys.objects возвращает строки для объектов, отличных от таблиц, например для хранимых процедур или представлений. После создания таблицы ее метаданные возвращаются в обоих представлениях. Хотя оба представления каталога возвращают различные уровни сведений о таблице, в метаданных этой таблицы существует только одна запись с одним именем и одним object_id. Это может быть описано следующим образом.

- Базовое представление содержит подмножество столбцов и надмножество строк.
- Производное представление содержит надмножество столбцов и подмножество строк.

> [!IMPORTANT]
> В будущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[msCoName](../../includes/msconame-md.md)] может расширить определение любого представления системного каталога путем добавления столбцов в конец списка столбцов. Мы советуем использовать синтаксис SELECT \* FROM *sys.catalog_view_name* в рабочем коде, так как количество возвращаемых столбцов может измениться и прерывать работу приложения.

Представления каталога в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] организованы в следующие категории:

:::row:::
    :::column:::
        [Представления каталога групп доступности AlwaysOn (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)
        
        [Представления каталога базы данных SQL Azure](../../relational-databases/system-catalog-views/azure-sql-database-catalog-views.md)
        
        [Отслеживание изменений представления каталога &#40;&#41;Transact-SQL ](../system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)
        
        [Представления каталога сборок среды CLR &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)
        
        [Представления сборщика данных (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)
        
        [Пространства данных &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)
        
        [Database Mail представления &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/database-mail-views-transact-sql.md)
        
        [Представления каталога следящего сервера зеркального отображения базы данных &#40;Transact-SQL&#41;](../system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
        
        [Представления каталогов баз данных и файлов (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)
        
        [Представления каталога конечных точек (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)
        
        [Представления каталога расширенных событий (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)
        
        [Представления каталога расширенных свойств (Transact-SQL)](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)
        
        [Представления каталога внешних операций &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/external-operations-catalog-views-transact-sql.md)
        
        [Представления каталога FILESTREAM и FileTable &#40;языке Transact-SQL&#41;](../../relational-databases/system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
        
        [Представления каталога полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)
        
        [Представления каталога связанных серверов &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)
    :::column-end:::
    :::column:::
        [Сообщения &#40;об ошибках&#41; представлениях каталога &#40;Transact-SQL&#41;](../system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)
        
        [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)
        
        [Представления каталога функции секционирования &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)
        
        [Административные представления на основе политик (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)
        
        [Resource Governor представления каталога &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)
        
        [Представления каталога хранилища запросов (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
        
        [Представления каталога скалярных типов &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)
        
        [Представления каталога схем &#40;Transact-SQL&#41;](../system-catalog-views/schemas-catalog-views-sys-schemas.md)
        
        [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
        
        [Представления каталога компонента Service Broker (Transact-SQL)](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)
        
        [Представления каталога конфигурации на уровне сервера &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)
        
        [Представления каталога пространственных данных](../../relational-databases/system-catalog-views/spatial-data-catalog-views.md)
        
        [Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)
        
        [Stretch Database представления каталога &#40;&#41;Transact-SQL ](../system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md)
        
        [Схемы XML &#40;представления каталога системы типов XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также:

- [Представления информационной схемы &#40;&#41;Transact-SQL ](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)
- [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)
