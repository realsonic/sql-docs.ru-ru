---
description: Инструкции RESTORE (Transact-SQL)
title: RESTORE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: 80e415f7f5029e96c6093b5cd72695201fa336d6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644369"
---
# <a name="restore-statements-transact-sql"></a>Инструкции RESTORE (Transact-SQL)

Восстанавливает резервные копии баз данных SQL, созданные с помощью команды BACKUP.

Щелкните одну из следующих вкладок, чтобы изучить синтаксис, аргументы, примечания, разрешения и примеры для используемой вам версии SQL.

Дополнительные сведения о соглашениях о синтаксисе см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Эта команда позволяет выполнить следующие сценарии восстановления.

- Восстановить базу данных из полной резервной копии (полное восстановление).
- Восстановить часть базы данных (частичное восстановление).
- Восстановить в базе данных определенные файлы или файловые группы (восстановление файлов).
- Восстановить в базе данных определенные страницы (восстановление страниц).
- Восстановить в базе данных журнал транзакций (восстановление журнала транзакций).
- Вернуть базу данных к моменту времени, на который был выполнен моментальный снимок базы данных.

Дополнительные сведения о сценариях восстановления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в статье [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md). Дополнительные сведения об аргументах см. в статье [Аргументы инструкций RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md). При восстановлении базы данных из другого экземпляра примите во внимание сведения из раздела [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).

> [!NOTE]
> Дополнительные сведения: [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
    [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
   | ,  <general_WITH_options> [ ,...n ]
   | , <replication_WITH_option>
   | , <change_data_capture_WITH_option>
   | , <FILESTREAM_WITH_option>
   | , <service_broker_WITH options>
   | , \<point_in_time_WITH_options-RESTORE_DATABASE>
   } [ ,...n ]
 ]
[;]

--To perform the first step of the initial restore sequence of a piecemeal restore:
RESTORE DATABASE { database_name | @database_name_var }
   <files_or_filegroups> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
      PARTIAL, NORECOVERY
      [  , <general_WITH_options> [ ,...n ]
       | , \<point_in_time_WITH_options-RESTORE_DATABASE>
      ] [ ,...n ]
[;]  
  
--To Restore Specific Files or Filegroups:
RESTORE DATABASE { database_name | @database_name_var }
   <file_or_filegroup> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
   {
      [ RECOVERY | NORECOVERY ]
      [ , <general_WITH_options> [ ,...n ] ]
   } [ ,...n ]
[;]  
  
--To Restore Specific Pages:
RESTORE DATABASE { database_name | @database_name_var }
   PAGE = 'file:page [ ,...n ]'
 [ , <file_or_filegroups> ] [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
       NORECOVERY
      [ , <general_WITH_options> [ ,...n ] ]
[;]

--To Restore a Transaction Log:
RESTORE LOG { database_name | @database_name_var }
 [ <file_or_filegroup_or_pages> [ ,...n ] ]
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
     [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
    | , <general_WITH_options> [ ,...n ]
    | , <replication_WITH_option>
    | , \<point_in_time_WITH_options-RESTORE_LOG>
   } [ ,...n ]
 ]
[;]

--To Revert a Database to a Database Snapshot:
RESTORE DATABASE { database_name | @database_name_var }
FROM DATABASE_SNAPSHOT = database_snapshot_name

<backup_device>::=
{
   { logical_backup_device_name |
      @logical_backup_device_name_var }
 | { DISK
     | TAPE
     | URL
   } = { 'physical_backup_device_name' |
      @physical_backup_device_name_var }
}
Note: URL is the format used to specify the location and the file name for the Microsoft Azure Blob. Although Microsoft Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seamless restore experience for all the three devices.
<files_or_filegroups>::=
{
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 | READ_WRITE_FILEGROUPS
}

<general_WITH_options> [ ,...n ]::=
--Restore Operation Options
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'
          [ ,...n ]
 | REPLACE
 | RESTART
 | RESTRICTED_USER | CREDENTIAL

--Backup Set Options
 | FILE = { backup_set_file_number | @backup_set_file_number }
 | PASSWORD = { password | @password_variable }

--Media Set Options
 | MEDIANAME = { media_name | @media_name_variable }
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
 | BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
 | { CHECKSUM | NO_CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Monitoring Options
 | STATS [ = percentage ]

--Tape Options.
 | { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

<replication_WITH_option>::=
 | KEEP_REPLICATION

<change_data_capture_WITH_option>::=
 | KEEP_CDC

<FILESTREAM_WITH_option>::=
 | FILESTREAM ( DIRECTORY_NAME = directory_name )

<service_broker_WITH_options>::=
 | ENABLE_BROKER
 | ERROR_BROKER_CONVERSATIONS
 | NEW_BROKER

\<point_in_time_WITH_options-RESTORE_DATABASE>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
   }

\<point_in_time_WITH_options-RESTORE_LOG>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
   }
