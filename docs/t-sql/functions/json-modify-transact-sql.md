---
description: JSON_MODIFY (Transact-SQL)
title: JSON_MODIFY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.assetid: 96bc8255-a037-4907-aec4-1a9c30814651
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 547a304b4f5a9b6069d023a751f73d99e8456bbc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092249"
---
# <a name="json_modify-transact-sql"></a>JSON_MODIFY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Обновляет значение свойства в строке JSON и возвращает обновленную строку JSON.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
JSON_MODIFY ( expression , path , newValue )  
```  
  
## <a name="arguments"></a>Аргументы

 *expression*  
 Выражение. Обычно имя переменной или столбца, содержащего текст JSON.  
  
 **JSON_MODIFY** возвращает ошибку, если *выражение* не содержит допустимых данных JSON.  
  
 *путь*  
 Выражение пути JSON, в котором указано обновляемое свойство.

 *Путь* имеет следующий синтаксис:  
  
 `[append] [ lax | strict ] $.<json path>`  
  
- *append*  
    Необязательный модификатор, который указывает, что новое значение следует добавить в массив, на который ссылается *\<json path>* .  
  
- *lax*  
    Указывает, что свойство, на которое ссылается *\<json path>* , может не существовать. Если свойство не существует, JSON_MODIFY пытается вставить новое значение по указанному пути. Если свойство не может быть вставлено в путь, вставка может завершиться ошибкой. Если *строгий* или *нестрогий* режим не указан, по умолчанию используется *нестрогий* режим (lax).  
  
- *строгий_режим*  
    Указывает, что свойство, на которое ссылается *\<json path>* , должно быть в выражении JSON. Если свойство не существует, JSON_MODIFY возвращает ошибку.  
  
- *\<json path>*  
    Указывает путь до обновляемого свойства. Дополнительные сведения см. в статье [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
В [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] можно указать переменную в качестве значения *пути*.

**JSON_MODIFY** возвращает ошибку, если *путь* имеет недопустимый формат.  
  
 *newValue*  
 Новое значение для свойства, указанного в *пути*.  
 Новое значение должно иметь тип [n]varchar или text.
  
 В нестрогом режиме JSON_MODIFY удаляет указанный ключ, если новое значение имеет значение NULL.  
  
JSON_MODIFY экранирует все специальные символы в новом значении, если значение имеет тип NVARCHAR или VARCHAR. Текстовое значение не экранируется, если оно имеет допустимый формат JSON и создано с помощью функций FOR JSON, JSON_QUERY или JSON_MODIFY.  
  
## <a name="return-value"></a>Возвращаемое значение

 Возвращает измененное значение *выражения* в виде текста JSON в допустимом формате.  
  
## <a name="remarks"></a>Remarks

 Функция JSON_MODIFY позволяет изменить значение существующего свойства, вставить новую пару ключ-значение или удалить ключ на основе сочетания режимов и предоставленных значений.  
  
 В следующей таблице сравнивается поведение **JSON_MODIFY** в нестрогом и в строгом режиме. Дополнительные сведения о необязательном режиме пути (строгий или нестрогий) см. в статье [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Существующее значение|Путь существует|Нестрогий режим|Строгий режим|  
|--------------------|-----------------|--------------|-----------------|  
|Не NULL|Да|Обновить существующее значение.|Обновить существующее значение.|  
|Не NULL|Нет|Попробуйте создать новую пару ключ-значение по указанному пути.<br /><br /> Эта операция может завершиться неудачно. Например, если указать путь `$.user.setting.theme`, JSON_MODIFY не вставляет ключ `theme` в тех случаях, если объекты `$.user` или `$.user.settings` не существуют или если параметр является массивом или скалярным значением.|Ошибка — INVALID_PROPERTY|  
|NULL|Да|Удалить существующее свойство.|Установить существующее значение в NULL.|  
|NULL|Нет|Никаких действий не выполняется. В качестве результата возвращается первый аргумент.|Ошибка — INVALID_PROPERTY|  
  
 В нестрогом режиме JSON_MODIFY пытается создать новую пару ключ-значение, но в некоторых случаях может произойти сбой.  
  
## <a name="examples"></a>Примеры  
  
### <a name="example---basic-operations"></a>Пример. Основные операции

 В следующем примере показаны основные операции, которые можно выполнить с текстом JSON.  
  
 **Запрос**
  
```sql

DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update name  

