---
title: Параметры конфигурации сервера (SQL Server)
description: Узнайте, как оптимизировать ресурсы SQL Server и управлять ими. Просмотрите доступные параметры конфигурации, возможные настройки, значения по умолчанию и требования к перезапуску.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
keywords:
- конфигурация сервера (SQL Server)
helpviewer_keywords:
- surface area configuration [SQL Server], sp_configure
- configuration options [SQL Server], when take effect
- server management [SQL Server], configuration options
- SQL Server Management Studio [SQL Server], servers
- servers [SQL Server], configuring
- configuration options [SQL Server], setting
- options [SQL Server], configuration
- RECONFIGURE statement
- performance [SQL Server], servers
- configuration options [SQL Server]
- RECONFIGURE WITH OVERRIDE statement
- SQL Server, configuring
- sp_configure
- stored procedures [SQL Server], configuration options
- server configuration [SQL Server]
- administering SQL Server, configuration options
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 07/20/2020
ms.openlocfilehash: bcadbc294414b2fe992d3864be240a1998a5e477
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684226"
---
# <a name="server-configuration-options-sql-server"></a>Параметры конфигурации сервера (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Управление и оптимизация ресурсов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] производятся на основе параметров конфигурации с применением среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или системной хранимой процедуры sp_configure. Наиболее часто используемые параметры конфигурации сервера доступны в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]; доступ ко всем параметрам конфигурации можно получить при помощи sp_configure. Взвесьте возможные последствия для системы, прежде чем устанавливать эти параметры. Дополнительные сведения см. в разделе [Просмотр или изменение свойств сервера (SQL Server)](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

>**ВАЖНО!** Дополнительные параметры должны изменяться только опытным администратором баз данных или сертифицированным техническим специалистом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="categories-of-configuration-options"></a>Категории параметров конфигурации

Параметры конфигурации могут вступать в силу:

- немедленно после установки параметра и выполнения инструкции **RECONFIGURE** (или, в некоторых случаях, **RECONFIGURE WITH OVERRIDE**). Повторная настройка определенных параметров аннулирует определенные параметры в кэше планов, что приводит к компиляции новых планов. Дополнительные сведения см. в разделе [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md).

  \- или -

- После выполнения вышеуказанных действий и перезапуска экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Измененные значения параметров, требующих перезапуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , первоначально будут отображены только в столбце value. После перезапуска новое значение отобразится в обоих столбцах, value и value_in_use.

Для некоторых параметров требуется перезапуск сервера прежде, чем новое конфигурационное значение вступит в силу. Если задано новое значение и выполнена процедура sp_configure перед перезапуском сервера, то новое значение появится в столбце **value** , но не в столбце **value_in_use** параметров конфигурации. После перезапуска сервера новое значение отобразится в столбце **value_in_use** .

Самонастраивающиеся параметры — это те, которые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] изменяет в соответствии с потребностями системы. В большинстве случаев это позволяет избавиться от необходимости устанавливать значения вручную. Например, к таким параметрам относятся **max worker threads** и user connections.

## <a name="configuration-options-table"></a>Таблица параметров конфигурации
 В нижеследующей таблице приведены все доступные параметры конфигурации, диапазон возможных значений и значения по умолчанию. Параметры конфигурации помечаются буквенными кодовыми обозначениями, как показано ниже:

- A= Дополнительные параметры, которые следует изменять только опытному администратору баз данных или сертифицированному по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] специалисту и для которых требуется установить для параметра "Отображение дополнительных параметров" значение 1.

- RR = Параметры, требующие перезапуска компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].

- RP = параметры, требующие перезапуска ядра PolyBase.

- SC = Самонастраивающиеся параметры.

