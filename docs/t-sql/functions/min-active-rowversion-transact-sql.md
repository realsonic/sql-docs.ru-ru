---
description: MIN_ACTIVE_ROWVERSION (Transact-SQL)
title: MIN_ACTIVE_ROWVERSION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MIN_ACTIVE_ROWVERSION
- MIN_ACTIVE_ROWVERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MIN_ACTIVE_ROWVERSION function [Transact-SQL]
ms.assetid: 87c89547-8ea1-4820-b75e-36be683e4e10
author: cawrites
ms.author: chadam
ms.openlocfilehash: 415d86958972d059b0cb9a27b8a741ac7a4ed136
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102516"
---
# <a name="min_active_rowversion-transact-sql"></a>MIN_ACTIVE_ROWVERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает наименьшее активное значение **rowversion** в текущей базе данных. Значение **rowversion** является активным, если оно используется в незафиксированной транзакции. Дополнительные сведения см. в статье [rowversion (Transact-SQL)](../../t-sql/data-types/rowversion-transact-sql.md).  
  
> [!NOTE]  
>   Тип данных **rowversion** также известен как **timestamp**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
MIN_ACTIVE_ROWVERSION ( ) 
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Возвращает значение **binary(8)**.  
  
## <a name="remarks"></a>Комментарии  
 MIN_ACTIVE_ROWVERSION — это недетерминированная функция, возвращающая наименьшее активное значение **rowversion** в текущей базе данных. Новое значение **rowversion** обычно формируется, когда в таблице, содержащей столбец с типом **rowversion**, производится операция вставки или обновления. Если в базе данных не существует активных значений, то функция MIN_ACTIVE_ROWVERSION возвращает то же значение, что и @@DBTS + 1.  
  
 Функция MIN_ACTIVE_ROWVERSION полезна, например, когда при синхронизации данных значения **rowversion** используются для группирования наборов изменений. Если приложение использует функцию @@DBTS, а не MIN_ACTIVE_ROWVERSION, могут быть пропущены изменения, произведенные во время синхронизации.  
  
 Функция MIN_ACTIVE_ROWVERSION не затрагивается изменениями на уровнях изоляции транзакции.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере значения **rowversion** возвращаются с помощью функций `MIN_ACTIVE_ROWVERSION` и `@@DBTS`. Обратите внимание, что при отсутствии в базе данных активных транзакций эти значения будут другими.  
  
```sql  
-- Create a table that has a ROWVERSION column in it.  
CREATE TABLE RowVersionTestTable (rv ROWVERSION)  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E2  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E3  
  
-- Insert a row.  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E3  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Insert a new row inside a transaction but do not commit.  
BEGIN TRAN  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
--0x00000000000007E4  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Commit the transaction.  
COMMIT  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E5  
```  
  
## <a name="see-also"></a>См. также:  
 [@@DBTS (Transact-SQL)](../../t-sql/functions/dbts-transact-sql.md)   
 [rowversion (Transact-SQL)](../../t-sql/data-types/rowversion-transact-sql.md)  
  
  
