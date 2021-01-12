---
title: GETDATE (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции GETDATE, которая возвращает текущее системное время базы данных в виде значения datetime.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GETDATE_TSQL
- GETDATE
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- GETDATE function [SQL Server]
- current date and time [SQL Server]
- time [SQL Server], current
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- date and time [SQL Server], GETDATE
- dates [SQL Server], system date and time
- time [SQL Server], system
ms.assetid: bebe3b65-2b3e-4c73-bf80-ff1132c680a7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd2f4a5c889679da88cb1232a2833f1b6497bb58
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102606"
---
# <a name="getdate-transact-sql"></a>GETDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает текущую системную метку времени базы данных в виде значения **datetime** без смещения часового пояса базы данных. Это значение наследуется от операционной системы компьютера, на котором работает экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
>  SYSDATETIME и SYSUTCDATETIME имеют большую точность в долях секунды, чем GETDATE и GETUTCDATE. SYSDATETIMEOFFSET включает смещение часового пояса, заданное в системе. SYSDATETIME, SYSUTCDATETIME и SYSDATETIMEOFFSET можно присваивать переменным любого типа даты и времени.

База данных SQL Azure (за исключением Управляемого экземпляра SQL Azure) и Azure Synapse Analytics используют время в формате UTC. Если необходимо интерпретировать сведения о дате и времени в часовом поясе, отличном от UTC, используйте [AT TIME ZONE](../../t-sql/queries/at-time-zone-transact-sql.md) в Базе данных SQL Azure или Azure Synapse Analytics.

 Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
GETDATE()
```

## <a name="return-type"></a>Тип возвращаемых данных
 **datetime**

## <a name="remarks"></a>Remarks
 Функция GETDATE может использоваться в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] везде, где допустимо использование выражения **datetime**.

 GETDATE является недетерминированной функцией. Невозможно проиндексировать представления и выражения, ссылающиеся на эту функцию в столбце.

 Использование SWITCHOFFSET с функцией GETDATE() может вызвать замедление выполнения запроса, поскольку оптимизатор запросов не может получить точные оценки количества элементов для значения GETDATE. Рекомендуется заранее вычислить значение GETDATE, а затем указать это значение в запросе, как показано в следующем примере. Кроме того, с помощью указания запроса OPTION (RECOMPILE) можно вынудить оптимизатор запросов повторно компилировать план запроса при каждом выполнении одного и того же запроса. В этом случае оптимизатор будет иметь точные оценки количества элементов для GETDATE(), что позволит ему сформировать более эффективный план запроса.

```sql
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');
SELECT * FROM t
WHERE c1 > @dt OPTION (RECOMPILE);
```

## <a name="examples"></a>Примеры
 В следующих примерах с помощью шести системных функций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые возвращают текущую дату и время, происходит возврат даты, времени или и того и другого. Значения возвращаются последовательно и поэтому могут различаться на доли секунды.

### <a name="a-getting-the-current-system-date-and-time"></a>A. Получение текущей системной даты и времени

```sql
SELECT SYSDATETIME()
    ,SYSDATETIMEOFFSET()
    ,SYSUTCDATETIME()
    ,CURRENT_TIMESTAMP
    ,GETDATE()
    ,GETUTCDATE();
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

 ```
SYSDATETIME()      2007-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
GETDATE()          2007-04-30 13:10:02.047
GETUTCDATE()       2007-04-30 20:10:02.047
```

### <a name="b-getting-the-current-system-date"></a>Б. Получение текущей системной даты

```sql
SELECT CONVERT (date, SYSDATETIME())
    ,CONVERT (date, SYSDATETIMEOFFSET())
    ,CONVERT (date, SYSUTCDATETIME())
    ,CONVERT (date, CURRENT_TIMESTAMP)
    ,CONVERT (date, GETDATE())
    ,CONVERT (date, GETUTCDATE());
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
SYSDATETIME()          2007-05-03
SYSDATETIMEOFFSET()    2007-05-03
SYSUTCDATETIME()       2007-05-04
CURRENT_TIMESTAMP      2007-05-03
GETDATE()              2007-05-03
GETUTCDATE()           2007-05-04
```

### <a name="c-getting-the-current-system-time"></a>В. Получение текущего системного времени

```sql
SELECT CONVERT (time, SYSDATETIME())
    ,CONVERT (time, SYSDATETIMEOFFSET())
    ,CONVERT (time, SYSUTCDATETIME())
    ,CONVERT (time, CURRENT_TIMESTAMP)
    ,CONVERT (time, GETDATE())
    ,CONVERT (time, GETUTCDATE());
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
SYSDATETIME()       13:18:45.3490361
SYSDATETIMEOFFSET() 13:18:45.3490361
SYSUTCDATETIME()    20:18:45.3490361
CURRENT_TIMESTAMP   13:18:45.3470000
GETDATE()           13:18:45.3470000
GETUTCDATE()        20:18:45.3470000
```

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 В приведенных ниже примерах с помощью трех системных функций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые возвращают текущую дату и время, происходит получение даты, времени или и того и другого. Значения возвращаются последовательно и поэтому могут различаться на доли секунды.

### <a name="d-getting-the-current-system-date-and-time"></a>Г. Получение текущей системной даты и времени

```sql
SELECT SYSDATETIME()
    ,CURRENT_TIMESTAMP
    ,GETDATE();
```

### <a name="e-getting-the-current-system-date"></a>Д. Получение текущей системной даты

```sql
SELECT CONVERT (date, SYSDATETIME())
    ,CONVERT (date, CURRENT_TIMESTAMP)
    ,CONVERT (date, GETDATE());
```

### <a name="f-getting-the-current-system-time"></a>Е. Получение текущего системного времени

```sql
SELECT CONVERT (time, SYSDATETIME())
    ,CONVERT (time, CURRENT_TIMESTAMP)
    ,CONVERT (time, GETDATE());
```

## <a name="see-also"></a>См. также:
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
