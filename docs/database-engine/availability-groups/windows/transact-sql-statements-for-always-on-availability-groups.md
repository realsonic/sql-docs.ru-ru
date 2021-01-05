---
title: Инструкции Transact-SQL для групп доступности
description: Содержит общие сведения об инструкциях Transact-SQL (T-SQL), поддерживающих развертывание, создание групп доступности Always On и управление ими.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: 184d0a81-2259-4db9-9d0d-01aac0b502c8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 54f521c62ebae90a36ccbd315dc8d04a9ec7c61e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642949"
---
# <a name="transact-sql-statements-for-always-on-availability-groups"></a>Инструкции Transact-SQL для групп доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе приведены общие сведения об инструкциях [!INCLUDE[tsql](../../../includes/tsql-md.md)] , которые поддерживают развертывание [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , а также создание и управление данной группой доступности, репликой доступности и базой данных доступности.  
  
 
##  <a name="create-endpoint"></a><a name="CreateEndpoint"></a> CREATE ENDPOINT  
 Инструкция [CREATE ENDPOINT ... FOR DATABASE_MIRRORING](../../../t-sql/statements/create-endpoint-transact-sql.md) создает конечную точку зеркального отображения базы данных, если таковая не существует на экземпляре сервера. Для каждого экземпляра сервера, на котором намечено развертывание [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] или зеркального отображения базы данных, требуется конечная точка зеркального отображения базы данных.  
  
 Эта инструкция выполняется на экземпляре сервера, на котором создается конечная точка. На каждом конкретном экземпляре сервера можно создать только одну конечную точку зеркального отображения базы данных. Дополнительные сведения см. в разделе [Конечная точка зеркального отображения базы данных (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
##  <a name="create-availability-group"></a><a name="CreateAG"></a> CREATE AVAILABILITY GROUP  
 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) создает новую группу доступности и, при необходимости, прослушиватель группы доступности. Как минимум необходимо указать экземпляр локального сервера, который станет начальной первичной репликой. Дополнительно можно указать до четырех вторичных реплик.  
  
 Выполните CREATE AVAILABILITY GROUP в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], на котором должна размещаться начальная первичная реплика создаваемой группы доступности. Этот экземпляр сервера должен находиться на узле отказоустойчивого кластера WSFC. Дополнительные сведения см. в статье [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="alter-availability-group"></a><a name="AlterAG"></a> ALTER AVAILABILITY GROUP  
 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) поддерживает изменение существующей группы доступности или прослушивателя группы доступности для перехода на группу доступности.  
  
 Выполните ALTER AVAILABILITY GROUP в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором размещается текущая первичная реплика.  
  
##  <a name="alter-database--set-hadr-"></a><a name="AlterDb"></a> ALTER DATABASE ... SET HADR ...  
 Параметры предложения [SET HADR](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) инструкции ALTER DATABASE позволяют присоединить базу данных-получателя к группе доступности соответствующей базы данных-источника, удалить присоединенную базу данных, отложить синхронизацию данных в присоединенной базе данных, а также возобновить синхронизацию данных.  
  
##  <a name="drop-availability-group"></a><a name="DropAG"></a> DROP AVAILABILITY GROUP  
 Инструкция[DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md) удаляет указанную группу доступности и все ее реплики. Инструкция DROP AVAILABILITY GROUP может быть запущена с любого узла [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в отказоустойчивом кластере WSFC.  
  
##  <a name="restrictions-on-the-availability-group-transact-sql-statements"></a><a name="Restrictions"></a> Ограничения на инструкции AVAILABILITY GROUP языка Transact-SQL  
 Инструкции CREATE AVAILABILITY GROUP, ALTER AVAILABILITY GROUP и DROP AVAILABILITY GROUP [!INCLUDE[tsql](../../../includes/tsql-md.md)] имеют следующие ограничения.  
  
-   За исключением DROP AVAILABILITY GROUP, для выполнения этих инструкций требуется, чтобы была включена служба HADR на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [Включение и отключение групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
-   Эти инструкции не могут выполняться в пределах транзакций или пакетов.  
  
-   Несмотря на то, что эти инструкции предназначены для восстановления после сбоя, они не гарантируют выполнения отката всех изменений при сбое. Однако системы должны быть способны четко выполнять обработку и пропуск частичных сбоев.  
  
-   Эти инструкции не поддерживают выражения или переменные.  
  
-   Если во время действия группы доступности или восстановления выполняется инструкция [!INCLUDE[tsql](../../../includes/tsql-md.md)] , эта инструкция возвращает ошибку. В случае необходимости дождитесь завершения действия или восстановления, а затем повторно выполните инструкцию.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
