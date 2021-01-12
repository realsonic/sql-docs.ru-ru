---
description: Таблицы системы отслеживания измененных данных (Transact-SQL)
title: Таблицы системы отслеживания измененных данных (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a4372d0b-50ca-4e58-80f6-2ed3cb52a84a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6ef80682d8d95188aa764c6e04e768c57298715a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094957"
---
# <a name="change-data-capture-tables-transact-sql"></a>Таблицы системы отслеживания измененных данных (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Система отслеживания измененных данных позволяет отслеживать изменения в таблицах так, что изменения на языках DML и DDL, вносимые в таблицы, постепенно загружались в хранилище данных. Подразделы настоящего раздела описывают системные таблицы, которые хранят сведения, необходимые для операций системы отслеживания измененных данных.  
  
## <a name="in-this-section"></a>В этом разделе  
 [CDC. <capture_instance>_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)  
 Возвращает одну строку для каждого изменения, сделанного в отслеживаемом столбце в связанной исходной таблице.  
  
 [cdc.captured_columns](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
 Возвращает по одной строке для каждого столбца, отслеживаемого в экземпляре отслеживания.  
  
 [cdc.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
 Возвращает по одной строке для каждой таблицы изменений в базе данных.  
  
 [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md)  
 Возвращает одну строку для каждого изменения языка DDL, внесенного в таблицы, поддерживающие систему отслеживания измененных данных.  
  
 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)  
 Возвращает по одной строке для каждой транзакции, имеющей строки в таблице изменений. Эта таблица используется, чтобы сопоставить значения номера LSN фиксации и время фиксации транзакции.  
  
 [cdc.index_columns](../../relational-databases/system-tables/cdc-index-columns-transact-sql.md)  
 Возвращает по одной строке для каждого столбца индекса, связанного с таблицей изменений.  
  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
 Возвращает параметры конфигурации для заданий агента системы отслеживания измененных данных.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры системы отслеживания измененных данных (Transact-SQL)](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [Функции системы отслеживания измененных данных (Transact-SQL)](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)  
  
  