```

## <a name="arguments"></a>Аргументы

Описание аргументов см. в статье [Аргументы инструкций RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

## <a name="about-restore-scenarios"></a>Сведения о сценариях восстановления

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает различные сценарии восстановления.

- полное восстановление базы данных

  Восстанавливает всю базу данных, начиная с полной резервной копии, за которой может последовать восстановление из копии разностной резервной копии базы данных (и резервных копий журналов). Дополнительные сведения см. в статьях [Выполнение полного восстановления базы данных (простая модель восстановления)](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) и [Выполнение полного восстановления базы данных (модель полного восстановления)](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).

- Восстановление файлов

  Восстанавливается файл или файловая группа в базе данных, содержащей несколько файловых групп. Следует заметить, что при использовании простой модели восстановления файл должен принадлежать к файловой группе, находящейся в режиме только для чтения. После полного восстановления файлов может быть восстановлена разностная резервная копия файлов. Дополнительные сведения см. в статьях [Файлы из резервных копий (модель полного восстановления)](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) и [Восстановление файлов (простая модель восстановления)](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).

- Восстановление страницы

  Производится восстановление отдельных страниц. Восстановление страницы возможно только в моделях полного восстановления и восстановления с неполным протоколированием. Дополнительные сведения см. в статье [Восстановление страниц (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md).

- Поэтапное восстановление

  Восстановление базы данных производится по этапам, начиная с первичной файловой группы и одной или нескольких вторичных файловых групп. Поэтапное восстановление начинается с инструкции RESTORE DATABASE с параметром PARTIAL и указания одной или нескольких восстанавливаемых вторичных файловых групп. Дополнительные сведения см. в статье [Поэтапное восстановление (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).

- Только восстановление

  Данные восстанавливаются в том случае, если они уже согласованы с базой данных и необходимо только сделать их доступными. Дополнительные сведения см. в статье [Восстановление базы данных без восстановления данных (Transact-SQL)](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).

- Восстановление журнала транзакций

  В моделях полного восстановления и восстановления с неполным протоколированием восстановление резервных копий журналов необходимо для достижения необходимой точки восстановления. Дополнительные сведения о восстановлении резервных копий журналов см. в статье [Применение резервных копий журналов транзакций (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).

- Подготовка базы данных доступности для группы доступности AlwaysOn

  Дополнительные сведения см. в статье [Подготовка базы данных-получателя для присоединения к группе доступности Always On](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).

- Подготовка зеркальной базы данных к зеркальному отображению

  Дополнительные сведения см. в статье [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).

- Восстановление в сети

  > [!NOTE]
  > Восстановление в сети допускается только в выпуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise.

Если поддерживается оперативное восстановление и база данных доступна в сети, автоматически выполняется оперативное восстановление файлов и страниц. Кроме того, после начального этапа поэтапного восстановления выполняется восстановление вторичной файловой группы.

  > [!NOTE]
  > При оперативном восстановлении могут использоваться [отложенные транзакции](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).

Дополнительные сведения см. в статье [Восстановление в сети (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md).

## <a name="additional-considerations-about-restore-options"></a>Дополнительные сведения о параметрах инструкции RESTORE

### <a name="discontinued-restore-keywords"></a>Неподдерживаемые ключевые слова инструкции RESTORE

В [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] следующие ключевые слова больше не поддерживаются.

|Неподдерживаемое ключевое слово|Заменены на...|Пример заменяющего ключевого слова|
|--------------------------|------------------|------------------------------------|
|LOAD|RESTORE|`RESTORE DATABASE`|
|TRANSACTION|LOG|`RESTORE LOG`|
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|

### <a name="restore-log"></a>RESTORE LOG

Инструкция RESTORE LOG может включать список файлов, создаваемых во время наката. Эта возможность используется, если резервная копия журналов содержит журнальные записи, произведенные во время добавления файла в базу данных.

> [!NOTE]
> Для базы данных, использующей модель полного восстановления или модель восстановления с неполным протоколированием, необходимо, чтобы перед восстановлением базы данных была создана резервная копия конца журнала. Восстановление базы данных без создания резервной копии заключительного фрагмента журнала приведет к ошибке, если инструкция RESTORE DATABASE не содержит предложение WITH REPLACE или WITH STOPAT, в котором должно указываться время или транзакция, выполняемая после завершения резервного копирования данных. Дополнительные сведения о резервных копиях заключительного фрагмента журнала см. в статье [Резервные копии заключительного фрагмента журнала (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

### <a name="comparison-of-recovery-and-norecovery"></a>Сравнение параметров RECOVERY и NORECOVERY
Откат контролируется инструкцией RESTORE при помощи параметров [ RECOVERY | NORECOVERY ].

- Параметр NORECOVERY указывает на то, что откат не выполняется. Это позволяет продолжать накат при помощи следующей инструкции в последовательности.

  В этом случае последовательность восстановления может восстановить другие резервные копии и выполнить их накат.

- Параметр RECOVERY (параметр по умолчанию) указывает на то, что откат должен быть выполнен после завершения наката для текущей резервной копии.

  Для восстановления базы данных необходимо, чтобы весь набор восстанавливаемых данных (*набор данных наката*) был согласован с базой данных. Если набор данных наката, необходимый для согласования с базой данных, достаточно велик и указан параметр RECOVERY, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] формирует ошибку. Дополнительные сведения о процессе восстановления см. в статье [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

## <a name="compatibility-support"></a>Поддержка совместимости
Резервные копии баз данных **master**, **model** и **msdb**, созданные в более ранней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], не могут быть восстановлены в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].

> [!NOTE]
> Резервная копия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может быть восстановлена на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имеющем более раннюю версию.

В каждой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется путь по умолчанию, отличный от пути в предыдущих версиях. Поэтому, чтобы восстановить из резервной копии базу данных, созданную в расположении по умолчанию для архивов предыдущих версий, необходимо использовать параметр MOVE. Сведения о новом пути по умолчанию см. в разделе [Расположение файлов для экземпляра по умолчанию и именованных экземпляров SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

Если восстановить базу данных предыдущей версии до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], то эта база данных автоматически обновится. Как правило, база данных сразу становится доступной. Но если база данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] содержит полнотекстовые индексы, при обновлении будет выполнен их импорт, сброс или повторное создание в зависимости от установленного значения свойства сервера **upgrade_option**. Если при обновлении выбран режим импорта (**upgrade_option** = 2) или перестроения (**upgrade_option** = 0), полнотекстовые индексы во время обновления будут недоступны. В зависимости от объема индексируемых данных процесс импорта может занять несколько часов, а перестроение — в несколько (до десяти) раз больше. Обратите внимание, что если для обновления выбран режим «Импортировать», а полнотекстовый каталог недоступен, то связанные с ним полнотекстовые индексы будут перестроены. Чтобы изменить значение свойства сервера **upgrade_option** , следует использовать процедуру [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).

При первом присоединении базы данных к новому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или ее восстановлении копия главного ключа базы данных (зашифрованная главным ключом службы) еще не хранится на сервере. Необходимо расшифровать главный ключ базы данных с помощью инструкции **OPEN MASTER KEY**. Как только главный ключ базы данных будет расшифрован, появится возможность разрешить автоматическую расшифровку в будущем с помощью инструкции **ALTER MASTER KEY REGENERATE**, чтобы оставить на сервере копию главного ключа базы данных, зашифрованного с помощью главного ключа службы. После обновления базы данных с переходом от более ранней версии главный ключ базы данных должен быть создан повторно для использования нового алгоритма шифрования AES. См. дополнительные сведения о [повторном создании главного ключа базы данных](../../t-sql/statements/alter-master-key-transact-sql.md). Время, необходимое для повторного создания главного ключа базы данных с обновлением до алгоритма шифрования AES, зависит от числа объектов, защищаемых главным ключом базы данных. Повторное создание главного ключа базы данных с обновлением до алгоритма шифрования AES необходимо произвести только один раз. Это никак не повлияет на последующие операции повторного создания, выполняемые в соответствии со стратегией смены ключей.

## <a name="general-remarks"></a>Общие замечания
Во время автономного восстановления, если указанная база данных используется, функция RESTORE после короткой задержки принудительно отключает пользователей. Для восстановления в режиме «в сети» непервичной файловой группы база данных может продолжать использоваться, за исключением тех случаев, когда восстанавливаемая файловая группа переводится в режим вне сети. Данные в указанной базе данных заменяются восстановленными данными.

Операции восстановления между несколькими платформами, даже между процессорами различных типов, могут выполняться, если параметры сортировки базы данных поддерживаются операционной системой.

После возникновения ошибки инструкция RESTORE может быть запущена вновь. Кроме того, можно указать, чтобы, несмотря на ошибки, инструкция RESTORE продолжала работу и восстановила как можно большее количество данных (см. параметр `CONTINUE_AFTER_ERROR`).

Инструкция RESTORE недопустима в явной или неявной транзакции.

Восстановление поврежденной базы данных **master** выполняется при помощи специальной процедуры. Дополнительные сведения см. в статье [Резервное копирование и восстановление системных баз данных (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).

Восстановление базы данных из копии удаляет кэш планов для восстанавливаемой базы данных. Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. 

Чтобы восстановить базу данных доступности, сначала восстановите базу данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем добавьте базу данных в группу доступности.

## <a name="interoperability"></a>Совместимость

### <a name="database-settings-and-restoring"></a>Настройка базы данных и восстановление

Во время восстановления большинство параметров базы данных, устанавливаемых при помощи инструкции ALTER DATABASE, сбрасывается в значения, в которых они находились в конце резервного копирования.

Однако использование параметра WITH RESTRICTED_USER переопределяет этот режим работы для настройки параметра доступа пользователя. Эта настройка всегда сохраняется, если инструкция RESTORE включает параметр WITH RESTRICTED_USER.

### <a name="restoring-an-encrypted-database"></a>Восстановление зашифрованной базы данных

Чтобы восстановить зашифрованную базу данных, необходимо иметь доступ к сертификату или асимметричному ключу, который использовался для шифрования базы данных. Без сертификата или асимметричного ключа восстановить базу данных нельзя. Поэтому сертификат, используемый для шифрования ключа шифрования базы данных, должен храниться в течение всего времени, пока есть необходимость в резервной копии. Дополнительные сведения см. в статье [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Восстановление базы данных с включенным форматом хранения vardecimal

Резервное копирование и восстановление правильно работают с форматом хранения **vardecimal**. Дополнительные сведения о формате хранения **vardecimal** см. в статье [sp_db_vardecimal_storage_format (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).

### <a name="restore-full-text-data"></a>Восстановление полнотекстовых данных

Во время полного восстановления полнотекстовые данные восстанавливаются вместе с другими данными базы данных. С помощью обычного синтаксиса `RESTORE DATABASE database_name FROM backup_device` полнотекстовые файлы восстанавливаются как часть файлов базы данных.

Инструкция RESTORE может также использоваться для восстановления в разные расположения, для разностного восстановления, восстановления файлов и файловых групп, а также для восстановления файлов и файловых групп полнотекстовых данных. Кроме того, инструкция RESTORE может восстанавливать как полнотекстовые файлы, так и файлы с данными базы данных.

> [!NOTE]
> Полнотекстовые каталоги, импортированные из [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], по-прежнему рассматриваются как файлы базы данных. Для них остается применимой процедура [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] по резервному копированию полнотекстовых каталогов, за исключением того, что более нет необходимости использовать паузу и возобновление в процессе выполнения резервного копирования. Дополнительные сведения см. в разделе [Резервное копирование и восстановление полнотекстовых каталогов](https://go.microsoft.com/fwlink/?LinkId=107381).

### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="metadata"></a>Метаданные

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] включает таблицы журналов резервного копирования и восстановления, в которые заносятся данные слежения за резервным копированием и восстановлением для каждого экземпляра сервера. При выполнении восстановления таблицы журнала резервного копирования также изменяются. Сведения об этих таблицах см. в статье [Журнал и сведения о заголовке резервной копии (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).

## <a name="replace-option-impact"></a><a name="REPLACEoption"></a> Влияние параметра REPLACE
Параметр REPLACE должен использоваться редко и после тщательного анализа. Восстановление обычно не допускает случайной перезаписи базы данных другой базой данных. Если указанная в инструкции RESTORE база данных уже существует на текущем сервере, а идентификатор GUID семейства для заданной базы данных отличается от идентификатора GUID семейства для базы данных, записанного в резервном наборе данных, то ее восстановление не будет выполнено. Это является важной защитной мерой.

Параметр REPLACE отменяет несколько важных проверок, обычно выполняемых операцией восстановления. Отменяются следующие проверки.

- Проверка на восстановление поверх существующей базы данных резервной копии, созданной для другой базы данных.

  При использовании параметра REPLACE при восстановлении можно записать данные поверх существующей базы данных независимо от того, какие базы данных содержатся в резервном наборе данных, даже если указанное имя данных отличается от записанного в резервном наборе. Это может привести к случайной перезаписи поверх базы данных другой базы данных.
  
- Проверка на восстановление базы данных, использующей модель полного восстановления или модель восстановления с неполным протоколированием, для которой не была создана резервная копия заключительного фрагмента журнала, и не был применен параметр STOPAT.

  При использовании параметра REPLACE возможна потеря зафиксированных данных, поскольку последние записанные в журнал данные еще не были скопированы в резервную копию.

- Перезапись существующих файлов.

  Проверка на перезапись существующих файлов неверного типа (например XLS-файлов) или файлов, используемых другой базой данных, в данный момент находящейся не в режиме в сети. Возможна случайная потеря данных в случае перезаписи существующих файлов, несмотря на то, что восстановленная база данных будет полной.

## <a name="redoing-a-restore"></a>Повторное восстановление
Отменить результаты восстановления невозможно, однако можно отменить результаты копирования данных и произвести накат для каждого файла по отдельности. Для перезапуска восстановите нужный файл и выполните накат повторно. Например, если случайно было восстановлено слишком много резервных копий журналов и нужная точка остановки восстановления пройдена, нужно перезапустить последовательность.

Последовательность восстановления может быть прервана и перезапущена при восстановлении всего содержимого соответствующих файлов.

## <a name="reverting-a-database-to-a-database-snapshot"></a>Восстановление базы данных к моментальному снимку базы данных
*Операция восстановления базы данных*, указываемая с помощью параметра DATABASE_SNAPSHOT, переносит всю базу данных-источник назад во времени, восстанавливая ее на момент создания моментального снимка базы данных, т. е. перезаписывая базу данных-источник данными из указанного моментального снимка. В данный момент может существовать только моментальный снимок, к которому выполняется восстановление. Затем операция отката вновь создает журнал (поэтому впоследствии нельзя будет выполнить накат возвращенной базы данных к точке пользовательской ошибки).

Потеря данных затронет только изменения в базе данных, произведенные после создания моментального снимка. Метаданные возвращенной базы данных соответствуют метаданным времени создания моментального снимка. Однако при возвращении к моментальному снимку удаляются все полнотекстовые каталоги.

Возврат из моментальных снимков базы данных не предназначен для восстановления носителей. В отличие от обычного резервного набора данных, моментальный снимок базы данных является неполной копией файлов базы данных. В том случае, если база данных или моментальный снимок базы данных повреждены, возврат из моментального снимка может оказаться невозможным. Более того, даже если оно окажется возможным, восстановление в случае повреждения вряд ли поможет устранить проблему.

### <a name="restrictions-on-reverting"></a>Ограничения на возврат
Возврат не поддерживается в следующих условиях.

- Если база данных-источник содержит файловые группы, доступные только для чтения, или сжатые файловые группы.
- Если какие-либо файлы, работавшие в режиме «вне сети» во время создания моментального снимка, сейчас работают автономно.
- Если в настоящий момент существует несколько моментальных снимков базы данных.

Дополнительные сведения см. в разделе [Восстановление базы данных до состояния, сохраненного в моментальном снимке](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

## <a name="security"></a>Безопасность
В операции создания резервной копии могут дополнительно указываться пароли для набора носителей, резервного набора данных или и того и другого. Если для набора носителей или резервного набора данных установлен пароль, то в инструкции RESTORE необходимо указывать правильные пароли. Эти пароли предотвращают несанкционированные операции восстановления и присоединения резервных наборов данных к носителю при помощи инструментальных средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако защищенный паролем носитель может быть переписан инструкцией BACKUP с параметром FORMAT.

> [!IMPORTANT]
> Данный пароль не обеспечивает надежную защиту. Он предназначен для предотвращения неверного восстановления при использовании средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] авторизованными или неавторизованными пользователями. При этом остается возможным чтение данных резервных копий с помощью других средств или замена пароля. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Рекомендуемым способом защиты резервных копий является хранение лент с резервными копиями в безопасном месте или создание резервных копий на диске в виде файлов, защищенных соответствующими списками управления доступом (ACL). Списки ACL должны располагаться в корневом каталоге, в котором создаются резервные копии.

> [!NOTE]
> Сведения о резервном копировании и восстановлении SQL Server в хранилище BLOB-объектов Microsoft Azure см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

### <a name="permissions"></a>Разрешения
Если восстанавливаемая база данных не существуют, для выполнения инструкции RESTORE у пользователя должны быть разрешения `CREATE DATABASE`. Если база данных существует, нужно восстановить (RESTORE) разрешения по умолчанию для членов предопределенных ролей сервера `sysadmin` и `dbcreator` и владельца (`dbo`) базы данных (для параметра `FROM DATABASE_SNAPSHOT` база данных всегда существует).

Разрешения на выполнение инструкции RESTORE даются ролям, в которых данные о членстве всегда доступны серверу. Так как членство в предопределенной роли базы данных может быть проверено только тогда, когда база данных доступна и не повреждена, что не всегда имеет место при выполнении инструкции RESTORE, члены предопределенной роли базы данных `db_owner` не имеют разрешений RESTORE.

## <a name="examples"></a><a name="examples"></a> Примеры

Во всех примерах предполагается, что выполнено полное резервное копирование базы данных.

Примеры выполнения инструкции RESTORE.

- A. [Восстановление всей базы данных](#restoring_full_db)
- Б. [Восстановление полной и разностной резервных копий базы данных](#restoring_full_n_differential_db_backups)
- В. [Восстановление базы данных с использованием синтаксиса RESTART](#restoring_db_using_RESTART)
- Г. [Восстановление базы данных и перемещение файлов](#restoring_db_n_move_files)
- Д. [Копирование базы данных с помощью инструкций BACKUP и RESTORE](#copying_db_using_bnr)
- Е. [Восстановление состояния на определенный момент времени с помощью STOPAT](#restoring_to_pit_using_STOPAT)
- Ж. [Восстановление журнала транзакций до метки](#restoring_transaction_log_to_mark)
- З. [Восстановление с использованием синтаксиса TAPE](#restoring_using_TAPE)
- И. [Восстановление с использованием синтаксиса FILE и FILEGROUP](#restoring_using_FILE_n_FG)
- К. [Восстановление базы данных из моментального снимка](#reverting_from_db_snapshot)
- Л. [Восстановление из службы хранилища BLOB-объектов Microsoft Azure](#Azure_Blob)

> [!NOTE]
> Дополнительные примеры см. в разделах по восстановлению из статьи [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

### <a name="a-restoring-a-full-database"></a><a name="restoring_full_db"></a> A. Восстановление всей базы данных

В следующем примере производится восстановление базы данных из полной резервной копии, находящейся на логическом устройстве резервного копирования на магнитной ленте `AdventureWorksBackups`. Пример создания этого устройства см. в разделе [Устройства резервного копирования](../../relational-databases/backup-restore/backup-devices-sql-server.md).

```sql
RESTORE DATABASE AdventureWorks2012
  FROM AdventureWorks2012Backups;
