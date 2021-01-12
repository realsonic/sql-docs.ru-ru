---
description: COLUMNS_UPDATED (Transact-SQL)
title: COLUMNS_UPDATED (Transact-SQL)
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLUMNS_UPDATED_TSQL
- COLUMNS_UPDATED
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS_UPDATED function
- testing columns
- column testing [SQL Server]
- updated columns
ms.assetid: 765fde44-1f95-4015-80a4-45388f18a42c
author: cawrites
ms.author: chadam
ms.openlocfilehash: c927fbad08c222aad4e284a66d3b3e61061e82e9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101297"
---
# <a name="columns_updated-transact-sql"></a>COLUMNS_UPDATED (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает битовый шаблон **varbinary**, который показывает, какие столбцы таблицы или представления добавлялись или изменялись. `COLUMNS_UPDATED` используется в любом месте внутри тела триггера INSERT или UPDATE [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы проверить необходимость выполнения определенных действий.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
COLUMNS_UPDATED ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
**varbinary**
  
## <a name="remarks"></a>Remarks  
Функция `COLUMNS_UPDATED` проверяет возможность выполнения операций триггером UPDATE или INSERT над несколькими столбцами. Чтобы проверить срабатывание триггера UPDATE или INSERT в одном столбце, используйте инструкцию [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md).
  
Функция `COLUMNS_UPDATED` возвращает один байт или несколько, которые упорядочены слева направо. Крайний правый бит каждого байта является наименее значимым битом. Крайний правый бит крайнего левого байта представляет первый столбец в таблице, следующий бит слева представляет второй столбец и так далее. Функция `COLUMNS_UPDATED` возвращает несколько байт, если таблица в которой триггер создается, содержит более чем восемь столбцов, с наименее значащим байтом в крайней левой позиции. Функция `COLUMNS_UPDATED` возвращает значение TRUE для всех столбцов при операциях INSERT, так как при вставке столбцам присваиваются явно указанные значения или неявные значения (NULL).
  
