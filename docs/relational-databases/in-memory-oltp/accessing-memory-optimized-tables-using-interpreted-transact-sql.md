---
title: Таблицы, оптимизированные для памяти, с интерпретируемыми инструкциями T-SQL
description: Сведения о доступе к оптимизированным для памяти таблицам с использованием интерпретируемых пакетов Transact-SQL (пакеты Transact-SQL или хранимые процедуры в SQL Server).
ms.custom: seo-dt-2019
ms.date: 05/31/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 92a44d4d-0e53-4fb0-b890-de264c65c95a
author: MightyPen
ms.author: genemi
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c88fd07e9af86a3c12561935364514b23bc2ea91
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172286"
---
# <a name="accessing-memory-optimized-tables-using-interpreted-transact-sql"></a>Доступ к таблицам, оптимизированным для памяти, с помощью интерпретируемых инструкций Transact-SQL
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

 Лишь за некоторыми исключениями, можно получить доступ к оптимизированным для памяти таблицам с помощью любого запроса [!INCLUDE[tsql](../../includes/tsql-md.md)] или операции DML (SELECT, INSERT, UPDATE или DELETE), нерегламентированных пакетов и модулей SQL, таких как хранимые процедуры, триггеры, функции, возвращающие табличное значение, и представления.  
  
Интерпретируемый [!INCLUDE[tsql](../../includes/tsql-md.md)] — это пакеты или хранимые процедуры [!INCLUDE[tsql](../../includes/tsql-md.md)] , отличные от скомпилированных в собственном коде хранимых процедур. Доступ интерпретированного [!INCLUDE[tsql](../../includes/tsql-md.md)] к оптимизированным для памяти таблицам называется доступом взаимодействия.  

Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], запросы в интерпретируемом [!INCLUDE[tsql](../../includes/tsql-md.md)] могут сканировать оптимизированные для памяти таблицы не только в последовательном, но и в параллельном режиме.

Доступ к оптимизированным для памяти таблицам можно также осуществлять с помощью скомпилированной в собственном коде хранимой процедуры. Скомпилированные в собственном коде хранимые процедуры рекомендуется использовать для важных с точки зрения производительности операций OLTP.  
  
Интерпретируемый доступ [!INCLUDE[tsql](../../includes/tsql-md.md)] рекомендуется в следующих случаях.  
  
- Нерегламентированные запросы и административные задачи.  
  
- Запросы для отчетов, в которых обычно используются конструкции, недоступные в скомпилированных в собственном коде хранимых процедурах (например, *оконные* функции, также известные как функции [OVER](../../t-sql/queries/select-over-clause-transact-sql.md) ).  
  
- Для переноса критически важных для производительности частей приложения в оптимизированные для памяти таблицы с минимальными изменениями кода приложения или без изменений. Потенциально перенесенные таблицы могут обеспечить улучшение производительности. Дополнительного роста производительности можно добиться путем переноса хранимых процедур в скомпилированные в собственном коде хранимые процедуры.  
  
- Когда инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] недоступна для скомпилированных в собственном коде хранимых процедур.  
  
Однако следующие конструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] не поддерживаются в хранимых процедурах интерпретированного [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые обращаются к данным из оптимизированных для памяти таблиц.  
  
|Область|Не поддерживается|  
|----------|-----------------|  
|Доступ к таблицам|TRUNCATE TABLE<br /><br /> MERGE (оптимизированная для памяти таблица в качестве назначения)<br /><br /> Динамический курсор и курсор ключевого набора (они автоматически понижаются в статический).<br /><br /> Доступ из модулей CLR с использованием контекстного соединения.<br /><br /> Ссылка на оптимизированную для памяти таблицу из индексированного представления|  
|Между базами данных|Межбазовые запросы<br /><br /> Межбазовые транзакции<br /><br /> Связанные серверы|  
  
## <a name="table-hints"></a>Табличные указания

Дополнительные сведения о табличных указаниях см. в разделе. [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md). Функция SNAPSHOT добавлена для поддержки [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
При доступе к оптимизированной для памяти таблице с использованием интерпретируемого кода [!INCLUDE[tsql](../../includes/tsql-md.md)]не поддерживаются следующие табличные указания.  

:::row:::
    :::column:::
        HOLDLOCK

        PAGLOCK

        READUNCOMMITTED

        TABLOCKXX
    :::column-end:::
    :::column:::
        IGNORE_CONSTRAINTS

        READCOMMITTED

        ROWLOCK

        UPDLOCK
    :::column-end:::
    :::column:::
        IGNORE_TRIGGERS

        READCOMMITTEDLOCK

        SPATIAL_WINDOW_MAX_CELLS = *целое число*

        XLOCK
    :::column-end:::
    :::column:::
        NOWAIT

        READPAST

        TABLOCK
    :::column-end:::
:::row-end:::

При доступе к оптимизированной для памяти таблице из явной или неявной транзакции с помощью интерпретируемого [!INCLUDE[tsql](../../includes/tsql-md.md)]необходимо выполнить как минимум одно из следующих действий:  
  
- указать [табличное указание уровня изоляции](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md) , например SNAPSHOT, REPEATABLEREAD или SERIALIZABLE;  
  
- задать для параметра базы данных [MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT](../../t-sql/statements/alter-database-transact-sql-set-options.md) значение ON (включено).  
  
Табличное указание уровня изоляции не требуется при обращении к оптимизированным для памяти таблицам с помощью запросов с [автоматической фиксацией](../../odbc/reference/develop-app/auto-commit-mode.md).  
  
## <a name="see-also"></a>См. также:

[Поддержка Transact-SQL для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   

[Миграция в In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)