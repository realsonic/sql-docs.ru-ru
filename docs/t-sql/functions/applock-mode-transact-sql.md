---
description: APPLOCK_MODE (Transact-SQL)
title: APPLOCK_MODE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APPLOCK_MODE_TSQL
- APPLOCK_MODE
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- APPLOCK_MODE function
ms.assetid: e43d4917-77f1-45cc-b231-68ba7fee3385
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13df43d6934c00232451c90b236be89cd7644ff2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090259"
---
# <a name="applock_mode-transact-sql"></a>APPLOCK_MODE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает режим блокировки, полученный владельцем блокировки на конкретный ресурс приложения. Функция блокировки приложения APPLOCK_MODE действует в текущей базе данных. База данных является областью блокировок приложений.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
APPLOCK_MODE( 'database_principal' , 'resource_name' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
'*database_principal*'  
Пользователь, роль или роль приложения, которым могут быть предоставлены разрешения на доступ к объектам базы данных. Для успешного вызова функции вызывающий ее объект должен быть членом предопределенной роли базы данных *database_principal*, dbo или db_owner.
  
'*resource_name*'  
Имя ресурса блокировки, указанное клиентским приложением. Приложение должно гарантировать уникальность имени ресурса. Указанное имя внутренне хэшируется в значение, которое может быть сохранено в диспетчере блокировок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Аргумент *resource_name* имеет тип **nvarchar(255)** и не имеет значения по умолчанию. Аргумент *resource_name* сравнивается в двоичном режиме с учетом регистра символов независимо от параметров сортировки текущей базы данных.
  
'*lock_owner*'  
Владелец блокировки, которая имеет значение *lock_owner* на момент запроса блокировки. Аргумент *lock_owner* имеет тип **nvarchar(32)** и может иметь значение **Transaction** (по умолчанию) или **Session**.
  
## <a name="return-types"></a>Типы возвращаемых данных
**nvarchar(32)**
  
## <a name="return-value"></a>Возвращаемое значение
Возвращает режим блокировки, полученный владельцем блокировки на конкретный ресурс приложения. Режим блокировки может принимать следующие значения:
  
||||  
|-|-|-|  
|**NoLock**|**Обновление**|**\*SharedIntentExclusive**|  
|**IntentShared**|**IntentExclusive**|**\*UpdateIntentExclusive**|  
|**Общий**|**Монопольный доступ**||  
  
* Этот режим блокировки представляет собой сочетание других режимов и не может быть явным образом получен процедурой sp_getapplock.
  
## <a name="function-properties"></a>Свойства функции
**Недетерминированная**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Примеры  
Два пользователя (пользователь А и пользователь Б) с отдельными сеансами запускают приведенную ниже последовательность инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
Пользователь А запускает:
  
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
  
Затем пользователь Б запускает:
  
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
  
Затем пользователь А запускает:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
Затем пользователь Б запускает:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Затем оба пользователя (А и Б) запускают:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>См. также
[APPLOCK_TEST (Transact-SQL)](../../t-sql/functions/applock-test-transact-sql.md)  
[sp_getapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
  
  
