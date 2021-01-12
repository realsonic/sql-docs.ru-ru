---
description: '&#x40;&#x40;CURSOR_ROWS (Transact-SQL)'
title: '@@CURSOR_ROWS (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CURSOR_ROWS'
- '@@CURSOR_ROWS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CURSOR_ROWS function'
- cursors [SQL Server], last-opened
- last-opened cursor
- asynchronous cursors [SQL Server]
ms.assetid: 31bd7a97-7f28-42a8-ba24-24d16d22973d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7df1f0df488313e5d13afe829ec25710477ffbcf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097206"
---
# <a name="x40x40cursor_rows-transact-sql"></a>&#x40;&#x40;CURSOR_ROWS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает число выбранных строк, имеющихся в последнем открытом курсоре в данном соединении. Для повышения производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может выполнять заполнение большого набора ключей и статических курсоров асинхронно. Функцию `@@CURSOR_ROWS` можно вызвать для определения того, получено ли количество строк, определенных для курсора, во время вызова @@CURSOR_ROWS.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
@@CURSOR_ROWS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
**integer**
  
## <a name="return-value"></a>Возвращаемое значение  
  
|Возвращаемое значение|Описание|  
|---|---|
|-*m*|Курсор заполняется асинхронно. Возвращаемое значение (–*m*) является текущим числом строк в наборе ключей.|  
|-1|Курсор является динамическим. Так как динамический курсор отражает все изменения, количество строк для курсора постоянно изменяется. Курсор не обязательно извлекает все определенные строки.|  
|0|Ни один курсор не был открыт, не было строк для последнего открытого курсора или последний открытый курсор закрыт или освобожден.|  
|*n*|Курсор полностью заполнен. Возвращенное значение (*n*) является общим количеством строк в курсоре.|  
  
## <a name="remarks"></a>Комментарии  
`@@CURSOR_ROWS` возвращает отрицательное число, если последний курсор был открыт асинхронно. Управляемые набором ключей или статические курсоры открываются асинхронно, если значение параметра cursor threshold процедуры sp_configure больше 0, а количество строк в результирующем наборе курсора больше, чем пороговое значение курсора.
  
## <a name="examples"></a>Примеры  
В приведенном ниже примере объявляется курсор и выполняется инструкция `SELECT` для вывода на экран значения `@@CURSOR_ROWS`. Параметр имеет значение `0` перед открытием курсора, а затем принимает значение `-1`, которое указывает на то, что набор ключей курсора заполняется асинхронно.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT @@CURSOR_ROWS;  
DECLARE Name_Cursor CURSOR FOR  
SELECT LastName ,@@CURSOR_ROWS FROM Person.Person;  
OPEN Name_Cursor;  
FETCH NEXT FROM Name_Cursor;  
SELECT @@CURSOR_ROWS;  
CLOSE Name_Cursor;  
DEALLOCATE Name_Cursor;  
GO             
```  
  
Ниже приведены результирующие наборы.
  
```
-----------
0  
```

```
LastName
---------------
Sanchez
```

```
-----------
-1
```  
  
## <a name="see-also"></a>См. также
[Функции работы с курсорами (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)  
[OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)
  
  
