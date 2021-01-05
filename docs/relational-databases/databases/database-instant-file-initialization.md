---
title: Мгновенная инициализация файлов базы данных
description: Сведения о быстрой инициализации файлов и о том, как включить ее в базе данных SQL Server.
ms.custom: contperf-fy20q4
ms.date: 07/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- initializing files [SQL Server]
- instant file initialization [SQL Server]
- fast file initialization [SQL Server]
- file initialization [SQL Server]
- IFI [SQL Server]
- database instant file initialization [SQL Server]
ms.assetid: 1ad468f5-4f75-480b-aac6-0b01b048bd67
author: stevestein
ms.author: sstein
ms.openlocfilehash: 6598dc9812aed56b170c22d0ed1b4a470c5f7209
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878775"
---
# <a name="database-instant-file-initialization"></a>Мгновенная инициализация файлов базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
В этой статье вы узнаете о быстрой инициализации файлов и о том, как включить ее для ускорения роста файлов базы данных SQL Server.  

По умолчанию файлы данных и журналов инициализируются, чтобы перезаписать все существующие данные на диске, оставшиеся после удаленных файлов. Файлы данных и журналов сначала инициализируются путем обнуления (заполнения нулями) при выполнении следующих операций:  
  
- создавать базу данных;  
- Добавление файлов данных и журналов к существующей базе данных.  
- Увеличение размера существующего файла (включая операции автоувеличения).  
- Восстановление базы данных или файловой группы.  

В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] мгновенная инициализация файлов (IFI) позволяет быстрее выполнять ранее упомянутые файловые операции, так как она освобождает занятое место на диске, не заполняя его нулями. Вместо этого содержимое диска перезаписывается, поскольку в файлы записываются новые данные. Файлы журналов не могут быть инициализированы мгновенно.


## <a name="enable-instant-file-initialization"></a>Включить мгновенную инициализацию файлов

Мгновенная инициализация файлов доступна, только если стартовой учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставлено разрешение *SE_MANAGE_VOLUME_NAME*. Участники группы администраторов Windows обладают этим правом и могут предоставить его другим пользователям, добавив их в политику безопасности **Выполнение задач обслуживания томов** .  
> [!IMPORTANT]
> Некоторые условия, например [прозрачное шифрование данных](../../relational-databases/security/encryption/transparent-data-encryption.md), могут не допускать мгновенную инициализацию файлов.  

> [!NOTE]
> Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], это разрешение можно предоставлять учетной записи службы во время установки. <br><br>При использовании [установки из командной строки](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) добавьте аргумент /SQLSVCINSTANTFILEINIT либо установите флажок *Предоставить право на выполнение задач обслуживания тома службе ядра СУБД SQL Server* в [мастере установки](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).
  
Предоставление учетной записи разрешения `Perform volume maintenance tasks` .  
  
1.  На компьютере, где будет создан файл данных, откройте приложение **Локальная политика безопасности** (`secpol.msc`).  
  
1.  Разверните на левой панели узел **Локальные политики**, а затем щелкните пункт **Назначение прав пользователей**.  
  
1.  На правой панели дважды щелкните **Выполнение задач по обслуживанию томов**.  
  
1.  Выберите пункт **Добавить пользователя или группу** и добавьте учетную запись, от имени которой запущена служба SQL Server.  
  
1.  Нажмите кнопку **Применить** и закройте все диалоговые окна **Локальная политика безопасности** .  

1. Перезапустите службу SQL Server.

