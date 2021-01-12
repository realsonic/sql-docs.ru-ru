---
description: CURSOR_STATUS (Transact-SQL)
title: CURSOR_STATUS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 853cae9252c43a176d208b7208f2ca595366a151
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093696"
---
# <a name="cursor_status-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Для данного параметра функция `CURSOR_STATUS` сообщает, вернуло ли объявление курсора курсор и результирующий набор.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
'local'  
Задает константу, показывающую, что источник курсора — это имя локального курсора.
  
'*cursor_name*'  
Имя курсора. Имя курсора должно соответствовать [требованиям, предъявляемым к идентификаторам базы данных](../../relational-databases/databases/database-identifiers.md).
  
'global'   
Задает константу, показывающую, что источник курсора — это имя глобального курсора.
  
'variable'   
Задает константу, показывающую, что источник курсора — это локальная переменная.
  
'*cursor_variable*'  
Имя переменной курсора. Переменная курсора должна быть определена с типом данных **cursor**.
  
## <a name="return-types"></a>Типы возвращаемых данных
**smallint**
  
|Возвращаемое значение|Имя курсора|Переменная курсора|  
|---|---|---|
|1|Результирующий набор курсора включает как минимум одну строку.<br /><br /> В случае статических и управляемых набором ключей курсоров результирующий набор включает как минимум одну строку.<br /><br /> В случае динамических курсоров результирующий набор может включать одну или более строк или не включать их.|Курсор, выделенный этой переменной, открыт.<br /><br /> В случае статических и управляемых набором ключей курсоров результирующий набор включает как минимум одну строку.<br /><br /> В случае динамических курсоров результирующий набор может включать одну или более строк или не включать их.|  
|0|Результирующий набор курсора пуст.*|Курсор, выделенный этой переменной, открыт, но результирующий набор пуст.*|  
|-1|Курсор закрыт.|Курсор, выделенный этой переменной, закрыт.|  
|-2|Неприменимо.|Возможен один из вариантов:<br /><br /> ранее вызванная процедура не назначила курсор этой переменной с аргументом OUTPUT;<br /><br /> ранее вызванная процедура назначила этой переменной с аргументом OUTPUT курсор, но при завершении процедуры он находился в закрытом состоянии. Таким образом, курсор освобождается и не возвращается вызвавшей процедуре.<br /><br /> Объявленной переменной курсора курсор не назначен.|  
|–3|Курсор с указанным именем не существует.|Переменная курсора с указанным именем не существует или существует, но ей еще не выделен курсор.|  
  
*Динамические курсоры никогда не возвращают этот результат.
  
## <a name="examples"></a>Примеры  
В приведенном ниже примере функция `CURSOR_STATUS` используется для отображения состояния курсора после его объявления, открытия и закрытия.
  
```sql
CREATE TABLE #TMP  
(  
   ii INT  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>См. также
[Функции работы с курсорами (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
  
  
