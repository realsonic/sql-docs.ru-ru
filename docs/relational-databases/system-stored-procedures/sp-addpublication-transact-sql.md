---
description: sp_addpublication (Transact-SQL)
title: sp_addpublication (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_addpublication_TSQL
- sp_addpublication
helpviewer_keywords:
- sp_addpublication
ms.assetid: c7167ed1-2b7e-4824-b82b-65f4667c4407
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9aa65ff9b5b9c14441ac555d40788e92082f01c5
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564446"
---
# <a name="sp_addpublication-transact-sql"></a>sp_addpublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Создает моментальный снимок публикации транзакций. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addpublication [ @publication = ] 'publication'  
    [ , [ @taskid = ] tasked ]  
    [ , [ @restricted = ] 'restricted' ]  
    [ , [ @sync_method = ] 'sync_method' ]  
    [ , [ @repl_freq = ] 'repl_freq' ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @status = ] 'status' ]  
    [ , [ @independent_agent = ] 'independent_agent' ]  
    [ , [ @immediate_sync = ] 'immediate_sync' ]  
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]  
    [ , [ @allow_push = ] 'allow_push'  
    [ , [ @allow_pull = ] 'allow_pull' ]  
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]  
    [ , [ @allow_sync_tran = ] 'allow_sync_tran' ]  
    [ , [ @autogen_sync_procs = ] 'autogen_sync_procs' ]  
    [ , [ @retention = ] retention ]  
    [ , [ @allow_queued_tran= ] 'allow_queued_updating' ]  
    [ , [ @snapshot_in_defaultfolder= ] 'snapshot_in_default_folder' ]  
    [ , [ @alt_snapshot_folder= ] 'alternate_snapshot_folder' ]  
    [ , [ @pre_snapshot_script= ] 'pre_snapshot_script' ]  
    [ , [ @post_snapshot_script= ] 'post_snapshot_script' ]  
    [ , [ @compress_snapshot= ] 'compress_snapshot' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port= ] ftp_port ]  
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @allow_dts = ] 'allow_dts' ]  
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]  
    [ , [ @conflict_policy = ] 'conflict_policy' ]  
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @queue_type = ] 'queue_type' ]  
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]  
    [ , [ @logreader_job_name = ] 'logreader_agent_name' ]  
    [ , [ @qreader_job_name = ] 'queue_reader_agent_name' ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @allow_initialize_from_backup = ] 'allow_initialize_from_backup' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @enabled_for_p2p = ] 'enabled_for_p2p' ]  
    [ , [ @publish_local_changes_only = ] 'publish_local_changes_only' ]  
    [ , [ @enabled_for_het_sub = ] 'enabled_for_het_sub' ]  
    [ , [ @p2p_conflictdetection = ] 'p2p_conflictdetection' ]  
    [ , [ @p2p_originator_id = ] p2p_originator_id  
    [ , [ @p2p_continue_onconflict = ] 'p2p_continue_onconflict'  
    [ , [ @allow_partition_switch = ] 'allow_partition_switch'  
    [ , [ @replicate_partition_switch = ]'replicate_partition_switch'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ \@publication = ] 'publication'` Имя создаваемой публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию. В базе данных это имя должно быть уникальным.  
  
`[ \@taskid = ] taskid` Поддерживается только для обратной совместимости; Используйте [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md).  
  
`[ \@restricted = ] 'restricted'` Поддерживается только для обратной совместимости; Используйте *default_access*.  
  
`[ \@sync_method = ] _'sync_method'` Режим синхронизации. *sync_method* имеет тип **nvarchar (13)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**native**|Производит выходные данные программы массового копирования всех таблиц. *Не поддерживается для издателей Oracle*.|  
|**character**|Производит выходные данные программы массового копирования всех таблиц в символьном режиме. _Для издателя Oracle_ **символ** _допустим только для репликации моментальных снимков_.|  
|**одновременных**|Создает собственный программный вывод массового копирования всех таблиц, но не блокирует таблицы во время формирования моментальных снимков. Поддерживается только для публикаций транзакций. *Не поддерживается для издателей Oracle*.|  
|**concurrent_c**|Создает символьный программный вывод массового копирования всех таблиц, но не блокирует таблицы во время формирования моментальных снимков. Поддерживается только для публикаций транзакций.|  
|**моментальный снимок базы данных**|Производит выходные данные программы массового копирования всех таблиц в моментальном снимке базы данных. Моментальные снимки базы данных доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|**database snapshot character**|Производит выходные данные программы массового копирования в символьном режиме всех таблиц в моментальном снимке базы данных. Моментальные снимки базы данных доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|NULL (по умолчанию)|По умолчанию используется **native** для [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей. Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей, отличных от, по умолчанию используется **символьный** , если значение *repl_freq* — **snapshot** , а для **Concurrent_c** во всех остальных случаях.|  
  
`[ \@repl_freq = ] 'repl_freq'` Тип частоты репликации, *repl_freq* — **nvarchar (10)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**Непрерывная** (по умолчанию)|Издатель предоставляет выходные данные всех транзакций, записываемых в журнал. Для издателей, не относящихся [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] к, это требует, чтобы для *sync_method* было установлено значение **Concurrent_c**.|  
|**обновляем**|Издатель предоставляет только запланированные события синхронизации. Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей, отличных от, это требует, чтобы *sync_method* были установлены в **символьный**.|  
  
`[ \@description = ] 'description'` Необязательное описание публикации. *Description* имеет тип **nvarchar (255)** и значение по умолчанию NULL.  
  
`[ \@status = ] 'status'` Указывает, доступны ли данные публикации. *Status* имеет значение **nvarchar (8)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**active**|Публикация доступна подписчикам немедленно.|  
|**неактивен** (по умолчанию)|Публикация не доступна для подписчиков при создании (они могут подписаться, но эти подписки не обрабатываются).|  
  
 *Не поддерживается для издателей Oracle*.  
  
`[ \@independent_agent = ] 'independent_agent'` Указывает, существует ли изолированная агент распространения для этой публикации. *independent_agent* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение — true**, то для этой публикации существует изолированный агент распространения. Если **значение равно false**, публикация использует общий агент распространения, а каждая пара базы данных издателя или подписчика имеет один общий агент.  
  
`[ \@immediate_sync = ] 'immediate_synchronization'` Указывает, создаются ли файлы синхронизации для публикации при каждом запуске агент моментальных снимков. *immediate_synchronization* имеет тип **nvarchar (5)** и значение по умолчанию false. Если задано **значение true**, файлы синхронизации создаются или создаются повторно при каждом запуске агент моментальных снимков. Подписчики могут получить файлы синхронизации немедленно, если агент моментальных снимков завершил работу до создания подписки. Новые подписки получают самые свежие файлы синхронизации, сформированные при последнем выполнении агента моментальных снимков. чтобы *immediate_synchronization* **значение true**, *independent_agent* должно иметь **значение true** . Если задано **значение false**, файлы синхронизации создаются только при наличии новых подписок. При постепенном добавлении новой статьи в существующую публикацию необходимо вызвать [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) для каждой подписки. Подписчики не могут получать файлы синхронизации после подписки, пока агенты моментальных снимков не будут запущены и не завершат работу.  
  
`[ \@enabled_for_internet = ] 'enabled_for_internet'` Указывает, включена ли публикация в Интернете, и определяет, может ли использоваться протокол FTP для пересылки файлов моментальных снимков на подписчик. *enabled_for_internet* имеет тип **nvarchar (5)** и значение по умолчанию false. Если задано **значение true**, файлы синхронизации для публикации помещаются в каталог C:\PROGRAM Files\Microsoft SQL сервер\мсскл\мсскл.кс\реплдата\фтп. Пользователь должен создать каталог Ftp.  
  
`[ \@allow_push = ] 'allow_push'` Указывает, могут ли быть созданы принудительные подписки для данной публикации. *allow_push* имеет тип **nvarchar (5)** и значение по умолчанию true, которое разрешает принудительные подписки на публикацию.  
  
`[ \@allow_pull = ] 'allow_pull'` Указывает, могут ли быть созданы подписки по запросу для данной публикации. *allow_pull* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение равно false**, подписки по запросу не разрешены для публикации.  
  
`[ \@allow_anonymous = ] 'allow_anonymous'` Указывает, могут ли быть созданы анонимные подписки для данной публикации. *allow_anonymous* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение — true**, *immediate_synchronization* также должно иметь значение **true**. Если задано **значение false**, анонимные подписки не разрешены для публикации.  
  
`[ \@allow_sync_tran = ] 'allow_sync_tran'` Указывает, разрешены ли для публикации немедленно обновляемые подписки. *allow_sync_tran* имеет тип **nvarchar (5)** и значение по умолчанию false. **значение true** *не поддерживается для издателей Oracle*.  
  
`[ \@autogen_sync_procs = ] 'autogen_sync_procs'` Указывает, формируется ли на издателе синхронизированная хранимая процедура для обновляемых подписок. *autogen_sync_procs* имеет тип **nvarchar (5)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**true**|Устанавливается автоматически, если включена обновляемая подписка.|  
|**false**|Устанавливается автоматически для издателей Oracle или если обновляемая подписка выключена.|  
|NULL (по умолчанию)|По умолчанию имеет **значение true** , если обновляемые подписки включены, и **значение false** , если обновление подписок не включено.|  
  
> [!NOTE]  
>  Указанное пользователем значение для *autogen_sync_procs* будет переопределено в зависимости от значений, указанных для *allow_queued_tran* и *allow_sync_tran*.  
  
`[ \@retention = ] retention` Срок хранения в часах для действия подписки. параметр *retention* имеет **тип int** и значение по умолчанию 336 часов. Если подписка не активна в течение указанного периода, ее срок действия истекает, и она удаляется. Это значение может превышать максимальный срок хранения базы данных распространителя на издателе. Если значение **равно 0**, срок действия известных подписок на публикацию никогда не истечет и он будет удален агентом очистки истекшей подписки.  
  
`[ \@allow_queued_tran = ] 'allow_queued_updating'` Включает или отключает очередь изменений на подписчике, пока они не могут быть применены на издателе. *allow_queued_updating* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение равно false**, изменения на подписчике не помещаются в очередь. **значение true** *не поддерживается для издателей Oracle*.  
  
`[ \@snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Указывает, хранятся ли файлы моментальных снимков в папке по умолчанию. *snapshot_in_default_folder* имеет тип **nvarchar (5)** и значение по умолчанию true. Если **значение — true**, файлы моментальных снимков можно найти в папке по умолчанию. Если **значение равно false**, файлы моментальных снимков хранились в альтернативном расположении, указанном *alternate_snapshot_folder*. Другим местом может быть другой сервер, сетевой диск или съемный носитель (такой как компакт-диск или съемные диски). Файлы моментальных снимков можно также хранить на FTP-сайте, откуда подписчик позже может их получить. Обратите внимание, что этот параметр может иметь значение true и по-прежнему иметь расположение в параметре **\@ alt_snapshot_folder** . Это сочетание размещает файлы моментальных снимков одновременно и в месте по умолчанию, и в альтернативном месте.  
  
`[ \@alt_snapshot_folder = ] 'alternate_snapshot_folder'` Указывает расположение альтернативной папки для моментального снимка. *alternate_snapshot_folder* имеет тип **nvarchar (255)** и значение по умолчанию NULL.  
  
`[ \@pre_snapshot_script = ] 'pre_snapshot_script'` Задает указатель на расположение файла **SQL** . *pre_snapshot_script* имеет тип **nvarchar (255)** и значение по умолчанию NULL. Если моментальный снимок применяется на подписчике, то агент распространителя запускает предварительный скрипт моментального снимка до выполнения скриптов реплицируемых объектов. Скрипт выполняется в контексте безопасности, использованном для агента распространителя при подключении к базе данных подписки.  
  
`[ \@post_snapshot_script = ] 'post_snapshot_script'` Задает указатель на расположение файла **SQL** . *post_snapshot_script* имеет тип **nvarchar (255)** и значение по умолчанию NULL. Агент распространителя запускает заключительный скрипт после применения скриптов и данных всех реплицируемых объектов во время первоначальной синхронизации. Скрипт выполняется в контексте безопасности, использованном для агента распространителя при подключении к базе данных подписки.  
  
`[ \@compress_snapshot = ] 'compress_snapshot'`Указывает, что моментальный снимок, записываемый в **\@ alt_snapshot_folder** расположение, должен быть сжат в [!INCLUDE[msCoName](../../includes/msconame-md.md)] формате CAB. *compress_snapshot* имеет тип **nvarchar (5)** и значение по умолчанию false. **значение false** указывает, что моментальный снимок не будет сжиматься. **значение true** указывает, что моментальный снимок будет сжат. Нельзя сжать моментальные снимки размером более 2 гигабайт (ГБ). Сжатые файлы моментальных снимков распаковываются в расположении запуска агента распространителя. Со сжатыми моментальными снимками обычно используются подписки по запросу, чтобы сжатые файлы распаковывались на подписчике. Моментальный снимок в папке по умолчанию сжать нельзя.  
  
`[ \@ftp_address = ] 'ftp_address'` Сетевой адрес службы FTP для распространителя. Аргумент *ftp_address* имеет тип **sysname** и значение по умолчанию NULL. Указывает расположение файлов моментальных снимков публикаций для агента распространителя или агента слияния подписчика. Так как это свойство хранится для каждой публикации, каждая публикация может иметь разные *ftp_address*. Публикация должна поддерживать распространение моментальных снимков с помощью протокола FTP.  
  
`[ \@ftp_port = ] ftp_port` Номер порта службы FTP для распространителя. *ftp_port* имеет **тип int** и значение по умолчанию 21. Указывает расположение файлов моментальных снимков публикации для агента распространителя или агента слияния подписчика. Так как это свойство хранится для каждой публикации, каждая публикация может иметь собственный *ftp_port*.  
  
`[ \@ftp_subdirectory = ] 'ftp_subdirectory'` Указывает, где будут доступны файлы моментальных снимков для агент распространения или агент слияния подписчика, если публикация поддерживает распространение моментальных снимков по протоколу FTP. *ftp_subdirectory* имеет тип **nvarchar (255)** и значение по умолчанию NULL. Так как это свойство хранится для каждой публикации, каждая публикация может иметь собственный *ftp_subdirctory* или не иметь подкаталога, обозначенного значением NULL.  
  
`[ \@ftp_login = ] 'ftp_login'` Имя пользователя, используемое для подключения к службе FTP. Аргумент *ftp_login* имеет тип **sysname** и значение по умолчанию Anonymous.  
  
`[ \@ftp_password = ] 'ftp_password'` Пароль пользователя, используемый для подключения к службе FTP. Аргумент *ftp_password* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ \@allow_dts = ] 'allow_dts'` Указывает, что публикация допускает преобразования данных. При создании подписки можно указать пакет служб DTS. *allow_transformable_subscriptions* имеет тип **nvarchar (5)** и значение по умолчанию false, которое не допускает преобразования DTS. Если *allow_dts* имеет значение true, *sync_method* должен иметь значение либо **символ** , либо **Concurrent_c**.  
  
 **значение true** *не поддерживается для издателей Oracle*.  
  
`[ \@allow_subscription_copy = ] 'allow_subscription_copy'` Включает или отключает возможность копирования баз данных подписки, подписанных на эту публикацию. *allow_subscription_copy* имеет тип **nvarchar (5)** и значение по умолчанию false.  
  
`[ \@conflict_policy = ] 'conflict_policy'` Указывает политику разрешения конфликтов, после чего используется параметр подписчика, обновляемого посредством очередей. *conflict_policy* имеет тип **nvarchar (100)** со ЗНАЧЕНИЕМ по умолчанию NULL и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**pub wins**|Разрешение конфликта в пользу издателя.|  
|**sub reinit**|Повторная инициализация подписки.|  
|**sub wins**|Разрешение конфликта в пользу подписчика.|  
|NULL (по умолчанию)|Если значение равно NULL, а публикация является публикацией моментальных снимков, то политика по умолчанию становится **подпрограммой reinit**. Если значение NULL и публикация не является публикацией моментальных снимков, значение по умолчанию становится **Pub**.|  
  
 *Не поддерживается для издателей Oracle*.  
  
`[ \@centralized_conflicts = ] 'centralized_conflicts'` Указывает, хранятся ли на издателе конфликтующие записи. *centralized_conflicts* имеет тип **nvarchar (5)** и значение по умолчанию true. Если **значение — true**, конфликтующие записи хранятся на издателе. Если **значение равно false**, конфликтующие записи хранятся как на издателе, так и на подписчике, вызвавшем конфликт. *Не поддерживается для издателей Oracle*.  
  
`[ \@conflict_retention = ] conflict_retention` Указывает срок хранения конфликтов в днях. Время, в течение которого метаданные конфликта хранятся для одноранговой транзакционной репликации и подписок, обновляемых посредством очереди. *conflict_retention* имеет **тип int** и значение по умолчанию 14. *Не поддерживается для издателей Oracle*.  
  
`[ \@queue_type = ] 'queue_type'` Указывает, какой тип очереди используется. *queue_type* имеет тип **nvarchar (10)**, значение по умолчанию NULL и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**sql**|Для хранения транзакций используется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|NULL (по умолчанию)|По умолчанию используется значение **SQL**, которое указывает, что следует использовать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для хранения транзакций.|  
  
> [!NOTE]  
>  Поддержка использования [!INCLUDE[msCoName](../../includes/msconame-md.md)] очереди сообщений прекращена. Указание значения **MSMQ** приведет к появлению предупреждения, и репликация автоматически установит значение **SQL**.  
  
 *Не поддерживается для издателей Oracle*.  
  
`[ \@add_to_active_directory = ] 'add\to_active_directory'` Этот параметр является устаревшим и поддерживается только для обратной совместимости скриптов. Больше нельзя добавлять данные публикации в службу [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ \@logreader_job_name = ] 'logreader_agent_name'` Имя существующего задания агента. Аргумент *logreader_agent_name* имеет тип **sysname** и значение по умолчанию NULL. Он указывается только в том случае, если агент чтения журнала будет использовать существующее задание, а не создающееся заново.  
  
`[ \@qreader_job_name = ] 'queue_reader_agent_name'` Имя существующего задания агента. Аргумент *queue_reader_agent_name* имеет тип **sysname** и значение по умолчанию NULL. Он указывается, только если агент чтения очереди будет использовать существующее задание, а не создающееся заново.  
  
`[ \@publisher = ] 'publisher'` Указывает издателя, отличного от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аргумент *Publisher* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  *Издатель* не следует использовать при добавлении публикации в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издатель.  
  
`[ \@allow_initialize_from_backup = ] 'allow_initialize_from_backup'` Указывает, могут ли подписчики инициализировать подписку на эту публикацию из резервной копии, а не из исходного моментального снимка. *allow_initialize_from_backup* имеет тип **nvarchar (5)** и может принимать одно из следующих значений:  
  
|Значение|Описание|  
|-----------|-----------------|  
|**true**|Включает инициализацию из резервной копии.|  
|**false**|Отключает инициализацию из резервной копии.|  
|NULL (по умолчанию)|По умолчанию **имеет значение true** для публикации в одноранговой топологии репликации и **значение false** для всех остальных публикаций.|  
  
 Дополнительные сведения см. в статье [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!WARNING]  
>  Чтобы предотвратить отсутствие данных подписчика, при использовании процедуры **sp_addpublication** с `@allow_initialize_from_backup = N'true'`всегда используйте `@immediate_sync = N'true'`.  
  
`[ \@replicate_ddl = ] replicate_ddl` Указывает, поддерживается ли репликация схемы для публикации. *replicate_ddl* имеет **тип int** и значение по умолчанию **1** для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей и **0** для издателей, отличных от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **1** указывает, что инструкции языка описания данных DDL, выполняемые на издателе, реплицируются, а **0** означает, что инструкции DDL не реплицируются. *Репликация схемы не поддерживается для издателей Oracle.* Дополнительные сведения см. в статье [Внесение изменений в схемы баз данных публикации](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Параметр *\@ replicate_ddl* учитывается, когда инструкция DDL добавляет столбец. Параметр *\@ replicate_ddl* игнорируется, если инструкция DDL изменяет или удаляет столбец по следующим причинам.  
  
-   При удалении столбца столбцы sysarticlecolumns должны обновляться, чтобы новые инструкции DML не включали удаленный столбец, из-за чего работа агента распространителя завершилась бы ошибкой. Параметр *\@ replicate_ddl* игнорируется, так как репликация должна всегда реплицировать изменения схемы.  
  
-   Когда в столбец вносится изменение, исходный тип данных или возможность допустимости значения NULL может измениться, в результате чего инструкции DML будут содержать значение, которое может быть несовместимым с таблицей на подписчике. Такие инструкции DML могут привести к тому, что работа агента распространителя завершится ошибкой. Параметр *\@ replicate_ddl* игнорируется, так как репликация должна всегда реплицировать изменения схемы.  
  
-   Когда инструкция DDL добавляет новый столбец, в столбцах sysarticlecolumns этого нового столбца нет. Инструкции DML не будут пытаться реплицировать данные для этого нового столбца. Этот параметр учитывается потому, что допустимо как выполнение, так и не выполнение репликации DDL.  
  
`[ \@enabled_for_p2p = ] 'enabled_for_p2p'` Позволяет использовать публикацию в одноранговой топологии репликации. *enabled_for_p2p* имеет тип **nvarchar (5)** и значение по умолчанию false. **значение true** указывает, что публикация поддерживает одноранговую репликацию. При установке *enabled_for_p2p* в **значение true** применяются следующие ограничения.  
  
-   значение *allow_anonymous* должно быть **равно false**.  
  
-   значение *allow_dts* должно быть **равно false**.  
  
-   значение *allow_initialize_from_backup* должно быть **true**.  
  
-   значение *allow_queued_tran* должно быть **равно false**.  
  
-   значение *allow_sync_tran* должно быть **равно false**.  
  
-   значение *conflict_policy* должно быть **равно false**.  
  
-   значение *independent_agent* должно быть **true**.  
  
-   *repl_freq* должны быть **непрерывными**.  
  
-   *replicate_ddl* должен быть равен **1**.  
  
 Дополнительные сведения см. в разделе [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
`[ \@publish_local_changes_only = ] 'publish_local_changes_only'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ \@enabled_for_het_sub = ] 'enabled_for_het_sub'` Позволяет публикации поддерживать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от. *enabled_for_het_sub* имеет тип **nvarchar (5)** и значение по умолчанию false. Значение **true** означает, что публикация поддерживает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от. Если *enabled_for_het_sub* имеет **значение true**, применяются следующие ограничения.  
  
-   значение *allow_initialize_from_backup* должно быть **равно false**.  
  
-   значение *allow_push* должно быть **true**.  
  
-   значение *allow_queued_tran* должно быть **равно false**.  
  
-   значение *allow_subscription_copy* должно быть **равно false**.  
  
-   значение *allow_sync_tran* должно быть **равно false**.  
  
-   значение *autogen_sync_procs* должно быть **равно false**.  
  
-   *conflict_policy* должен иметь значение null.  
  
-   значение *enabled_for_internet* должно быть **равно false**.  
  
-   значение *enabled_for_p2p* должно быть **равно false**.  
  
-   *ftp_address* должен иметь значение null.  
  
-   *ftp_subdirectory* должен иметь значение null.  
  
-   *ftp_password* должен иметь значение null.  
  
-   *pre_snapshot_script* должен иметь значение null.  
  
-   *post_snapshot_script* должен иметь значение null.  
  
-   значение *replicate_ddl* должно быть равно 0.  
  
-   *qreader_job_name* должен иметь значение null.  
  
-   *queue_type* должен иметь значение null.  
  
-   *sync_method* не может быть **собственным** или **параллельным**.  
  
 Дополнительные сведения см. в статье [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
`[ \@p2p_conflictdetection = ] 'p2p_conflictdetection'` Позволяет агент распространения обнаруживать конфликты, если публикация включена для одноранговой репликации. *p2p_conflictdetection* имеет тип **nvarchar (5)** и значение по умолчанию true. Дополнительные сведения см. в разделе [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
`[ \@p2p_originator_id = ] p2p_originator_id` Указывает идентификатор для узла в одноранговой топологии. *p2p_originator_id* имеет **тип int** и значение по умолчанию NULL. Этот идентификатор используется для обнаружения конфликтов, если *p2p_conflictdetection* имеет значение true. Укажите положительное, ненулевое значение идентификатора, которое никогда не использовалось в топологии. Чтобы получить список уже использованных идентификаторов, выполните [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).  
  
`[ \@p2p_continue_onconflict = ] 'p2p_continue_onconflict'` Определяет, будет ли агент распространения продолжать обработку изменений после обнаружения конфликта. *p2p_continue_onconflict* имеет тип **nvarchar (5)** и значение по умолчанию false.  
  
> [!CAUTION]  
>  Рекомендуется использовать значение по умолчанию FALSE. Если присвоить этому аргументу значение TRUE, агент распространителя будет пытаться обеспечить конвергентность данных в топологии, применяя конфликтующую строку из узла с наибольшим значением идентификатора инициатора. Этот метод не гарантирует конвергенции. После обнаружения конфликта следует убедиться, что топология остается согласованной. Дополнительные сведения см. в подразделе «Обработка конфликтов» раздела [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
  
`[ \@allow_partition_switch = ] 'allow_partition_switch'` Указывает, будет ли ALTER TABLE... Операторы SWITCH можно выполнять в опубликованной базе данных. *allow_partition_switch* имеет тип **nvarchar (5)** и значение по умолчанию false. Дополнительные сведения см. в статье [Replicate Partitioned Tables and Indexes](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md) (Репликация секционированных таблиц и индексов).  
  
`[ \@replicate_partition_switch = ] 'replicate_partition_switch'` Указывает, будет ли ALTER TABLE... Инструкции SWITCH, которые выполняются в опубликованной базе данных, должны реплицироваться на подписчики. *replicate_partition_switch* имеет тип **nvarchar (5)** и значение по умолчанию false. Этот параметр допустим, только если *allow_partition_switch* имеет значение true.  

## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Комментарии  
 **sp_addpublication** используется в репликации моментальных снимков и репликации транзакций.  
  
 Если существует несколько публикаций, которые публикуют один и тот же объект базы данных, только публикации с *replicate_ddl* значением **1** будут реплицировать инструкции ALTER TABLE, ALTER VIEW, ALTER PROCEDURE, ALTER FUNCTION и ALTER TRIGGER DDL. Однако инструкция ALTER TABLE DROP COLUMN DDL будет реплицирована всеми публикациями, публикующими столбец, который был удален.  
  
 При включенной репликации DDL (*replicate_ddl*  =  **1**) для публикации, чтобы не выполнять репликацию изменений DDL в публикацию, необходимо сначала выполнить [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) , чтобы установить значение *replicate_ddl* равным **0**. После выполнения нереплицируемых инструкций DDL можно снова выполнить [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) , чтобы снова включить репликацию DDL.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-transa_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addpublication**. Имена входа, используемые для проверки подлинности Windows, должны иметь учетные записи пользователей в базе данных, представляющие их учетные записи пользователей Windows. Учетной записи пользователя, представляющей группу Windows, недостаточно.  
  
## <a name="see-also"></a>См. также:  
 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Публикация данных и объектов базы данных](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
