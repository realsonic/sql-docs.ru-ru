---
description: sys.database_mirroring (Transact-SQL)
title: sys.database_mirroring (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_mirroring
- database_mirroring
- sys.database_mirroring_TSQL
- database_mirroring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_mirroring catalog view
ms.assetid: 480de2b0-2c16-497d-a6a3-bf7f52a7c9a0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7ecce00c7981f59cfa1d145336e757b3d443379b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093147"
---
# <a name="sysdatabase_mirroring-transact-sql"></a>sys.database_mirroring (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой базы данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если база данных не находится в режиме в сети или зеркальное отображение базы данных не включено, значения всех столбцов, кроме database_id, будут равны NULL.  
  
 Для просмотра строки какой-либо базы данных, кроме master или tempdb, нужно быть либо владельцем базы данных, либо обладать, как минимум, разрешением уровня сервера ALTER ANY DATABASE или VIEW ANY DATABASE, либо разрешением CREATE DATABASE в базе данных master.  Для просмотра значений, отличных от NULL, в зеркальной базе данных необходимо быть членом предопределенной роли сервера **sysadmin** .  
  
> [!NOTE]  
>  Если база данных не участвует в зеркальном отображении, то все столбцы, обладающие префиксом «mirroring_», имеют значение NULL.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных. Он уникален внутри экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**mirroring_guid**|**uniqueidentifier**|Идентификатор участника зеркального отображения.<br /><br /> NULL = база данных недоступна или не зеркально отображена.<br /><br /> Примечание. Если база данных не участвует в зеркальном отображении, все столбцы с префиксом "mirroring_" имеют значение NULL.|  
|**mirroring_state**|**tinyint**|Состояние зеркальной базы данных и сеанса зеркального отображения базы данных:<br /><br /> 0 = приостановлено;<br /><br /> 1 = отключено от другого участника;<br /><br /> 2 = идет процесс синхронизации;<br /><br /> 3 = ожидание отработки отказа;<br /><br /> 4 = синхронизирована;<br /><br /> 5 = участники не синхронизированы. Отработка отказа сейчас невозможна;<br /><br /> 6 = участники синхронизированы. Отработка отказа возможна. Сведения о требованиях для отработки отказа см. в разделе [режимы работы зеркального отображения баз данных](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_state_desc**|**nvarchar(60)**|Описание состояния зеркальной базы данных и сеанса зеркального отображения базы данных может быть одним из следующих:<br /><br /> DISCONNECTED<br /><br /> SYNCHRONIZED<br /><br /> SYNCHRONIZING<br /><br /> PENDING_FAILOVER<br /><br /> SUSPENDED<br /><br /> UNSYNCHRONIZED<br /><br /> SYNCHRONIZED<br /><br /> NULL<br /><br /> Дополнительные сведения см. в разделе [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/mirroring-states-sql-server.md).|  
|**mirroring_role**|**tinyint**|Текущая роль локальной базы данных в сеансе зеркального отображения базы данных:<br /><br /> 1 = основная;<br /><br /> 2 = зеркальная;<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_role_desc**|**nvarchar(60)**|Описание роли локальной базы данных в зеркальном отображении может быть одним из следующих:<br /><br /> PRINCIPAL<br /><br /> MIRROR|  
|**mirroring_role_sequence**|**int**|Количество переключений ролей между участниками зеркального отображения с роли главной базы данных на роль зеркала и наоборот вследствие отработки отказа или во время принудительного обслуживания.<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_safety_level**|**tinyint**|Настройка безопасности для обновлений в зеркальной базе данных:<br /><br /> 0 = неизвестное состояние.<br /><br /> 1 = выключена [асинхронное состояние];<br /><br /> 2 = полная [синхронное состояние];<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_safety_level_desc**|**nvarchar(60)**|Настройка безопасности транзакции для обновлений в зеркальной базе данных может быть одной из следующих:<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL<br /><br /> NULL|  
|**mirroring_safety_sequence**|**int**|Обновление порядкового номера для изменений на уровне безопасности транзакции.<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_partner_name**|**nvarchar(128)**|Имя сервера участника зеркального отображения базы данных.<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_partner_instance**|**nvarchar(128)**|Имя экземпляра и имя компьютера другого участника. Эти сведения требуются клиентам для подключения к участнику, если он становится основным сервером.<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_witness_name**|**nvarchar(128)**|Имя следящего сервера зеркального отображения базы данных.<br /><br /> NULL = следящего сервера не существует.|  
|mirroring_witness_state|**tinyint**|Состояние следящего сервера в сеансе зеркального отображения базы данных может принимать одно из следующих значений:<br /><br /> 0 = неизвестное состояние;<br /><br /> 1 = подключен;<br /><br /> 2 = отключен;<br /><br /> NULL = следящий сервер отсутствует, база данных находится не в режиме в сети или не подвергнута зеркальному отображению.|  
|**mirroring_witness_state_desc**|**nvarchar(60)**|Описание состояния, может быть одним из следующих:<br /><br /> UNKNOWN<br /><br /> CONNECTED<br /><br /> DISCONNECTED<br /><br /> NULL|  
|**mirroring_failover_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале (LSN) последней записи в журнале транзакций, которая гарантированно сохранена на диски обоих участников. После отработки отказа **mirroring_failover_lsn** используется партнерами в качестве точки сверки, с которой новый зеркальный сервер начинает синхронизировать новую зеркальную базу данных с новой основной базой данных.|  
|**mirroring_connection_timeout**|**int**|Время ожидания соединения с зеркальным отображением базы данных в секундах. Время, в течение которого сервер ждет отклика от участника или следящего сервера перед тем, как решить, что они недоступны. По умолчанию время ожидания равно 10 секундам.<br /><br /> NULL= база данных недоступна или не подвергнута зеркальному отображению.|  
|**mirroring_redo_queue**|**int**|Максимальный объем данных журнала, перезаписываемый на зеркало. Если параметру mirroring_redo_queue_type присвоено значение UNLIMITED, которое является значением по умолчанию, этот столбец равен NULL. Если база данных находится не в режиме в сети, этот столбец также равен NULL.<br /><br /> В противном случае в этом столбце записан максимальный объем данных журнала в мегабайтах. При достижении максимума журнал на основном сервере временно останавливается, пока зеркальный сервер его не догонит. Эта возможность ограничивает время отработки отказа.<br /><br /> Дополнительные сведения см. в статье [Оценка прерывания обслуживания во время переключения ролей (зеркальное отображение базы данных)](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**mirroring_redo_queue_type**|**nvarchar(60)**|Значение UNLIMITED означает, что зеркальное отображение не ограничивает очередь повтора. Это параметр по умолчанию.<br /><br /> Значение MB показывает максимальный размер очереди повтора в мегабайтах. Обратите внимание, что если размер очереди определен в килобайтах или гигабайтах, то компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] преобразует это значение в мегабайты.<br /><br /> Если база данных находится не в режиме в сети, этот столбец равен NULL.|  
|**mirroring_end_of_log_lsn**|**numeric(25,0)**|Локальный конец журнала, записанный на диск. Это сравнимо с зафиксированным номером LSN с зеркального сервера (см. столбец **mirroring_failover_lsn** ).|  
|**mirroring_replication_lsn**|**numeric(25,0)**|Максимальный номер LSN, отправляемый репликацией.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Представления каталогов баз данных и файлов (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