Чтобы проверить обновление и вставку в определенные столбцы, следуйте синтаксису битовых операторов и целой битовой маске проверяемого столбца. Например, пусть таблица **t1** содержит столбцы **C1**, **C2**, **C3**, **C4** и **C5**. Чтобы проверить, все ли столбцы **C2**, **C3** и **C4** (в таблице **t1** под действием триггера UPDATE) успешно обновлены, следуйте синтаксису **& 14**. Чтобы проверить обновление только столбца **C2**, укажите **& 2**. Фактические примеры см. в разделе [Пример A](#a-using-columns_updated-to-test-the-first-eight-columns-of-a-table) и [Примере Б](#b-using-columns_updated-to-test-more-than-eight-columns).
  
Функция `COLUMNS_UPDATED` может быть использована внутри триггера INSERT или UPDATE языка [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
Столбец ORDINAL_POSITION представления INFORMATION_SCHEMA.COLUMNS несовместим с битовым шаблоном столбцов, возвращаемым функцией `COLUMNS_UPDATED`. Чтобы получить битовый шаблон, совместимый с функцией `COLUMNS_UPDATED`, обратитесь к свойству `ColumnID` системной функции `COLUMNPROPERTY` при запросе представления `INFORMATION_SCHEMA.COLUMNS`, как показано в следующем примере.
  
```sql
SELECT TABLE_NAME, COLUMN_NAME,  
    COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME),  
    COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
```  

Если триггер применяется к столбцу, значение `COLUMNS_UPDATED` будет возвращаться в виде `true` или `1`, даже если значение столбца остается неизменным. Это нормальное поведение, и триггер должен реализовывать бизнес-логику, которая определяет, допустимы ли операции вставки, обновления и удаления. 
  
## <a name="column-sets"></a>Наборы столбцов
Если в таблице определен набор столбцов, функция `COLUMNS_UPDATED` действует следующими способами.
-   Если явным образом обновляется столбец из набора столбцов, соответствующему биту для этого столбца присваивается значение 1, а биту для набора столбцов присваивается значение 1.  
-   Если явным образом обновляется набор столбцов, биту для набора столбцов присваивается значение 1 и биты для всех разреженных столбцов в этой таблице получают значение 1.  
-   При операциях вставки всем битам присваивается значение 1.  
  
     Так как при изменениях в наборе столбцов битам для всех столбцов в наборе столбцов присваивается значение 1, столбцы в наборе столбцов, не подвергавшиеся изменениям, будут выглядеть как измененные. Дополнительные сведения о наборах столбцов см. в разделе [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-columns_updated-to-test-the-first-eight-columns-of-a-table"></a>A. Использование функции COLUMNS_UPDATED для проверки первых восьми столбцов таблицы  
В этом примере создается две таблицы: `employeeData` и `auditEmployeeData`. Таблица `employeeData` содержит сведения о заработной плате служащих и может быть изменена членами отдела кадров. Если номер социальной страховки (SSN), ежегодная заработная плата или номер банковского счета служащего изменяется, то создается запись аудита и вставляется в таблицу аудита `auditEmployeeData`.
  
С помощью функции `COLUMNS_UPDATED()` можно быстро проверить все изменения, внесенные в столбцы, содержащие важные сведения о служащих. Такое использование функции `COLUMNS_UPDATED()` будет оправдано, только если пытаться выявить изменения в первых восьми столбцах таблицы.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'employeeData')  
   DROP TABLE employeeData;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'auditEmployeeData')  
   DROP TABLE auditEmployeeData;  
GO  
CREATE TABLE dbo.employeeData (  
   emp_id INT NOT NULL PRIMARY KEY,  
   emp_bankAccountNumber CHAR (10) NOT NULL,  
   emp_salary INT NOT NULL,  
   emp_SSN CHAR (11) NOT NULL,  
   emp_lname NCHAR (32) NOT NULL,  
   emp_fname NCHAR (32) NOT NULL,  
   emp_manager INT NOT NULL  
   );  
GO  
CREATE TABLE dbo.auditEmployeeData (  
   audit_log_id uniqueidentifier DEFAULT NEWID() PRIMARY KEY,  
   audit_log_type CHAR (3) NOT NULL,  
   audit_emp_id INT NOT NULL,  
   audit_emp_bankAccountNumber CHAR (10) NULL,  
   audit_emp_salary INT NULL,  
   audit_emp_SSN CHAR (11) NULL,  
   audit_user sysname DEFAULT SUSER_SNAME(),  
   audit_changed DATETIME DEFAULT GETDATE()  
   );  
GO  
CREATE TRIGGER dbo.updEmployeeData   
ON dbo.employeeData   
AFTER UPDATE AS  
/* Check whether columns 2, 3 or 4 have been updated. If any or all  
columns 2, 3 or 4 have been changed, create an audit record.
The bitmask is: power(2, (2-1)) + power(2, (3-1)) + power(2, (4-1)) = 14.
This bitmask translates into base_10 as: 2 + 4 + 8 = 14.
To test whether all columns 2, 3, and 4 are updated, use = 14 instead of > 0  
(below). */
  
   IF (COLUMNS_UPDATED() & 14) > 0  
/* Use IF (COLUMNS_UPDATED() & 14) = 14 to see whether all columns 2, 3,   
and 4 are updated. */  
      BEGIN  
-- Audit OLD record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'OLD',   
            del.emp_id,  
            del.emp_bankAccountNumber,  
            del.emp_salary,  
            del.emp_SSN  
         FROM deleted del;  
  
-- Audit NEW record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'NEW',  
            ins.emp_id,  
            ins.emp_bankAccountNumber,  
            ins.emp_salary,  
            ins.emp_SSN  
         FROM inserted ins;  
   END;  
GO  
  
/* Inserting a new employee does not cause the UPDATE trigger to fire. */  
INSERT INTO employeeData  
   VALUES ( 101, 'USA-987-01', 23000, 'R-M53550M', N'Mendel', N'Roland', 32);  
GO  
  
/* Updating the employee record for employee number 101 to change the   
salary to 51000 causes the UPDATE trigger to fire and an audit trail to   
be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_salary = 51000  
   WHERE emp_id = 101;  
GO  
SELECT * FROM auditEmployeeData;  
GO  
  
/* Updating the employee record for employee number 101 to change both   
the bank account number and social security number (SSN) causes the   
UPDATE trigger to fire and an audit trail to be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_bankAccountNumber = '133146A0', emp_SSN = 'R-M53550M'  
   WHERE emp_id = 101;  
GO  
SELECT * FROM dbo.auditEmployeeData;  
  
GO  
```  
  
### <a name="b-using-columns_updated-to-test-more-than-eight-columns"></a>Б. Использование функции COLUMNS_UPDATED для проверки более чем восьми столбцов  
Чтобы проверить обновления других столбцов таблицы, кроме первых восьми, используйте функцию `SUBSTRING` для проверки корректности бита, возвращенного функцией `COLUMNS_UPDATED`. В этом примере проверяется обновление столбцов `3`, `5` и `9` таблицы `AdventureWorks2012.Person.Person`.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.uContact2', N'TR') IS NOT NULL  
    DROP TRIGGER Person.uContact2;  
GO  
CREATE TRIGGER Person.uContact2 ON Person.Person  
AFTER UPDATE AS  
    IF ( (SUBSTRING(COLUMNS_UPDATED(), 1, 1) & 20 = 20)   
        AND (SUBSTRING(COLUMNS_UPDATED(), 2, 1) & 1 = 1) )   
    PRINT 'Columns 3, 5 and 9 updated';  
GO  
  
UPDATE Person.Person   
   SET NameStyle = NameStyle,  
      FirstName=FirstName,  
      EmailPromotion=EmailPromotion;  
GO  
```  
  
## <a name="see-also"></a>См. также раздел
[Побитовые операторы (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
[CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
[UPDATE() (Transact-SQL)](../../t-sql/functions/update-trigger-functions-transact-sql.md)
  
  
