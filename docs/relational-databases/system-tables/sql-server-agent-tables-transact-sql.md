---
description: Таблицы агента SQL Server (Transact-SQL)
title: Таблицы агент SQL Server (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Agent, system tables
- system tables [SQL Server], SQL Server Agent
ms.assetid: 6cb39bfd-079e-4be4-9c42-2fa234c65ce1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3cf53dd13bd94c9632c4f3c2464bda38d50e3742
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096157"
---
# <a name="sql-server-agent-tables-transact-sql"></a>Таблицы агента SQL Server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В подразделах данного раздела описаны системные таблицы, в которых хранятся данные агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Все таблицы находятся в схеме dbo в базе данных msdb.  
  
## <a name="in-this-section"></a>В этом разделе  
 [dbo.sysalerts](../../relational-databases/system-tables/dbo-sysalerts-transact-sql.md)  
 Содержит одну строку для каждого предупреждения.  
  
 [dbo.syscategories](../../relational-databases/system-tables/dbo-syscategories-transact-sql.md)  
 Содержит категории, используемые средой [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] для организации заданий, предупреждений и операторов.  
  
 [dbo.sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
 Содержит очередь инструкций по загрузке для всех целевых серверов.  
  
 [dbo.sysjobactivity](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
 Содержит сведения о текущей активности и состоянии выполнения заданий агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
 Содержит сведения о выполнении назначенных заданий агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
 Хранит сведения для каждого задания, назначенного к выполнению агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobschedules](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
 Содержит сведения о расписании заданий для выполнения агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 [dbo.sysjobservers](../../relational-databases/system-tables/dbo-sysjobservers-transact-sql.md)  
 Хранит взаимосвязь или связь определенного задания с одним или более целевых серверов.  
  
 [dbo.sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
 Содержит сведения для каждого этапа задания, исполняемого агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobstepslogs](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
 Содержит сведения о журналах шагов заданий.  
  
 [dbo.sysnotifications](../../relational-databases/system-tables/dbo-sysnotifications-transact-sql.md)  
 Содержит одну строку для каждого уведомления.  
  
 [dbo.sysoperators](../../relational-databases/system-tables/dbo-sysoperators-transact-sql.md)  
 Содержит по одной строке для каждого оператора агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
 Содержит сведения об учетных записях-посредниках агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxylogin](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)  
 Содержит записи об именах входа в среду [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], связанных с каждой из учетных записей-посредников агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxysubsystem](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Содержит записи, используемые подсистемой агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для каждой из учетных записей-посредников.  
  
 [dbo.sysschedules](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
 Содержит сведения о расписании заданий агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssessions](../../relational-databases/system-tables/dbo-syssessions-transact-sql.md)  
 Содержит дату запуска агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для каждого сеанса агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сеанс создается каждый раз при запуске службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssubsystems](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Содержит сведения обо всех доступных подсистемах учетных записей-посредников агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.systargetservergroupmembers](../../relational-databases/system-tables/dbo-systargetservergroupmembers-transact-sql.md)  
 Записывает, какие целевые серверы связаны в настоящее время с данной многосерверной группой.  
  
 [dbo.systargetservergroups](../../relational-databases/system-tables/dbo-systargetservergroups-transact-sql.md)  
 Записывает, какие группы целевых серверов в настоящее время прикреплены к данной многосерверной среде.  
  
 [dbo.systargetservers](../../relational-databases/system-tables/dbo-systargetservers-transact-sql.md)  
 Записи, занесенные целевыми серверами в этот многосерверный операционный домен.  
  
 [dbo.systaskids](../../relational-databases/system-tables/dbo-systaskids-transact-sql.md)  
 Содержит сопоставление задач, созданных в более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с заданиями среды [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] текущей версии.  
  
  
