---
description: '&#x40;&#x40;FETCH_STATUS (Transact-SQL)'
title: FETCH_STATUS (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@FETCH_STATUS'
- '@@FETCH_STATUS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- status information [SQL Server], FETCH
- '@@FETCH_STATUS function'
ms.assetid: 93659193-e4ff-4dfb-9043-0c4114921b91
author: cawrites
ms.author: chadam
ms.openlocfilehash: ef3c22679bed84906f325095f28155dd34f16a09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092343"
---
# <a name="x40x40fetch_status-transact-sql"></a>&#x40;&#x40;FETCH_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает состояние последней инструкции FETCH, вызванной в любом курсоре, открытом в рамках этого подключения.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
@@FETCH_STATUS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Тип возвращаемых данных  
 **integer**  
  
## <a name="return-value"></a>Возвращаемое значение  
  
|Возвращаемое значение|Описание|  
|------------------|-----------------|  
|&nbsp;0|Инструкция FETCH была выполнена успешно.|  
|-1|Выполнение инструкции FETCH завершилось неудачно или строка оказалась вне пределов результирующего набора.|  
|-2|Выбранная строка отсутствует.|
|–9|Курсор не выполняет операцию выборки.|  
  
## <a name="remarks"></a>Комментарии  
Глобальный статус `@@FETCH_STATUS` для всех курсоров в рамках подключения предполагает внимательное использование. После выполнения инструкции FETCH следует выполнить тест для `@@FETCH_STATUS`, прежде чем любая другая инструкция FETCH будет выполнена применительно к другому курсору. `@@FETCH_STATUS` не определяется до выполнения выборки в рамках подключения.  
  
Например, пользователь выполняет инструкцию FETCH из одного курсора, а затем вызывает хранимую процедуру, которая открывает и обрабатывает результаты из другого курсора. Когда управление возвращается от вызванной хранимой процедуры, `@@FETCH_STATUS` отражает результаты последней инструкции FETCH, выполненной в хранимой процедуре, а не результаты инструкции FETCH, выполненной до вызова хранимой процедуры.  
  
Чтобы получить состояние последней выборки конкретного курсора, запросите столбец **fetch_status** функции динамического управления **sys.dm_exec_cursors**.  
  
## <a name="examples"></a>Примеры  
В этом примере используется `@@FETCH_STATUS` для управления действиями курсора в цикле `WHILE`.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT BusinessEntityID, JobTitle  
FROM AdventureWorks2012.HumanResources.Employee;  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Функции работы с курсорами (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
