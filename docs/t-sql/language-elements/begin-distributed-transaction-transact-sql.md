---
description: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
title: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/29/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DISTRIBUTED
- BEGIN_DISTRIBUTED_TRANSACTION_TSQL
- DISTRIBUTED_TSQL
- BEGIN DISTRIBUTED TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN DISTRIBUTED TRANSACTION statement
- distributed transactions [SQL Server], starting
- MS DTC, transaction management
- distributed transactions [SQL Server], BEGIN DISTRIBUTED TRANSACTION statement
- servers [SQL Server], distributed transactions
- starting distributed transactions
- remote servers [SQL Server], distributed transactions
- starting transactions
ms.assetid: c3bc2716-39d3-4061-8c6a-8734899231ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: 15f3221d2e7848a64f3ba22d618bdc54927afeba
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096007"
---
# <a name="begin-distributed-transaction-transact-sql"></a>BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Запускает распределенную транзакцию на языке [!INCLUDE[tsql](../../includes/tsql-md.md)], управляемую координатором распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC).  
    
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BEGIN DISTRIBUTED { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *transaction_name*  
 Имя определяемой пользователем транзакции, используемое для отслеживания распределенной транзакции в программах MS DTC. Аргумент *transaction_name* должен соответствовать правилам для идентификаторов и быть \<= 32 символам по длине.  
  
 @*tran_name_variable*  
 Имя пользовательской переменной, содержащей имя транзакции, которая используется для отслеживания распределенной транзакции в служебных программах координатора MS DTC. Переменная должна быть объявлена с типом данных **char**, **varchar**, **nchar** или **nvarchar**.  
  
## <a name="remarks"></a>Комментарии  
 Экземпляр компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], на котором выполняется инструкция BEGIN DISTRIBUTED TRANSACTION, является инициатором транзакции, контролирующим ее завершение. Если после этого в сеансе выполняется инструкция COMMIT TRANSACTION или ROLLBACK TRANSACTION, управляющий экземпляр передает координатору MS DTC управление распределенной транзакцией во всех экземплярах.  
  
 Изоляция моментального снимка уровня транзакции не поддерживает распределенные транзакции.  
  
 Основным способом прикрепления удаленных экземпляров компонентов [!INCLUDE[ssDE](../../includes/ssde-md.md)] к распределенной транзакции является выполнение распределенного запроса, ссылающегося на связанный сервер, в сеансе, который уже прикреплен к распределенной транзакции.  
  
 Например, если инструкция BEGIN DISTRIBUTED TRANSACTION выполняется на сервере А, сеанс вызывает хранимую процедуру на сервере Б и еще одну хранимую процедуру на сервере В. Хранимая процедура на сервере В выполняет распределенный запрос к серверу Г, и в результате в распределенную транзакцию включаются все четыре сервера. Экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] на сервере А является автором транзакции и управляет ею.  
  
 Сеансы, вовлеченные в распределенную транзакцию на языке [!INCLUDE[tsql](../../includes/tsql-md.md)], не получают объект транзакции, который они могли бы передать другому сеансу, чтобы явным образом прикрепить его к распределенной транзакции. Единственный способ, при помощи которого удаленный сервер можно прикрепить к распределенной транзакции, — это представить его в качестве целевого сервера для распределенного запроса или вызова удаленной хранимой процедуры.  
  
 Если в локальной транзакции выполняется распределенный запрос, ее уровень автоматически повышается до распределенной при условии, что целевой источник данных OLE DB поддерживает интерфейс ITransactionLocal. Если целевой источник данных OLE DB не поддерживает интерфейс ITransactionLocal, в распределенном запросе разрешаются только операции чтения.  
  
 Сеанс, уже прикрепленный к распределенной транзакции, выполняет удаленную хранимую процедуру, ссылаясь на удаленный сервер.  
  
 Параметр **sp_configure remote proc trans** управляет тем, будет ли уровень локальной транзакции, вызывающей удаленные хранимые процедуры, автоматически повышаться до распределенной транзакции, управляемой координатором MS DTC. Параметр SET уровня подключения REMOTE_PROC_TRANSACTIONS может использоваться для переопределения значения по умолчанию, заданного процедурой **sp_configure remote proc trans** для экземпляра. Если этот параметр включен, вызов удаленной хранимой процедуры приводит к повышению уровня локальной транзакции до распределенной транзакции. Соединение, создающее транзакцию координатора MS DTC, становится инициатором этой транзакции. Инструкция COMMIT TRANSACTION фиксирует транзакцию, управляемую координатором MS DTC. Если параметр **sp_configure remote proc trans** включен (ON), вызовы удаленных хранимых процедур в локальной транзакции автоматически защищаются как часть распределенной транзакции. При этом не требуется переписывать приложения с целью замены инструкций BEGIN TRANSACTION на BEGIN DISTRIBUTED TRANSACTION.  
  
 Дополнительные сведения о среде и обработке распределенных транзакций см. в документации по координатору распределенных транзакций ([!INCLUDE[msCoName](../../includes/msconame-md.md)]).  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
 В этом примере удаляется кандидат из базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] как на локальном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], так и на удаленном сервере. И локальная, и удаленная база данных зафиксирует транзакцию или выполнит ее откат.  
  
> [!NOTE]  
>  Этот пример приводит к сообщению об ошибке, если координатор MS DTC не установлен на компьютере, где выполняется экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Дополнительные сведения об установке координатора MS DTC см. в документации по координатору распределенных транзакций Microsoft.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN DISTRIBUTED TRANSACTION;  
-- Delete candidate from local instance.  
DELETE AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
-- Delete candidate from remote instance.  
DELETE RemoteServer.AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
