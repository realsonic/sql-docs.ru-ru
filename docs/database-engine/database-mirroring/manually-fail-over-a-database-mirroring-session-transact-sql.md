---
title: Переключение зеркальной базы данных на участника
description: Инструкции для ручного переключения зеркальной главной базы данных на вторичного участника с помощью Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- failover [SQL Server], database mirroring
- manual failover [SQL Server]
- database mirroring [SQL Server], failover
ms.assetid: 36218d61-b5f5-4194-905a-608e0e903db4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: aa36232584afbdc58793cb2a4742d5d5227ddb0b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642295"
---
# <a name="manually-fail-over-a-database-mirroring-session-transact-sql"></a>Переключение сеанса зеркального отображения базы данных на другой ресурс вручную (язык Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Когда зеркальная база данных синхронизирована (то есть база данных находится в состоянии SYNCHRONIZED), владелец базы данных может инициировать отработку отказа на зеркальном сервере вручную. Переход на другой ресурс вручную может быть инициирован только с основного сервера.  
  
### <a name="to-manually-fail-over-a-database-mirroring-session"></a>Отработка отказа в сеансе зеркального отображения базы данных вручную  
  
1.  Подключитесь к основному серверу.  
  
2.  Задайте базу данных **master** в качестве контекста базы данных:  
  
     **USE master;**  
  
3.  Введите следующую инструкцию на основном сервере:  
  
     [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) *имя_базы_данных* SET PARTNER FAILOVER, где *имя_базы_данных* — это зеркально отображаемая база данных.  
  
     В результате роль основного сервера немедленно перейдет к зеркальному серверу.  
  
 На бывшем основном сервере произойдет отключение клиентов базы данных с откатом незавершенных транзакций.  
  
> [!NOTE]  
>  Транзакции, подготовленные с помощью координатора распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] , но еще не зафиксированные на момент отработки отказа, после отработки отказа базы данных будут считаться отмененными.  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Переключение сеанса зеркального отображения базы данных на другой ресурс вручную (среда SQL Server Management Studio)](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-sql-server-management-studio.md)   
 [Переключение ролей во время сеанса зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)  
  
  
