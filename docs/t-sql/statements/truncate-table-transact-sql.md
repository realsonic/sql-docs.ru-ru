---
description: TRUNCATE TABLE (Transact-SQL)
title: TRUNCATE TABLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRUNCATE
- TRUNCATE TABLE
- TRUNCATE_TSQL
- TRUNCATE_TABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server], TRUNCATE TABLE statement
- table truncating [SQL Server]
- removing rows
- TRUNCATE TABLE statement
- deleting rows
- dropping rows
ms.assetid: 3d544eed-3993-4055-983d-ea334f8c5c58
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 04e6a49adfc24016557c1c55f1d7370b4dd80a80
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171256"
---
# <a name="truncate-table-transact-sql"></a>TRUNCATE TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Удаляет все строки в таблице или указанные секции таблицы, не записывая в журнал удаление отдельных строк. Инструкция TRUNCATE TABLE похожа на инструкцию DELETE без предложения WHERE, однако TRUNCATE TABLE выполняется быстрее и требует меньших ресурсов системы и журналов транзакций.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
TRUNCATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( PARTITIONS ( { <partition_number_expression> | <range> }   
    [ , ...n ] ) ) ]  
[ ; ]  
  
<range> ::=  
<partition_number_expression> TO <partition_number_expression>  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
TRUNCATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *database_name*  
 Имя базы данных.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица.  
  
 *table_name*  
 Имя таблицы, которая должна быть усечена, или таблицы, из которой удаляются все строки. *table_name* должно быть литералом. *table_name* не может быть функцией **OBJECT_ID()** или переменной.  
  
 WITH ( PARTITIONS ( { \<*partition_number_expression*> | \<*range*> } [ , ...n ] ) )    
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 Указывает секции для усечения или секции, из которых удаляются все строки. Если таблица не секционирована, аргумент `WITH PARTITIONS` приведет к возникновению ошибки. Если предложение `WITH PARTITIONS` не указано, будет усечена вся таблица.  
  
 *\<partition_number_expression>* можно указать одним из следующих способов. 
  
-   Указав номер секции, например `WITH (PARTITIONS (2))`  
  
-   Указав номера нескольких секций, разделив их запятыми, например `WITH (PARTITIONS (1, 5))`  
  
-   Указав диапазоны секций и отдельные секции, например `WITH (PARTITIONS (2, 4, 6 TO 8))`  
  
-   *\<range>* можно указать в виде номеров секций, разделенных словом **TO**, например `WITH (PARTITIONS (6 TO 8))`.  
  
 Для усечения секционированной таблицы таблицы и индексы должны быть выровнены (секционированы одной функцией секционирования).  
  
## <a name="remarks"></a>Remarks  
 Инструкция `TRUNCATE TABLE` обладает следующими преимуществами по сравнению с инструкцией DELETE.  
  
-   Используется меньший объем журнала транзакций.  
  
     Инструкция DELETE производит удаление по одной строке и заносит в журнал транзакций запись для каждой удаляемой строки. Инструкция `TRUNCATE TABLE` удаляет данные, освобождая страницы данных, используемые для хранения данных таблиц, и в журнал транзакций записывает только данные об освобождении страниц.  
  
-   Обычно используется меньшее количество блокировок.  
  
     Если инструкция DELETE выполняется с блокировкой строк, для удаления блокируется каждая строка таблицы. Инструкция `TRUNCATE TABLE` всегда блокирует таблицу (включая блокировку схемы (SCH-M)) и страницу, но не каждую строку.  
  
-   В таблице остается нулевое количество страниц, без исключений.  
  
     После выполнения инструкции DELETE в таблице могут все еще оставаться пустые страницы. Например, чтобы освободить пустые страницы в куче, необходима, как минимум, монопольная блокировка таблицы (LCK_M_X). Если операция удаления не использует блокировку таблицы, таблица (куча) будет содержать множество пустых страниц. В индексах после операции удаления могут оказаться пустые страницы, хотя эти страницы будут быстро освобождены процессом фоновой очистки.  
  
 Инструкция `TRUNCATE TABLE` удаляет все строки таблицы, но структура таблицы и ее столбцы, ограничения, индексы и т. п. сохраняются. Чтобы удалить не только данные таблицы, но и ее определение, следует использовать инструкцию `DROP TABLE`.  
  
 Если таблица содержит столбец идентификаторов, счетчик этого столбца сбрасывается до начального значения, определенного для этого столбца. Если начальное значение не задано, используется значение по умолчанию, равное 1. Чтобы сохранить столбец идентификаторов, используйте инструкцию DELETE.  
 
 > [!NOTE]
 > Для операции `TRUNCATE TABLE` можно выполнить откат.
  
## <a name="restrictions"></a>Ограничения  
 Нельзя использовать `TRUNCATE TABLE` в таблицах в следующих случаях.  
  
-   На таблицу ссылается ограничение FOREIGN KEY. Таблицу, имеющую внешний ключ, ссылающийся сам на себя, можно усечь. 
  
-   Таблица является частью индексированного представления.  
  
-   Таблица опубликована с использованием репликации транзакций или репликации слиянием.  

-   Это темпоральная таблица с управлением версиями.

-   На таблицу ссылается ограничение EDGE.  
  
 Для таблиц с какими-либо из этих характеристик следует использовать инструкцию DELETE.  
  
 Инструкция TRUNCATE TABLE не может активировать триггер, поскольку она не записывает в журнал удаление отдельных строк. Дополнительные сведения см. в разделе [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md). 
 
 В [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[sspdw](../../includes/sspdw-md.md)]:

- Инструкцию `TRUNCATE TABLE` нельзя использовать в инструкции EXPLAIN.

- Инструкцию `TRUNCATE TABLE` невозможно выполнить внутри транзакции.
  
## <a name="truncating-large-tables"></a>Усечение больших таблиц  
 В [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] существует возможность удалять или усекать таблицы, которые имеют больше 128 экстентов, не удерживая одновременные блокировки для всех экстентов, предназначенных для удаления.  
  
## <a name="permissions"></a>Разрешения  
 Минимально необходимым разрешением является `ALTER` для *table_name*. Разрешения `TRUNCATE TABLE` назначаются по умолчанию владельцу таблицы, членам предопределенной роли сервера `sysadmin`, а также предопределенных ролей базы данных `db_owner` и `db_ddladmin`, и они не могут быть переданы. Тем не менее инструкцию `TRUNCATE TABLE` можно встроить в модуль, например в хранимую процедуру, и предоставить соответствующие разрешения этому модулю с помощью предложения `EXECUTE AS`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-truncate-a-table"></a>A. Усечение таблицы  
 В ходе выполнения следующего примера удаляются все данные таблицы `JobCandidate`. Инструкции `SELECT` включены до и после инструкции `TRUNCATE TABLE` для сравнения результатов.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT COUNT(*) AS BeforeTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
TRUNCATE TABLE HumanResources.JobCandidate;  
GO  
SELECT COUNT(*) AS AfterTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
```  
  
### <a name="b-truncate-table-partitions"></a>Б. Усечение секций таблицы  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 В следующем примере выполняется усечение указанных секций секционированной таблицы. Синтаксис `WITH (PARTITIONS (2, 4, 6 TO 8))` задает усечение секций с номерами 2, 4, 6, 7 и 8.  
  
```sql  
TRUNCATE TABLE PartitionTable1   
WITH (PARTITIONS (2, 4, 6 TO 8));  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)   
 [Свойство IDENTITY (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
  
  

