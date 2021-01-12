---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 98e5e9b90449836a4370771c35e12ea82007aed6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099756"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку с текущими настройками для свойств ресурса кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], описанных в этом разделе. При запуске этого представления на изолированном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] никакие данные не возвращаются.  
  
 Данные свойства используются для задания значений, которые влияют на обнаружение отказов, время реакции на отказ и ведение журнала для мониторинга состояния работоспособности экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

|Имя столбца|Свойство|Описание|  
|-----------------|--------------|-----------------|  
|VerboseLogging|BIGINT|Уровень ведения журнала для отказоустойчивого кластера SQL Server. Для записи дополнительных сведений в журналы ошибок с целью последующего устранения неполадок можно включить подробный уровень ведения журнала. Одно из следующих значений:<br /><br /> 0 — ведение журнала отключено (по умолчанию)<br /><br /> 1 — только ошибки<br /><br /> 2 — ошибки и предупреждения<br /><br /> Дополнительные сведения см. в разделе [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|BIGINT|Флаги дампа SQLDumper определяют тип файлов дампа, созданных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Значение по умолчанию — 0.|  
|SqlDumperDumpPath|nvarchar(260)|Место, в котором программа SQLDumper создает файлы дампа.|  
|SqlDumperDumpTimeOut|BIGINT|Значение тайм-аута в миллисекундах, отведенное программе SQLDumper для создания дампа в случае возникновения сбоя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Значение по умолчанию — 0.|  
|FailureConditionLevel|BIGINT|Задает условия, при которых отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен отказать или перезапуститься. Значение по умолчанию равно 3. Подробное объяснение или изменение параметров свойства см. в разделе [Configure FailureConditionLevel Property Settings](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|BIGINT|Время, в течение которого библиотека ресурсов компонента SQL Server Database Engine будет ждать сведений о состоянии сервера, прежде чем сервер переводится в категорию неотвечающих. Время ожидания указывается в миллисекундах. Значение по умолчанию — 60000. Дополнительные сведения или изменение этого параметра свойства см. в разделе [Configure HealthCheckTimeout Property Settings](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW SERVER STATE на экземпляре отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано использование sys.dm_os_cluster_properties для возвращения настроек свойств ресурса отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 Ниже приводится образец результирующего набора.  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60 000|  
  
  