1. Проверьте журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при запуске.
   
  
    **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 4 (SP4), [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и версий [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше).
    1. Если учетной записи запуска службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставлено разрешение *SE_MANAGE_VOLUME_NAME*, регистрируется подобное информационное сообщение:

        `Database Instant File Initialization: enabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`

    1. Если учетной записи запуска службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**не** предоставлено разрешение *SE_MANAGE_VOLUME_NAME*, регистрируется подобное информационное сообщение:

        `Database Instant File Initialization: disabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`
    > [!NOTE]
    > Вы можете использовать столбец *instant_file_initialization_enabled* в динамическом административном представлении [sys.dm_server_services](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md) для определения того, включена ли мгновенная инициализация файлов.

## <a name="security-considerations"></a>Вопросы безопасности

Мы советуем включить мгновенную инициализацию файлов, так как преимущества перевешивают риски безопасности.

При использовании мгновенной инициализации файлов удаленное содержимое диска перезаписывается только по мере того, как в файлы записываются новые данные. Поэтому доступ к удаленному содержимому может получить неавторизованный субъект, пока в эту область файла данных не будут записаны другие данные.

По мере подключения файла базы данных к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] риск раскрытия информации уменьшается благодаря списку управления доступом на уровне пользователей (DACL) в файле. DACL разрешает доступ к файлу только учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и локальному администратору. Но при отсоединении файла доступ к нему может получить пользователь или служба, которым не предоставлено разрешение *SE_MANAGE_VOLUME_NAME*.

Аналогичные соображения существуют в следующих случаях.

* *Создается резервная копия базы данных.* Если файл резервной копии не защищен с помощью соответствующего DACL, удаленное содержимое может стать доступным неавторизованному пользователю или службе.  

* *Файл расширяется с помощью IFI*. Администратор SQL Server потенциально может получить доступ к содержимому необработанных страниц и просматривать ранее удаленное содержимое.

* *Файлы базы данных размещаются в сети хранения*. Она может всегда представлять новые страницы в виде предварительно инициализированных, что способно создавать излишнюю нагрузку, так как операционной системе требуется инициализировать эти страницы повторно.

Если вероятность раскрытия удаленного содержимого является серьезной проблемой, необходимо выполнить одно следующее действие или оба.  
  
- Всегда проверяйте, что все отсоединенные файлы данных и резервные копии имеют ограничивающие DACL.  
- Отключите быструю инициализацию для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    Для этого отмените *SE_MANAGE_VOLUME_NAME* из стартовой учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
    
    > [!NOTE]
    > Отключение увеличивает время выделения файлов для файлов данных и влияет только на создаваемые или увеличивающиеся в размере файлы после отзыва прав пользователя.
  
### <a name="se_manage_volume_name-user-right"></a>Право пользователя SE_MANAGE_VOLUME_NAME

Право пользователя *SE_MANAGE_VOLUME_NAME* можно назначить в **средствах администрирования Windows** с помощью приложения **Локальная политика безопасности**. В разделе **Локальные политики** выберите **Назначение прав пользователя** и измените свойство **Выполнение задач по обслуживанию томов**.

## <a name="performance-considerations"></a>Вопросы производительности

Процесс инициализации файла базы данных записывает нули в новые регионы файла при инициализации. Длительность этого процесса зависит от размера инициализированной части файла и времени отклика и емкости системы хранения. Если инициализация занимает много времени, могут отобразиться следующие сообщения, записанные в журнал ошибок SQL Server и в журнал приложения.

```
Msg 5144
Autogrow of file '%.*ls' in database '%.*ls' was cancelled by user or timed out after %d milliseconds.  Use ALTER DATABASE to set a smaller FILEGROWTH value for this file or to explicitly set a new file size.
```

```
Msg 5145
Autogrow of file '%.*ls' in database '%.*ls' took %d milliseconds.  Consider using ALTER DATABASE to set a smaller FILEGROWTH for this file.
```

Длительное автоматическое увеличение размера базы данных или файла журнала транзакций может привести к проблемам с производительностью запросов. Это обусловлено тем, что операция, требующая автоматического увеличения размера файла, будет приостановлена на ресурсах, таких как блокировки или кратковременные блокировки, по мере увеличения размера файла. Длительные ожидания кратковременных блокировок могут отображаться для страниц распределения. Для операции, требующей длительного автоматического увеличения размера, будет показан тип ожидания PREEMPTIVE_OS_WRITEFILEGATHER.





## <a name="see-also"></a>См. также:  
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)