---
description: SET XACT_ABORT (Transact-SQL)
title: SET XACT_ABORT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- XACT_ABORT_TSQL
- XACT_ABORT
- SET XACT_ABORT
- SET_XACT_ABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- XACT_ABORT option
- automatic transaction roll backs
- transactions [SQL Server], rolling back
- rolling back transactions, SET XACT_ABORT
- roll back transactions [SQL Server]
- SET XACT_ABORT statement
ms.assetid: cbcaa433-58f2-4dc3-a077-27273bef65b5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29737fa62f99028ebf35bca5d8e9c0e94fa1af21
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080115"
---
# <a name="set-xact_abort-transact-sql"></a>SET XACT_ABORT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> Инструкция **THROW** учитывает **SET XACT_ABORT**. **Инструкция RAISERROR** — нет. В новых приложениях следует использовать инструкцию **THROW** вместо **RAISERROR**.

Указывает, выполняет ли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматический откат текущей транзакции, если инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)] вызывает ошибку выполнения.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
SET XACT_ABORT { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks

Если выполнена инструкция SET XACT_ABORT ON и инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)] вызывает ошибку, вся транзакция завершается и выполняется ее откат.

Если выполнена инструкция SET XACT_ABORT OFF, в некоторых случаях выполняется откат только вызвавшей ошибку инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], а обработка транзакции продолжается. В зависимости от серьезности ошибки возможен откат всей транзакции при выполненной инструкции SET XACT_ABORT OFF. OFF — это значение по умолчанию в инструкции T-SQL, а ON — значение по умолчанию в триггере.

Инструкция SET XACT_ABORT не влияет на компиляцию ошибок (например, синтаксических).

Параметр XACT_ABORT должен иметь значение ON для инструкций изменения данных в явных или неявных транзакциях, применяющихся к большинству поставщиков OLE DB, включая [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Единственным случаем, когда этот параметр не требуется, является поддержка поставщиком вложенных транзакций.

Когда ANSI_WARNINGS=OFF, нарушения разрешений вызывают отмену транзакций.

Значение параметра XACT_ABORT устанавливается во время выполнения, а не во время синтаксического анализа.

Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос.

```sql
DECLARE @XACT_ABORT VARCHAR(3) = 'OFF';
IF ( (16384 & @@OPTIONS) = 16384 ) SET @XACT_ABORT = 'ON';
SELECT @XACT_ABORT AS XACT_ABORT;

```

## <a name="examples"></a>Примеры

Следующий программный код вызывает ошибку нарушения внешнего ключа в транзакции, в состав которой входят другие инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. В первом наборе инструкций происходит ошибка, но остальные инструкции выполняются успешно, и транзакция фиксируется. Во втором наборе инструкций параметру `SET XACT_ABORT` присваивается значение `ON`. Это приводит к тому, что ошибка инструкции завершает пакет и выполняется откат транзакции.

```sql
IF OBJECT_ID(N't2', N'U') IS NOT NULL
    DROP TABLE t2;
GO
IF OBJECT_ID(N't1', N'U') IS NOT NULL
    DROP TABLE t1;
GO  
CREATE TABLE t1
    (a INT NOT NULL PRIMARY KEY);
CREATE TABLE t2
    (a INT NOT NULL REFERENCES t1(a));
GO
INSERT INTO t1 VALUES (1);
INSERT INTO t1 VALUES (3);
INSERT INTO t1 VALUES (4);
INSERT INTO t1 VALUES (6);
GO
SET XACT_ABORT OFF;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (1);
INSERT INTO t2 VALUES (2); -- Foreign key error.
INSERT INTO t2 VALUES (3);
COMMIT TRANSACTION;
GO
SET XACT_ABORT ON;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (4);
INSERT INTO t2 VALUES (5); -- Foreign key error.
INSERT INTO t2 VALUES (6);
COMMIT TRANSACTION;
GO
-- SELECT shows only keys 1 and 3 added.
-- Key 2 insert failed and was rolled back, but
-- XACT_ABORT was OFF and rest of transaction
-- succeeded.
-- Key 5 insert error with XACT_ABORT ON caused
-- all of the second transaction to roll back.
SELECT *
 FROM t2;
GO
```

## <a name="see-also"></a>См. также:

- [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)
- [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)
- [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)
- [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
- [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)
- [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
