---
title: CREATE XML INDEX (Transact-SQL)
description: CREATE XML INDEX (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_XML_INDEX_TSQL
- XML INDEX
- CREATE_XML_TSQL
- XML
- CREATE XML
- CREATE XML INDEX
- XML_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- CREATE INDEX statement
- index creation [SQL Server], XML indexes
- XML indexes [SQL Server], creating
ms.assetid: c510cfbc-68be-4736-b3cc-dc5b7aa51f14
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
ms.custom: ''
ms.date: 08/10/2017
ms.openlocfilehash: 03295249061e1ed6210c8dfbc086d0326137ad26
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082384"
---
# <a name="create-xml-index-transact-sql"></a>CREATE XML INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает XML-индекс по заданной таблице. Индекс может быть создан до появления данных в таблице. XML-индексы можно создавать на основе таблиц другой базы данных — для этого нужно указать полное имя базы данных.  
  
> [!NOTE]  
>  Чтобы создать реляционный индекс, обратитесь к разделу [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md). Дополнительные сведения о создании пространственного индекса см. в разделе [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
--Create XML Index   
CREATE [ PRIMARY ] XML INDEX index_name   
    ON <object> ( xml_column_name )  
    [ USING XML INDEX xml_index_name   
        [ FOR { VALUE | PATH | PROPERTY } ] ]  
    [ WITH ( <xml_index_option> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{ database_name.schema_name.table_name | schema_name.table_name | table_name }
  
<xml_index_option> ::=  
{   
    PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 [PRIMARY] XML  
 Создает XML-индекс по заданному столбцу **XML**. Если присутствует ключевое слово PRIMARY, создается кластеризованный индекс с ключом, образованным из ключа кластеризации таблицы пользователя и идентификатора XML-узла. Для каждой таблицы можно создать до 249 XML-индексов. При создании XML-индекса помните следующее.  
  
-   Кластеризованный индекс должен существовать для первичного ключа таблицы пользователя.  
  
-   Максимальное количество столбцов в ключе кластеризации таблицы пользователя — 15.  
  
-   У каждого столбца **XML** в таблице может быть один первичный XML-индекс и несколько вторичных.  
  
-   При создании вторичного XML-индекса для столбца **XML** первичный XML-индекс для этого столбца уже должен существовать.  
  
-   Индекс XML можно создать только для одного столбца **XML**. Невозможно создать XML-индекс для столбца, не относящегося к типу **XML**, а также реляционный индекс для столбца **XML**.  
  
-   Невозможно создать первичный или вторичный XML-индекс для столбца **XML** в представлении для переменной со столбцами типа **XML**, возвращающей табличное значение, или для переменных типа **XML**.  
  
-   Невозможно создать первичный XML-индекс для вычисляемого столбца **XML**.  
  
-   Значения параметров SET должны быть теми же, что и для индексированных представлений и индексов вычисляемых столбцов. В частности, при вставке, удалении или обновлении значений в столбце **XML** необходимо установить значение ON для параметра ARITHABORT.  
  
 Дополнительные сведения см в разделе [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
 *index_name*  
 Имя индекса. Имена индексов должны быть уникальными в пределах таблицы, но не обязательно должны быть уникальными в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 Имена первичных XML-индексов не должны начинаться со следующих символов: **#** , **##** , **@** или **@@** .  
  
 *xml_column_name*  
 Столбец **XML**, на котором основан индекс. В одном определении XML-индекса может быть задан только один столбец **XML**, но для одного столбца **XML** можно создать несколько вторичных XML-индексов.  
  
 USING XML INDEX *имя_индекса_XML*  
 Указывает первичный XML-индекс, который должен использоваться при создании вторичного XML-индекса.  
  
 FOR { VALUE | PATH | PROPERTY }  
 Указывает тип вторичного XML-индекса.  
  
 Значение  
 Создает вторичный XML-индекс для столбцов, где ключевые столбцы (значение узла и путь) входят в первичный XML-индекс.  
  
 PATH  
 Создает вторичный XML-индекс для столбцов, построенных на основе значений путей и узлов в первичном XML-индексе. Во вторичном индексе типа PATH значениями путей и узлов являются ключевые столбцы, обеспечивающие эффективный поиск путей.  
  
 PROPERTY  
 Создает вторичный XML-индекс по столбцам первичного XML-индекса (PK, путь и узел), где PK — первичный ключ базовой таблицы.  
  
 **\<object>::=**  
  
 Полное или неполное имя индексируемого объекта.  
  
 *database_name*  
 Имя базы данных.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица.  
  
 *table_name*  
 Имя таблицы для индексирования.  
  
 **\<xml_index_option> ::=** 
  
 Указывает параметры, которые должны использоваться при создании индекса.  
  
 PAD_INDEX **=** { ON | **OFF** }  
 Определяет разреженность индекса. Значение по умолчанию — OFF.  
  
 ON  
 Процент свободного места, определяемый параметром *fillfactor*, применяется к страницам индекса промежуточного уровня.  
  
 OFF или *fillfactor* не указан  
 Страницы промежуточного уровня заполняются почти полностью, при этом остается достаточно места по крайней мере для одной строки максимального размера, возможного в этом индексе при заданном наборе ключей на промежуточных страницах.  
  
 Параметр PAD_INDEX имеет смысл только в случае, если указан параметр FILLFACTOR, так как использует процентное значение, указанное в нем. Если процент, заданный аргументом FILLFACTOR, недостаточно велик для размещения одной строки, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] внутренне переопределит это значение, чтобы обеспечить минимум. Количество строк на странице индекса промежуточного уровня никогда не бывает менее двух даже при самых малых значениях аргумента *fillfactor*.  
  
 FILLFACTOR **=** _fillfactor_  
 Определяет величину в процентах, показывающую, насколько компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять конечный уровень каждой страницы индекса во время его создания или перестроения. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0. Если параметр *fillfactor* равен 100 или 0, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] создает индексы с полностью заполненными страницами конечного уровня.  
  
> [!NOTE]  
>  Значения коэффициентов заполнения 0 и 100 идентичны.  
  
 Аргумент FILLFACTOR действует только при создании или перестройке индекса. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не сохраняет динамически указанный процентный объем свободного места на страницах. Значение коэффициента заполнения можно увидеть в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
>  Создание кластеризованного индекса с аргументом FILLFACTOR меньше 100 влияет на объем пространства хранения, занимаемого данными, т. к. компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] перераспределяет данные, когда создает кластеризованный индекс.  
  
 Дополнительные сведения см. в статье [Указание коэффициента заполнения для индекса](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 Указывает, сохранять ли временные результаты сортировки в базе данных **tempdb**. Значение по умолчанию — OFF.  
  
 ON  
 Промежуточные результаты сортировки, которые используются при индексировании, хранятся в базе данных **tempdb**. Это может уменьшить время, необходимое для создания индекса, если база данных **tempdb** и база данных пользователя находятся на разных наборах дисков. Однако это увеличивает использование места на диске, которое используется при индексировании.  
  
 OFF  
 Промежуточные результаты сортировки хранятся в той же базе данных, где и индекс.  
  
 Кроме места в базе данных пользователя, необходимого для создания индекса, требуется примерно столько же дополнительного места в базе данных **tempdb** для хранения промежуточных результатов сортировки. Дополнительные сведения см. в разделе [Параметр SORT_IN_TEMPDB для индексов](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=OFF**  
 Не влияет на XML-индексы, поскольку этот тип индекса никогда не уникален. Не устанавливайте этот параметр в значение ON, иначе произойдет ошибка.  
  
 DROP_EXISTING **=** { ON | **OFF** }  
 Указывает, что именованный существующий XML-индекс удален и перестраивается. Значение по умолчанию — OFF.  
  
 ON  
 Существующий индекс удаляется и перестраивается. Указанное имя индекса должно совпадать с уже существующим индексом, но определение индекса может быть изменено. Например, можно указать другие столбцы, порядок сортировки, схему секционирования или параметры индекса.  
  
 OFF  
 Выдается ошибка, если индекс с указанным именем уже существует.  
  
 Тип индекса не может быть изменен с помощью аргумента DROP_EXISTING. Кроме того, первичный XML-индекс не может быть переопределен как вторичный XML-индекс и наоборот.  
  
 ONLINE **=OFF**  
 Указывает, что базовые таблицы и связанные индексы будут недоступны для запросов и изменения данных во время операций с индексами. В этой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не разрешено применять построение индекса в сети для XML-индексов. Если этому параметру присвоено значение ON для XML-индекса, то возникает ошибка. Не указывайте параметр ONLINE или установите его в значение OFF.  
  
 Операция вне сети с индексами, в ходе которой создается, перестраивается или удаляется XML-индекс, получает блокировку изменения схемы для таблицы. Это предотвращает доступ к базовой таблице всех пользователей во время операции.  
  
> [!NOTE]
>  Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.  
  
 ON  
 Блокировки строк допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.  
  
 OFF  
 Блокировки строк не используются.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.  
  
 ON  
 Блокировки страниц возможны при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.  
  
 OFF  
 Блокировки страниц не используются.  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 Переопределяет параметр конфигурации сервера [Максимальная степень параллелизма](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) на время выполнения операции с индексами. MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
> [!IMPORTANT]  
>  Несмотря на синтаксическую поддержку параметра MAXDOP для всех XML-индексов, инструкция CREATE XML INDEX использует только один процессор для первичного XML-индекса.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 1  
 Подавляет формирование параллельных планов.  
  
 \>1  
 Ограничивает максимальное количество процессоров, используемых в параллельных операциях с индексами, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы.  
  
 0 (по умолчанию)  
 В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
## <a name="remarks"></a>Remarks  
 Вычисляемые столбцы, производные от типов данных **XML**, могут быть проиндексированы или как ключевые, или как неключевые столбцы, если тип данных для вычисляемого столбца может быть использован в качестве ключевого столбца или неключевого столбца индекса. Невозможно создать первичный XML-индекс для вычисляемого столбца **XML**.  
  
 Для просмотра сведений об XML-индексах можно воспользоваться представлением каталога [sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md).  
  
 Дополнительные сведения об XML-индексах см. в разделе [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="additional-remarks-on-index-creation"></a>Дополнительные сведения о создании индексов  
 Дополнительные сведения о создании индексов см. в подразделе "Примечания" раздела [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-primary-xml-index"></a>A. Создание первичного XML-индекса  
 В следующем примере создается первичный XML-индекс по столбцу `CatalogDescription` таблицы `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT * FROM sys.indexes  
            WHERE name = N'PXML_ProductModel_CatalogDescription')  
    DROP INDEX PXML_ProductModel_CatalogDescription   
        ON Production.ProductModel;  
GO  
CREATE PRIMARY XML INDEX PXML_ProductModel_CatalogDescription  
    ON Production.ProductModel (CatalogDescription);  
GO  
```  
  
### <a name="b-creating-a-secondary-xml-index"></a>Б. Создание вторичного XML-индекса  
 В следующем примере создается вторичный XML-индекс по столбцу `CatalogDescription` таблицы `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.indexes  
            WHERE name = N'IXML_ProductModel_CatalogDescription_Path')  
    DROP INDEX IXML_ProductModel_CatalogDescription_Path  
        ON Production.ProductModel;  
GO  
CREATE XML INDEX IXML_ProductModel_CatalogDescription_Path   
    ON Production.ProductModel (CatalogDescription)  
    USING XML INDEX PXML_ProductModel_CatalogDescription FOR PATH ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)   
 [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
