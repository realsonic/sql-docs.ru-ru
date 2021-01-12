---
description: sys.endpoints (Transact-SQL)
title: sys. Endpoints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- endpoints
- sys.endpoints
- endpoints_TSQL
- sys.endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoints catalog view
ms.assetid: e6dafa4e-e47e-43ec-acfc-88c0af53c1a1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 765e7a2029f28a543a9b0ae6a08a7f6fb6fdb775
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093141"
---
# <a name="sysendpoints-transact-sql"></a>sys.endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке на одну конечную точку, созданную в данной системе. Конечная точка SYSTEM всегда существует в единственном экземпляре.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя конечной точки. Уникален в пределах сервера. Не допускает значение NULL.|  
|**endpoint_id**|**int**|Идентификатор конечной точки. Уникален в пределах сервера. Конечная точка с идентификатором менее 65536 — системная конечная точка. Не допускает значение NULL.|  
|**principal_id**|**int**|Идентификатор сервера-участника, создавшего данную конечную точку и владеющего ей. Допускает значение NULL.|  
|**protocol**|**tinyint**|Протокол конечной точки.<br /><br /> 1 = HTTP.<br /><br /> 2 = TCP.<br /><br /> 3 = именованные каналы.<br /><br /> 4 = общая память.<br /><br /> 5 = адаптер виртуального интерфейса (Virtual Interface Adapter, VIA).<br /><br /> Не допускает значение NULL.|  
|**protocol_desc**|**nvarchar(60)**|Описание протокола конечной точки. Допускает значение NULL. Одно из следующих значений:<br /><br /> **HTTP**<br /><br /> **TCP**<br /><br /> **NAMED_PIPES**<br /><br /> **SHARED_MEMORY**<br /><br /> **Через** Примечание. протокол VIA является устаревшим. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|**type**|**tinyint**|Тип полезных данных конечной точки.<br /><br /> 1 = SOAP.<br /><br /> 2 = TSQL.<br /><br /> 3 = SERVICE_BROKER.<br /><br /> 4 = DATABASE_MIRRORING.<br /><br /> Не допускает значение NULL.|  
|**type_desc**|**nvarchar(60)**|Описание типа полезных данных конечной точки. Допускает значение NULL. Одно из следующих значений:<br /><br /> **Протокол SOAP**<br /><br /> **TSQL**<br /><br /> **SERVICE_BROKER**<br /><br /> **DATABASE_MIRRORING**|  
|**state**|**tinyint**|Состояние конечной точки.<br /><br /> 0 = запущена: прослушивает и обрабатывает запросы.<br /><br /> 1 = остановлена: прослушивает, но не обрабатывает запросы.<br /><br /> 2 = отключена: не прослушивает.<br /><br /> Состояние по умолчанию — 1. Допускает значение NULL.|  
|**state_desc**|**nvarchar(60)**|Описание состояния конечной точки.<br /><br /> Запущена: прослушивает и обрабатывает запросы.<br /><br /> Остановлена: прослушивает, но не обрабатывает запросы.<br /><br /> Отключена: не прослушивает.<br /><br /> По умолчанию конечная точка остановлена.<br /><br /> Допускает значение NULL.|  
|**is_admin_endpoint**|**bit**|Указывает, является ли конечная точка административной.<br /><br /> 0 = конечная точка не является административной.<br /><br /> 1 = административная конечная точка.<br /><br /> Не допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога конечных точек (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
