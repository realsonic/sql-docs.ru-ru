---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dd06b9daff476b26667c0ef5900f34325b05f7c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100894"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает объект «Значение по умолчанию». Если этот объект привязан к столбцу или псевдониму типа данных, он указывает значение, которое должно вставляться в столбец (или во все столбцы псевдонима типа данных), если при вставке значение не задано явно.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте определения по умолчанию, создаваемые с помощью ключевого слова DEFAULT инструкций ALTER TABLE и CREATE TABLE.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*schema_name*  
 Имя схемы, которой принадлежит значение по умолчанию.  
  
*default_name*  
 Имя значения по умолчанию. Имена значений по умолчанию должны соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Указывать имя владельца по умолчанию не обязательно.  
  
*constant_expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md), содержащее только постоянные значения (не может включать имена столбцов или других объектов баз данных). Вы можете использовать любые константы, встроенные функции или математические выражения, за исключением тех, которые содержат типы данных псевдонимов. Определяемые пользователем функции нельзя использовать. Константы символьного типа и даты необходимо заключать в одинарные кавычки (**'**). Константы, имеющие тип денежных данных, а также целочисленные и с плавающей точкой в кавычки не заключаются. Двоичные данные должны сопровождаться знаком 0x, а денежные данные — знаком доллара ($). Тип значения по умолчанию должен соответствовать типу данных столбца.  
  
## <a name="remarks"></a>Remarks  
 Имя по умолчанию может быть создано только в текущей базе данных. Внутри базы данных имена по умолчанию должны быть уникальны в схеме. Чтобы созданное значение по умолчанию привязать к типу данных столбца или псевдонима, используйте **sp_bindefault**.  
  
 Если значение по умолчанию не совместимо со столбцом, к которому оно привязывается, то при попытке его вставки в столбец [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует сообщение об ошибке. Например, значение N/A не может быть использовано по умолчанию для столбца, содержащего **числовые** данные.  
  
 Если значение является слишком длинным для столбца, к которому оно привязывается, то происходит его усечение.  
  
 Инструкции CREATE DEFAULT не могут использоваться в одном пакете с другими инструкциями [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Перед созданием нового значения по умолчанию необходимо удалить старое значение с таким же именем, предварительно удалив все его связи с помощью **sp_unbindefault**.  
  
 Каждому столбцу соответствуют значение по умолчанию и связанное с ним правило, причем значение по умолчанию должно соответствовать правилу. Значение по умолчанию, не удовлетворяющее правилу, не будет вставлено, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет выдавать ошибку при каждой попытке подобной вставки.  
  
 При привязке к столбцу значение по умолчанию вставляется при следующих условиях.  
  
-   Значение вставляется неявным образом.  
  
-   При выполнении функции INSERT для вставки значений по умолчанию используются ключевые слова DEFAULT VALUES или DEFAULT.  
  
 Если при создании столбца было указано NOT NULL и не были созданы значения по умолчанию, то при попытке записи в данный столбец будет выдаваться сообщение об ошибке. В следующей таблице представлена связь между фактом существования значения по умолчанию и определением столбца как NULL или NOT NULL. Записи таблицы отображают результаты.  
  
|Определение столбца|Нет записи, значение по умолчанию отсутствует|Нет записи, присвоено значение по умолчанию|Введено NULL, значение по умолчанию отсутствует|Введено NULL, значение по умолчанию|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|NULL|default|NULL|NULL|  
|**NOT NULL**|Ошибка|default|ошибка|error|  
  
 Чтобы переименовать значение по умолчанию, используйте **sp_rename**. Чтобы получить отчет о значении по умолчанию, используйте **sp_help**.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы использовать команду CREATE DEFAULT, пользователь должен обладать разрешением CREATE DEFAULT в текущей базе данных и разрешением ALTER на схему, в которой создается значение по умолчанию.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-simple-character-default"></a>A. Создание простого символьного значения по умолчанию  
 В следующем примере создается символьное значение по умолчанию с именем `unknown`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>Б. Привязка значения по умолчанию  
 При выполнении следующего примера производится привязка созданного в примере A значения по умолчанию. Значение по умолчанию используется в случае, когда в столбце `Phone` таблицы `Contact` нет записи. 
 
 > [!Note] 
 >  Пропуск записи отличается от явного указания значения NULL в инструкции INSERT.  
  
 Выполнение следующей инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] заканчивается сбоем, так как не существует значения по умолчанию `phonedflt`. Данный пример служит только для демонстрационных целей.  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE (Transact-SQL)](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
