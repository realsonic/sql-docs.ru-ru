---
title: Функции и типы данных даты и времени
description: Ссылки на статьи о типах данных и функциях даты и времени.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server]
- date and time [SQL Server], all data types and functions
- date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 83e378a2-6e89-4c80-bc4f-644958d9e0a9
author: cawrites
ms.author: chadam
monikerRange: = azure-sqldw-latest||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 461b4db55574fcad2fb5ff7250ce88f508999fa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102625"
---
# <a name="date-and-time-data-types-and-functions-transact-sql"></a>Типы данных и функции даты и времени (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

В разделах этой статьи представлен обзор всех типов данных и функций даты и времени [!INCLUDE[tsql](../../includes/tsql-md.md)].

- [Типы данных даты и времени](#DateandTimeDataTypes)
- [Функции даты и времени](#DateandTimeFunctions)
  - [Функции, возвращающие значения системной даты и времени](#GetSystemDateandTimeValues)
  - [Функции, возвращающие компоненты даты и времени](#GetDateandTimeParts)
  - [Функции, возвращающие значения даты и времени из их компонентов](#fromParts)
  - [Функции, возвращающие значения разности даты и времени](#GetDateandTimeDifference)
  - [Функции, изменяющие значения даты и времени](#ModifyDateandTimeValues)
  - [Функции, устанавливающие или возвращающие функции формата сеанса](#SetorGetSessionFormatFunctions)
  - [Функции, проверяющие значения даты и времени](#ValidateDateandTimeValues)
- [Дата и время — см. также](#DateandTimeRelatedTopics)

## <a name="date-and-time-data-types"></a><a name="DateandTimeDataTypes"></a> Типы данных даты и времени

Типы данных даты и времени [!INCLUDE[tsql](../../includes/tsql-md.md)] перечислены в следующей таблице:

|Тип данных|Формат|Диапазон|Точность|Объем памяти (в байтах)|Определяемая пользователем точность в долях секунды|Смещение часового пояса|
|---|---|---|---|---|---|---|
|[time](../../t-sql/data-types/time-transact-sql.md)|чч:мм:сс[.ннннннн]|От 00:00:00.0000000 до 23:59:59.9999999|100 наносекунд|от 3 до 5|Да|нет|
|[date](../../t-sql/data-types/date-transact-sql.md)|ГГГГ-ММ-ДД|От 0001-01-01 до 31.12.99|1 день|3|нет|нет|
|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|ГГГГ-ММ-ДД чч:мм:сс|От 01.01.1900 до 06.06.2079|1 минута|4|нет|нет|
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|ГГГГ-ММ-ДД чч:мм:сс[.ннн]|От 01.01.1753 до 31.12.9999|0,00333 секунды|8|нет|нет|
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн]|От 0001-01-01 00:00:00.0000000 до 9999-12-31 23:59:59.9999999|100 наносекунд|От 6 до 8|Да|нет|
|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн] [+&#124;-]чч:мм|От 0001-01-01 00:00:00.0000000 до 9999-12-31 23:59:59.9999999 (время в формате UTC)|100 наносекунд|От 8 до 10|Да|Да|

> [!NOTE]
> Тип данных [!INCLUDE[tsql](../../includes/tsql-md.md)] [rowversion](../../t-sql/data-types/rowversion-transact-sql.md) не относится к типам данных даты и времени. Тип данных **timestamp** является устаревшим синонимом **rowversion**.

## <a name="date-and-time-functions"></a><a name="DateandTimeFunctions"></a> Функции даты и времени

В следующих таблицах приводятся функции даты и времени [!INCLUDE[tsql](../../includes/tsql-md.md)]. Дополнительные сведения о детерминизме см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

### <a name="function-that-return-system-date-and-time-values"></a><a name="GetSystemDateandTimeValues"></a> Функции, возвращающие значения системной даты и времени

[!INCLUDE[tsql](../../includes/tsql-md.md)] наследует все значения системной даты и времени от операционной системы компьютера, на котором работает экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

#### <a name="higher-precision-system-date-and-time-functions"></a>Высокоточные функции системной даты и времени

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] получает значения даты и времени с помощью функции GetSystemTimeAsFileTime() Windows API. Точность зависит от физического оборудования и версии Windows, в которой запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Точность возвращаемых значений этого API-интерфейса задана равной 100 нс. Точность может быть определена с помощью метода GetSystemTimeAdjustment() API-интерфейса Windows.

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[SYSDATETIME](../../t-sql/functions/sysdatetime-transact-sql.md)|SYSDATETIME ()|Возвращает значение типа **datetime2(7)** , которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возвращаемое значение не содержит смещение часового пояса.|**datetime2(7)**|Недетерминированная|
|[SYSDATETIMEOFFSET](../../t-sql/functions/sysdatetimeoffset-transact-sql.md)|SYSDATETIMEOFFSET ( )|Возвращает значение типа **datetimeoffset(7)** , которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возвращаемое значение содержит смещение часового пояса.|**datetimeoffset(7)**|Недетерминированная|
|[SYSUTCDATETIME](../../t-sql/functions/sysutcdatetime-transact-sql.md)|SYSUTCDATETIME ( )|Возвращает значение типа **datetime2(7)** , которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Функция возвращает значения даты и времени в формате UTC.|**datetime2(7)**|Недетерминированная|

#### <a name="lower-precision-system-date-and-time-functions"></a>Функции системной даты и времени меньшей точности

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[CURRENT_TIMESTAMP](../../t-sql/functions/current-timestamp-transact-sql.md)|CURRENT_TIMESTAMP|Возвращает значение типа **datetime**, которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возвращаемое значение не содержит смещение часового пояса.|**datetime**|Недетерминированная|
|[GETDATE](../../t-sql/functions/getdate-transact-sql.md)|GETDATE ( )|Возвращает значение типа **datetime**, которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возвращаемое значение не содержит смещение часового пояса.|**datetime**|Недетерминированная|
|[GETUTCDATE](../../t-sql/functions/getutcdate-transact-sql.md)|GETUTCDATE ( )|Возвращает значение типа **datetime**, которое содержит дату и время компьютера, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Функция возвращает значения даты и времени в формате UTC.|**datetime**|Недетерминированная|

### <a name="functions-that-return-date-and-time-parts"></a><a name="GetDateandTimeParts"></a> Функции, возвращающие компоненты даты и времени

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|--------------|------------|------------------|----------------------|-----------------|
|[DATENAME](../../t-sql/functions/datename-transact-sql.md)|DATENAME ( *datepart* , *date* )|Возвращает строку символов, представляющую указанную часть *datepart* заданного типа date.|**nvarchar**|Недетерминированная|
|[DATEPART](../../t-sql/functions/datepart-transact-sql.md)|DATEPART ( *datepart* , *date* )|Возвращает целое число, представляющее указанную часть *datepart* заданного типа *date*.|**int**|Недетерминированная|
|[DAY](../../t-sql/functions/day-transact-sql.md)|DAY ( *date* )|Возвращает целое число, представляющее часть дня указанного типа *date*.|**int**|Детерминированное|
|[MONTH](../../t-sql/functions/month-transact-sql.md)|MONTH ( *date* )|Возвращает целое число, представляющее часть месяца указанного типа *date*.|**int**|Детерминированное|
|[YEAR](../../t-sql/functions/year-transact-sql.md)|YEAR ( *date* )|Возвращает целое число, представляющее часть года указанного типа *date*.|**int**|Детерминированное|

### <a name="functions-that-return-date-and-time-values-from-their-parts"></a><a name="fromParts"></a> Функции, возвращающие значения даты и времени из их компонентов

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[DATEFROMPARTS](../../t-sql/functions/datefromparts-transact-sql.md)|DATEFROMPARTS ( *year*, *month*, *day* )|Возвращает значение **date**, соответствующее указанному числу, месяцу и году.|**date**|Детерминированное|
|[DATETIME2FROMPARTS](../../t-sql/functions/datetime2fromparts-transact-sql.md)|DATETIME2FROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *precision*)|Возвращает значение **datetime2**, соответствующее указанной дате и времени с заданной точностью.|**datetime2(** _precision_ **)**|Детерминированное|
|[DATETIMEFROMPARTS](../../t-sql/functions/datetimefromparts-transact-sql.md)|DATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *milliseconds*)|Возвращает значение **datetime**, соответствующее указанной дате и времени.|**datetime**|Детерминированное|
|[DATETIMEOFFSETFROMPARTS](../../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)|DATETIMEOFFSETFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *hour_offset*, *minute_offset*, *precision*)|Возвращает значение **datetimeoffset** для указанных даты и времени с указанными смещением и точностью.|**datetimeoffset(** _precision_ **)**|Детерминированное|
|[SMALLDATETIMEFROMPARTS](../../t-sql/functions/smalldatetimefromparts-transact-sql.md)|SMALLDATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute* )|Возвращает значение **smalldatetime**, соответствующее указанной дате и времени.|**smalldatetime**|Детерминированное|
|[TIMEFROMPARTS](../../t-sql/functions/timefromparts-transact-sql.md)|TIMEFROMPARTS ( *hour*, *minute*, *seconds*, *fractions*, *precision* )|Возвращает значение **time**, соответствующее указанному времени с заданной точностью.|**time(** _precision_ **)**|Детерминированное|

### <a name="functions-that-return-date-and-time-difference-values"></a><a name="GetDateandTimeDifference"></a> Функции, возвращающие значения разности даты и времени

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[DATEDIFF](../../t-sql/functions/datediff-transact-sql.md)|DATEDIFF ( *datepart* , *startdate* , *enddate* )|Возвращает количество границ даты или времени *datepart*, пересекающихся между двумя указанными датами.|**int**|Детерминированное|
|[DATEDIFF_BIG](../../t-sql/functions/datediff-big-transact-sql.md)|DATEDIFF_BIG ( *datepart* , *startdate* , *enddate* )|Возвращает количество границ даты или времени *datepart*, пересекающихся между двумя указанными датами.|**bigint**|Детерминированное|

### <a name="functions-that-modify-date-and-time-values"></a><a name="ModifyDateandTimeValues"></a> Функции, изменяющие значения даты и времени

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[DATEADD](../../t-sql/functions/dateadd-transact-sql.md)|DATEADD (*datepart* , *number* , *date* )|Возвращает новое значение **datetime**, добавляя интервал к указанной части *datepart* заданной даты *date*.|Тип данных аргумента *date*|Детерминированное|
|[EOMONTH](../../t-sql/functions/eomonth-transact-sql.md)|EOMONTH ( *start_date* [, *month_to_add* ] )|Возвращает последний день месяца, содержащего указанную дату, с необязательным смещением.|Тип возвращаемого значения — это тип аргумента *start_date* или тип данных **date**.|Детерминированное|
|[SWITCHOFFSET](../../t-sql/functions/switchoffset-transact-sql.md)|SWITCHOFFSET (*DATETIMEOFFSET*, *time_zone*)|Функция SWITCHOFFSET изменяет смещение часового пояса для значения DATETIMEOFFSET и сохраняет значение UTC.|Значение **datetimeoffset** с точностью в долях секунд, заданной в аргументе *DATETIMEOFFSET*|Детерминированное|
|[TODATETIMEOFFSET](../../t-sql/functions/todatetimeoffset-transact-sql.md)|TODATETIMEOFFSET (*expression* , *time_zone*)|TODATETIMEOFFSET преобразует значение типа datetime2 в значение типа datetimeoffset. Функция *TODATETIMEOFFSET* преобразует значение datetime2 в местное время для указанного time_zone.|Значение **datetimeoffset** с точностью в долях секунд, заданной в аргументе *datetime*|Детерминированное|

### <a name="functions-that-set-or-return-session-format-functions"></a><a name="SetorGetSessionFormatFunctions"></a> Функции, устанавливающие или возвращающие функции формата сеанса

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)|@@DATEFIRST|Возвращает текущее значение параметра SET DATEFIRST для сеанса.|**tinyint**|Недетерминированная|
|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)|SET DATEFIRST { *number* &#124; **\@** _number_var_ }|Устанавливает первый день недели в виде числа от 1 до 7.|Неприменимо|Неприменимо|
|[SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|SET DATEFORMAT { *format* &#124; **@** _format_var_ }|Задает порядок составляющих даты (месяц/день/год) для ввода данных типа **datetime** или **smalldatetime**.|Неприменимо|Неприменимо|
|[@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)|@@LANGUAGE|Возвращает название использующегося в настоящий момент языка. Функция @@LANGUAGE не является функцией даты или времени. Однако на данные, выводимые функциями даты, могут повлиять настройки языка.|Неприменимо|Неприменимо|
|[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)|SET LANGUAGE { [ N ] **'** _language_ **'** &#124; **\@** _language_var_ }|Устанавливает языковую среду сеанса и системных сообщений. SET LANGUAGE не является функцией даты или времени. Однако на данные, выводимые функциями даты, влияет параметр языка.|Неприменимо|Неприменимо|
|[sp_helplanguage](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)|**sp_helplanguage** [ [ **@language =** ] **'** _language_ **'** ]|Возвращает сведения о формате даты всех поддерживаемых языков. **sp_helplanguage** не является хранимой процедурой даты или времени. Однако на данные, выводимые функциями даты, влияет параметр языка.|Неприменимо|Неприменимо|

### <a name="functions-that-validate-date-and-time-values"></a><a name="ValidateDateandTimeValues"></a> Функции, проверяющие значения даты и времени

|Компонент|Синтаксис|Возвращаемое значение|Тип возвращаемых данных|Детерминизм|
|---|---|---|---|---|
|[ISDATE](../../t-sql/functions/isdate-transact-sql.md)|ISDATE ( *expression* )|Определяет, является ли входное выражение типа **datetime** или **smalldatetime** допустимым значением даты или времени.|**int**|Функция ISDATE детерминирована, только если используется совместно с функцией CONVERT и если заданный параметр стиля CONVERT не равен 0, 100, 9 или 109.|

## <a name="date-and-time-related-topics"></a><a name="DateandTimeRelatedTopics"></a> Дата и время — см. также

|Раздел|Описание|
|-----------|-----------------|
|[FORMAT](../../t-sql/functions/format-transact-sql.md)|Возвращает значение в указанных формате и культуре (не обязательно). Для выполнения форматирования значения даты, времени и чисел с учетом локали в виде строк используется функция FORMAT.|
|[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)|Предоставляет сведения о преобразовании значений даты и времени в строковые литералы и обратно, а также в другие форматы даты и времени.|
|[Написание инструкций Transact-SQL, адаптированных к международному использованию](../../relational-databases/collations/write-international-transact-sql-statements.md)|Предоставляет рекомендации относительно переносимости баз данных и приложений баз данных, использующих инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], с одного языка на другой или в многоязычную среду.|
|[Скалярные функции ODBC (Transact-SQL)](../../t-sql/functions/odbc-scalar-functions-transact-sql.md)|Предоставляет сведения о скалярных функциях ODBC, которые могут использоваться в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)]. К ним относятся функции даты и времени ODBC.|
|[AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)|Обеспечивает преобразование часовых поясов.|

## <a name="see-also"></a>См. также раздел

- [Функции](../../t-sql/functions/functions.md)
- [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
