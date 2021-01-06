---
title: Переход на вторичный сервер доставки журналов
description: Узнайте, как выполнить переход на вторичный сервер доставки журналов SQL Server с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- primary databases [SQL Server]
- secondary data files [SQL Server], manual fail over
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: edfe5d59-4287-49c1-96c9-dd56212027bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 19302cafdd7619107684d39fb7e84f0e5540a30d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643958"
---
# <a name="fail-over-to-a-log-shipping-secondary-sql-server"></a>Переход на вторичный сервер доставки журналов (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Переход на вторичный сервер доставки журналов может быть полезен в случаях, когда происходит сбой экземпляра сервера-источника или требуется его обслуживание.  
  
## <a name="preparing-for-a-controlled-failover"></a>Подготовка к управляемой отработке отказа  
 Обычно базы данных-источник и получатель не синхронизированы, так как обновление первой продолжается и после завершения последнего задания резервного копирования. Также в некоторых случаях последние резервные копии журнала транзакций не скопированы на экземпляры сервера-получателя или же некоторые из этих копий еще не применены к базе данных-получателю. Рекомендуется начать с синхронизации всех баз данных-получателей с базой данных-источником, если это возможно.  
  
 Сведения о заданиях доставки журналов см. в разделе [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="failing-over"></a>Переход на другой ресурс  
 Для перехода на базу данных-получатель:  
  
1.  Скопируйте все нескопированные файлы резервных копий из ресурса резервных копий в папку назначения на каждом из серверов-получателей.  
  
2.  Примените все непримененные резервные копии журнала транзакций последовательно к каждой из баз данных-получателей. Дополнительные сведения см. в разделе [Применение резервных копий журналов транзакций (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
3.  Если база данных-источник доступна, выполните резервное копирование активного журнала транзакций и примените полученную копию к базам данных-получателям. Может потребоваться задать для базы данных [однопользовательский режим](../../relational-databases/databases/set-a-database-to-single-user-mode.md), чтобы получить монопольный доступ перед выполнением команды восстановления, а затем снова переключиться на несколько пользователей после завершения восстановления.  
  
     Если исходный экземпляр сервера-источника не поврежден, выполните резервное копирование заключительного фрагмента журнала транзакций базы данных-источника с параметром WITH NORECOVERY. Это действие оставляет базу данных в состоянии восстановления из копии и, следовательно, недоступной для пользователей. Со временем можно будет выполнить накат этой базы данных путем применения резервных копий журнала транзакций из заменяющей базы данных-источника.  
  
     Дополнительные сведения см. в разделе [Резервные копии журналов транзакций (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).   
  
4.  После синхронизации серверов-получателей можно выполнить переход на любой из них путем восстановления его базы данных-получателя и перенаправления клиентов на этот экземпляр сервера. При восстановлении база данных помещается в согласованное состояние и переводится в режим в сети.  
  
    > [!NOTE]  
    >  При переводе базы данных-получателя в доступный режим следует убедиться, что ее метаданные согласованы с метаданными исходной базы данных-источника. Дополнительные сведения см. в статье [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
5.  После восстановления базы данных-получателя можно перенастроить ее для работы в качестве базы данных-источника для других баз данных-получателей.  
  
     Если нет другой доступной базы данных-получателя, см. раздел [Настройка доставки журналов (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Обмен ролями между сервером-источником и сервером-получателем доставки журналов (SQL Server)](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md)  
  
-   [Управление именами входа и заданиями после переключения ролей (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Таблицы доставки журналов и хранимые процедуры](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)   
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Резервные копии заключительного фрагмента журнала (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  
  
