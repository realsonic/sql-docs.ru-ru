---
title: Масштабируемость | Документация Майкрософт
description: Узнайте о новых функциях масштабируемости в хранилище на диске для таблиц, оптимизированных для памяти в SQL Server, таких как использование нескольких потоков для хранения таблиц.
ms.custom: ''
ms.date: 08/27/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a4891c57-56bb-49f4-9bb5-f11b745279e5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e72e67bf033ecef94b318d69b31f33604572cf8
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172066"
---
# <a name="scalability"></a>Масштабируемость
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] содержит улучшения масштабируемости для хранения на дисках таблиц, оптимизированных для памяти. 

## <a name="multiple-threads-to-persist-memory-optimized-tables"></a>Несколько потоков для сохранения таблиц, оптимизированных для памяти.  
  
В [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] использовался один поток автономных контрольных точек, который проверял журнал транзакций на наличие изменений в таблицах, оптимизированных для памяти, и сохранял их в файлы контрольных точек (например, файлы данных и разностные файлы). В компьютерах с большим числом ядер один поток контрольных точек может отставать от времени.  
  
Начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] за сохранение изменений в таблицах, оптимизированных для памяти, отвечают несколько параллельных потоков.  
  
## <a name="multi-threaded-recovery"></a>Многопоточное восстановление.
В предыдущем выпуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]журнал, применяемый при операции восстановления, использовал один поток. Начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] для применения журнала используется несколько потоков.  
  
## <a name="merge-operation"></a>Операция MERGE  
Теперь операция MERGE является многопоточной.  
   
> [!NOTE]
> Слияние вручную было отключено ввиду того, что многопоточные операции слияния должны справиться с нагрузкой. 

## <a name="dynamic-management-views"></a>Динамические административные представления  
Динамические административные представления [sys.dm_db_xtp_checkpoint_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md) и [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) существенно изменились.  

## <a name="storage-management"></a>Управление хранением
Подсистема, выполняющаяся в памяти OLTP, продолжает использовать оптимизированную для памяти файловую группу в зависимости от FILESTREAM, но отдельные файлы в файловой группе не связаны с FILESTREAM. Эти файлы полностью управляются (создание, удаление и сборка мусора) подсистемой, выполняющейся в памяти OLTP. 

> [!NOTE]
> Представление [DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) не поддерживается.  
  
## <a name="see-also"></a>См. также:   
[Создание и управление хранилищем для оптимизированных для памяти объектов](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)     
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)    
