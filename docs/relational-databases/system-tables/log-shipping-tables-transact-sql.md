---
description: Таблицы доставки журналов (Transact-SQL)
title: Таблицы доставки журналов (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- log shipping [SQL Server], system tables
- system tables [SQL Server], log shipping
ms.assetid: f8910aae-2013-4645-880c-134577cbcbe0
author: cawrites
ms.author: chadam
ms.openlocfilehash: d9fa899e50b6d6cb260f7fa598bf16b508b567d8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097404"
---
# <a name="log-shipping-tables-transact-sql"></a>Таблицы доставки журналов (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В следующих разделах описываются системные таблицы, в которых хранятся сведения, используемые операциями доставки журналов.  
  
## <a name="in-this-section"></a>В этом разделе  
 [log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)  
 Сохраняет идентификаторы предупреждений заданий для доставки журналов.  
  
 [log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)  
 Сохраняет подробное описание ошибок для заданий доставки журналов.  
  
 [log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)  
 Сохраняет подробные данные журнала для заданий доставки журналов.  
  
 [log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)  
 Хранит одну запись монитора для базы данных-источника в каждой конфигурации доставки журналов.  
  
 [log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)  
 Содержит по одной записи монитора для каждой базы данных-получателя в конфигурации доставки журналов.  
  
 [log_shipping_primary_databases](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
 Сохраняет одну запись для базы данных-источника в конфигурации доставки журналов.  
  
 [log_shipping_primary_secondaries](../../relational-databases/system-tables/log-shipping-primary-secondaries-transact-sql.md)  
 Сопоставляет каждую базу данных-источник с базой данных-получателем.  
  
 [log_shipping_secondary](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)  
 Хранит одну запись для каждого вторичного идентификатора.  
  
 [log_shipping_secondary_databases](../../relational-databases/system-tables/log-shipping-secondary-databases-transact-sql.md)  
 Содержит одну запись для каждой базы данных-получателя в конфигурации доставки журнала.  
  
  
