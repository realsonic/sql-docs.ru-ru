---
description: KILL STATS JOB (Transact-SQL)
title: KILL STATS JOB (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: c862b3f53654278aeb1a6e69cd30ebb52dca3732
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084698"
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Завершает асинхронное задание обновления статистики в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
KILL STATS JOB job_id   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *job_id*  
 Аргумент job_id, возвращаемый динамическим административным представлением sys.dm_exec_background_job_queue для этого задания.  
  
## <a name="remarks"></a>Комментарии  
 Аргумент job_id не имеет отношения к идентификатору сеанса (session_id) или другим единицам работы, используемым в разных формах оператора KILL.  
  
## <a name="permissions"></a>Разрешения  
 У пользователя должно быть разрешение VIEW SERVER STATE для доступа к сведениям в динамическом административном представлении sys.dm_exec_background_job_queue.  
  
 Разрешения KILL STATS JOB присваиваются по умолчанию членам предопределенных ролей баз данных sysadmin и processadmin и не могут передаваться.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как остановить обновление статистики, связанное с заданием, где *job_id* = `53`.  
  
```sql  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [Статистика](../../relational-databases/statistics/statistics.md)  
  
  
