---
description: sys.dm_xe_database_session_targets (база данных SQL Azure)
title: sys.dm_xe_database_session_targets
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 7f353e2a-f8fc-4366-97e4-aa1c49eadaf4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 34933a5460e69be11bb67e6ee1769af21c732378
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095092"
---
# <a name="sysdm_xe_database_session_targets-azure-sql-database"></a>sys.dm_xe_database_session_targets (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Возвращает сведения о целевых объектах сеанса.  
  
||  
|-|  
|**Применимо к** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] версии 12 и всем будущим версиям.|  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|Адрес сеанса событий в памяти. Имеет связь «многие к одному» с sys.dm_xe_database_sessions. Address. Не допускает значение NULL.|  
|target_name|**nvarchar(60)**|Имя цели в сеансе. Не допускает значение NULL.|  
|target_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, в котором содержится цель. Не допускает значение NULL.|  
|execution_count|**bigint**|Количество выполнений цели для сеанса. Не допускает значение NULL.|  
|execution_duration_ms|**bigint**|Общее время в миллисекундах, в течение которого выполнялась цель. Не допускает значение NULL.|  
|target_data|**nvarchar(max)**|Данные, предоставляемые целью, такие как сведения статистической обработки событий. Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_database_session_targets sys.dm_xe_database_session_targets.event_session_address|sys.dm_xe_database_sessions. Address|«многие к одному»|  
  
  
