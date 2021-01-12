---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 1c7b948b1f883d1de6c6bb6556aee25c0e859695
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085994"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** — функция с табличным значением, которая выполняет синтаксический анализ текста JSON и возвращает объекты и свойства из входных данных JSON в виде строк и столбцов. Другими словами, **OPENJSON** предоставляет документ JSON в виде набора строк. Вы можете явно указать столбцы в наборе строк и пути к свойствам JSON, используемые для заполнения столбцов. Поскольку **OPENJSON** возвращает набор строк, вы можете использовать **OPENJSON** в предложении `FROM` инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] точно так же, как и в любой другой таблице, представлении или в функции с табличным значением.  
  
Используйте **OPENJSON**, чтобы импортировать данные JSON в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или чтобы преобразовать данные JSON в реляционный формат для приложений или служб, которые не могут использовать формат JSON напрямую.  
  
> [!NOTE]  
>  Функция **OPENJSON** доступна только для уровня совместимости 130 или выше. Если уровень совместимости вашей базы данных меньше 130, SQL Server не сможет найти и выполнить функцию **OPENJSON**. Другие функции JSON доступны на всех уровнях совместимости.
> 
> Проверить уровень совместимости можно в представлении `sys.databases` или в свойствах базы данных. Изменить уровень совместимости базы данных можно с помощью следующей команды:  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> Уровень совместимости 120 может использоваться по умолчанию даже в новой базе данных SQL Azure.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел")[Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

**OPENJSON** — функция с табличным значением, которая выполняет синтаксический анализ выражения *jsonExpression*, передаваемого в качестве первого аргумента, и возвращает одну или несколько строк, содержащих данные объектов JSON в выражении. Выражение *jsonExpression* может содержать вложенные дочерние объекты. Если вы хотите проанализировать дочерний объект из выражения *jsonExpression*, можете указать параметр **путь** для дочернего объекта JSON.

### <a name="openjson"></a>openjson

![Синтаксис функции с табличным значением OPENJSON](../../relational-databases/json/media/openjson-syntax.png "Синтаксис OPENJSON")  

По умолчанию функция с табличным значением **OPENJSON** возвращает три столбца, содержащих имя ключа, значение и тип для каждой пары {ключ: значение}, обнаруженной в *jsonExpression*. Кроме того, можно явно указать схему результирующего набора, который возвращает **OPENJSON**, указав *предложение_with*.
  
### <a name="with_clause"></a>предложение_with
  
![Синтаксис предложения WITH в функции с табличным значением OPENJSON](../../relational-databases/json/media/openjson-shema-syntax.png "Синтаксис OPENJSON WITH")

*предложение_with* содержит список столбцов с их типами, которые должна вернуть функция **OPENJSON**. По умолчанию **OPENJSON** сравнивает ключи в *jsonExpression* с именами столбцов в *предложении_with* (сравнение выполняется с учетом регистра). Если имя столбца не соответствует имени ключа, можно указать необязательный параметр *путь_столбца*, который является [выражением пути JSON](../../relational-databases/json/json-path-expressions-sql-server.md), ссылающимся на ключ в *jsonExpression*. 

## <a name="arguments"></a>Аргументы

### <a name="jsonexpression"></a>*jsonExpression*

Символьное выражение Юникода, содержащее текст JSON.  
  
OPENJSON выполняет итерацию по элементам массива или свойствам объекта в выражении JSON и возвращает одну строку для каждого элемента или свойства. В следующем примере возвращаются все свойства объекта *jsonExpression*:  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**Результаты:**

| ключ                                | value                 | type |
| :--                                | :----                 | :--- |
| String_value                       | Джон                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2.3456                | 2 |
| BooleanTrue_value                  | Да                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | NULL                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- DoublePrecisionFloatingPoint_value соответствует IEEE-754.

### <a name="path"></a>*путь*

Необязательное выражение пути JSON, которое ссылается на объект или массив в *jsonExpression*. **OPENJSON** обращается к тексту JSON в указанной позиции и анализирует только заданный фрагмент текста. Дополнительные сведения см. в статье [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).

В [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] можно указать переменную в качестве значения *пути*.
  
В следующем примере возвращается вложенный объект по указанному *пути*:  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **Результаты**  
  
|Клавиши|Значение|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

