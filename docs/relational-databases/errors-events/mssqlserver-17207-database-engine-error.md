---
description: MSSQLSERVER_17207
title: MSSQLSERVER_17207
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: d95f6b04265550d758064fe277c21de6be862ec5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642450"
---
# <a name="mssqlserver_17207"></a>MSSQLSERVER_17207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |
| :-------- | :---- |
|Название продукта|SQL Server|  
|Идентификатор события|17207|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|DBLKIO_OS2DISKERROR|  
|Текст сообщения|%ls: при создании или открытии файла "%ls" произошла ошибка операционной системы %ls. Определите причину, исправьте ошибку операционной системы, затем еще раз попытайтесь выполнить операцию.|  


## <a name="explanation"></a>Объяснение  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удалось открыть указанный файл из-за указанной ошибки ОС.  

Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удается открыть базу данных и (или) файлы журнала транзакций, в событии приложения Windows или в журнале ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может появиться сообщение об ошибке 17207. Вот пример такого сообщения.

``` 
Error: 17207, Severity: 16, State: 1.
FileMgr::StartSecondaryDataFiles: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'F:\MSSQL\DATA\MyDB_FG1_1.ndf'. Diagnose and correct the operating system error, and retry the operation.
```

Эти ошибки могут возникнуть во время запуска экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или любой операции с базой данных, при которой выполняется попытка запустить базу данных (например, ALTER DATABASE). В некоторых сценариях могут возникнуть ошибки 17207 и 17204, а в других случаях может возникнуть только одна из них.

Если такие ошибки происходят в пользовательской базе данных, она остается в состоянии RECOVERY_PENDING, а приложения не могут получить доступ к базе данных. Если такие ошибки происходят в системной базе данных, экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не запускается и вы не можете подключиться к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это также может привести к тому, что ресурс отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перейдет в режим "вне сети".

Если проблема связана с файловой группой FileStream в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], вы увидите, что вместо имени файла указан только полный путь к каталогу. Ниже приведен пример. 
```
Error: 17207, Severity: 16, State: 1.
STREAMFCB::Startup: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\bpa_files_test_fs_1\bpa_files_test_fs_1'. Diagnose and correct the operating system error, and retry the operation.
```

## <a name="cause"></a>Причина
Прежде чем можно будет использовать базу данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ее необходимо запустить. При запуске базы данных инициализируются различные структуры данных, представляющие базу данных и ее файлы, открываются все файлы, принадлежащие базе данных, и, наконец, база данных восстанавливается. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует функцию API [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) в Windows для открытия файлов, принадлежащих базе данных.
 
Сообщения 17207 (и 17204) указывают на то, что при попытке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] открыть файлы базы данных во время запуска произошла ошибка.
 
Эти сообщения об ошибках содержат следующие сведения:
1. Имя функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которая пытается открыть файл. В этих сообщениях об ошибках обычно содержится одно из следующих имен:
   - FCB::Open — ошибка файла при попытке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] открыть его.
   - FileMgr::StartPrimaryDataFiles — первичный файл данных или файл, принадлежащий первичной группе файлов.
   - FileMgr::StartSecondaryDataFiles — файл, принадлежащий вторичной группе файлов.
   - FileMgr::StartLogFiles — файл журнала транзакций.
   - STREAMFCB::Startup — контейнер FileStream в SQL.
   - FCB::RemoveAlternateStreams.
  
      
1. В сведениях о состоянии указаны различные данные о нескольких расположениях внутри функции, которая может создать это сообщение об ошибке.
1. Полный физический путь к файлу.
1. Идентификатор файла, соответствующий файлу.
1. Код ошибки операционной системы и описание ошибки. В некоторых экземплярах вы увидите только код ошибки.
 
Сведения об ошибках операционной системы, выводимые в этих сообщениях о них, являются основной причиной ошибки 17204. Распространенными причинами этих сообщений об ошибках являются проблемы с разрешениями или неверный путь к файлу.


