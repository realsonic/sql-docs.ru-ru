---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6420062352983a45cb1f5f81ddcae83e173b5130
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091866"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Связывает до 128 байт бинарных данных с текущим сеансом или соединением.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *binary_str*  
 Константа типа **binary** или константа, которая неявно преобразуется к типу **binary**, для связи с текущим сеансом или соединением.  
  
 **@** *binary_var*  
 Переменные типа **varbinary** или **binary**, удерживающие значение контекста для связи с текущим сеансом или соединением.  
  
## <a name="remarks"></a>Комментарии  
 Предпочтительным способом для получения контекстных данных по текущему сеансу является использование функции CONTEXT_INFO. Контекстные данные по сеансу также хранятся в столбцах **context_info** следующих системных представлений:  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 SET CONTEXT_INFO не может быть задан в определенной пользователем функции. Нельзя применить значение NULL к SET CONTEXT_INFO, так как представления, хранящие значения, не допускают значения NULL.  
  
 SET CONTEXT_INFO не принимает другие выражения, нежели имена констант и переменных. Чтобы задать сведения о контексте результатом вызываемой функции, необходимо сначала назначить результат вызова функции в переменную типа **binary** или **varbinary**.  
  
 Если SET CONTEXT_INFO вызывается в сохраненной процедуре или триггере, то в отличие от других инструкций SET, для контекстных данных установится новое значение, сохраненное после завершения хранимой процедуры или триггера.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>A. Задание контекстных данных с помощью константы  
 Следующий пример демонстрирует использование `SET CONTEXT_INFO` с заданием значения и отображением результатов. Заметьте, что запросы `sys.dm_exec_sessions` требуют разрешений на SELECT и VIEW SERVER STATE везде, где бы не использовалась функция CONTEXT_INFO.  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>Б. Задание контекстных данных с помощью функции  
 Следующий пример демонстрирует задание контекстного значения с использованием вывода функции, где значение от функции должно быть сначала помещено в переменную **binary**.  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [CONTEXT_INFO  (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)  
  
  
