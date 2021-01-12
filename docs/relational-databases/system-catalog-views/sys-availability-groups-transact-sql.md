---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c912d3cb46d4545bf0501281cec23c8f9ce4fb97
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101823"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по строке для каждой группы доступности, для которой в локальном экземпляре SQL Server размещена реплика доступности. Каждая строка содержит кэшированную копию метаданных группы доступности.  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Уникальный идентификатор (GUID) группы доступности.|  
|**name**|**sysname**|Имя группы доступности. Определяемое пользователем имя, которое должно быть уникальным в отказоустойчивом кластере Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|Идентификатор ресурса для ресурса кластера WSFC.|  
|**resource_group_id**|**nvarchar(40)**|Идентификатор группы ресурсов кластера WSFC, принадлежащей к группе доступности.|  
|**failure_condition_level**|**int**|Определяемый пользователем уровень условий сбоя, при котором должна быть активирована автоматическая отработка отказа, одно из целочисленных значений, приведенных в таблице непосредственно под этой таблицей.<br /><br /> Уровни условий сбоя (1–5) варьируются от наименее ограничительного уровня 1 до наиболее ограничительного уровня 5. Заданный уровень условий включает в себя ограничения всех предыдущих уровней. Таким образом, наиболее строгий уровень 5 включает в себя ограничения уровней с 1 по 4, уровень 4 содержит ограничения уровней с 1 по 3 и т. д.<br /><br /> Чтобы изменить это значение, используйте параметр FAILURE_CONDITION_LEVEL инструкции [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Время ожидания (в миллисекундах), в течение которого [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) системная хранимая процедура возвращает сведения о работоспособности сервера, прежде чем предполагается, что экземпляр сервера будет реагировать на запросы или не отвечает. Значение по умолчанию — 30 000 миллисекунд (30 секунд).<br /><br /> Чтобы изменить это значение, используйте параметр HEALTH_CHECK_TIMEOUT инструкции [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Предпочитаемое расположение для выполнения резервного копирования баз данных доступности в этой группе доступности. Ниже приведены возможные значения и их описания.<br /><br /> <br /><br /> 0: основной. Резервное копирование должно всегда выполняться в первичной реплике.<br /><br /> 1: только вторичная. Создание резервных копий во вторичной реплике является предпочтительным.<br /><br /> 2: предпочитать вторичную. Создание резервных копий во вторичной реплике является предпочтительным, но создание резервных копий в первичной реплике также является допустимым при отсутствии вторичных реплик для операций резервного копирования. Это поведение установлено по умолчанию.<br /><br /> 3. Любая реплика. Приоритет места выполнения резервного копирования отсутствует.<br /><br /> <br /><br /> Дополнительные сведения см. в статье [Активные вторичные реплики: резервное копирование во вторичных репликах (группы доступности Always On)](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Описание **AUTOMATED_BACKUP_PREFERENCE**, одно из следующих:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> NONE|  
|**version**|**smallint**|Версия метаданных группы доступности, хранящихся в отказоустойчивом кластере Windows. Этот номер версии увеличивается при добавлении новых компонентов.|  
|**basic_features**|**bit**|Указывает, является ли эта группа доступности базовой. Дополнительные сведения см. в разделе [Базовые группы доступности (группы доступности AlwaysOn)](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Указывает, включена ли поддержка DTC для этой группы доступности. Этот параметр управляет параметром **DTC_SUPPORT** **создания группы доступности** .|  
|**db_failover**|**bit**|Указывает, поддерживает ли группа доступности отработку отказа для условий работоспособности базы данных. Этот параметр управляет параметром **DB_FAILOVER** **создания группы доступности** .|  
|**is_distributed**|**bit**|Указывает, является ли это распределенной группой доступности. Дополнительные сведения см. в разделе [Распределенные группы доступности (группы доступности AlwaysOn)](../../database-engine/availability-groups/windows/distributed-availability-groups.md).|
|**cluster_type**|**tinyint**|0: отказоустойчивый кластер Windows Server <br/><br/>1: внешний кластер (например, Linux Pacemaker);<br/><br/>2: нет|
|**cluster_type_desc**|**nvarchar(60)**|Текстовое описание типа кластера|
|**required_synchronized_secondaries_to_commit**|**int**| Число вторичных реплик, которые должны находиться в состоянии SYNCHRONIZED для завершения фиксации|
|**sequence_number**|**bigint**|Определяет последовательность конфигурации группы доступности. Постепенно возрастает каждый раз, когда первичная реплика группы доступности обновляет конфигурацию группы.|
|**is_contained**|**bit**|1: экземпляр главного кластера больших данных, настроенный для обеспечения высокой доступности. <br/><br/> 0 — все остальные.|
  
## <a name="failure-condition-level--values"></a>Значения уровня условий сбоя  
 В следующей таблице описаны возможные уровни условий сбоя для **FAILURE_CONDITION_LEVEL** столбца.  
  
|Значение|Условие сбоя|  
|-----------|-----------------------|  
|1|Указывает, что следует запустить автоматический переход на другой ресурс при возникновении любой из следующих ситуаций.<br /><br /> <br /><br /> — [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Служба не работает.<br /><br /> — Аренда группы доступности для подключения к отказоустойчивому кластеру WSFC истекает, так как от экземпляра сервера не получено подтверждение. Дополнительные сведения см. в разделе [Как это работает: время ожидания аренды Always On в SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Указывает, что следует запустить автоматический переход на другой ресурс при возникновении любой из следующих ситуаций.<br /><br /> <br /><br /> — Экземпляр не [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подключается к кластеру, и превышено заданное пользователем пороговое значение **HEALTH_CHECK_TIMEOUT** группы доступности.<br /><br /> -Реплика доступности находится в состоянии сбоя.|  
|3|Указывает, что следует запустить автоматический переход на другой ресурс в случае появления критических внутренних ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], таких как потерянные спин-блокировки, серьезные нарушения доступа для записи или формирование слишком больших дампов.<br /><br /> Это значение по умолчанию.|  
|4|Указывает, что следует запустить автоматический переход на другой ресурс в случае появления не столь серьезных внутренних ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], например устойчивое состояние нехватки памяти в пуле внутренних ресурсов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Указывает, что следует запустить автоматический переход на другой ресурс при любом удовлетворяющим условиям состоянии сбоя, включая:<br /><br /> <br /><br /> — Исчерпание рабочих потоков SQL Engine.<br /><br /> — Обнаружение неразрешимой взаимоблокировки.|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW ANY DEFINITION на экземпляре сервера.  
  
## <a name="see-also"></a>См. также:  
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Мониторинг групп доступности &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Отслеживание групп доступности (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
