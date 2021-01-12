---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Документация Майкрософт
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 7a7c60b45397f0a06cb7543dad9d15fff107a15f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095776"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Создает определяемую пользователем функцию в [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Определяемая пользователем функция представляет собой подпрограмму [!INCLUDE[tsql](../../includes/tsql-md.md)], которая принимает параметры, выполняет действие, например сложное вычисление, а затем возвращает результат этого действия в виде значения. В [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] возвращаемое значение должно быть скалярным (одиночным). В [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] оператор CREATE FUNCTION может возвращать таблицу, если используется синтаксис встроенных функций с табличным значением (предварительная версия), или одиночное значение, если используется синтаксис для скалярных функций. При помощи этой инструкции можно создать подпрограмму, которую можно повторно использовать следующими способами.  
  
-   В инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)], например SELECT.  
  
-   В приложениях, вызывающих функцию.  
  
-   В определении другой пользовательской функции.  
  
-   Для определения ограничения CHECK на столбец.  
  
-   Для замены хранимой процедуры.  
  
-   Использование встроенной функции в качестве предиката фильтра для политики безопасности  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Аргументы  
 *schema_name*  
 Имя схемы, к которой принадлежит определяемая пользователем функция.  
  
 *function_name*  
 Имя определяемой пользователем функции. Имена функций должны удовлетворять правилам построения идентификаторов и быть уникальными в пределах базы данных и схемы.  
  
> [!NOTE]  
>  Даже при отсутствии аргументов скобки после имени функции обязательны.  
  
 @*parameter_name*  
 Аргумент пользовательской функции. Может быть объявлен один или несколько аргументов.  
  
 Для функций допускается не более 2 100 параметров. При выполнении функции значение каждого из объявленных параметров должно быть указано пользователем, если для них не определены значения по умолчанию.  
  
 Определяет имя параметра, используя знак @ как первый символ. Имя параметра должно соответствовать правилам для идентификаторов. Параметры являются локальными в пределах функции, в разных функциях могут быть использованы одинаковые имена параметров. Аргументы могут использоваться только вместо констант. Они не могут использоваться вместо имен таблиц, имен столбцов или имен других объектов базы данных.  
  
> [!NOTE]  
>  Параметры ANSI_WARNINGS не годятся для передачи в хранимые процедуры, пользовательские функции и при объявлении и установке переменных в пакетных инструкциях. Например, если объявить переменную как **char(3)** , а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция INSERT или UPDATE завершится без ошибок.  
  
 *parameter_data_type*  
 Тип данных параметра. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допускаются все скалярные типы данных, которые поддерживаются в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Тип данных timestamp (rowversion) не поддерживается.  
  
 [ =*default* ]  
 Значение по умолчанию для аргумента. Если определено значение *default*, то функция выполняется даже в том случае, если для данного параметра значение не указано.  
  
 Если параметр функции имеет значение по умолчанию, то для него должно быть указано ключевое слово DEFAULT для получения функцией значения по умолчанию. Применение ключевого слова DEFAULT следует отличать от использования аргументов со значениями по умолчанию в хранимых процедурах, когда не указанный аргумент неявно принимает значение по умолчанию.  
  
 *return_data_type*  
 Возвращаемое значение скалярной пользовательской функции. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допускаются все скалярные типы данных, которые поддерживаются в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Тип данных timestamp (rowversion) не поддерживается. Нескалярные типы курсора и таблицы не допускаются.  
  
 *function_body*  
 Ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  Аргумент function_body не может содержать инструкцию SELECT и ссылаться на данные базы данных.  Аргумент function_body не может ссылаться на таблицы или представления. Аргумент function_body может вызывать другие детерминированные функции, но не может вызывать недетерминированные. 
  
 Для скалярных функций *function_body* представляет собой ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], которые в совокупности вычисляют скалярное выражение.  
  
 *scalar_expression*  
 Указывает скалярное значение, возвращаемое скалярной функцией.  

 *select_stmt* **Область применения**: Azure Synapse Analytics  
 Единственная инструкция SELECT, которая определяет возвращаемое значение встроенной функции с табличным значением (предварительная версия).

 TABLE **Область применения**: Azure Synapse Analytics  
 Указывает, что возвращаемым значением функции с табличным значением (TVF) является таблица. Функциям с табличным значением могут передаваться только константы и @*local_variables*.

 Во встроенных функциях с табличным значением (предварительная версия) возвращаемое значение TABLE определяется единственной инструкцией SELECT. Встроенные функции не имеют соответствующих возвращаемых переменных.
  
 **\<function_option>::=** 
  
 Указывает, что функция будет иметь один или несколько из следующих параметров.  
  
 SCHEMABINDING  
 Указывает, что функция привязана к объектам базы данных, которые содержат ссылки на нее. Если аргумент SCHEMABINDING указан, нельзя изменить базовые объекты таким способом, который может повлиять на определение функции. Сначала нужно изменить или удалить само определение функции, чтобы удалить зависимости от объекта, который требуется изменить.  
  
 Привязка функции к ссылающимся на нее объектам удаляется в следующих случаях:  
  
