---
description: log_shipping_primary_databases (Transact-SQL)
title: log_shipping_primary_databases (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_primary_databases
- log_shipping_primary_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_databases system table
ms.assetid: 56888756-a798-42be-9b5e-0f9aa05a2cc6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8dd97d1baa37b71fec2e1ceb113e4b0a3614a2e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102154"
---
# <a name="log_shipping_primary_databases-transact-sql"></a>log_shipping_primary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Сохраняет одну запись для базы данных-источника в конфигурации доставки журналов. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|Идентификатор базы данных-источника в конфигурации доставки журналов.|  
|**primary_database**|**sysname**|Имя базы данных-источника в конфигурации доставки журналов.|  
|**каталог_резервной_копии**|**nvarchar (500)**|Каталог, в котором хранятся файлы резервной копии журнала транзакций с сервера-источника.|  
|**backup_share**|**nvarchar (500)**|Сетевой или UNC-путь к каталогу резервных копий.|  
|**backup_retention_period**|**int**|Время хранения файла резервной копии журнала в каталоге резервных копий (в минутах).|  
|**backup_job_id**|**uniqueidentifier**|Идентификатор задания агента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], связанный с заданием резервного копирования на сервере-источнике.|  
|**monitor_server**|**sysname**|Имя экземпляра [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], используемого в качестве сервера мониторинга в конфигурации доставки журналов.|  
|**monitor_server_security_mode**|**bit**|Режим безопасности, используемый для подключения к серверу мониторинга:<br /><br /> 1 = проверка подлинности Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности.|  
|**last_backup_file**|**nvarchar (500)**|Абсолютный путь последней резервной копии журнала транзакций.|  
|**last_backup_date**|**datetime**|Дата и время создания последней резервной копии журнала.|  
|**user_specified_monitor**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **sp_help_log_shipping_primary_database** и **sp_help_log_shipping_secondary_primary** используйте этот столбец для управления отображением параметров монитора в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> 0 = при вызове любой из этих двух хранимых процедур пользователь не указал явно значение для параметра **\@ monitor_server** .<br /><br /> 1 = пользователь задал значение явно.|  
|**backup_compression**|**tinyint**|Указывает, переопределяет ли конфигурация доставки журналов поведение сжатия резервной копии на уровне сервера.<br /><br /> 0 = отключено. Резервные копии журналов никогда не сжимаются независимо от параметров настраиваемого сервером сжатия резервной копии.<br /><br /> 1 = включено. Резервные копии журналов всегда сжимаются независимо от параметров настраиваемого сервером сжатия резервной копии.<br /><br /> 2 = использует конфигурацию сервера для параметра [Просмотреть или настроить параметр конфигурации сервера сжатие резервной копии по умолчанию](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) сервер-конфигурация. Это значение по умолчанию.<br /><br /> Сжатие резервной копии поддерживается только в выпуске Enterprise базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