SET @info=JSON_MODIFY(@info,'$.name','Mike')

PRINT @info

-- Insert surname  

SET @info=JSON_MODIFY(@info,'$.surname','Smith')

PRINT @info

-- Delete name  

SET @info=JSON_MODIFY(@info,'$.name',NULL)

PRINT @info

-- Add skill  

SET @info=JSON_MODIFY(@info,'append $.skills','Azure')

PRINT @info
```  
  
 **Результаты**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---multiple-updates"></a>Пример. Множественные обновления

 С помощью JSON_MODIFY можно обновить только одно свойство. Если необходимо выполнить несколько операций обновления, можно использовать несколько вызовов JSON_MODIFY.  
  
 **Запрос**
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Multiple updates  

SET @info=JSON_MODIFY(JSON_MODIFY(JSON_MODIFY(@info,'$.name','Mike'),'$.surname','Smith'),'append $.skills','Azure')

PRINT @info
```  
  
 **Результаты**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---rename-a-key"></a>Пример. Переименование ключа  
 В приведенном ниже примере показано, как переименовать свойство в тексте JSON с помощью функции JSON_MODIFY. Сначала можно взять значение существующего свойства и вставить его как новую пару ключ-значение. Затем можно удалить старый ключ, установив значение старого свойства в NULL.  
  
 **Запрос**
  
```sql  
DECLARE @product NVARCHAR(100)='{"price":49.99}'

PRINT @product

-- Rename property  

SET @product=
 JSON_MODIFY(
  JSON_MODIFY(@product,'$.Price',CAST(JSON_VALUE(@product,'$.price') AS NUMERIC(4,2))),
  '$.price',
  NULL
 )

PRINT @product
```  
  
 **Результаты**
  
```json  
{
    "price": 49.99
} {
    "Price": 49.99
}
```  
  
 Если не привести новое значение к числовому типу, функция JSON_MODIFY воспринимает его как текст и заключает в двойные кавычки.  
  
### <a name="example---increment-a-value"></a>Пример. Увеличение значения на единицу

 В приведенном ниже примере показано, как увеличить значение свойства в тексте JSON на единицу с помощью функции JSON_MODIFY. Сначала можно взять значение существующего свойства и вставить его как новую пару ключ-значение. Затем можно удалить старый ключ, установив значение старого свойства в NULL.  
  
 **Запрос**
  
```sql  
DECLARE @stats NVARCHAR(100)='{"click_count": 173}'

PRINT @stats

-- Increment value  

SET @stats=JSON_MODIFY(@stats,'$.click_count',
 CAST(JSON_VALUE(@stats,'$.click_count') AS INT)+1)

PRINT @stats
```  
  
 **Результаты**
  
```json  
{
    "click_count": 173
} {
    "click_count": 174
}
```  
  
### <a name="example---modify-a-json-object"></a>Пример. Изменение объекта JSON

 JSON_MODIFY рассматривает аргумент *newValue* как чистый текст, даже если он содержит текст JSON в допустимом формате. В результате выходные данные JSON в функции заключаются в двойные кавычки, и все специальные символы экранируются, как показано в следующем примере.  
  
 **Запрос**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array

SET @info=JSON_MODIFY(@info,'$.skills','["C#","T-SQL","Azure"]')

PRINT @info
```  
  
 **Результаты**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": "["C#","T-SQL","Azure"]"
}
```  
  
 Чтобы избежать автоматического экранирования, укажите аргумент *newValue* с помощью функции JSON_QUERY. В этом случае JSON_MODIFY определит, что значение, возвращаемое функцией JSON_MODIFY, является кодом JSON в допустимом формате, и не будет экранировать значение.  
  
 **Запрос**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array  

SET @info=JSON_MODIFY(@info,'$.skills',JSON_QUERY('["C#","T-SQL","Azure"]'))

PRINT @info
```  
  
 **Результаты**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": ["C#", "T-SQL", "Azure"]
}
```  
  
### <a name="example---update-a-json-column"></a>Пример. Обновление столбца JSON

 В следующем примере показано, как изменить значение свойства в столбце таблицы, который содержит данные в формате JSON.  
  
```sql  
UPDATE Employee
SET jsonCol=JSON_MODIFY(jsonCol,'$.info.address.town','London')
WHERE EmployeeID=17
```  
  
## <a name="see-also"></a>См. также:

- [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Данные JSON (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
  