Когда функция **OPENJSON** анализирует массив JSON, она возвращает индексы элементов в тексте JSON в виде ключей.

Сравнение, используемое для сопоставления шагов пути со свойствами выражения JSON, выполняется с учетом регистра и без учета параметров сортировки (сравнение BIN2). 

### <a name="with_clause"></a>*предложение_with*

Явным образом определяет выходные данные схемы, возвращаемые функцией **OPENJSON**. Необязательное *предложение_with* может содержать следующие элементы:

*colName* — имя выходного столбца.  
  
По умолчанию **OPENJSON** использует имя столбца для сравнения со свойством в тексте JSON. Например, если указать столбец *name* в схеме, OPENJSON попытается заполнить этот столбец свойством "name" в тексте JSON. Это сопоставление по умолчанию можно переопределить с помощью аргумента *путь_столбца*.  
  
*type*  
Тип данных выходного столбца.  

> [!NOTE]
> При использовании параметра **AS JSON** столбец *тип* должен иметь значение `NVARCHAR(MAX)`.
  
*путь_столбца*  
Путь JSON, который определяет возвращаемое свойство в указанном столбце. Дополнительные сведения см. в описании параметра *путь* ранее в этом разделе.  
  
Используйте параметр *путь_столбца* для переопределения правил сопоставления по умолчанию, когда имя выходного столбца не соответствует имени свойства.  
  
Сравнение, используемое для сопоставления шагов пути со свойствами выражения JSON, выполняется с учетом регистра и без учета параметров сортировки (сравнение BIN2).  
  
