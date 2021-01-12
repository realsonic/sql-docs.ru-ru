---
description: CREATE RULE (Transact-SQL)
title: CREATE RULE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RULE_TSQL
- CREATE RULE
- CREATE_RULE_TSQL
- RULE
dev_langs:
- TSQL
helpviewer_keywords:
- list rules [SQL Server]
- unbinding rules
- pattern rules [SQL Server]
- range rules [SQL Server]
- alias data types [SQL Server], rules
- CREATE RULE statement
- reports [SQL Server], rules
- status information [SQL Server], rules
- rules [SQL Server], precedence
- binding rules [SQL Server]
- rules [SQL Server], creating
ms.assetid: b016a289-3a74-46b1-befc-a13183be51e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fc137ea5a6c2b48a523f758619c129133b6a4eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091989"
---
# <a name="create-rule-transact-sql"></a>CREATE RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает объект, называемый правилом. Будучи привязанным к столбцу, имеющему псевдоним типа данных, правило определяет значения, которые могут быть вставлены в этот столбец.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этой инструкции рекомендуется применять проверку ограничений. Эти ограничения создаются при помощи ключевого слова CHECK инструкции CREATE TABLE или ALTER TABLE. Дополнительные сведения см. в статье [Ограничения уникальности и проверочные ограничения](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 К столбцу, имеющему псевдоним типа данных, можно привязать только одно правило. Однако, кроме правила, со столбцом может быть связано одно или несколько ограничений CHECK. Если это так, соблюдаются все ограничивающие условия.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE RULE [ schema_name . ] rule_name   
AS condition_expression  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы, к которой относится правило.  
  
 *rule_name*  
 Имя нового правила. Имена правил должны соответствовать требованиям, предъявляемым к [идентификаторам](../../relational-databases/databases/database-identifiers.md). Указывать имя владельца правила необязательно.  
  
 *condition_expression*  
 Условие или условия, определяющие правило. Правило может быть любым выражением, допустимым в предложении WHERE, и может включать такие элементы, как арифметические операторы, операторы отношений и предикаты (например, IN, LIKE и BETWEEN). Правило не может ссылаться на столбцы или другие объекты базы данных. В правило могут входить встроенные функции, не ссылающиеся на объекты базы данных. Определяемые пользователем функции использовать в правилах нельзя.  
  
 *condition_expression* содержит одну переменную. Каждой локальной переменной предшествует знак **@** . Выражение соответствует значению, введенному при помощи инструкции UPDATE или INSERT. Для представления значения при создании правила можно использовать любое имя или символ, но первым знаком должен быть знак **@** .  
  
> [!NOTE]  
>  Не следует создавать правила с выражениями, в которых используются псевдонимы типа данных. Хотя создание таких правил и возможно, но после привязки правил к столбцам или псевдониму типа данных эти выражения компилироваться не будут.  
  
## <a name="remarks"></a>Комментарии  
 Инструкцию CREATE RULE нельзя объединять с другими инструкциями [!INCLUDE[tsql](../../includes/tsql-md.md)] в одном пакете. Правила не распространяются на данные, существовавшие в базе данных на момент создания правил, и не могут быть привязаны к системным типам данных.  
  
 Правило может быть создано только в текущей базе данных. После создания правила необходимо выполнить хранимую процедуру **sp_bindrule** для привязки правила к столбцу или псевдониму типа данных. Правило должно быть совместимо с типом столбца. Например, правило "\@value LIKE A%" не может быть привязано к числовому столбцу. Правило не может быть привязано к **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, определяемому пользователем типу CLR или столбцу **timestamp**. Правило не может быть привязано к вычисляемому столбцу.  
  
 Символьные константы и константы-даты следует заключать в одиночные кавычки ('), а двоичные константы — предварять знаками 0x. Если правило несовместимо со столбцом, к которому оно привязано, компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] возвращает сообщение об ошибке при попытке вставки значения в столбец, но не во время привязки правила.  
  
 Правило, привязанное к псевдониму типа данных, активируется только при попытке вставить значение в столбец типа данных псевдонима или обновить такой столбец. Переменные в правилах не проверяются, поэтому не следует присваивать переменным псевдонима типа данных значения, которые были бы отклонены правилом, привязанным к столбцу такого же типа.  
  
 Для получения отчета о правиле следует использовать хранимую процедуру **sp_help**. Для отображения текста правила выполните процедуру **sp_helptext** с именем правила в качестве аргумента. Для переименования правила следует использовать хранимую процедуру **sp_rename**.  
  
 Перед созданием нового правила с именем уже существующего старое правило нужно сбросить с помощью инструкции DROP RULE, а перед сбрасыванием правила нужно отменить его привязку с помощью хранимой процедуры **sp_unbindrule**. Для отмены привязки правила к столбцу следует использовать процедуру **sp_unbindrule**.  
  
 Можно привязать к столбцу или типу данных новое правило без отмены привязки предыдущего правила; в этом случае старое правило будет переопределено новым. Правила, привязанные к столбцам, всегда имеют больший приоритет, чем правила, привязанные к псевдонимам типа данных. Привязка правила к столбцу приводит к замене правила, уже привязанного к псевдониму типа данных, соответствующему данному столбцу. Однако привязка правила к типу данных не заменяет правило, привязанное к столбцу, имеющему этот псевдоним типа данных. Следствия привязки правил к столбцам и псевдонимам типа данных, к которым уже привязаны правила, поясняет следующая таблица.  
  
|Новое правило привязано к|Старое правило привязано к<br /><br /> псевдоним типа данных|Старое правило привязано к<br /><br /> Столбец|  
|-----------------------|-------------------------------------------|----------------------------------|  
|Псевдоним типа данных|Старое правило заменяется|Без изменения.|  
|Столбец|Старое правило заменяется|Старое правило заменяется|  
  
 Если столбец имеет и значение по умолчанию, и привязанное к нему правило, значение по умолчанию должно попадать в диапазон, определяемый правилом. Значение по умолчанию, конфликтующее с правилом, никогда не вставляется в столбец. При попытке вставить в столбец такое значение по умолчанию ядро СУБД SQL Server формирует сообщение об ошибке.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения инструкции CREATE RULE необходимо как минимум разрешение CREATE RULE, связанное с текущей базой данных, и разрешение ALTER, связанное со схемой, в которой создается правило.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-rule-with-a-range"></a>A. Создание правила с диапазоном  
 Следующий код создает правило, ограничивающее диапазон целых чисел, которые могут быть вставлены в столбец или столбцы, связанные с данным правилом.  
  
```sql  
CREATE RULE range_rule  
AS   
@range>= $1000 AND @range <$20000;  
```  
  
### <a name="b-creating-a-rule-with-a-list"></a>Б. Создание правила со списком  
 Следующий код создает правило, ограничивающее значения, вставляемые в столбец или столбцы (к которым привязано данное правило) только теми значениями, которые указаны в правиле.  
  
```sql  
CREATE RULE list_rule  
AS   
@list IN ('1389', '0736', '0877');  
```  
  
### <a name="c-creating-a-rule-with-a-pattern"></a>В. Создание правила с шаблоном  
 Следующий код создает правило, позволяющее вставлять в столбец только значения, начинающиеся на два любых символа, за которыми следуют дефис (`-`), любое число символов или не следует никаких символов, и завершающиеся целым числом из диапазона от `0` до `9`.  
  
```sql  
CREATE RULE pattern_rule   
AS  
@value LIKE '__-%[0-9]'  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE (Transact-SQL)](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
