---
description: APPLOCK_TEST (Transact-SQL)
title: APPLOCK_TEST (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APPLOCK_TEST_TSQL
- APPLOCK_TEST
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- APPLOCK_TEST function
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- testing application locks
ms.assetid: 4ea33d04-f8e9-46ff-ae61-985bd3eaca2c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4babfd8fba4184e255b366b8a949fe195df466de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090059"
---
# <a name="applock_test-transact-sql"></a>APPLOCK_TEST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает сведения о том, может ли быть предоставлена блокировка конкретного ресурса приложения для указанного владельца блокировки без запроса на блокировку. Функция блокировки приложения APPLOCK_TEST действует в текущей базе данных. База данных является областью блокировок приложений.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
APPLOCK_TEST ( 'database_principal' , 'resource_name' , 'lock_mode' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
**'** *database_principal* **'**  
Пользователь, роль или роль приложения, которым могут быть предоставлены разрешения на доступ к объектам базы данных. Для успешного вызова функции вызывающий ее объект должен быть членом предопределенной роли базы данных *database_principal*, dbo или db_owner.
  
**'** *resource_name* **'**  
Имя ресурса блокировки, указанное клиентским приложением. Приложение должно гарантировать уникальность имени ресурса. Указанное имя внутренне хэшируется в значение, которое может быть сохранено в диспетчере блокировок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Аргумент *resource_name* имеет тип **nvarchar(255)** и не имеет значения по умолчанию. Аргумент *resource_name* сравнивается в двоичном режиме с учетом регистра символов независимо от параметров сортировки текущей базы данных.
  
**'** *lock_mode* **'**  
Режим блокировки, который нужно получить для определенного ресурса. Аргумент *lock_mode* имеет тип **nvarchar(32)** и не имеет значения по умолчанию. *lock_mode* может иметь одно из следующих значений: **Shared**, **Update**, **IntentShared**, **IntentExclusive**, **Exclusive**.
  
**'** *lock_owner* **'**  
Владелец блокировки, которая имеет значение *lock_owner* на момент запроса блокировки. Аргумент *lock_owner* имеет тип **nvarchar(32)** и может иметь значение **Transaction** (по умолчанию) или **Session**. Если явно указано значение по умолчанию или **Transaction**, APPLOCK_TEST должно выполняться из транзакции.
  
## <a name="return-types"></a>Типы возвращаемых данных
**smallint**
  
## <a name="return-value"></a>Возвращаемое значение
Значение 0, когда блокировка не может быть предоставлена указанному владельцу, или значение 1, если блокировка может быть предоставлена.
  
## <a name="function-properties"></a>Свойства функции
**Недетерминированная**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Примеры  
Два пользователя (**пользователь А** и **пользователь Б**) с отдельными сеансами запускают приведенную ниже последовательность инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
**Пользователь А** выполняет следующее:
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
Затем **пользователь Б** выполняет следующее:
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
Затем **пользователь А** выполняет следующее:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
Затем **пользователь Б** выполняет следующее:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Затем оба пользователя (**А** и **Б**) выполняют следующее:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>См. также
[APPLOCK_MODE (Transact-SQL)](../../t-sql/functions/applock-mode-transact-sql.md)  
[sp_getapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
