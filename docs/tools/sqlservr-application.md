---
title: Приложение sqlservr
description: Приложение sqlservr запускает, останавливает, приостанавливает и возобновляет работу экземпляра SQL Server из командной строки.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], sqlservr
- command prompt [SQL Server], pausing/resuming instance of SQL Server
- starting instance of SQL Server
- command prompt [SQL Server], continuing instance of SQL Server
- sqlservr utility
- pausing instance of SQL Server
- stopping instance of SQL Server
- resuming SQL Server
- command prompt [SQL Server], stopping instance of SQL Server
- command prompt [SQL Server], starting instance of SQL Server
- continuing instance of SQL Server
ms.assetid: 60e8ef0a-0851-41cf-a6d8-cca1e04cbcdb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 67f25ef21a7d4897bba7242da796bd41f9fc655e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642225"
---
# <a name="sqlservr-application"></a>Приложение sqlservr

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Приложение **sqlservr** запускает, останавливает, приостанавливает и возобновляет работу экземпляра [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] операциями из командной строки.

## <a name="syntax"></a>Синтаксис

```cmd
sqlservr [-s instance_name] [-c] [-d master_path] [-f] 
     [-e error_log_path] [-l master_log_path] [-m]
     [-n] [-T trace#] [-v] [-x]
```

## <a name="arguments"></a>Аргументы

**-s** *instance_name* — указывает экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] для подключения. Если именованный экземпляр не указан, программа **sqlservr** запускает экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]по умолчанию.

> [!IMPORTANT]
>Для запуска экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]приложение **sqlservr** необходимо запускать из каталога, соответствующего этому экземпляру. Для экземпляра по умолчанию **sqlservr** нужно запускать из каталога \MSSQL\Binn. Для именованного экземпляра **sqlservr** нужно запускать из каталога \MSSQL$*имя_экземпляра*\Binn.

 **-c** — указывает, что экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] будет запущен независимо от диспетчера управления службами Microsoft Windows. Этот параметр используется при запуске [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] из командной строки, чтобы сократить время, необходимое для запуска [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

> [!NOTE]
>При использовании этого параметра остановить [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с помощью Service Manager [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или команды **net stop** будет невозможно. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] будет остановлен, если выйти из системы на данном компьютере.)

**-d** *master_path* — указывает полный путь к файлу базы данных **master**. Между параметрами **-d** и *master_path* нет пробелов. Если этот параметр не задан, используются параметры из реестра.

**-f** — запускает экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с минимальной конфигурацией. Эта функция полезна в случае, если установленные значения конфигурации (например, слишком большой объем выделяемой памяти) не позволяют выполнить запуск сервера.

**-e** *error_log_path* — указывает абсолютный путь к файлу журнала ошибок. Если этот параметр не указан, используется расположение по умолчанию `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL\Log\Errorlog` для экземпляра по умолчанию и `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL$*instance_name*\Log\Errorlog` для именованного экземпляра. Между параметрами **-e** и *error_log_path* нет пробелов.

**-l** *master_log_path* — указывает полный путь к файлу журнала транзакций базы данных **master**. Между параметрами **-l** и *master_log_path* нет пробелов.

**-m** — указывается для запуска экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в однопользовательском режиме. Если [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запущен в однопользовательском режиме, к нему может подключиться только один пользователь. Механизм CHECKPOINT, гарантирующий, что завершенные транзакции регулярно записываются из дискового кэша в устройство хранения базы данных, не запускается. (Как правило, этот параметр используется при появлении проблем с системными базами данных, требующими исправления.) Включает параметр **sp_configure allow updates**. По умолчанию параметр **allow updates** отключен.

**-n** — позволяет запустить именованный экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Если не указать параметр **-s** , будет выполнена попытка запуска экземпляра по умолчанию. Перед запуском программы **sqlservr.exe** в командной строке необходимо перейти в каталог BINN соответствующего экземпляра. Например, если экземпляр Instance1 должен использовать \mssql$Instance1 для своих двоичных файлов, для запуска **sqlservr.exe -s instance1** пользователь должен быть в каталоге \mssql$Instance1\binn. Если экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запускается с параметром **-n** , целесообразно также использовать параметр **-e** , иначе события [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не будут регистрироваться.

**-T** *trace#*  — указывает, что экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] должен запускаться так, как если бы был установлен флаг трассировки (*trace#* ). Флаги трассировки используются для запуска сервера в нестандартном режиме. Дополнительные сведения см. в разделе [Флаги трассировки (Transact-SQL)](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

>[!IMPORTANT]
>При указании флага трассировки укажите **-T** , чтобы передать номер флага трассировки. Знак t в нижнем регистре ( **-t**) также принимается [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Однако в этом случае **-t** установит другие внутренние флаги трассировки, необходимые специалистам службы поддержки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

**-v** — отображает номер версии сервера.

**-x** — отключает ведение статистики по процессорному времени и коэффициенту попадания в кэш. Позволяет достичь максимальной производительности.

## <a name="remarks"></a>Remarks
В большинстве случаев программа sqlserver.exe используется только для устранения неполадок или в ходе масштабных операций обслуживания. Если [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запущен из командной строки с помощью программы sqlservr.exe, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запускается не в качестве службы, поэтому остановить [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с помощью команды **net** невозможно. Пользователи могут подключаться к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], однако средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] отображают состояние службы, поэтому диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] правильно показывает, что служба остановлена. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] может подключаться к серверу, но также укажет, что служба остановлена.

## <a name="compatibility-support"></a>Поддержка совместимости
Следующие параметры считаются устаревшими и не поддерживаются в [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].

|Параметр | Дополнительные сведения|
|:-----|:-----|
|**-h** | В более ранних версиях 32-битных экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] использовался для резервирования виртуального адресного пространства для метаданных памяти с «горячей» заменой при включенных расширениях AWE. Поддерживается вплоть до версии [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Дополнительные сведения см. в разделе [Неподдерживаемые функции SQL Server 2016](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).|
|**-g** | *memory_to_reserve*<br/><br>Применяется к более ранним версиям 32-разрядных экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Поддерживается вплоть до версии [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Определяет целое число мегабайтов (МБ) памяти, которую [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] оставляет доступной для распределения памяти в пределах процесса [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , но за пределами пула памяти [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Дополнительные сведения см. в [разделе документации по SQL Server 2014, посвященном параметрам конфигурации памяти сервера](/previous-versions/sql/2014/database-engine/configure-windows/server-memory-server-configuration-options?view=sql-server-2014&preserve-view=true).|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>См. также:
 [Параметры запуска службы Database Engine](../database-engine/configure-windows/database-engine-service-startup-options.md)