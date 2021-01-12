---
description: OPEN (Transact-SQL)
title: OPEN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPEN_TSQL
- OPEN
dev_langs:
- TSQL
helpviewer_keywords:
- opening cursors
- cursors [SQL Server], opening
- populating cursors [SQL Server]
- OPEN statement
- Transact-SQL cursors, opening
ms.assetid: fd1c5e3b-6045-4a42-b646-3fca76e874c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: c0cab0ea4ed5eecc224c478018c16bf9d078d02f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102406"
---
# <a name="open-transact-sql"></a>OPEN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Открывает серверный курсор языка [!INCLUDE[tsql](../../includes/tsql-md.md)] и заполняет его с помощью инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], определенной в инструкции DECLARE CURSOR или SET *cursor_variable*.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
OPEN { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 GLOBAL  
 Указывает, что аргумент *cursor_name* ссылается на глобальный курсор.  
  
 *cursor_name*  
 Имя объявленного курсора. Когда имеется как глобальный, так и локальный курсор с именем *cursor_name*, то *cursor_name* относится к глобальному курсору, если задано GLOBAL% в противном случае *cursor_name* относится к локальному курсору.  
  
 *cursor_variable_name*  
 Имя переменной курсора, которая ссылается на курсор.  
  
## <a name="remarks"></a>Примечания  
 Если курсор объявлен с параметром INSENSITIVE или STATIC, инструкция OPEN создает временную таблицу для хранения результирующего набора. Инструкция OPEN завершается ошибкой в случае, если размер любой строки в результирующем наборе превышает максимальный разрешенный размер строки для таблиц [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если курсор объявлен с параметром KEYSET, инструкция OPEN создает временную таблицу для хранения набора ключей. Временные таблицы хранятся в базе данных tempdb.  
  
 После того как курсор открыт, используйте функцию @@CURSOR_ROWS для получения количества выбранных строк в последнем открытом курсоре.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает асинхронное формирование управляемых набором ключей или статических курсоров [!INCLUDE[tsql](../../includes/tsql-md.md)] . [!INCLUDE[tsql](../../includes/tsql-md.md)] (такие как OPEN или FETCH) пакетированы, поэтому нет необходимости в асинхронном формировании курсоров [!INCLUDE[tsql](../../includes/tsql-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] продолжает поддерживать асинхронные курсоры, управляемые набором ключей, а также статические серверные курсоры API. При этом выполнение инструкции OPEN производится с небольшой задержкой, так как при каждой операции над курсором клиент осуществляет обмен данными с сервером.  
  
## <a name="examples"></a>Примеры  
 В следующем примере открывается курсор и выбираются все его строки:  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN Employee_Cursor;  
  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM Employee_Cursor  
END;  
  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
```  
  
## <a name="see-also"></a>См. также  
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [@@CURSOR_ROWS (Transact-SQL)](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [DEALLOCATE (Transact-SQL)](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