-   При удалении функции.  
  
-   При изменении функции инструкцией ALTER, если не указан параметр SCHEMABINDING.  
  
 Функция может быть привязана к схеме только в том случае, если выполняются следующие условия.  
  
-   Любые пользовательские функции, на которые ссылается данная функция, также привязаны к схеме.  
  
-   Функции и другие определяемые пользователем функции, на которые ссылается данная функция, указываются как одно- или двухкомпонентное имя.  
  
-   В теле определяемых пользователем функций может содержаться ссылка только на встроенные функции и другие определяемые пользователем функции в той же базе данных.  
  
-   Пользователь, выполняющий инструкцию CREATE FUNCTION, имеет разрешение REFERENCES на объекты базы данных, на которые ссылается функция.  
  
 Чтобы удалить SCHEMABINDING, используйте ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Указывает атрибут **OnNULLCall** скалярной функции. Если данный аргумент не указан, по умолчанию предполагается CALLED ON NULL INPUT. Это означает, что текст функции выполняется даже в том случае, если в качестве аргумента передано значение NULL.  
  
## <a name="best-practices"></a>Рекомендации  
 Если определяемая пользователем функция создан без применения предложения SCHEMABINDING, то изменения базовых объектов могут повлиять на определение функции и привести к непредвиденным результатам при вызове функции. Рекомендуется реализовать один из следующих методов, чтобы обеспечить, что функция не устареет из-за изменения ее базовых объектов.  
  
-   Укажите при создании функции предложение WITH SCHEMABINDING. Это обеспечит невозможность изменения объектов, на которые ссылается определение функции, если при этом не изменяется сама функция.  
  
## <a name="interoperability"></a>Совместимость  
 В скалярных функциях допустимы следующие инструкции с одиночным значением.  
  
-   Инструкции присваивания.  
  
-   Инструкции управления потоком, за исключением инструкций TRY...CATCH.  
  
-   Инструкции DECLARE, объявляющие локальные переменные.  

Во встроенной функции с табличным значением (предварительная версия) допускается только одна инструкция SELECT.
  
## <a name="limitations-and-restrictions"></a>ограничения  
 Определяемые пользователем функции не могут выполнять действия, изменяющие состояние базы данных.  
  
 Определяемые пользователем функции могут быть вложенными, то есть из одной функции может быть вызвана другая. Уровень вложенности увеличивается на единицу каждый раз, когда начинается выполнение вызванной функции и уменьшается на единицу, когда ее выполнение завершается. Вложенность определяемых пользователем функций не может превышать 32 уровней. Превышение максимального уровня вложенности приводит к ошибке выполнения для всей цепочки вызываемых функций.   
  
## <a name="metadata"></a>Метаданные  
 В следующем разделе приводятся системные представления каталога, возвращающие метаданные об определяемых пользователем функциях.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md): отображает определение определяемых пользователем функций [!INCLUDE[tsql](../../includes/tsql-md.md)]. Пример:  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md): Выводит сведения о параметрах, определенных в определяемых пользователем функциях.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md): Отображает базовые объекты, на которые ссылается функция.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение CREATE FUNCTION на базу данных и разрешение ALTER на схему, в которой создается функция.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>A. Применение скалярной определяемой пользователем функции для изменения типа данных  
 Эта скалярная функция принимает тип данных **int** и возвращает тип данных **decimal(10,2)** .  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>A. Создание встроенной функции с табличным значением (предварительная версия)
 В следующем примере создается встроенная функция с табличным значением, которая возвращает важные сведения о модулях, отфильтрованных по параметру `objectType`. По умолчанию эта функция возвращает все модули, если вызов осуществлялся с параметром DEFAULT. В этом примере используются некоторые представления системного каталога, упомянутые в [описании метаданных](#metadata).

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
Эту функцию можно вызвать, чтобы получить все объекты представления (**V**), следующим образом:
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>Б. Объединение результатов встроенной функции с табличным значением (предварительная версия)
 В этом простом примере используется ранее созданная встроенная функция и показано объединение ее результатов с другими таблицами с помощью инструкции CROSS APPLY. Здесь мы выберем все столбцы из sys.objects и результатов выполнения `ModulesByType`, отбирая строки по совпадению значений столбца *type*. Дополнительные сведения об использовании инструкции APPLY см. в статье [Предложение FROM и JOIN, APPLY, PIVOT (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>См. также раздел  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

