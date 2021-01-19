---
title: Управляемое резервное копирование SQL Server в Microsoft Azure | Документация Майкрософт
description: Управляемое резервное копирование SQL Server в Microsoft Azure управляет резервным копированием SQL Server в хранилище BLOB-объектов Microsoft Azure и автоматизирует его.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: afa01165-39e0-4efe-ac0e-664edb8599fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce723dfbf0171d3c1885c1574bf681fbb2c8e714
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172566"
---
# <a name="sql-server-managed-backup-to-microsoft-azure"></a>Управляемое резервное копирование SQL Server в Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] управляет резервным копированием SQL Server в хранилище BLOB-объектов Microsoft Azure и автоматизирует его. Серверу SQL Server можно разрешить определять расписание резервного копирования на основе рабочей нагрузки транзакций в базе данных. Кроме того, для настройки расписания можно воспользоваться дополнительными параметрами. Параметры хранения определяют продолжительность хранения резервных копий в хранилище BLOB-объектов Azure. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] поддерживает восстановление на момент времени для указанного периода хранения.  
  
 Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], процедуры и принцип действия [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] изменились. Дополнительные сведения см. в статье [Перенос параметров управляемой архивации SQL Server 2014 в SQL Server 2016](../../relational-databases/backup-restore/migrate-sql-server-2014-managed-backup-settings-to-sql-server-2016.md).  
  
> [!TIP]  
>  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] рекомендуется использовать для экземпляров SQL Server, работающих на виртуальных машинах Microsoft Azure.  
  
## <a name="benefits"></a>Преимущества  
 В настоящий момент автоматизация резервного копирования множества баз данных требует разработки стратегии резервного копирования, написания специального кода и планирования резервного копирования. С помощью [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]можно создать план резервного копирования, указав только период и место хранения. Хотя дополнительные параметры и доступны, они не требуются. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] позволяет планировать резервное копирование, выполнять его и обслуживать резервные копии.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] можно настроить на уровне базы данных или экземпляра SQL Server. При настройке на уровне экземпляра также автоматически создаются резервные копии новых баз данных. С помощью параметров на уровне базы данных можно переопределить значения по умолчанию на уровне экземпляра в каждом конкретном случае.  
  
 Кроме того, резервные копии можно зашифровать, чтобы обеспечить дополнительную безопасность. Вы можете также настроить пользовательское расписание, чтобы управлять резервным копированием. Дополнительные сведения о преимуществах использования хранилища BLOB-объектов Windows Azure для резервных копий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
##  <a name="prerequisites"></a><a name="Prereqs"></a> Предварительные требования  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] использует службу хранилища Microsoft Azure для хранения файлов резервных копий. Ниже перечислены необходимые компоненты.  
  
