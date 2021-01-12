---
description: JSON_VALUE (Transact-SQL)
title: JSON_VALUE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JSON_VALUE
- JSON_VALUE_TSQL
helpviewer_keywords:
- JSON_VALUE function
- JSON, extracting
- JSON, querying
ms.assetid: cd016e14-11eb-4eaf-bf05-c7cfcc820a10
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 9c6571a5938c503b99378003323168a695de58a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087041"
---
# <a name="json_value-transact-sql"></a>JSON_VALUE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Извлекает скалярное значение из строки JSON.  
  
 Чтобы извлечь объект или массив, а не скалярное значение из строки JSON, см. статью [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md). Сведения о различиях между **JSON_VALUE** и **JSON_QUERY** см. в разделе [Сравнение JSON_VALUE и JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
JSON_VALUE ( expression , path )  
```  
  
## <a name="arguments"></a>Аргументы

 *expression*  
 Выражение. Обычно имя переменной или столбца, содержащего текст JSON.  

 Если функция **JSON_VALUE** находит код JSON, который не является допустимым в *expression* перед тем, как она найдет значение, определяемое аргументом *path*, функция возвращает ошибку. Если функция **JSON_VALUE** не находит значение, определяемое аргументом *path*, она просматривает весь текст и возвращает ошибку, если найдет недопустимый код JSON в любом месте аргумента *expression*.
  
 *путь*  
 Путь JSON, указывающий на извлекаемое свойство. Дополнительные сведения см. в статье [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).  

В [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] можно указать переменную в качестве значения *пути*.
  
 Если аргумент *путь* имеет недопустимый формат, **JSON_VALUE** возвращает ошибку.  
  
## <a name="return-value"></a>Возвращаемое значение

 Возвращает одно текстовое значение типа nvarchar(4000). Параметры сортировки для возвращаемого значения совпадают с параметрами сортировки входного выражения.  
  
 Если длина значения превышает 4000 символов:  
  
- в нестрогом режиме **JSON_VALUE** возвращает значение NULL;  
  
- в строгом режиме **JSON_VALUE** возвращает сообщение об ошибке.  
  
 Если необходимо возвращать скалярные значения длиной более 4000 символов, используйте функцию **OPENJSON** вместо **JSON_VALUE**. Дополнительные сведения см. в разделе [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md).  
  
## <a name="remarks"></a>Remarks

### <a name="lax-mode-and-strict-mode"></a>Нестрогий и строгий режимы

 Рассмотрим следующий текст JSON:  
  
```json  
DECLARE @jsonInfo NVARCHAR(MAX)

SET @jsonInfo=N'{  
     "info":{    
       "type":1,  
       "address":{    
         "town":"Bristol",  
         "county":"Avon",  
         "country":"England"  
       },  
       "tags":["Sport", "Water polo"]  
    },  
    "type":"Basic"  
 }'  
```  
  
 В приведенной ниже таблице сравнивается поведение **JSON_VALUE** в нестрогом и в строгом режимах. Дополнительные сведения о необязательном режиме пути (строгий или нестрогий) см. в статье [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|путь|Возвращаемое значение в нестрогом режиме|Возвращаемое значение в строгом режиме|Дополнительные сведения|  
|----------|------------------------------|---------------------------------|---------------|  
|$|NULL|Error|Не является скалярным значением.<br /><br /> Используйте вместо этого функцию **JSON_QUERY**.|  
|$.info.type|N'1'|N'1'|Недоступно|  
|$.info.address.town|N'Bristol'|N'Bristol'|Недоступно|  
|$.info."address"|NULL|Error|Не является скалярным значением.<br /><br /> Используйте вместо этого функцию **JSON_QUERY**.|  
|$.info.tags|NULL|Error|Не является скалярным значением.<br /><br /> Используйте вместо этого функцию **JSON_QUERY**.|  
|$.info.type[0]|NULL|Error|Не является массивом.|  
|$.info.none|NULL|Error|Свойство не существует.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="examples"></a>Примеры  
  
### <a name="example-1"></a>Пример 1
 В приведенном ниже примере в результатах запроса используются значения свойств JSON `town` и `state`. Так как функция **JSON_VALUE** сохраняет параметры сортировки источника, порядок сортировки результатов зависит от параметров сортировки столбца `jsonInfo`. 

> [!NOTE]
> (В этом примере предполагается, что таблица с именем `Person.Person` содержит столбец `jsonInfo` с текстом JSON и что этот столбец имеет структуру, представленную ранее в разделе, посвященном нестрогому и строгому режимам. В образце базы данных AdventureWorks таблица `Person` не содержит столбец `jsonInfo`.)
  
```sql  
SELECT FirstName, LastName,
 JSON_VALUE(jsonInfo,'$.info.address[0].town') AS Town
FROM Person.Person
WHERE JSON_VALUE(jsonInfo,'$.info.address[0].state') LIKE 'US%'
ORDER BY JSON_VALUE(jsonInfo,'$.info.address[0].town')
```  
  
### <a name="example-2"></a>Пример 2
 В приведенном ниже примере извлекается значение свойства JSON `town` в локальную переменную.  
  
```sql
DECLARE @jsonInfo NVARCHAR(MAX)
DECLARE @town NVARCHAR(32)

SET @jsonInfo=N'{"info":{"address":[{"town":"Paris"},{"town":"London"}]}}';

SET @town=JSON_VALUE(@jsonInfo,'$.info.address[0].town'); -- Paris
SET @town=JSON_VALUE(@jsonInfo,'$.info.address[1].town'); -- London
```  
  
### <a name="example-3"></a>Пример 3
 В приведенном ниже примере создаются вычисляемые столбцы на основе значений свойств JSON.  
  
```sql  
CREATE TABLE dbo.Store
 (
  StoreID INT IDENTITY(1,1) NOT NULL,
  Address VARCHAR(500),
  jsonContent NVARCHAR(4000),
  Longitude AS JSON_VALUE(jsonContent, '$.address[0].longitude'),
  Latitude AS JSON_VALUE(jsonContent, '$.address[0].latitude')
 )
```  
  
## <a name="see-also"></a>См. также
 [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
 [Данные JSON (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
  
