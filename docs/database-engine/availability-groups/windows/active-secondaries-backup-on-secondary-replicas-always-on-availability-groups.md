---
title: Перенос резервных копий на вторичную реплику группы доступности
description: Сведения о типах резервных копий, поддерживающих перенос во вторичную реплику группы доступности Always On.
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 82afe51b-71d1-4d5b-b20a-b57afc002405
author: cawrites
ms.author: chadam
ms.openlocfilehash: 79972c5670370c28404b5cc93d7c3b833551f988
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644325"
---
# <a name="offload-supported-backups-to-secondary-replicas-of-an-availability-group"></a>Перенос поддерживаемых резервных копий во вторичные реплики группы доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Функции [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] по поддержке вторичных реплик обеспечивают выполнение резервного копирования на вторичных репликах. Операции резервного копирования могут оказывать значительную нагрузку на систему ввода-вывода и ЦП (при использовании сжатия резервных копий). Перенос резервного копирования в синхронизированную или синхронизирующуюся вторичную реплику позволяет использовать ресурсы на экземпляре сервера, где размещается первичная реплика, для рабочей нагрузки первого уровня.  

> [!NOTE]  
>  Инструкция RESTORE недопустима для базы данных-источника или базы данных-получателя группы доступности.  
  
 
##  <a name="backup-types-supported-on-secondary-replicas"></a><a name="SupportedBuTypes"></a> Поддерживаемые типы резервного копирования на вторичных репликах  
  
-   **BACKUP DATABASE** поддерживает только полные, доступные только для копирования резервные копии баз данных, файлов и файловых групп, которые выполняются во вторичных репликах. Резервные копии, доступные только для копирования, не влияют на цепочку журналов и не очищают битовую карту разностного резервного копирования.  
  
-   Разностные резервные копии не поддерживаются во вторичных репликах.

-   Одновременные резервные копии, такие как выполнение резервного копирования журнала транзакций на первичной реплике, в то время как полное резервное копирование базы данных выполняется на вторичной реплике, в настоящее время не поддерживаются. 
  
-   **BACKUP LOG** поддерживает только обычное резервное копирование журналов (параметр COPY_ONLY не поддерживается для резервных копий журналов во вторичных репликах).  
  
     Обеспечивается последовательная цепочка журналов по всем резервным копиям журналов в любой реплике (первичной или вторичной) независимо от их режима доступности (синхронной или асинхронной фиксации).  
  
-   Для выполнения резервного копирования базы данных-получателя вторичная реплика должна быть способна обмениваться данными с первичной репликой и находиться в состоянии **SYNCHRONIZED** или **SYNCHRONIZING**.  

В распределенной группе доступности резервное копирование может выполняться во вторичных репликах в той же группе доступности, что и активная первичная реплика, или в первичной реплике любой вторичной группы доступности. Резервное копирование не может выполняться во вторичной реплике вторичной группы доступности, так как вторичные реплики могут обмениваться данными только с первичной репликой в их собственной группе доступности. Выполнять операции резервного копирования могут только реплики, которые взаимодействуют с глобальной первичной репликой напрямую.

##  <a name="configuring-where-backup-jobs-run"></a><a name="WhereBuJobsRun"></a> Настройка места выполнения заданий резервного копирования  
 Выполнение резервного копирования во вторичной реплике для снятия рабочей нагрузки резервного копирования с основного рабочего сервера обеспечивает значительные преимущества. Однако выполнение резервного копирования на вторичных репликах создает значительные сложности в процессе определения, где должны запускаться задания резервного копирования. Для решения этой проблемы настройте расположение запуска заданий резервного копирования, как описано далее.  
  
1.  Настройте группу доступности, чтобы указать, на каких репликах доступности предпочтительно проведение резервного копирования. Дополнительные сведения см. в описании параметров *AUTOMATED_BACKUP_PREFERENCE* и *BACKUP_PRIORITY* в разделе [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md) или [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
2.  Создайте скрипты заданий резервного копирования для каждой из баз данных доступности на каждом экземпляре сервера, на котором размещается реплика доступности, потенциально используемая для выполнения резервного копирования. Дополнительные сведения см. в подразделе "Дальнейшие действия. После настройки резервного копирования во вторичных репликах" раздела [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Настройка резервного копирования во вторичных репликах**  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 **Определение, является ли текущая реплика предпочитаемой репликой резервного копирования**  
  
-   [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Создание задания резервного копирования**  
  
-   [Использование мастера планов обслуживания](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
-   [Реализация заданий](../../../ssms/agent/implement-jobs.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Резервные копии только для копирования (SQL Server)](../../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
