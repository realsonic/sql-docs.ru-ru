---
description: log_shipping_monitor_primary (Transact-SQL)
title: log_shipping_monitor_primary (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_monitor_primary
- log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_primary system table
ms.assetid: 5f629a29-1a62-40e6-ae33-6f6b7dd09a36
author: cawrites
ms.author: chadam
ms.openlocfilehash: 076398da53bf4886a0b8e3f387df949d29cb8b03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098713"
---
# <a name="log_shipping_monitor_primary-transact-sql"></a>log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Хранит одну запись монитора для базы данных-источника в каждой конфигурации доставки журналов. Эта таблица хранится в базе данных **msdb** .  
  
 Таблицы, связанные с ведением журналов и мониторингом, также используются на сервере-источнике и серверах-получателях.   
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|Идентификатор базы данных-источника в конфигурации доставки журналов.|  
|**primary_server**|**sysname**|Имя первичного экземпляра компонента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] в конфигурации доставки журнала.|  
|**primary_database**|**sysname**|Имя базы данных-источника в конфигурации доставки журналов.|  
|**backup_threshold**|**int**|Максимальное время ожидания (в минутах) между операциями резервного копирования перед созданием предупреждения.|  
|**threshold_alert**|**int**|Предупреждение, создаваемое при превышении порогового значения.|  
|**threshold_alert_enabled**|**bit**|Определяет, включены ли пороговые предупреждения резервного копирования. 1 = включено.<br /><br /> 0 = отключено.|  
|**last_backup_file**|**nvarchar (500)**|Абсолютный путь последней резервной копии журнала транзакций.|  
|**last_backup_date**|**datetime**|Время и дата последней операции резервного копирования журнала транзакций базы данных-источника.|  
|**last_backup_date_utc**|**datetime**|Время и дата создания последней резервной копии журнала транзакций в базе данных-источнике, выраженные в формате UTC.|  
|**history_retention_period**|**int**|Промежуток времени в минутах, после которого записи истории доставки журнала данной базы данных-источника будут удалены.|  
  
## <a name="remarks"></a>Комментарии  
 В дополнение к хранению на удаленном сервере мониторинга сведения, относящиеся к основному серверу, хранятся на сервере-источнике в его **log_shipping_monitor_primary** таблице.  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_change_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_help_log_shipping_monitor_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)   
 [sp_delete_log_shipping_alert_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
