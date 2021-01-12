---
description: Инструкции служебных программ SQL Server — GO
title: Инструкции служебных программ SQL Server — GO | Документация Майкрософт
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GO
- GO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- batches [SQL Server], ending
- ending batches [SQL Server]
- GO command
ms.assetid: b2ca6791-3a07-4209-ba8e-2248a92dd738
author: cawrites
ms.author: chadam
ms.openlocfilehash: f09f2dcdd59b0e0431dbdd92a4023a2e3296149f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100934"
---
# <a name="sql-server-utilities-statements---go"></a>Инструкции служебных программ SQL Server — GO
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляются команды, которые не являются инструкциями [!INCLUDE[tsql](../../includes/tsql-md.md)], но распознаются программами **sqlcmd** и **osql**, а также редактором кода в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Эти команды используются для повышения удобочитаемости и упрощения выполнения пакетов и скриптов.  
  
  GO информирует программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] об окончании пакета инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
GO [count]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *count*  
 Целое положительное число. Пакет, предшествующий команде GO, будет выполняться заданное количество раз.  
  
## <a name="remarks"></a>Remarks  
 GO — это не инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)]; это команда, распознаваемая программами **sqlcmd** и **osql**, а также редактором кода среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] интерпретируют команду GO как сигнал о том, что им следует отправить текущий пакет инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пакет инструкций состоит из всех инструкций, введенных за время, прошедшее с момента обработки последней команды GO, или, если данная команда GO является первой, с момента начала нерегламентированного сеанса или скрипта.  
  
 Инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] не может располагаться на той же строке, что и команда GO. Тем не менее строка с командой GO может содержать комментарии.  
  
 При использовании команды GO нужно соблюдать требования, предъявляемые к пакетам. Например, при любом вызове хранимой процедуры после первой инструкции пакета нужно использовать ключевое слово EXECUTE. Область видимости локальных (пользовательских) переменных ограничена пакетом, и к ним нельзя обращаться после команды GO.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyMsg VARCHAR(50)  
SELECT @MyMsg = 'Hello, World.'  
GO -- @MyMsg is not valid after this GO ends the batch.  
  
-- Yields an error because @MyMsg not declared in this batch.  
PRINT @MyMsg  
GO  
  
SELECT @@VERSION;  
-- Yields an error: Must be EXEC sp_who if not first statement in   
-- batch.  
sp_who  
GO  
```  
  
 Приложения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут отправлять экземпляру [!INCLUDE[tsql](../../includes/tsql-md.md)] множественные инструкции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы они были выполнены как пакет. Инструкции пакета компилируются в единый план выполнения. Программисты, выполняющие в программах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нерегламентированные инструкции или составляющие из инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] скрипты для программ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используют команду GO как сигнал об окончании пакета.  
  
 Приложения, основанные на API-интерфейсах ODBC или OLE DB, при попытке выполнить команду GO получают уведомление о синтаксической ошибке. Программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] никогда не отправляют команду GO серверу.  
  
 Не используйте точку с запятой в качестве признака конца инструкции после команды GO.
 
```sql
-- Yields an error because ; is not permitted after GO  
SELECT @@VERSION;  
GO;  
```
  
## <a name="permissions"></a>Разрешения  
 Для выполнения команды GO не требуются какие-либо разрешения. Она может быть выполнена любым пользователем.    
  
## <a name="examples"></a>Примеры  
 В следующем примере создаются два пакета. Первый содержит только инструкцию `USE AdventureWorks2012`, которая задает контекст базы данных. Остальные инструкции выполняют те или иные операции над локальной переменной и должны быть сгруппированы в один пакет. Поэтому следующая команда `GO` указывается только после последней инструкции, в которой используется переменная.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NmbrPeople INT  
SELECT @NmbrPeople = COUNT(*)  
FROM Person.Person;  
PRINT 'The number of people as of ' +  
      CAST(GETDATE() AS CHAR(20)) + ' is ' +  
      CAST(@NmbrPeople AS CHAR(10));  
GO  
```  
  
 В следующем примере инструкции в пакете выполняются дважды.  
  
```sql  
SELECT DB_NAME();  
SELECT USER_NAME();  
GO 2  
```  
  
  
