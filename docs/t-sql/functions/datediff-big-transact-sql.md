---
description: DATEDIFF_BIG (Transact-SQL)
title: DATEDIFF_BIG (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea087da6532c43493fd10f647788297d98f35f72
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093670"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Эта функция возвращает количество пересеченных границ (целое число со знаком), указанных в аргументе *datepart*, за период времени, указанный в аргументах *startdate* и *enddate*.
  
Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>Аргументы
*datepart*  
Часть аргументов *startdate* и *enddate*, которая задает тип пересекаемых границ.

> [!NOTE]
> `DATEDIFF_BIG` не принимает значения *datepart* из переменных, определяемых пользователем, или как строки в кавычках.

В приведенной ниже таблице перечислены все допустимые имена аргументов *datepart* и их сокращения.
  
|Имя *datepart*| Сокращение *datepart*|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  

> [!NOTE]
> Каждое конкретное имя аргумента *datepart* и сокращение этого имени *datepart* будут возвращать одно и то же значение.

*startdate*  
Выражение, которое может быть разрешено в одно из следующих значений.

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Для *date*`DATEDIFF_BIG` будет принимать столбец выражения, выражение, строковый литерал или определяемую пользователем переменную. Значение строкового литерала должно разрешаться в **datetime**. Во избежание неоднозначности используйте четырехзначную запись года. `DATEDIFF_BIG` вычитает *startdate* из *enddate*. Во избежание неоднозначности используйте четырехзначную запись года. Сведения о двузначном обозначении года см. в статье [Настройка параметра конфигурации сервера two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
См. описание аргумента *startdate*.
  
## <a name="return-type"></a>Тип возвращаемых данных  
**bigint** со знаком  
  
## <a name="return-value"></a>Возвращаемое значение  
Значение типа **bigint**, представляющее разницу между аргументами *startdate* и *enddate* в границах, определяемых аргументом *datepart*.
  
В качестве возвращаемого значения вне диапазона для **bigint** (от –9 223 372 036 854 775 808 до 9 223 372 036 854 775 807) `DATEDIFF_BIG` возвращает сообщение об ошибке. В отличие от функции `DATEDIFF`, которая возвращает значение типа **int** и поэтому может переполняться с точностью до **минуты** или более высокой точностью, `DATEDIFF_BIG` может переполняться только при использовании точности до **наносекунды**, если разница между *enddate* и *startdate* больше 292 лет, 3 месяцев, 10 дней, 23 часов, 47 минут и 16,8547758 секунд.
  
Если обоим аргументам, *startdate* и *enddate*, присвоено только значение времени, а аргумент *datepart* не содержит значения времени *datepart*, то `DATEDIFF_BIG` возвращает значение 0.
  
При вычислении возвращаемого значения `DATEDIFF_BIG` не учитывает компонент смещения часовых поясов для аргументов *startdate* или *enddate*.
  
Так как значение типа **smalldatetime** имеет точность до минуты, то при использовании в аргументах *startdate* или *enddate*`DATEDIFF_BIG` всегда задает 0 в качестве возвращаемого значения [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) секунд и миллисекунд.
  
Если переменной типа данных date присвоено только значение времени, в качестве недостающей части даты `DATEDIFF_BIG` задает значение по умолчанию: `1900-01-01`. Если переменной типа данных time или date присвоено только значение даты, в качестве недостающей части времени `DATEDIFF_BIG` задает значение по умолчанию: `00:00:00`. Если в одном из аргументов *startdate* или *enddate* указано только время, а в другом только дата, в качестве недостающей информации `DATEDIFF_BIG` задает значения по умолчанию.
  
Если аргументы *startdate* и *enddate* имеют разные типы данных даты, но при этом один из них имеет больше частей времени или обладает более высокой точностью, `DATEDIFF_BIG` присваивает значения 0 недостающим частям другого аргумента.
  
## <a name="datepart-boundaries"></a>Границы, задаваемые аргументом datepart
Приведенные ниже инструкции имеют одинаковые значения аргументов *startdate* и *enddate*. Указанные даты являются соседними, а временная разница между ними составляет одну микросекунду (0,0000001 секунды). Разница между аргументами *startdate* и *enddate* в каждой инструкции пересекает одну календарную или временную границу аргумента *datepart*. Каждое выражение возвращает значение 1. Если аргументы *startdate* и *enddate* имеют разные значения года, но одинаковые значения календарной недели, `DATEDIFF_BIG` вернет 0 для части **week** аргумента *datepart*.

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Remarks  
Используйте `DATEDIFF_BIG` в предложениях `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` и `ORDER BY`.
  
Функция `DATEDIFF_BIG` неявно приводит строковые литералы к типу **datetime2**. Это означает, что `DATEDIFF_BIG` не поддерживает формат ГЧМ (год, число, месяц) при передаче даты в виде строки. Для использования формата ГЧМ (год, число, месяц) необходимо явно привести строку к типу **datetime** или **smalldatetime**.
  
Указание `SET DATEFIRST` не влияет на `DATEDIFF_BIG`. `DATEDIFF_BIG` всегда считает воскресенье первым днем недели, чтобы обеспечить детерминизм работы функции.

`DATEDIFF_BIG` может переполняться с точностью до **наносекунды**, если разница между *enddate* и *startdate* представляет собой значение, выходящее за пределы диапазона, допустимого для **bigint**.
  
## <a name="examples"></a>Примеры 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Указание столбцов в качестве начальной и конечной даты  
В этом примере выражения различного типа используются в качестве аргументов для параметров *startdate* и *enddate*. В нем подсчитывается количество границ дней, пересекаемых между датами, указанными в двух столбцах таблицы.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>Определение разницы между startdate и enddate в виде строковых компонентов даты

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

Тесно связанные примеры см. в статье [DATEDIFF (Transact-SQL)](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF (Transact-SQL)](../../t-sql/functions/datediff-transact-sql.md)
  
  