```

> [!NOTE]
> Для базы данных, использующей модель полного восстановления или модель восстановления с неполным протоколированием, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в большинстве случаев требует, чтобы перед восстановлением базы данных была создана резервная копия конца журнала. Дополнительные сведения см. в статье [Резервные копии заключительного фрагмента журнала (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

[[В начало списка примеров]](#examples)

### <a name="b-restoring-full-and-differential-database-backups"></a><a name="restoring_full_n_differential_db_backups"></a> Б. Восстановление полной и разностной резервных копий базы данных

В данном примере производится восстановление полной резервной копии базы данных, за которым следует восстановление из разностной резервной копии с устройства резервного копирования `Z:\SQLServerBackups\AdventureWorks2012.bak`, на котором содержатся обе резервные копии. Полная резервная копия базы данных — это шестой резервный набор данных на устройстве (`FILE = 6`), а разностная резервная копия базы данных — девятый резервный набор данных на устройстве (`FILE = 9`). База данных будет восстановлена после восстановления разностной резервной копии.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 6
      NORECOVERY;
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 9
      RECOVERY;
```

[[В начало списка примеров]](#examples)

### <a name="c-restoring-a-database-using-restart-syntax"></a><a name="restoring_db_using_RESTART"></a> В. Восстановление базы данных с использованием синтаксиса RESTART

В данном примере параметр `RESTART` используется для перезапуска операции `RESTORE`, прерванной ошибкой питания на сервере.

```sql
-- This database RESTORE halted prematurely due to power failure.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups;
-- Here is the RESTORE RESTART operation.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups WITH RESTART;
```

[[В начало списка примеров]](#examples)

### <a name="d-restoring-a-database-and-move-files"></a><a name="restoring_db_n_move_files"></a> Г. Восстановление базы данных и перемещение файлов

В следующем примере производится полное восстановление базы данных и журнала транзакций, после чего восстановленная база данных перемещается в каталог `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH NORECOVERY,
      MOVE 'AdventureWorks2012_Data' TO
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',
      MOVE 'AdventureWorks2012_Log'
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH RECOVERY;
```

[[В начало списка примеров]](#examples)

### <a name="e-copying-a-database-using-backup-and-restore"></a><a name="copying_db_using_bnr"></a> Д. Копирование базы данных с помощью BACKUP и RESTORE

В следующем примере с помощью инструкций `BACKUP` и `RESTORE` создается копия базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Инструкция `MOVE` восстанавливает файлы данных и журнала в указанные места. Количество и имена восстанавливаемых файлов базы данных можно определить с помощью инструкции `RESTORE FILELISTONLY`. Новая копия базы данных называется `TestDB`. Дополнительные сведения см. в статье [Инструкции RESTORE — FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).

```sql
BACKUP DATABASE AdventureWorks2012
    TO AdventureWorksBackups ;

RESTORE FILELISTONLY
    FROM AdventureWorksBackups ;

RESTORE DATABASE TestDB
    FROM AdventureWorksBackups
    WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',
    MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';
GO
```

[[В начало списка примеров]](#examples)

### <a name="f-restoring-to-a-point-in-time-using-stopat"></a><a name="restoring_to_pit_using_STOPAT"></a> Е. Восстановление состояния на определенный момент времени с помощью STOPAT

В следующем примере база данных восстанавливается в состояние на `12:00 AM``April 15, 2020` и демонстрируется операция восстановления, использующая несколько резервных копий журналов. На устройстве резервного копирования `AdventureWorksBackups` полная резервная копия базы данных, подлежащей восстановлению, — это третий резервный набор данных на устройстве (`FILE = 3`), резервная копия первого журнала — это четвертый резервный набор (`FILE = 4`), резервная копия второго журнала — это пятый резервный набор (`FILE = 5`).

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=3, NORECOVERY;
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;

```

[[В начало списка примеров]](#examples)

### <a name="g-restoring-the-transaction-log-to-a-mark"></a><a name="restoring_transaction_log_to_mark"></a> G. Восстановление журнала транзакций до метки

В следующем примере журнал транзакций восстанавливается до метки в помеченной транзакции с именем `ListPriceUpdate`.

```sql
USE AdventureWorks2012
GO
BEGIN TRANSACTION ListPriceUpdate
    WITH MARK 'UPDATE Product list prices';
GO

UPDATE Production.Product
    SET ListPrice = ListPrice * 1.10
    WHERE ProductNumber LIKE 'BK-%';
GO

COMMIT TRANSACTION ListPriceUpdate;
GO

-- Time passes. Regular database
-- and log backups are taken.
-- An error occurs in the database.
USE master;
GO

RESTORE DATABASE AdventureWorks2012
FROM AdventureWorksBackups
WITH FILE = 3, NORECOVERY;
GO

RESTORE LOG AdventureWorks2012
  FROM AdventureWorksBackups
    WITH FILE = 4,
    RECOVERY,
    STOPATMARK = ListPriceUpdate;
```

[[В начало списка примеров]](#examples)

### <a name="h-restoring-using-tape-syntax"></a><a name="restoring_using_TAPE"></a> H. Восстановление с использованием синтаксиса TAPE

В следующем примере производится восстановление базы данных из полной резервной копии, находящейся на устройстве резервного копирования `TAPE`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM TAPE = '\\.\tape0';
```

[[В начало списка примеров]](#examples)

### <a name="i-restoring-using-file-and-filegroup-syntax"></a><a name="restoring_using_FILE_n_FG"></a> I. Восстановление с использованием синтаксиса FILE и FILEGROUP

В следующем примере производится восстановление базы данных `MyDatabase`, содержащей два файла, одну вторичную файловую группу и один журнал транзакций. Эта база данных использует модель полного восстановления.

Резервная копия базы данных представляет собой девятый резервный набор данных в наборе носителей на логическом устройстве резервного копирования с именем `MyDatabaseBackups`. Затем производится восстановление трех резервных копий журнала из следующих трех резервных наборов данных (`10`, `11` и `12`) на устройстве `MyDatabaseBackups` с помощью предложения `WITH NORECOVERY`. База данных будет восстановлена после восстановления последней резервной копии журналов.

> [!NOTE]
> Восстановление выполняется как отдельный шаг с целью снижения возможности преждевременного восстановления по журналу, до того как будут восстановлены все резервные копии журналов. Дополнительные сведения о процессе восстановления см. в статье [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

Обратите внимание на два типа параметров `RESTORE DATABASE` в инструкции `FILE`. Параметры `FILE`, предшествующие имени устройства резервного копирования, указывают логические имена файлов базы данных, которые будут восстановлены из резервного набора данных, например `FILE = 'MyDatabase_data_1'`. Этот резервный набор данных не является первой резервной копией базы данных в наборе носителей, поэтому его позиция указывается параметром `FILE` в предложении `WITH`: `FILE=9`.

```sql
RESTORE DATABASE MyDatabase
    FILE = 'MyDatabase_data_1',
    FILE = 'MyDatabase_data_2',
    FILEGROUP = 'new_customers'
    FROM MyDatabaseBackups
    WITH
      FILE = 9,
      NORECOVERY;
GO  
-- Restore the log backups
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 10,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 11,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 12,
      NORECOVERY;
GO
--Recover the database
RESTORE DATABASE MyDatabase WITH RECOVERY;
GO
```

[[В начало списка примеров]](#examples)

### <a name="j-reverting-from-a-database-snapshot"></a><a name="reverting_from_db_snapshot"></a> J. Возврат из моментального снимка базы данных

В следующем примере производится восстановление базы данных к моментальному снимку базы данных. В этом примере предполагается, что в базе данных в настоящее время существует только один моментальный снимок. Пример создания этого моментального снимка базы данных см. в [этой статье](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).

> [!NOTE]
> Восстановление до моментального снимка удаляет все полнотекстовые каталоги.

```sql
USE master;
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';
GO
```

Дополнительные сведения см. в разделе [Восстановление базы данных до состояния, сохраненного в моментальном снимке](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

[[В начало списка примеров]](#examples)

### <a name="k-restoring-from-the-microsoft-azure-blob-storage-service"></a><a name="Azure_Blob"></a> K. Восстановление из службы хранилища BLOB-объектов Microsoft Azure

В следующих трех примерах используется служба хранилища Microsoft Azure. Имя учетной записи хранилища — `mystorageaccount`. Контейнер для файлов данных называется `myfirstcontainer`. Контейнер для файлов резервных копий называется `mysecondcontainer`. Хранимая политика доступа была создана с правами на чтение, запись, удаление и составление списков для каждого контейнера. Учетные данные SQL Server были созданы с использованием подписанных URL-адресов, которые связаны с хранимыми политиками доступа. Сведения о резервном копировании и восстановлении SQL Server в хранилище BLOB-объектов Microsoft Azure см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

**K1. Восстановление полной резервной копии базы данных из службы хранилища Microsoft Azure**    
Полная резервная копия базы данных в `mysecondcontainer` из `Sales` будет восстановлена в `myfirstcontainer`. `Sales` в настоящее время не существует на сервере.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

**K2. Восстановление полной резервной копии базы данных из службы хранилища Microsoft Azure в локальное хранилище.** Полная резервная копия базы данных `Sales`, расположенная в `mysecondcontainer`, будет восстановлена в локальное хранилище. `Sales` в настоящее время не существует на сервере.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf',
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf',
  STATS = 10;
```

**K3. Восстановление полной резервной копии базы данных из локального хранилища в службу хранилища Microsoft Azure**

```sql
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

[[В начало списка примеров]](#examples)

## <a name="more-information"></a>Дополнительные сведения

[Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)     
[Резервное копирование и восстановление баз данных SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)    
[Резервное копирование и восстановление системных баз данных (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)      
[Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)     
[Создание резервных копий и восстановление полнотекстовых каталогов и индексов](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)      
[Создание резервной копии и восстановление из копий реплицируемых баз данных](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)      
[BACKUP](../../t-sql/statements/restore-statements-transact-sql.md)      
[Наборы носителей, семейства носителей и резервные наборы данных](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)      
[RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)     
[RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)     
[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)     
[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)     
[Журнал и сведения о заголовке резервной копии (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)       

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-2017&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Управляемый экземпляр SQL Azure

Эта команда позволяет восстановить всю базу данных из полной резервной копии (полное восстановление) в учетной записи хранилища BLOB-объектов Azure.

Другие поддерживаемые команды RESTORE:

- [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY ONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md);
- [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).

> [!IMPORTANT]
> Сведения о восстановлении из автоматических резервных копий Управляемого экземпляра SQL Azure см. в статье [Восстановление базы данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups).

## <a name="syntax"></a>Синтаксис

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 FROM URL = { 'physical_device_name' | @physical_device_name_var } [ ,...n ]
[;]

```

## <a name="arguments"></a>Аргументы

DATABASE

Указывает целевую базу данных.

FROM URL

Указывает одно устройство резервного копирования или несколько по URL-адресам, которые будут использоваться для операции восстановления. Формат URL-адреса используется для восстановления резервных копий из службы хранилища Microsoft Azure.

> [!IMPORTANT]
> Чтобы выполнить восстановление с нескольких устройств при помощи URL-адреса, необходимо использовать токены подписанных URL-адресов (SAS). Примеры создания подписанного URL-адреса см. в разделах [Резервное копирование SQL Server на URL-адрес](../../relational-databases/backup-restore/sql-server-backup-to-url.md) и [Упрощение создания учетных данных SQL с токенами подписанных URL-адресов в хранилище Azure с помощью Powershell](https://docs.microsoft.com/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell).

*n* — заполнитель, который показывает, что можно указать до 64 устройств резервного копирования через запятую.

## <a name="general-remarks"></a>Общие замечания

Перед началом необходимо создать учетные данные с именем, совпадающим с URL-адресом учетной записи хранения BLOB-объектов и подписанным URL-адресом, сохраненным в виде секрета. Команда RESTORE выполнит поиск учетных данных с помощью URL-адреса хранилища BLOB-объектов, чтобы найти сведения, необходимые для считывания данных с устройства резервного копирования.

Операция RESTORE асинхронна и будет продолжаться даже в случае разрыва соединения с клиентом. В случае разрыва соединения вы можете просмотреть состояние операции восстановления (а также инструкций CREATE и DROP для базы данных) в представлении [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md).

Следующие параметры базы данных устанавливаются или переопределяются и в последующем не могут быть изменены:

- NEW_BROKER (если брокер не включен в BAK-файле)
- ENABLE_BROKER (если брокер не включен в BAK-файле)
- AUTO_CLOSE=OFF (если для базы данных в BAK-файле установлен параметр AUTO_CLOSE=ON)
- RECOVERY FULL (если для базы данных в BAK-файле установлен режим восстановления SIMPLE или BULK_LOGGED)
- Добавляется оптимизированная для операций в памяти файловая группа с названием XTP (если она отсутствовала в исходном BAK-файле). Любые существующие оптимизированные для операций в памяти файловые группы переименовываются в XTP
- Параметры SINGLE_USER и RESTRICTED_USER преобразуются в MULTI_USER

## <a name="limitations---sql-managed-instance"></a>Ограничения — Управляемый экземпляр SQL

Применяются следующие ограничения:

- BAK-файлы, содержащие несколько резервных наборов данных, не могут быть восстановлены.
- BAK-файлы, содержащие несколько файлов журнала, не могут быть восстановлены.
- Если BAK-файл содержит данные FILESTREAM, восстановление завершится сбоем.
- Резервные копии, которые содержат базы данных с активными объектами в памяти, невозможно восстановить на уровне производительности "Общего назначения".
- На данный момент невозможно восстановление резервных копий, которые содержат базы данных, находящиеся в режиме только для чтения. В ближайшее время эти ограничения будут устранены.

Дополнительные сведения см. в статье [Управляемый экземпляр SQL Azure](/azure/sql-database/sql-database-managed-instance).

## <a name="restoring-an-encrypted-database"></a>Восстановление зашифрованной базы данных
Чтобы восстановить зашифрованную базу данных, необходимо иметь доступ к сертификату или асимметричному ключу, который использовался для шифрования базы данных. Без сертификата или асимметричного ключа восстановить базу данных нельзя. Поэтому сертификат, используемый для шифрования ключа шифрования базы данных, должен храниться в течение всего времени, пока есть необходимость в резервной копии. Дополнительные сведения см. в статье [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="permissions"></a>Разрешения
У пользователя должны быть разрешения `CREATE DATABASE`, чтобы он имел возможность выполнять восстановление.

```sql
CREATE LOGIN mylogin WITH PASSWORD = 'Very Strong Pwd123!';
GRANT CREATE ANY DATABASE TO [mylogin];
```

Разрешения на выполнение инструкции RESTORE даются ролям, в которых данные о членстве всегда доступны серверу. Так как членство в предопределенной роли базы данных может быть проверено только тогда, когда база данных доступна и не повреждена, что не всегда имеет место при выполнении инструкции RESTORE, члены предопределенной роли базы данных `db_owner` не имеют разрешений RESTORE.

## <a name="examples"></a><a name="examples"></a> Примеры

В следующих примерах при помощи URL-адреса выполняется восстановление резервной копии базы данных только для копирования, а также создаются учетные данные.

### <a name="a-restore-database-from-four-backup-devices"></a><a name="restore-mi-database"></a> A. Восстановление базы данных с четырех устройств резервного копирования

```sql

-- Create credential
CREATE CREDENTIAL [https://mybackups.blob.core.windows.net/wide-world-importers]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
      SECRET = 'sv=2017-11-09&ss=bq&srt=sco&sp=rl&se=2022-06-19T22:41:07Z&st=2018-06-01T14:41:07Z&spr=https&sig=s7wddcf0w%3D';
GO
-- Restore database
RESTORE DATABASE WideWorldImportersStandard
FROM URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/00-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/01-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/02-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/03-WideWorldImporters-Standard.bak'
```

Отображается следующая ошибка, если база данных уже существует:

```
Msg 1801, Level 16, State 1, Line 9
Database 'WideWorldImportersStandard' already exists. Choose a different database name.
```

### <a name="b-restore-database-specified-via-variable"></a><a name="restore-mi-database-variables"></a> Б. Восстановление базы данных, указанной с помощью переменной

```sql
DECLARE @db_name sysname = 'WideWorldImportersStandard';
DECLARE @url nvarchar(400) = N'https://mybackups.blob.core.windows.net/wide-world-importers/WideWorldImporters-Standard.bak';

RESTORE DATABASE @db_name
FROM URL = @url
```

### <a name="c-track-progress-of-restore-statement"></a><a name="restore-mi-database-progress"></a> В. Отслеживание хода выполнения инструкции RESTORE

```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command = 'RESTORE DATABASE'
```

> [!Note]
> В этом представлении, вероятно, будут отображаться два запроса на восстановление: исходная инструкция RESTORE, поступившая от клиента, и фоновая инструкция RESTORE, выполняемая даже в случае разрыва соединения с клиентом.

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Система платформы аналитики

Восстанавливает пользовательскую базу данных [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] из резервной копии базы данных на устройство [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. База данных восстанавливается из резервной копии, созданной ранее с помощью команды [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] [BACKUP DATABASE - Analytics Platform System](../../t-sql/statements/backup-transact-sql.md). Используйте команды резервного копирования и восстановления для создания плана аварийного восстановления или для перемещения баз данных с одного устройства на другое.

> [!NOTE]
> Восстановления базы данных master включает восстановление учетных данных для входа на устройство. Чтобы восстановить базу данных master, воспользуйтесь страницей [Restore the master Database](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md) (Восстановление базы данных master) средства **Configuration Manager**. Эту операцию может выполнить администратор, у которого есть доступ к управляющему узлу. Дополнительные сведения о резервном копировании баз данных [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] см. в разделе "Резервное копирование и восстановление" в [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Синтаксис

```syntaxsql

-- Restore the master database
-- Use the Configuration Manager tool.

Restore a full user database backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\full_backup_directory'
[;]

--Restore a full user database backup and then a differential backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\differential_backup_directory'
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]
[;]

--Restore header information for a full or differential user database backup.
RESTORE HEADERONLY
    FROM DISK = '\\UNC_path\backup_directory'
[;]
```

## <a name="arguments"></a>Аргументы

RESTORE DATABASE *database_name*. Указывает, что нужно восстановить пользовательскую базу данных в базу данных с заданным именем *database_name*. Имя восстановленной базы данных может отличаться от имени базы данных, для которой была создана резервная копия. База данных с указанным *именем* не должна существовать на устройстве назначения перед восстановлением. Дополнительные сведения о разрешенных именах баз данных см. в разделе "Правила именования объектов" в статье [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

При восстановлении пользовательской базы данных происходит восстановление полной резервной копии базы данных и затем при необходимости восстановление разностной резервной копии на устройство. Восстановление пользовательской базы данных включает пользователей и роли базы данных.

FROM DISK = '\\\\*UNC_path*\\*backup_directory*'. Сетевой путь к каталогу, из которого [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] восстановит файлы резервных копий. Например, FROM DISK = '\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup'.

*backup_directory*. Указывает имя каталога, который содержит полную или разностную резервную копию. Например, операцию RESTORE HEADERONLY можно выполнить для полной или разностной резервной копии.

*full_backup_directory*. Указывает имя каталога, который содержит полную резервную копию.

*differential_backup_directory*. Указывает имя каталога, который содержит разностную резервную копию.

- Путь к каталогу резервного копирования должен существовать; его необходимо указать в виде полного пути UNC.
- Путь к каталогу резервного копирования не может быть локальным путем или расположением на любом из узлов устройства [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- Максимальная длина пути UNC и имени каталога резервного копирования равна 200 символов.
- Для сервера или узла необходимо указать IP-адрес.

RESTORE HEADERONLY. Указывает, что необходимо вернуть только сведения заголовка для одной резервной копии пользовательской базы данных. Среди прочего заголовок содержит текстовое описание и имя резервной копии. Имя резервной копии может не совпадать с именем каталога, в котором хранятся файлы резервной копии.

Результаты инструкции RESTORE HEADERONLY обрабатываются в соответствии с шаблоном после результатов инструкции RESTORE HEADERONLY [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Результат включает более 50 столбцов, и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] использует не все из этих столбцов. Описание столбцов в результатах инструкции RESTORE HEADERONLY [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в статье [Инструкции RESTORE — HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).

## <a name="permissions"></a>Разрешения
Требуется разрешение `CREATE ANY DATABASE`.

Требуется учетная запись Windows с разрешениями на доступ к каталогу резервного копирования и на чтение этого каталога. Необходимо также сохранить имя учетной записи Windows и пароль в [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

- Чтобы проверить наличие учетных данных, используйте [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).
- Для добавления или изменения учетных данных используйте [sp_pdw_add_network_credentials — [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).
- Чтобы удалить учетные данные из [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], используйте [sp_pdw_remove_network_credentials — [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

## <a name="error-handling"></a>Обработка ошибок

Команда RESTORE DATABASE возвращает ошибки при выполнении следующих условий:

- Имя базы данных для восстановления уже существует на целевом устройстве. Чтобы избежать этой ошибки, выберите уникальное имя базы данных или удалите существующую базу данных перед началом восстановления.
- В каталоге резервной копии находится недопустимый набор файлов резервной копии.
- Разрешений на вход недостаточно для восстановления базы данных.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] не имеет необходимых разрешений для сетевого расположения, в котором размещаются файлы резервной копии.
- Сетевое расположение для каталога резервного копирования не существует или недоступно.
- На вычислительных узлах или на управляющем узле недостаточно места. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] не подтверждает наличие достаточного места на диске устройства перед началом восстановления. Поэтому при выполнении инструкции RESTORE DATABASE можно получить сообщение о нехватке места на диске. В случае нехватки места на диске [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] выполняет откат операции восстановления.
- Целевое устройство, на которое восстанавливается база данных, содержит меньшее количество вычислительных узлов по сравнению с исходным устройством, с которого была создана резервная копия.
- Предпринимается попытка восстановить базу данных в рамках транзакции.

## <a name="general-remarks"></a>Общие замечания

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] отслеживает успешность восстановления базы данных. Перед восстановлением разностной резервной копии базы данных [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] проверяет, что полное восстановление всей базы данных успешно завершено.

После восстановления базы данных пользовательская база данных будет иметь уровень совместимости 120. Это справедливо для всех баз данных независимо от их исходного уровня совместимости.

## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Восстановление на устройство с большим количеством вычислительных узлов

После восстановления базы данных с устройства меньшего размера на устройство большего размера выполните [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md), так как повторное распределение увеличит размер журнала транзакций.

Восстановление резервной копии на устройство с большим числом вычислительных узлов приводит к увеличению размера базы данных пропорционально количеству вычислительных узлов.

Например, при восстановлении базы данных размером 60 ГБ с устройства с двумя узлами (30 ГБ на каждом узле) на устройство с шестью узлами [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] создает базу данных размером 180 ГБ (6 узлов по 30 ГБ на каждом узле) на устройстве с шестью узлами. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] изначально восстанавливает базу данных на 2 узла, чтобы она соответствовала исходной конфигурации, и затем перераспределяет данные на все 6 узлов.

После распределения каждый вычислительный узел будет содержать меньше фактических данных и больше свободного пространства, чем каждый исходный узел на исходном устройстве меньшего размера. Используйте дополнительное место для добавления дополнительных данных в базу данных. Если размер восстановленной базы данных больше, чем вам нужно, можно сжать файлы базы данных с помощью инструкции [ALTER DATABASE - PDW](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true).

## <a name="limitations-and-restrictions"></a>ограничения

В этих ограничениях исходное устройство — это устройство, с которого была создана резервная копия базы данных, а целевое устройство — это устройство, на которое будет восстановлена база данных.

- При восстановлении базы данных статистика не пересчитывается автоматически.
- На одном устройстве в один момент времени может быть запущена только одна из инструкций RESTORE DATABASE или BACKUP DATABASE. Если одновременно было отправлено несколько инструкций резервного копирования и восстановления, устройство поместит их в очередь и будет обрабатывать их по одной.
- Восстановить резервную копию базы данных можно только на целевое устройство [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], количество вычислительных узлов на котором не меньше количества узлов на исходном устройстве. Количество вычислительных узлов на целевом устройстве не может быть меньше количества вычислительных узлов на исходном устройстве.
- Вы не можете восстановить резервную копию, которая была создана на устройстве с оборудованием SQL Server 2012 PDW, на устройство с оборудованием SQL Server 2008 R2. Это справедливо даже в том случае, если устройство было изначально приобретено с оборудованием SQL Server 2008 R2 PDW, а сейчас на нем запущено программное обеспечение SQL Server 2012 PDW.

## <a name="locking"></a>Блокировка
Осуществляет монопольную блокировку объекта DATABASE.

## <a name="examples"></a>Примеры

### <a name="a-simple-restore-examples"></a>A. Простые примеры использования инструкции RESTORE

В следующем примере восстанавливается полная резервная копия в базу данных `SalesInvoices2013`. Файлы резервной копии хранятся в каталоге `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. База данных SalesInvoices2013 не может существовать на целевом устройстве, в противном случае команда завершится с ошибкой.

```sql
RESTORE DATABASE SalesInvoices2013
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="b-restore-a-full-and-differential-backup"></a>Б. Восстановление полной и разностной резервной копии

В следующем примере восстанавливается полная и разностная резервная копия базы данных SalesInvoices2013

Полная резервная копия базы данных восстанавливается из полной резервной копии, которая хранится в каталоге `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Если восстановление завершится успешно, разностная резервная копия восстанавливается в базу данных SalesInvoices2013. Разностная резервная копия хранится в каталоге `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff`.

```syntaxsql
RESTORE DATABASE SalesInvoices2013
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]

```

### <a name="c-restoring-the-backup-header"></a>В. Восстановление заголовка резервной копии

В этом примере восстанавливаются сведения о заголовке для резервного копирования базы данных `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Результаты выполнения команды в одной строке для резервной копии Invoices2013Full.

```syntaxsql
RESTORE HEADERONLY
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]
```

Данные заголовка можно использовать для проверки содержимого резервной копии, или чтобы убедиться в том, что целевое устройство восстановления совместимо с исходным устройством резервного копирования, перед восстановлением резервной копии.

## <a name="see-also"></a>См. также:
[BACKUP DATABASE — Analytics Platform System](../../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)     

::: moniker-end