Дополнительные сведения о путях см. в разделе [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
*AS JSON*  
Используйте параметр **AS JSON** в определении столбца, чтобы указать, что указанное свойство содержит внутренний объект JSON или массив. При указании параметра **AS JSON** необходимо использовать тип столбца NVARCHAR(MAX).

- Если не указать параметр **AS JSON** для столбца, функция возвращает скалярное значение (например, целочисленное значение, строковое значение, true, false) для заданного свойства JSON по указанному пути. Если путь представляет объект или массив и по указанному пути не удается найти свойство, функция возвращает NULL в нестрогом режиме или сообщение об ошибке в строгом режиме. Это поведение похоже на поведение функции **JSON_VALUE**.  
  
- Если указать параметр **AS JSON** для столбца, функция возвращает фрагмент JSON для заданного свойства JSON по указанному пути. Если путь представляет скалярное значение и по указанному пути не удается найти свойство, функция возвращает NULL в нестрогом режиме или сообщение об ошибке в строгом режиме. Это поведение похоже на поведение функции **JSON_QUERY**.  
  
> [!NOTE]  
> Если вы хотите возвратить вложенный фрагмент JSON из свойства JSON, необходимо указать флаг **AS JSON**. Если этот параметр не указан и свойство не удается найти, OPENJSON возвращает значение NULL вместо указанного объекта или массива JSON или массива или возвращает ошибку времени выполнения в строгом режиме.  
  
Например, следующий запрос возвращает и форматирует элементы массива:  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**Результаты**
  
|Number|Дата|Customer|Количество|Порядок|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>Возвращаемое значение
Столбцы, которые возвращает функция OPENJSON, зависят от параметра WITH.  
  
1. При вызове OPENJSON со схемой по умолчанию (то есть без указания явной схемы в предложении WITH) функция возвращает таблицу со следующими столбцами:  
    1.  **Key**. Значение nvarchar(4000), содержащее имя указанного свойства или индекс элемента в указанном массиве. Этот столбец имеет параметры сортировки BIN2.  
    2.  **Значение**. Значение типа nvarchar(max), содержащее значение свойства. Значение столбца наследует параметры сортировки из *jsonExpression*.
    3.  **Type**. Целочисленное значение, содержащее тип значения. Столбец **Type** возвращается только при использовании OPENJSON со схемой по умолчанию. Столбец Type имеет одно из следующих значений:  
  
        |Значение столбца Type|Тип данных JSON|  
        |------------------------------|--------------------|  
        |0|null|  
        |1|строка|  
        |2|number|  
        |3|true/false|  
        |4|массиве|  
        |5|объект|  
  
     Возвращаются только свойства первого уровня. Если текст JSON имеет неправильный формат, выполнение инструкции завершается с ошибкой.  

2. При вызове OPENJSON с указанием явной схемы в предложении WITH функция возвращает таблицу со схемой, заданной в предложении WITH.

> [!NOTE]  
> Столбцы **Key**, **Value** и **Type** возвращаются только при использовании OPENJSON со схемой по умолчанию. Они недоступны с явной схемой.

## <a name="remarks"></a>Remarks  

Параметр *путь_json*, который используется в качестве второго аргумента инструкции **OPENJSON** или в *предложении_with*, может начинаться с ключевого слова **lax** или **strict**.

- В **нестрогом** режиме **OPENJSON** не выдает ошибку, если не удалось найти объект или значение по указанному пути. Если не удается найти путь, **OPENJSON** возвращает пустой результирующий набор или значение NULL.
- В **строгом** режиме **OPENJSON** возвращает ошибку, если путь не найден.

В некоторых примерах на этой странице явно указывается режим пути: нестрогий или строгий режим. Режим пути является необязательным. Если режим пути не указан явно, по умолчанию используется нестрогий режим. Дополнительные сведения о режиме пути и выражениях пути см. в разделе [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md).    

Имена столбцов в *предложении_with* сопоставляются с ключами в тексте JSON. Если указать имя столбца `[Address.Country]`, он сопоставляется с ключом `Address.Country`. Если требуется сослаться на вложенный ключ `Country` в объекте `Address`, нужно указать путь `$.Address.Country` в пути столбца.

*Путь_json* может содержать ключи с буквенно-цифровые символами. Если в ключах используются специальные символы, экранируйте имя ключа в *пути_json* двойными кавычками. Например, `$."my key $1".regularKey."key with . dot"` соответствует значению 1 в следующем тексте JSON:

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>Примеры  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>Пример 1. Преобразование массива JSON во временную таблицу

В следующем примере список идентификаторов представлен как массив чисел JSON. Запрос преобразует массив JSON в таблицу идентификаторов и выбирает все продукты с указанными идентификаторами.  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
Этот запрос эквивалентен следующему примеру. Тем не менее в приведенном ниже примере необходимо вставить числовые значения в запрос вместо того, чтобы передавать их в качестве параметров.  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>Пример 2. Слияние свойств из двух объектов JSON

В следующем примере возвращается объединение всех свойств для двух объектов JSON. У двух объектов есть одинаковое свойство *name*. В примере используется значение ключа, чтобы исключить повторяющиеся строки из результатов.  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>Пример 3. Соединение строк с данными JSON, хранящимися в ячейках таблицы, с помощью CROSS APPLY

В следующем примере в таблице `SalesOrderHeader` есть текстовый столбец `SalesReason`, содержащий массив `SalesOrderReasons` в формате JSON. У объектов `SalesOrderReasons` есть такие свойства, как *Quality* и *Manufacturer*. В примере создается отчет, который соединяет каждую строку заказа на продажу с соответствующими причинами покупки. Оператор OPENJSON разворачивает массив JSON причин покупки, как если бы причины хранились в отдельной дочерней таблице. Затем оператор CROSS APPLY соединяет каждую строку заказа на продажу со строками, возвращенными функцией с табличным значением OPENJSON.  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> Если необходимо развернуть массивы JSON, которые хранятся в отдельных полях, и соединить их с родительскими строками, обычно используется оператор CROSS APPLY [!INCLUDE[tsql](../../includes/tsql-md.md)]. Дополнительные сведения о CROSS APPLY см. в разделе [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
Тот же запрос можно переписать с помощью `OPENJSON` с явно определенной схемой возвращаемых строк:  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
В этом примере путь `$` ссылается на каждый элемент в массиве. Если необходимо явно привести возвращаемое значение, можно использовать этот тип запроса.  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>Пример 4. Объединение реляционных строк и элементов JSON с помощью CROSS APPLY

Следующий запрос объединяет реляционные строки и элементы JSON в результаты, показанные в следующей таблице.  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**Результаты**
  
|title|street|postcode|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA 98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA 98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>Пример 5. Импорт данных JSON в SQL Server

В следующем примере весь объект JSON загружается в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>Пример 6. Простой пример с содержимым JSON

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>См. также раздел

- [Выражения пути JSON (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Преобразование данных JSON в строки и столбцы с помощью функции OPENJSON (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [Использование OPENJSON со схемой по умолчанию &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [Использование OPENJSON с явной схемой (SQL Server)](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  
