---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5345bbdc161730ba0d08c79e0d52b7e2ec901498
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097243"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает значение 1, если аргумент *expression* имеет допустимое значение типа **date**, **time** или **datetime**; в противном случае возвращает значение 0.  
  
 Функция ISDATE возвращает значение 0, если аргумент *expression* имеет значение типа **datetime2**.  
  
 Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md). Обратите внимание на то, что диапазоном для данных типа datetime являются значения от 1753-01-01 до 9999-12-31, в то время как диапазоном для данных даты являются значения от 0001-01-01 до 9999-12-31.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Символьная строка или [выражение](../../t-sql/language-elements/expressions-transact-sql.md), которое можно преобразовать в символьную строку. Выражение должно включать не более 4 000 символов. Типы данных даты и времени (за исключением типа datetime и smalldatetime), не разрешены к использованию в качестве аргумента функции ISDATE.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **int**  
  
## <a name="remarks"></a>Remarks  
 Функция ISDATE детерминирована, только если используется совместно с функцией [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) и если заданный параметр стиля CONVERT не равен 0, 100, 9 или 109.  
  
 Возвращаемое функцией ISDATE значение зависит от настроек, установленных инструкциями [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md), [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) и [параметром конфигурации сервера "язык по умолчанию"](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).  
  
## <a name="isdate-expression-formats"></a>Форматы выражения ISDATE  
 Примеры допустимых форматов, для которых функция ISDATE возвращает 1, см. в разделе "Поддерживаемые форматы строковых литералов для типа данных datetime" в статьях, посвященных типам данных [datetime](../../t-sql/data-types/datetime-transact-sql.md) и [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md). Дополнительные примеры см. также в столбце "Ввод/Вывод" раздела "Аргументы" функций [CAST и CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 В следующей таблице приводятся недопустимые форматы входных выражений, для которых возвращается 0 или ошибка.  
  
|ISDATE, выражение|Возвращенное значение ISDATE|  
|-----------------------|-------------------------|  
|NULL|0|  
|Значения типов данных, приведенные в списке [Типы данных](../../t-sql/data-types/data-types-transact-sql.md), в любой категории типов данных, кроме символьных строк, символьных строк в Юникоде и даты и времени.|0|  
|Значения типов данных **text**, **ntext** и **image**.|0|  
|Любое значение, имеющее более трех позиций долей секунды (от 0,0000 до 0,0000000...n) Функция ISDATE возвращает значение 0, если аргумент *expression* имеет значение типа **datetime2**, но возвращает значение 1, если *expression* имеет допустимое значение типа **datetime**.|0|  
|Любое значение, сочетающее допустимую дату с недопустимым значением, например 1995-10-1a.|0|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>A. Использование функции ISDATE для проверки допустимого выражения datetime  
 В приведенном ниже примере показано, как с помощью функции `ISDATE` проверить, является ли символьная строка допустимым значением **datetime**.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>Б. Влияние настроек SET DATEFORMAT и SET LANGUAGE на возвращаемые значения  
 Следующие инструкции возвращают значения, зависящие от настроек `SET DATEFORMAT` и `SET LANGUAGE`.  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>В. Использование функции ISDATE для проверки допустимого выражения datetime  
 В приведенном ниже примере показано, как с помощью функции `ISDATE` проверить, является ли символьная строка допустимым значением **datetime**.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
