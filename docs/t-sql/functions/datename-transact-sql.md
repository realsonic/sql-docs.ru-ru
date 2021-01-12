---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 021c8e18b0767fcd844a92f598b4fbcb434740a4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097287"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает строку символов, представляющую указанную часть *datepart* заданного типа *date*.

Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*datepart*  
Определенная часть аргумента *date*, которую вернет функция `DATENAME`. В приведенной ниже таблице перечислены все допустимые аргументы *datepart*.

> [!NOTE]
> `DATENAME` не принимает эквивалентные переменные, определяемые пользователем, для аргументов *datepart*.
  
|*datepart*|Сокращения|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

Выражение, которое может быть разрешено в один из следующих типов данных: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Для *date*`DATENAME` будет принимать столбец выражения, выражение, строковый литерал или определяемую пользователем переменную. Во избежание неоднозначности используйте четырехзначную запись года. Сведения о двузначном обозначении года см. в статье [Настройка параметра конфигурации сервера two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Тип возвращаемых данных  
**nvarchar**
  
## <a name="return-value"></a>Возвращаемое значение  
  
-   Каждое выражение *datepart* и его краткие формы возвращают одно и то же значение.  
  
Возвращаемое значение зависит от языка среды, задаваемого инструкцией [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md), и от [параметра конфигурации сервера "язык по умолчанию"](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) для имени входа. Если значение *date* является строковым литералом некоторого формата, то возвращаемое значение зависит от функции [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md). Инструкция SET DATEFORMAT не изменяет возвращаемое значение, если дата представляется выражением столбца типа данных даты или времени.
  
Если параметр *date* имеет аргумент типа **date**, то возвращаемое значение зависит от настроек, заданных с помощью функции [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
## <a name="tzoffset-datepart-argument"></a>Аргумент TZoffset функции datepart  
Если в качестве аргумента *datepart* выступает переменная **TZoffset** (**tz**), а аргумент *date* не содержит смещения часового пояса, функция `DATEADD` возвращает значение 0.
  
## <a name="smalldatetime-date-argument"></a>Аргумент даты типа smalldatetime  
Если аргумент *date* имеет тип [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), функция `DATENAME` возвращает значение секунд 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Возвращается значение по умолчанию для аргумента функции datepart, который отличен от даты  
Если тип данных аргумента *date* не содержит указанной части *datepart*, функция `DATENAME` вернет значение по умолчанию для этой части *datepart*, только если аргумент *date* содержит литерал.
  
Например, значение "год-месяц-день" по умолчанию для любого типа данных **date** равно 1900-01-01. Приведенная ниже инструкция содержит аргументы компонентов даты для *datepart*, аргумент времени для *date*, а функция `DATENAME` возвращает `1900, January, 1, 1, Monday`.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Если аргумент *date* указан как переменная или столбец таблицы и тип данных этой переменной или столбца не содержит указанной части *datepart*, функция `DATENAME` возвращает ошибку 9810. В этом примере переменная *\@t* имеет тип данных **time**. Этот пример завершается ошибкой, потому что год даты не является допустимым для типа данных **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Remarks  

Используйте `DATENAME` в следующих предложениях.

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] функция DATENAME неявно приводит строковые литералы к типу **datetime2**. Иными словами, `DATENAME` не поддерживает формат ГЧМ (год, число, месяц) при передаче даты в виде строки. Для использования формата ГЧМ (год, число, месяц) необходимо явно привести строку к типу **datetime** или **smalldatetime**.
  
## <a name="examples"></a>Примеры  
В приведенном ниже примере возвращаются компоненты указанной даты. Подставьте значение *datepart* из таблицы для аргумента `datepart` в инструкции SELECT:
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Возвращаемое значение|  
|---|---|
|**year, yyyy, yy**|2007 г.|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Октябрь|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Вторник|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В приведенном ниже примере возвращаются компоненты указанной даты. Подставьте значение *datepart* из таблицы для аргумента `datepart` в инструкции SELECT:
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Возвращаемое значение|  
|---|---|
|**year, yyyy, yy**|2007 г.|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Октябрь|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Вторник|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