## <a name="user-action"></a>Рекомендуемые действия  
1. Для устранения ошибки 17207 нужно расшифровать соответствующий код ошибки операционной системы и провести диагностику. После устранения ошибки операционной системы можно попытаться перезапустить базу данных (например, с помощью инструкции ALTER DATABASE SET ONLINE) или экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы перевести затронутую базу данных в режим "в сети". В некоторых случаях не удается устранить ошибку операционной системы, поэтому необходимо предпринять определенные корректирующие действия. Мы обсудим их в этом разделе.
1. Если сообщение об ошибке 17207 содержит только код ошибки, а не ее описание, можно попробовать расшифровать этот код. Для этого выполните в оболочке операционной системы команду net helpmsg <error code>. Если кодом ошибки является 8-значный код состояния, ознакомьтесь с инструкциями на [этой странице](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133), чтобы декодировать код состояния в ошибку ОС.
1. В случае возникновения ошибки операционной системы 5 (```Access is Denied```), рассмотрите следующие методы устранения:
   -  Проверьте разрешения, заданные в файле, просмотрев свойства файла в проводнике Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует группы Windows, чтобы подготовить службу контроля доступа для различных файловых ресурсов. Убедитесь, что соответствующая группа (с именем SQLServerMSSQLUser$ComputerName$MSSQLSERVER или SQLServerMSSQLUser$ComputerName$InstanceName) имеет необходимые разрешения для файла базы данных, указанного в сообщении об ошибке. Дополнительные сведения см. в статье [Настройка разрешений файловой системы для доступа к компоненту ядра СУБД](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Убедитесь, что группа Windows содержит учетную запись запуска службы или идентификатор безопасности службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
   -  Проверьте учетную запись пользователя, от имени которой сейчас запущена служба [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для получения этих сведений можно использовать диспетчер задач Windows. Найдите значение "Имя пользователя" для исполняемого файла "sqlservr.exe". Если вы недавно изменили учетную запись службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], поддерживаемым способом выполнения этой операции является использование служебной программы "Диспетчер конфигурации SQL Server". Дополнительные сведения см. в статье [Диспетчер конфигурации SQL Server](../sql-server-configuration-manager.md). 
   -  В зависимости от типа операции — открытие баз данных во время запуска сервера, присоединение базы данных, восстановление базы данных и т. д. — учетная запись, используемая для олицетворения и получения доступа к файлу базы данных, может варьироваться. Сведения о том, какая операция позволяет задавать разрешения к каким учетным записям, см. в статье [Защита данных и файлов журналов](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)). Используйте такие средства, как [Process Monitor](/sysinternals/downloads/procmon) из пакета Windows SysInternals, чтобы узнать, каким образом предоставляется доступ к файлу: в контексте безопасности учетной записи запуска службы экземпляра SQL Server или через олицетворенную учетную запись.

      Если SQL Server олицетворяет учетные данные пользователя, выполняющего операцию ALTER DATABASE или CREATE DATABASE, в средстве Process Monitor отобразятся следующие сведения (пример).

        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
1. Если вы получаете ошибку ОС `The system cannot find the file specified` = 3
   - Проверьте полный путь из сообщения об ошибке.
   - Убедитесь, что диск и путь к папке видимы и доступны в проводнике Windows.
   - Проверьте журнал событий Windows, чтобы узнать, существуют ли проблемы с этим диском.
   - Если путь указан неправильно и база данных уже существует в системе, можно изменить пути к файлам базы данных, используя методы, описанные в разделе [Перемещение файлов базы данных](../databases/move-database-files.md). Используйте эту процедуру, особенно для файлов системных баз данных, в которых возникают ошибки 17204 или 17207, если вы работаете со сценарием аварийного восстановления, в котором указанные диски недоступны. В этом разделе также объясняется, как можно найти текущее расположение различных системных баз данных [master, model, tempdb, msdb и mssqlsystemresource].
   - Если эта ошибка возникает из-за отсутствия файлов базы данных, необходимо восстановить базу данных из допустимой резервной копии.
     - Если файл базы данных, связанный с ошибкой, принадлежит вторичной файловой группе, можно дополнительно пометить эту файловую группу как автономную, перевести базу данных в режим "в сети", а затем выполнить восстановление только этой файловой группы. Дополнительные сведения см. в разделе об автономной работе в статье [Параметры файлов и файловых групп ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
     - Если файл, вызвавший ошибку, является файлом журнала транзакций, ознакомьтесь со сведениями в разделах FOR ATTACH и FOR ATTACH_REBUILD_LOG в статье [CREATE DATABASE (Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md), чтобы понять, как можно повторно создать отсутствующие файлы журнала транзакций.
   - Прежде чем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] попытается получить доступ к файлам базы данных в этих расположениях, убедитесь, что доступно дисковое или сетевое расположение (например, iSCSI-диск). При необходимости создайте нужные зависимости в администраторе кластера или диспетчере служб.

1. Если возникает ошибка операционной системы `The process cannot access the file because it is being used by another process` = 32
   - Используйте такие средства, как [Обозреватель процессов](/sysinternals/downloads/process-explorer) или [Дескриптор](/sysinternals/downloads/handle) из Windows Sysinternals, чтобы определить, нет ли у другого процесса или службы эксклюзивной блокировки для этого файла базы данных.
   - Запретите этому процессу доступ к файлам базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Распространенными примерами являются антивирусные программы (см. руководство по исключению файлов в следующей [статье базы знаний](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server)).
   - В кластерной среде убедитесь, что процесс sqlservr.exe предыдущего узла-владельца освободил дескрипторы для файлов базы данных. Обычно этого не происходит, но неправильная настройка кластера или путей ввода-вывода может привести к таким проблемам.