|Предварительные требования|Описание|  
|------------------|-----------------|  
|**Учетная запись Microsoft Azure**|Прежде чем просмотреть [варианты приобретения](https://azure.microsoft.com/pricing/free-trial/) , можно начать работу с Azure, используя [бесплатную пробную версию](https://azure.microsoft.com/pricing/purchase-options/).|  
|**Учетная запись хранения Azure**|Резервные копии хранятся в хранилище BLOB-объектов Azure, связанном с учетной записью хранения Azure. Чтобы создать учетную запись хранения, воспользуйтесь подробной пошаговой инструкцией в статье [об учетных записях хранения Azure](/azure/storage/common/storage-account-create).|  
|**Контейнер больших двоичных объектов**|Для упорядочивания больших двоичных объектов используются контейнеры. Необходимо указать целевой контейнер для файлов резервных копий. Контейнер можно создать на [портале управления Azure](https://manage.windowsazure.com/)или с помощью команды **New-AzureStorageContainer**[Azure PowerShell](/powershell/azure/) .|  
|**Подписанный URL-адрес**|Доступ к целевому контейнеру зависит от подписанного URL-адреса. Общие сведения о SAS см. в статье [Подписанные URL-адреса. Часть 1. подписанных URL-адресов (SAS)](/azure/storage/common/storage-sas-overview). Маркер SAS можно создать в коде или с помощью команды PowerShell **New-AzureStorageContainerSASToken** . Скрипт PowerShell, упрощающий этот процесс, см. в статье [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell)(Упрощение создания учетных данных SQL с использованием маркера подписанного URL-адреса в службе хранилища Azure с помощью Powershell). Маркер SAS можно хранить в **SQL Credential** и использовать с [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|**Агент SQL Server**|Чтобы компонент [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] работал, должен быть запущен агент SQL Server. Рекомендуется установить автоматический запуск.|  
  
## <a name="components"></a>Components  
 Transact-SQL — это основной интерфейс для взаимодействия с [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Системные хранимые процедуры используются для активации, настройки и отслеживания [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Системные функции используются для получения существующих параметров конфигурации, значений параметров и данных файлов резервных копий. Расширенные события используются для отображения ошибок и предупреждений. Механизмы предупреждений включаются с помощью заданий агента SQL Server и управления на основе политик SQL Server. Далее представлен список объектов и описание их функций по отношению к [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Можно также настроить [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]с помощью командлетов PowerShell. SQL Server Management Studio поддерживает восстановление резервных копий, созданных [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , с помощью задачи **Восстановление базы данных** .  
  
|Системный объект|Описание|  
|-|-|  
|**MSDB**|Хранит метаданные, журнал резервного копирования для всех резервных копий, созданных [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)|Запускает компонент [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_advanced (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)|Настраивает дополнительные параметры для [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], например шифрование.|  
|[managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|Создает пользовательское расписание для [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_ backup_master_switch (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-master-switch-transact-sql.md)|Приостанавливает и возобновляет работу [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_set_parameter (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql.md)|Включает и настраивает мониторинг для [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Примеры: включение расширенных событий, настроек почты для уведомлений.|  
|[managed_backup.sp_backup_on_demand (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-on-demand-transact-sql.md)|Выполняет резервное копирование ad-hoc для базы данных, которая использует [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] без нарушения цепочки журналов.|  
|[managed_backup.fn_backup_db_config (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-backup-db-config-transact-sql.md)|Возвращает текущее состояние [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] и значения конфигурации для базы данных или всех баз данных в экземпляре.|  
|[managed_backup.fn_is_master_switch_on (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-is-master-switch-on-transact-sql.md)|Возвращает состояние основного переключателя.|  
|[managed_backup.sp_get_backup_diagnostics (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql.md)|Возвращает события, записанные в журнал подсистемой расширенных событий.|  
|[managed_backup.fn_get_parameter (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-parameter-transact-sql.md)|Возвращает текущие значения системных параметров резервного копирования, например параметры мониторинга и почтовые параметры для оповещений.|  
|[managed_backup.fn_available_backups (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)|Извлекает доступные резервные копии заданной базы данных или всех баз данных в экземпляре.|  
|[managed_backup.fn_get_current_xevent_settings (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-current-xevent-settings-transact-sql.md)|Возвращает текущие параметры расширенных событий.|  
|[managed_backup.fn_get_health_status (Transact-SQL)](../../relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql.md)|Возвращает объединенное число ошибок, зарегистрированных подсистемой расширенных событий за указанный период.|  
  
## <a name="backup-strategy"></a>Стратегия резервного копирования  
  
### <a name="backup-scheduling"></a>Расписание резервного копирования  
 С помощью системной хранимой процедуры [managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md). Если не указать пользовательское расписание, тип запланированных резервных копий и частота резервного копирования определяются на основе рабочей нагрузки в базе данных. Настройки срока хранения определяют длительность хранения файлов резервных копий в хранилище и для восстановления базы данных на момент времени в течение срока хранения.  
  
### <a name="backup-file-naming-conventions"></a>Соглашения об именовании файлов резервных копий  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] использует указанный контейнер. Таким образом, вы можете выбрать имя контейнера. Имя файла резервной копии для баз данных, не являющихся базами данных доступности, задается в соответствии со следующим соглашением: Имя создается с помощью первых 40 символов имени базы данных, идентификатора GUID базы данных без "-" и отметки времени. Между сегментами в качестве разделителей вставляется подчеркивание. Для полной резервной копии используется расширение **BAK** , а для резервной копии журналов — **LOG** . Для баз данных группы доступности в дополнении к схеме именования, описанной выше, после 40 символов имени базы данных добавляется GUID группы доступности. Значение GUID базы данных группы доступности — это значение для group_database_id в sys.databases.  
  
### <a name="full-database-backup"></a>Полная резервная копия базы данных  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] планирует полную резервную копию базы данных, если выполняется одно из следующих условий.  
  
-   Функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] включается для базы данных впервые, или функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] активируется с параметрами по умолчанию на уровне экземпляра.  
  
-   Увеличение журнала после создания полной резервной копии не меньше 1 ГБ.  
  
-   Максимальный интервал времени (1 неделя) прошел с момента последнего полного резервного копирования.  
  
-   Цепочка журналов прервана. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] периодически проверяет, сохранилась ли цепочка журналов, сравнивая первый и последний номера LSN файлов резервной копии. Если по какой-то причине цепочка журналов прервана, [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] планирует полное резервное копирование базы данных. Самая распространенная причина разрыва цепочки журналов — выполнение команды резервного копирования с помощью Transact-SQL или задачи резервного копирования в SQL Server Management Studio.  К другим возможным причинам относится случайное удаление фалов журнала резервного копирования или случайная перезапись резервных копий.  
  
### <a name="transaction-log-backup"></a>Резервная копия журналов транзакций  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] планирует резервную копию журналов, если выполняется одно из следующих условий.  
  
-   Не удается обнаружить историю резервного копирования журналов. Это условие обычно выполняется, если [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] включается впервые.  
  
-   Используемый объем журнала транзакций равен 5 МБ или больше.  
  
-   Достигнут максимальный интервал времени (2 часа) с момента создания последней резервной копии журналов.  
  
-   Каждый раз, когда резервная копия журнала транзакций отстает от полной резервной копии базы данных, целью является сохранение цепочки журналов до полной резервной копии.  
  
## <a name="retention-period-settings"></a>Параметры срока хранения  
 При включении резервного копирования необходимо задать срок хранения в днях: минимальное значение — 1 день, максимальное — 30 дней.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] оценивает возможность восстановления базы данных на момент времени в течение заданного периода, чтобы определить, какие файлы резервной копии необходимо сохранить, а какие — удалить. Параметр backup_finish_date резервной копии используется для определения и сопоставления времени, заданного настройками срока хранения.  
  
## <a name="important-considerations"></a>Важные соображения  
 Если для базы данных выполняется текущее задание полного резервного копирования базы данных, [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ожидает завершения текущего задания до перехода к следующему заданию полного резервного копирования этой же базы данных. Аналогичным образом в заданный момент времени может выполняться только одно задание резервного копирования журнала транзакций. Однако операции полного резервного копирования базы данных и резервного копирования журнала транзакций могут выполняться одновременно. Ошибки записываются в журнал как расширенные события.  
  
 Если запланировано больше 10 параллельных операций полного резервного копирования базы данных, через канал отладки расширенных событий передается предупреждение. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] хранит очередь приоритетов для оставшихся баз данных, для которых нужно создать резервные копии, пока все операции резервного копирования не будут запланированы и завершены.  

> [!NOTE]
> Управляемое резервное копирование SQL Server не поддерживается для прокси-серверов.
>
  
##  <a name="supportability"></a><a name="support_limits"></a> Поддержка  
 Ниже приведены ограничения и рекомендации, связанные [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
-   Поддерживается резервное копирование системных баз данных **master**, **model** и **msdb** . Резервное копирование **tempdb** не поддерживается. 
  
-   [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]поддерживает все модели восстановления (модель полного восстановления, модель восстановления с неполным протоколированием и простую модель восстановления).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] поддерживает только полные резервные копии базы данных и резервные копии журналов. Автоматическое резервное копирование файлов не поддерживается.  
  
-   Хранилище BLOB-объектов Microsoft Azure — единственное поддерживаемое хранилище резервных копий. Резервные копии на диски или ленточные накопители не поддерживаются.  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] использует функцию резервного копирования в блочные BLOB-объекты. Максимальный размер блочного BLOB-объекта составляет 200 ГБ. За счет чередования максимальный размер отдельной резервной копии можно увеличить до 12 ТБ. Если вам нужен больший размер, рекомендуется использовать сжатие и проверить размер файла резервной копии до настройки [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Проверку можно выполнить, создав резервную копию на локальном диске или создав ее вручную в службе хранилища Microsoft Azure с использованием инструкции Transact-SQL **BACKUP TO URL** . Дополнительные сведения см. в разделе [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] может накладывать определенные ограничения, если она настроена с другими технологиями, поддерживающими резервное копирование, высокий уровень доступности и аварийное восстановление.  
  
## <a name="see-also"></a>См. также:  
- [Включение управляемого резервного копирования SQL Server в Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)   
- [Настройка дополнительных параметров управляемого резервного копирования SQL Server в Microsoft Azure](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)   
- [Отключение управляемого резервного копирования SQL Server в Microsoft Azure](../../relational-databases/backup-restore/disable-sql-server-managed-backup-to-microsoft-azure.md)
- [Резервное копирование и восстановление системных баз данных](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Резервное копирование и восстановление баз данных SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
  
