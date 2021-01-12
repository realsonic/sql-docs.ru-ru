---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9927dd1e15f04a073d0fb3cf7f95e13b1f425e0c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100507"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSsubscription_properties** содержит строки для сведений о параметрах, необходимых для запуска агентов репликации на подписчике. Эта таблица хранится в базе данных подписки на подписчике для подписки по запросу или в базе данных распространителя на распространителе для принудительной подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Имя издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**публикации**|**sysname**|Имя публикации.|  
|**publication_type**|**int**|Тип публикации:<br /><br /> **0** = транзакционная.<br /><br /> **2** = слияние.|  
|**publisher_login**|**sysname**|Идентификатор входа, используемый на издателе для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Пароль (зашифрованный), используемый на издателе для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**int**|Режим безопасности, реализованный на издателе:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server проверка подлинности.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Проверка подлинности Windows.<br /><br /> **2** = триггеры синхронизации используют статическую запись **sysservers** для удаленного вызова процедур (RPC), а *Издатель* должен быть определен в таблице **sysservers** как удаленный сервер или связанный сервер.|  
|**распространение**|**sysname**|Имя распространителя.|  
|**distributor_login**|**sysname**|Идентификатор (ID) учетной записи, используемый для проверки подлинности SQL Server на распространителе.|  
|**distributor_password**|**nvarchar (524)**|Пароль (зашифрованный), используемый на распространителе для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**int**|Режим безопасности, реализованный на распространителе:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности.<br /><br /> **1** = проверка подлинности Windows.|  
|**ftp_address**|**sysname**|Сетевой адрес службы FTP для распространителя.|  
|**ftp_port**|**int**|Номер порта службы FTP для распространителя.|  
|**ftp_login**|**sysname**|Имя пользователя для подключения к службе FTP.|  
|**ftp_password**|**nvarchar (524)**|Пароль пользователя для подключения к службе FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Указывает местоположение альтернативной папки для моментального снимка.|  
|**working_directory**|**nvarchar(255)**|Имя рабочего каталога, используемого для хранения данных и файлов схем.|  
|**use_ftp**|**bit**|Указывает использование протокола FTP вместо обычного протокола для получения моментальных снимков. Если значение равно **1**, используется протокол FTP.|  
|**dts_package_name**;|**sysname**|Указывает имя пакета служб DTS.|  
|**dts_package_password**;|**nvarchar (524)**|Указывает пароль на пакет.|  
|**dts_package_location**.|**int**|Местоположение, где хранится пакет служб DTS.|  
|**enabled_for_syncmgr**|**bit**|Устанавливает, может ли быть подписка синхронизирована посредством диспетчера синхронизации [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> **0** = подписка не зарегистрирована в диспетчере синхронизации.<br /><br /> **1** = подписка зарегистрирована в диспетчере синхронизации и может быть синхронизирована без запуска [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**offload_agent**|**bit**|Указывает, может ли агент быть активирован удаленно. Если значение **равно 0**, агент не может быть активирован удаленно.|  
|**offload_server**|**sysname**|Указывает сетевое имя сервера, используемого для удаленной активации.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Указывает путь к папке, в которой сохраняются файлы моментальных снимков.|  
|**use_web_sync**|**bit**|Указывает, может ли подписка быть синхронизирована через протокол HTTP или нет. Значение **1** означает, что эта функция включена.|  
|**internet_url**|**nvarchar(260)**|URL-адрес, который представляет собой размещение средства прослушивания репликации для веб-синхронизации.|  
|**internet_login**|**sysname**|Имя входа, используемое агент слияния при соединении с веб-сервером, на котором размещается веб-синхронизация с использованием [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] проверки подлинности.|  
|**internet_password**|**nvarchar (524)**|Пароль для имени входа, используемого агент слияния при соединении с веб-сервером, на котором размещается веб-синхронизация с использованием [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] проверки подлинности.|  
|**internet_security_mode**|**int**|Режим проверки подлинности, используемый при соединении с веб-сервером, на котором размещена веб-синхронизация, где значение **1** означает проверку подлинности Windows, а значение **0** означает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] проверку подлинности.|  
|**internet_timeout**|**int**|Продолжительность времени в секундах перед отменой запроса на веб-синхронизацию.|  
|**hostname**|**sysname**|Задает значение для **HOST_NAME** , когда эта функция используется в предложении **WHERE** фильтра соединений или связи логических записей.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
