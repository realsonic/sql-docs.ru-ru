---
description: Косая черта-звездочка (блочный комментарий) (Transact-SQL)
title: Косая черта-звездочка (блочный комментарий) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- /*...*/_TSQL
- Comment
- /*...*/
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- /*...*/ (comment)
- remarks [SQL Server]
- comments [SQL Server]
ms.assetid: 4d9ab1b2-4bbb-4c16-beb1-cafc1af7417c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00dbdd9a6fb7641c00b57eff996bf94b9b683aa3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096985"
---
# <a name="slash-star-block-comment-transact-sql"></a>Косая черта-звездочка (блочный комментарий) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]


  Обозначает текст комментария пользователя. Текст, помещенный между /* и \*/, не вычисляется сервером.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
/*  
text_of_comment  
*/  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *text_of_comment*  
 Текст комментария. Это одна или более символьных строк.  
  
## <a name="remarks"></a>Комментарии  
 Комментарии могут вставляться в отдельную строку или в пределах инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Многострочные комментарии необходимо отмечать сочетаниями символов /* и \*/. Для многострочных комментариев часто используется следующий стиль: первую строку начинают с сочетания символов /\*, последующие строки — с сочетания символов \*\*, а заканчивают комментарий сочетанием символов \*/.  
  
 Длина комментариев не ограничена.  
  
 Поддерживаются вложенные комментарии. Если сочетание символов /* используется в пределах существующего комментария, оно рассматривается как начало вложенного комментария и, следовательно, требует метки \*/, закрывающей этот комментарий. Если метки, закрывающей комментарий, нет, выдается ошибка.  
  
 Например, следующий код вызовет ошибку:  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/   
SELECT @@VERSION;  
GO   
```  
  
 Чтобы избежать этой ошибки, внесите следующее изменение:  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/ */  
SELECT @@VERSION;  
GO  
```  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере комментарии используются для пояснения действий, выполняемых блоком кода.  
  
```sql  
USE AdventureWorks2012;  
GO  
/*  
This section of the code joins the Person table with the Address table,   
by using the Employee and BusinessEntityAddress tables in the middle to   
get a list of all the employees in the AdventureWorks2012 database   
and their contact information.  
*/  
SELECT p.FirstName, p.LastName, a.AddressLine1, a.AddressLine2, a.City, a.PostalCode  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e ON p.BusinessEntityID = e.BusinessEntityID   
JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
JOIN Person.Address AS a ON ea.AddressID = a.AddressID;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [-- (комментарий) (Transact-SQL)](../../t-sql/language-elements/comment-transact-sql.md)   
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)  
  
  