| Параметр конфигурации | Минимальное значение | Максимальное значение | По умолчанию |
|--|--|--|--|
| [access check cache bucket count](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 16384 | 0 |
| [access check cache quota](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 2147483647 | 0 |
| [ad hoc distributed queries](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Время ожидания повторных попыток очистки ADR (мин)](../../database-engine/configure-windows/adr-cleaner-retry-timeout-configuration-option.md)<br><br> Впервые представлено в SQL Server 2019. | 0 | 32767 | 15 |
| [Коэффициент предварительного выделения ADR](../../database-engine/configure-windows/adr-preallocation-factor-server-configuration-option.md)<br><br> Впервые представлено в SQL Server 2019. | 0 | 32767 | 4 |
| [affinity I/O mask](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md) (A, RR) | -2147483648 | 2147483647 | 0 |
| [affinity mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md) (A) | -2147483648 | 2147483647 | 0 |
| [affinity64 I/O mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md) (A, доступно только в 64-разрядной версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) | -2147483648 | 2147483647 | 0 |
| [affinity64 mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) (A, RP), доступно только в 64-разрядной версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | -2147483648 | 2147483647 | 0 |
| [Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) (A) | 0 | 1 | 0<br /><br /> (Изменяется на 1, если запускается агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; Значение по умолчанию равно 0, если при установке был указан автоматический запуск агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .) |
| [разрешить экспорт polybase](../../database-engine/configure-windows/allow-polybase-export.md)<br/><br/> [!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)].| 0 | 1 | 0 |
| [allow updates](../../database-engine/configure-windows/allow-updates-server-configuration-option.md) (Устаревший. Не используйте. Вызовет ошибку во время повторной настройки.) | 0 | 1 | 0 |
| [automatic soft-NUMA disabled](soft-numa-sql-server.md) | 0 | 1 | 0 |
| [Контрольная сумма резервной копии: значение по умолчанию](../../database-engine/configure-windows/backup-checksum-default.md) | 0 | 1 | 0 |
| [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) | 0 | 1 | 0 |
| [blocked process threshold](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md) (A) | 5 | 86400 | 0 |
| [c2 audit mode](../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [clr strict security](../../database-engine/configure-windows/clr-strict-security.md) (A) <br /> [!INCLUDE [sqlserver2017](../../includes/applies-to-version/sqlserver2017.md)]. | 0 | 1 | 0 |
| [column encryption enclave type ](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md) (A, RR) | 0 | 1 | 0 |
| [common criteria compliance enabled](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [Проверка подлинности автономной базы данных](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md) | 0 | 1 | 0 |
| [cost threshold for parallelism](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 5 |
| [cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md) | 0 | 1 | 0 |
| [cursor threshold](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [default full-text language](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) (A) | 0 | 2147483647 | 1033 |
| [язык по умолчанию](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) | 0 | 9999 | 0 |
| [default trace enabled](../../database-engine/configure-windows/default-trace-enabled-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [disallow results from triggers](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Поставщик расширенного управления ключами включен](../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md) (SC) (RR)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 1 | 0 |
| [filestream_access_level](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md) | 0 | 2 | 0 |
| [fill factor](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md) (A, RR) | 0 | 100 | 0 |
| [ft crawl bandwidth (max)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft crawl bandwidth (min)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [ft notify bandwidth (max)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft notify bandwidth (min)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [hadoop connectivity](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (RP)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 7 | 0 |
| [in-doubt xact resolution](../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md) (A) | 0 | 2 | 0 |
| [index create memory](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) (A, SC) | 704 | 2147483647 | 0 |
| [lightweight pooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md) (A, RR, SC) | 5000 | 2147483647 | 0 |
| [max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 0 |
| [max full-text crawl range](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md) (A) | 0 | 256 | 4 |
| [max server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 16 | 2147483647 | 2147483647 |
| [max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) | 0 | 2147483647 | 65536 |
| [max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) (A) | 128 | 32767<br /><br /> 1024 является максимальным значением, рекомендуемым для 32-разрядных операционных систем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], 2048 — для 64-разрядных систем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Примечание**. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] — последняя версия, которая была доступна в 32-разрядной операционной системе. | 0<br /><br /> При нулевом значении максимальное число рабочих потоков исполнителя настраивается автоматически в зависимости от количества процессоров по формуле (256 + ( *\<processors>*  – 4) * 8) для 32-разрядных систем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и (512 + ( *\<processors>*  – 4) * 8) для 64-разрядных систем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Примечание**. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] — последняя версия, которая была доступна в 32-разрядной операционной системе. |
| [media retention](../../database-engine/configure-windows/configure-the-media-retention-server-configuration-option.md) (A, RR) | 0 | 365 | 0 |
| [min memory per query](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md) (A) | 512 | 2147483647 | 1024 |
| [min server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 0 | 2147483647 | 0 |
| [вложенные триггеры](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) | 0 | 1 | 1 |
| [network packet size](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md) (A) | 512 | 32767 | 4096 |
| [Ole Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [open objects](../../database-engine/configure-windows/open-objects-server-configuration-option.md) (A, RR, устаревший) | 0 | 2147483647 | 0 |
| [optimize for ad hoc workloads](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [PH_timeout](../../database-engine/configure-windows/ph-timeout-server-configuration-option.md) (A) | 1 | 3600 | 60 |
| [polybase enabled](../../relational-databases/polybase/polybase-installation.md#enable) (RR) <br/><br/>[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]| 0 | 1 | 0 |
| [Сетевое шифрование polybase](../../relational-databases/polybase/polybase-installation.md#enable) | 0 | 1 | 1 |
| [precompute rank](../../database-engine/configure-windows/precompute-rank-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [query governor cost limit](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) (A) | 0 | 2147483647 | 0 |
| [query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [recovery interval](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md) (A, SC) | 0 | 32767 | 0 |
| [remote access](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md) (RR) | 0 | 1 | 1 |
| [remote admin connections](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md) | 0 | 1 | 0 |
| [remote data archive](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) | 0 | 1 | 0 |
| [remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md) | 0 | 2147483647 | 10 |
| [remote proc trans](../../database-engine/configure-windows/configure-the-remote-proc-trans-server-configuration-option.md) | 0 | 1 | 0 |
| [remote query timeout](../../database-engine/configure-windows/configure-the-remote-query-timeout-server-configuration-option.md) | 0 | 2147483647 | 600 |
| [Replication XPs Option](../../database-engine/configure-windows/replication-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [server trigger recursion](../../database-engine/configure-windows/server-trigger-recursion-server-configuration-option.md) | 0 | 1 | 1 |
| [set working set size](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md) (A, RR, устаревший) | 0 | 1 | 0 |
| [show advanced options](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md) | 0 | 1 | 0 |
| [SMO and DMO XPs](../../database-engine/configure-windows/smo-and-dmo-xps-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [suppress recovery model errors](../../database-engine/configure-windows/suppress-recovery-model-errors-server-configuration-option.md) (A) <br/><br/>[!INCLUDE [asdbmi](../../includes/applies-to-version/_asdbmi.md)]| 0 | 1 | 0 |
| [tempdb metadata memory-optimized](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) (A) <br/><br/> [!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)].| 0 | 1 | 0 |
| [transform noise words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) (A) | 1753 | 9999 | 2049 |
| [user connections](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md) (A, RR, SC) | 0 | 32767 | 0 |
| [user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) | 0 | 32767 | 0 |
| [xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md) (A) | 0 | 1 | 0 |  |

## <a name="see-also"></a>См. также раздел
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md) [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)


