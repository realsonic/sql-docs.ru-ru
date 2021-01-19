---
description: FORMATMESSAGE (Transact-SQL)
title: FORMATMESSAGE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FORMATMESSAGE
- FORMATMESSAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- sys.messages catalog view
- FORMATMESSAGE function
- messages [SQL Server], formats
- errors [SQL Server], formats
ms.assetid: 83f18102-2035-4a87-acd0-8d96d03efad5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27fa2389740a7b2d15cb019d86ba1d02559ce6d4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172666"
---
# <a name="formatmessage-transact-sql"></a>FORMATMESSAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Форматирует сообщение на основе текста, имеющегося в sys.messages, или предоставленной строки. Функция FORMATMESSAGE работает аналогично инструкции RAISERROR, но в отличие от нее не выводит сообщение немедленно, а возвращает сформированный текст для дальнейшей обработки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FORMATMESSAGE ( { msg_number  | ' msg_string ' | @msg_variable} , [ param_value [ ,...n ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *msg_number*  
 Идентификатор сообщения, хранящегося в sys.messages. Если *msg_number* <= 13000 или сообщение отсутствует в sys.messages, возвращается значение NULL.  
  
 *msg_string*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Строка, заключенная в одинарные кавычки и содержащая заполнители значений параметров. Это сообщение об ошибке не должно содержать более 2 047 символов. Если сообщение содержит 2 048 и более символов, то отображаются только первые 2 044, а за ними появляется знак многоточия, показывающий, что сообщение было усечено. Обратите внимание, что параметры подстановки содержат больше символов, чем видно на выходе из-за внутренней структуры хранения.  Сведения о структуре сообщения и использовании параметров в строке см. в описании аргумента *msg_str* в статье [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md).  

 *@msg_variable*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Переменная типа nvarchar или varchar, которая содержит строку, удовлетворяющую критериям для *msg_string* выше.  
  
 *param_value*  
 Значение параметра для включения в текст сообщения. Может быть указано несколько значений, при этом они должны быть перечислены в том же порядке, в котором заполнители присутствуют в сообщении. Допускается не более 20 значений.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **nvarchar**  
  
## <a name="remarks"></a>Remarks  
 Как и инструкция RAISERROR, функция FORMATMESSAGE производит формирование сообщения, подставляя указанные значения параметров вместо заполнителей. Дополнительные сведения о заполнителях, которые допускаются в сообщениях об ошибках, а также о процессе их изменения см. в статье [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md).  
  
 Она находит сообщение для текущего языка, установленного для пользователя. В случае системных сообщений (*msg_number* <= 50 000) при отсутствии локализованной версии сообщения используется языковая версия ОС. В случае пользовательских сообщений (*msg_number* > 50 000) при отсутствии локализованной версии сообщения используется английская версия.
  
 Для локализованных сообщений указанные параметры должны соответствовать заполнителям в версиях для языка "Английский (США)". Таким образом, параметр 1 в локализованной версии должен соответствовать параметру 1 в версии для языка "Английский (США)", параметр 2 — параметру 2 и т. д.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-example-with-a-message-number"></a>A. Пример с номером сообщения  
 В приведенном ниже примере используется сообщение репликации `20009`, которое хранится в sys.messages в виде "Статью "%s" не удалось добавить в публикацию "%s"". Функция FORMATMESSAGE подставляет вместо заполнителей параметров значения `First Variable` и `Second Variable`. Результирующая строка "Статью "Первая переменная" не удалось добавить в публикацию "Вторая переменная"." сохраняется в локальной переменной `@var1`.  
  
```sql
SELECT text FROM sys.messages WHERE message_id = 20009 AND language_id = 1033;  
DECLARE @var1 VARCHAR(200);   
SELECT @var1 = FORMATMESSAGE(20009, 'First Variable', 'Second Variable');   
SELECT @var1;  
```  
  
### <a name="b-example-with-a-message-string"></a>Б. Пример со строкой сообщения  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 В приведенном ниже примере в качестве входных данных принимается строка.  
  
```sql
SELECT FORMATMESSAGE('This is the %s and this is the %s.', 'first variable', 'second variable') AS Result;  
```  
  
 Возвращаемый результат: `This is the first variable and this is the second variable.`  
  
### <a name="c-additional-message-string-formatting-examples"></a>В. Дополнительные примеры форматирования строки сообщения  
 В приведенных ниже примерах показаны различные параметры форматирования.  
  
```sql
SELECT FORMATMESSAGE('Signed int %i, %d %i, %d, %+i, %+d, %+i, %+d', 5, -5, 50, -50, -11, -11, 11, 11);
SELECT FORMATMESSAGE('Signed int with up to 3 leading zeros %03i', 5);  
SELECT FORMATMESSAGE('Signed int with up to 20 leading zeros %020i', 5);  
SELECT FORMATMESSAGE('Signed int with leading zero 0 %020i', -55);  
SELECT FORMATMESSAGE('Bigint %I64d', 3000000000);
SELECT FORMATMESSAGE('Unsigned int %u, %u', 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal %o, %o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal %x, %X, %X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal with prefix: %#o, %#o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal with prefix: %#x, %#X, %#X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Hello %s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %20s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %-20s!', 'TEST');  
```  
  
## <a name="see-also"></a>См. также:  
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)  
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
  
  
