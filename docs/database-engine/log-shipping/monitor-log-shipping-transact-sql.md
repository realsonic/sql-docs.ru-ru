---
title: Наблюдение за доставкой журналов (Transact-SQL) | Документы Майкрософт
description: Узнайте, в каких таблицах хранятся журналы, содержащие сведения о мониторинге и хранимые процедуры для мониторинга доставки журналов в SQL Server.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], status
- history tables [SQL Server]
- historical information [SQL Server], log shipping
- log shipping [SQL Server], monitoring
- alerts [SQL Server], log shipping
- status information [SQL Server], log shipping
- monitoring log shipping [SQL Server]
ms.assetid: acf3cd99-55f7-4287-8414-0892f830f423
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5086ce48d0b1b31c81e8e1d70e613e087e91a5d6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643871"
---
# <a name="monitor-log-shipping-transact-sql"></a>Наблюдение за доставкой журналов (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  После того, как была настроена доставка журналов, можно отслеживать данные о состоянии всех серверов доставки журналов. Журнал и состояние операций доставки журналов всегда сохраняются локально заданиями доставки журналов. Журнал и состояние операций резервного копирования сохраняются на сервере-источнике, а журнал и состояние операций копирования и восстановления сохраняются на сервере-получателе. Если был реализован удаленный сервер мониторинга, эти данные также сохраняются и на нем.  
  
 Можно настраивать предупреждения, которые работают в случае сбоев при выполнении операций доставки журналов по расписанию. Сообщения об ошибке создаются заданием предупреждения, отслеживающим состояние операций резервного копирования и восстановления. Можно определить предупреждения, которые отправят сообщение оператору при возникновении таких ошибок. Если сервер мониторинга настроен, то одно задание предупреждения выполняется на нем и отвечает за сообщения об ошибках для всех операций данной конфигурации доставки журналов. Если сервер мониторинга не указан, задание предупреждения выполняется на экземпляре сервера-источника, который отслеживает операции резервного копирования. Если сервер мониторинга не указан, задание предупреждения также выполняется на каждом экземпляре сервера-получателя, где отслеживает локальные операции копирования и восстановления.  
  
> [!IMPORTANT]  
>  Чтобы отслеживать конфигурацию доставки журналов, во время включения доставки журналов необходимо добавить сервер мониторинга. Если добавлять сервер мониторинга позже, то придется удалить существующую конфигурацию доставки журналов и заменить ее новой с сервером мониторинга. Дополнительные сведения см. в разделе [Настройка доставки журналов (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md). Кроме того, заданную конфигурацию сервера мониторинга невозможно изменить, не удалив прежде конфигурацию доставки журналов.  
  
## <a name="history-tables-containing-monitoring-information"></a>Таблицы журналов, содержащие данные мониторинга  
 Таблицы журналов мониторинга содержат метаданные, хранящиеся на сервере мониторинга. Копия данных, относящихся именно к данному серверу-источнику или серверу-получателю, также хранится локально.  
  
 Эти таблицы можно опрашивать, чтобы следить за состоянием сеанса доставки журналов. Например, чтобы узнать состояние доставки журналов, можно проверить состояние и историю заданий резервного копирования, копирования и восстановления. Можно просматривать отдельные журналы доставки и подробные сведения об ошибках, выполняя запросы к описанным ниже таблицам мониторинга.  
  
|Таблица|Описание|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Содержит идентификатор задания предупреждения.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Содержит подробное описание ошибок заданий доставки журналов. Выполняя запросы к этой таблице, можно получать сведения об ошибках в сеансах агентов. При необходимости можно выполнить сортировку ошибок по дате и времени их внесения в журнал. Каждая ошибка записывается в журнал как последовательность исключений, и на один сеанс агента может приходиться несколько ошибок (последовательностей).|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Содержит подробные журналы агентов доставки журналов. Выполнив запросы к этой таблице, можно получить подробные сведения о предыдущих сеансах агентов.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Содержит одну запись монитора для базы данных-источника в каждой из конфигураций доставки журналов, включая данные о последнем файле резервной копии и о последнем восстановленном файле, которые полезны для мониторинга.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Содержит одну запись монитора для каждой базы данных-получателя, включающую данные о последнем файле резервной копии и последнем восстановленном файле, которые полезны для мониторинга.|  
  
## <a name="stored-procedures-for-monitoring-log-shipping"></a>Хранимые процедуры для мониторинга доставки журналов  
 Сведения мониторинга и данные журналов хранятся в таблицах в базе данных **msdb**, к которым можно получить доступ посредством использования хранимых процедур доставки журналов. Выполняйте эти хранимые процедуры на указанных в следующей таблице серверах.  
  
|Хранимая процедура|Описание|Место выполнения процедуры|  
|----------------------|-----------------|---------------------------|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Возвращает записи монитора для указанной базы данных-источника из таблицы **log_shipping_monitor_primary** .|Сервер мониторинга или сервер-источник|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Возвращает записи монитора для указанной базы данных-получателя из таблицы **log_shipping_monitor_secondary** .|Сервер мониторинга или сервер-получатель|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Возвращает идентификатор задания предупреждения.|Сервер мониторинга, сервер-источник или сервер-получатель, если сервер мониторинга не определен.|  
|[sp_help_log_shipping_primary_database, хранимая процедура](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Получает настройки базы данных-источника и отображает значения из таблиц **log_shipping_primary_databases** и **log_shipping_monitor_primary** .|Сервер-источник|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Получает имена баз данных-получателей для базы данных-источника.|Сервер-источник|  
|[sp_help_log_shipping_secondary_database, хранимая процедура](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Получает настройки базы данных-получателя из таблиц **log_shipping_secondary**, **log_shipping_secondary_databases** и **log_shipping_monitor_secondary** .|Сервер-получатель|  
|[sp_help_log_shipping_secondary_primary (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Эта хранимая процедура получает настройки для данной базы данных-источника с сервера-получателя.|Сервер-получатель|  
  
## <a name="see-also"></a>См. также:  
 [Просмотр отчета о доставке журналов (среда SQL Server Management Studio)](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)   
 [Хранимые процедуры и таблицы доставки журналов](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
