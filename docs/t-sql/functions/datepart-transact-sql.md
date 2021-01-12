---
title: DATEPART (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции DATEPART. Эта функция возвращает целое число, соответствующее части указанной даты.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adae6be66b149f930bee51c636bf86d7614e3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093678"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Эта функция возвращает целое число, представляющее указанную часть *datepart* заданного типа *date*.
  
Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*datepart*  
Определенная часть аргумента *date*, для которого функция `DATEPART` вернет значение типа **integer**. В приведенной ниже таблице перечислены все допустимые аргументы *datepart*.

> [!NOTE]
> `DATEPART` не принимает эквивалентные переменные, определяемые пользователем, для аргументов *datepart*.
  
|*datepart*|Сокращения|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
Выражение, которое разрешается в один из следующих типов данных: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Для *date*`DATEPART` будет принимать столбец выражения, выражение, строковый литерал или определяемую пользователем переменную. Во избежание неоднозначности используйте четырехзначную запись года. Сведения о двузначном обозначении года см. в статье [Настройка параметра конфигурации сервера two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **int**  
  
## <a name="return-value"></a>Возвращаемое значение  
Каждое выражение *datepart* и его краткие формы возвращают одно и то же значение.
  
Возвращаемое значение зависит от языка среды, задаваемого инструкцией [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md), и от [параметра конфигурации сервера "язык по умолчанию"](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) для имени входа. Если значение *date* является строковым литералом некоторого формата, то возвращаемое значение зависит от функции [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md). Инструкция SET DATEFORMAT не изменяет возвращаемое значение, если дата представляется выражением столбца типа данных даты или времени.
  
Ниже представлена таблица соответствия аргументов функции *datepart* и значений, возвращенных выражением `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')`. Аргумент *date* имеет тип данных **datetimeoffset(7)** . Последние две позиции значения, возвращаемого функцией **nanosecond** *datepart*, всегда `00`. Масштаб этого значения составляет 9.

**.123456700**
  
|*datepart*|Возвращаемое значение|  
|---|---|
|**year, yyyy, yy**|2007 г.|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>Аргументы функции datepart, содержащие информацию о номере недели и дня
Для **week** (**wk**, **ww**) или **weekday** (**dw**) *datepart* возвращаемое значение `DATEPART` зависит от значения, заданного функцией [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
1 января любого года определяет начальное число для компонента даты **week** _datepart_. Пример:

DATEPART (**wk**, 'Jan 1, *xxx* x') = 1,

где *xxxx* — любой год.
  
В этой таблице представлены возвращаемые значения параметров **week** и **weekday** *datepart* для даты 21.04.2007 с каждым аргументом функции SET DATEFIRST. 1 января 2007 г. приходится на понедельник. 21 апреля 2007 г. приходится на субботу. Для региональных настроек "Английский (США)"

`SET DATEFIRST 7 -- ( Sunday )`

SET DATEFIRST 7 (воскресенье). После задания DATEFIRST используйте предлагаемую инструкцию SQL для табличных значений datepart:

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> — аргумент|week<br /><br /> возвращаемое|weekday<br /><br /> возвращаемое|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>Аргументы функции datepart, отображающие год, месяц и день даты  
Значения, возвращаемые в результате выполнения инструкций DATEPART (**year**, *date*), DATEPART (**month**, *date*) и DATEPART (**day**, *date*), совпадают с результатами выполнения функций [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) и [DAY](../../t-sql/functions/day-transact-sql.md), соответственно.
  
## <a name="iso_week-datepart"></a>iso_week datepart  
Стандарт ISO 8601 включает в себя систему отсчета дней и недель ISO. Каждая неделя приписывается тому году, в котором находится ее четверг. Например, первая неделя 2004 г. (2004W01) считается с понедельника 29 декабря 2003 г. по воскресенье 4 января 2004 г. Этот стиль нумерации обычно используется в странах и регионах Европы. В других странах он применяется редко.

Примечание. Наибольшее число недель в году может составлять 52 или 53.
  
Система отсчета недель в разных странах и регионах может не совпадать со стандартом ISO. В таблице ниже представлено шесть возможных вариантов.
  
|Первый день недели|Содержание первой недели года|Двойное присвоение недель|Применяется в:|  
|---|---|---|---|
|Воскресенье|1 января,<br /><br /> Первая суббота,<br /><br /> 1–7 дней года|Да|США|  
|Понедельник|1 января,<br /><br /> Первое воскресенье,<br /><br /> 1–7 дней года|Да|Большинство стран Европы, а также Великобритания|  
|Понедельник|4 января,<br /><br /> Первый четверг<br /><br /> 4–7 дней года|нет|ISO 8601, Норвегия и Швеция|  
|Понедельник|7 января,<br /><br /> Первый понедельник<br /><br /> 7 дней года|нет||  
|Среда|1 января,<br /><br /> Первый вторник,<br /><br /> 1–7 дней года|Да||  
|Суббота|1 января,<br /><br /> Первая пятница,<br /><br /> 1–7 дней года|Да||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` возвращает значение **tzoffset** (**tz**) как количество минут (со знаком). В результате выполнения этой инструкции возвращается смещение часового пояса 310 минут:
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
Функция `DATEPART` отображает значение tzoffset описанным ниже образом.
- Для datetimeoffset и datetime2 значение tzoffset возвращает временное смещение в минутах, причем для datetime2 смещение всегда равно 0 минут.
- Для типов данных, которые могут быть неявно преобразованы в **datetimeoffset** или **datetime2**, функция `DATEPART` возвращает временное смещение в минутах. Для остальных типов даты и времени этого не происходит.
- Для параметров любых других типов возвращается ошибка.
  
  
## <a name="smalldatetime-date-argument"></a>Аргумент даты типа smalldatetime  
Для значения *date* типа [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) функция `DATEPART` возвращает количество секунд 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Возвращается значение по умолчанию для аргумента функции datepart, который отличен от даты  
Если тип данных аргумента *date* не содержит указанной части *datepart*, функция `DATEPART` вернет значение по умолчанию для этой части *datepart*, только если для *date* указан литерал.
  
Например, значение "год-месяц-день" по умолчанию для любого типа данных **date** равно 1900-01-01. Приведенная ниже инструкция содержит аргументы компонентов даты для *datepart*, аргумент времени для *date* и возвращает `1900, 1, 1, 1, 2`.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Если аргумент *date* указан как переменная или столбец таблицы и тип данных этой переменной или столбца не содержит указанной части *datepart*, функция `DATEPART` возвращает ошибку 9810. В этом примере переменная *\@t* имеет тип данных **time**. Этот пример завершается ошибкой, потому что год даты не является допустимым для типа данных **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Доли секунды
Эти инструкции демонстрируют, что функция `DATEPART` возвращает доли секунды:
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Remarks  
Функция `DATEPART` может использоваться в предложениях выбора из списка, WHERE, HAVING, GROUP BY и ORDER BY.
  
В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] функция DATEPART неявно приводит строковые литералы к типу **datetime2**. Это означает, что DATENAME не поддерживает формат ГЧМ (год, число, месяц) при передаче даты в виде строки. Для использования формата ГЧМ (год, число, месяц) необходимо явно привести строку к типу **datetime** или **smalldatetime**.
  
## <a name="examples"></a>Примеры  
В этом примере возвращается базовый год. Его значение используется при расчетах, связанных с датами. В примере дата указана как число. Обратите внимание на то, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] интерпретирует 0 как 1 января 1900 г.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
В приведенном ниже примере возвращается часть даты `12/20/1974`, представляющая день.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
В приведенном ниже примере возвращается часть даты `12/20/1974`, представляющая год.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
