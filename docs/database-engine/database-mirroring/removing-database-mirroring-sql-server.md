---
title: Удаление зеркального отображения базы данных (SQL Server) | Документы Майкрософт
description: Узнайте о влиянии остановки сеанса зеркального отображения базы данных, которую владелец базы данных может выполнять в любое время на любом участнике в SQL Server.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- stopping database mirroring [SQL Server]
- removing database mirroring [SQL Server]
ms.assetid: 40c72091-8f03-4037-8b55-5e95309fe145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5b921f442c84d94615655b7aeeea2686775d33cf
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641320"
---
# <a name="removing-database-mirroring-sql-server"></a>Удаление зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Владелец базы данных может в любое время и на любом из участников вручную остановить сеанс зеркального отображения базы данных.  
  
## <a name="impact-of-removing-mirroring"></a>Последствия удаления зеркального отображения  
 При удалении зеркального отображения происходит следующее:  
  
-   Прерывается связь между участниками, а также между каждым из участников и следящим сервером, если такая связь существует.  
  
     Если участники в момент остановки сеанса обмениваются данными друг с другом, их связь немедленно обрывается на обоих компьютерах. Если участники не обмениваются данными (база данных во время остановки находится в состоянии DISCONNECTED), связь немедленно обрывается на участнике, с которого останавливается зеркальное отображение. Когда другой участник пытается восстановить соединение, он обнаруживает, что сеанс зеркального отображения завершен.  
  
-   Удаляются сведения о сеансе зеркального отображения (в этом заключается отличие от приостановки сеанса). Зеркальное отображение удаляется и на основной, и на зеркальной базе данных. В представлении **sys.databases** столбец **mirroring_state** и все остальные столбцы зеркального отображения получают значение NULL. Дополнительные сведения см. в разделе [sys.database_mirroring (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   На каждом из экземпляров серверов-партнеров остается собственная копия базы данных.  
  
-   Зеркальная база данных остается в состоянии RESTORING (см. столбец **state** в представлении **sys.databases**), так как зеркальная база данных создавалась с помощью RESTORE WITH NORECOVERY. В этот момент можно удалить бывшую зеркальную базу данных или восстановить ее с параметром WITH RECOVERY. Если база данных восстанавливается, она будет иметь расхождения с бывшей основной базой данных, так как восстановление начинает новую вилку восстановления.  
  
> [!NOTE]  
>  Чтобы продолжить зеркальное отображение после остановки сеанса, необходимо установить новый сеанс зеркального отображения базы данных. Если резервная копия журнала создана после остановки зеркального отображения, то перед возобновлением зеркального отображения ее необходимо применить к зеркальной базе данных.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Удаление зеркального отображения базы данных**  
  
-   [Удаление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
 **Запуск зеркального отображения базы данных**  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Приостановка и возобновление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
  
