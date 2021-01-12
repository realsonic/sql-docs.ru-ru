---
title: Запуск, остановка, приостановка, возобновление, перезапуск служб SQL Server
description: Узнайте, как запускать, останавливать, приостанавливать, возобновлять или перезапускать различные службы SQL Server. Узнайте, как использовать Transact-SQL, PowerShell и другие средства для этих действий.
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091777"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Запуск, остановка, приостановка, возобновление, перезапуск служб SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этом разделе описываются запуск, остановка, приостановка, возобновление или перезапуск ядра СУБД SQL Server, агента SQL Server или службы обозревателя SQL Server с помощью диспетчера конфигурации SQL Server, SQL Server Management Studio (SSMS), команд NET из командной строки, Transact-SQL или PowerShell.

## <a name="identify-the-service"></a>Указание службы

Компоненты SQL Server являются исполняемыми программами, работающими в качестве служб Windows. Программы, запущенные в качестве служб Windows, работают, не проявляя никакой активности на экране компьютера.

### <a name="database-engine-service"></a>Служба компонента Database Engine

Исполняемый процесс, который является ядром СУБД SQL Server. Ядро СУБД может быть экземпляром по умолчанию (может быть только один на одном компьютере) либо может быть одним из нескольких именованных экземпляров ядра СУБД. С помощью диспетчера конфигурации SQL Server определите, какие экземпляры ядра СУБД установлены на компьютере. Экземпляр по умолчанию (если вы его установили) указан в списке под именем **SQL Server (MSSQLSERVER)** . Именованные экземпляры (если вы установили их) перечислены как **SQL Server (<имя_экземпляра>)** . По умолчанию SQL Server Express устанавливается как **SQL Server (SQLEXPRESS)** .

### <a name="sql-server-agent-service"></a>служба агента SQL Server

Служба Microsoft Windows, выполняющая запланированные административные задачи, которые называются заданиями и предупреждениями. Дополнительные сведения см. в статье [SQL Server Agent](../../ssms/agent/sql-server-agent.md). Агент SQL Server доступен не во всех выпусках SQL Server. Сведения о функциях, поддерживаемых различными выпусками SQL Server, см. в статье [Возможности, поддерживаемые выпусками SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

### <a name="sql-server-browser-service"></a>служба «SQL Server, браузер»

Служба Windows, прослушивающая входящие запросы к ресурсам SQL Server и предоставляющая клиентам сведения об экземплярах SQL Server, установленных на компьютере. Один экземпляр службы обозревателя SQL Server используется для всех экземпляров SQL Server, установленных на этом компьютере.

### <a name="additional-information"></a>Дополнительные сведения

- Приостановка службы ядра СУБД делает невозможным подключение новых пользователей к ядру СУБД, однако уже подключенные пользователи могут работать до тех пор, пока их соединения не будут разорваны. Приостановите работу службы, если нужно дождаться окончания работы пользователей, прежде чем совсем остановить службу. Это позволяет им завершить транзакции, которые в данный момент выполняются. *Возобновление* позволяет ядру СУБД снова принимать входящие подключения. Службу агента SQL Server нельзя приостановить или возобновить.  

- Диспетчер конфигурации SQL Server и SSMS отображают текущее состояние служб с помощью следующих значков.  

    **Диспетчер конфигурации SQL Server**

  - Зеленая стрелка на значке рядом с именем службы указывает на то, что служба запущена.

  - Красный квадрат на значке рядом с именем службы означает, что служба остановлена.

  - Пара вертикальных синих полосок на значке рядом с именем службы указывает на то, что служба приостановлена.

  - При перезапуске ядра СУБД красный квадрат обозначает, что служба остановлена, затем зеленая стрелка покажет, что служба успешно запущена.

    **SQL Server Management Studio (SSMS)**

  - Белая стрелка на значке с зеленым кругом рядом с именем службы указывает на то, что служба запущена.  

  - Белый квадрат на значке с красным кругом рядом с именем службы означает, что служба остановлена.  

  - Пара вертикальных белых полосок на значке с синим кругом рядом с именем службы указывает, что служба приостановлена.  

- При использовании диспетчера конфигурации SQL Server или SSMS доступны только применимые параметры. Например, если служба уже запущена, кнопка **Пуск** будет недоступна.

- При эксплуатации на кластере службой ядра СУБД SQL Server лучше всего управлять с помощью администратора кластера.  

### <a name="security"></a>Безопасность

#### <a name="permissions"></a>Разрешения

По умолчанию только участники локальной группы "Администраторы" могут запускать, останавливать, приостанавливать, возобновлять или перезапускать службу. При необходимости предоставить возможность управления службой для пользователей, не обладающих правами администратора, см. раздел [Как предоставить пользователям права для управления службами в Windows Server 2003](https://support.microsoft.com/kb/325349). (Процесс такой же, как и в других версиях Windows Server.)

Остановка ядра СУБД с помощью команды **SHUTDOWN** Transact-SQL требует членства в предопределенных ролях сервера **sysadmin** или **serveradmin** и не предназначена для передачи.

## <a name="sql-server-configuration-manager"></a>Диспетчер конфигурации SQL Server

### <a name="starting-sql-server-configuration-manager"></a>Запуск диспетчера конфигурации SQL Server

В меню **Пуск** укажите **Все программы**, **Microsoft SQL Server**, **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.

Поскольку диспетчер конфигурации SQL Server является оснасткой консоли управления (Майкрософт), а не изолированной программой, при работе в более новых версиях Windows диспетчер конфигурации SQL Server не отображается как приложение. Ниже приведены расположения последних четырех версий этого диспетчера при установке Windows на диск C.  

|Версия|путь|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> Запуск, остановка, приостановка, возобновление или перезапуск экземпляра ядра СУБД SQL Server

1. Запустите диспетчер конфигурации SQL Server с помощью приведенных выше инструкций.

2. В диалоговом окне **Контроль учетных записей** нажмите кнопку **Да**.

3. В диспетчере конфигурации SQL Server на панели слева выберите **Службы SQL Server**.

4. На панели результатов щелкните правой кнопкой мыши **SQL Server (MSSQLServer)** или именованный экземпляр, затем выберите **Пуск**, **Остановка**, **Пауза**, **Продолжить** или **Перезапуск**.

5. Нажмите кнопку **ОК**, чтобы закрыть диспетчер конфигурации SQL Server.

> [!NOTE]
> Инструкции по запуску экземпляра ядра СУБД SQL Server с параметрами запуска см. в статье [Настройка параметров запуска сервера &#40;диспетчер конфигурации SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>Запуск, остановка, приостановка, возобновление или перезапуск обозревателя SQL Server или экземпляра агента SQL Server

1. Запустите диспетчер конфигурации SQL Server с помощью приведенных выше инструкций.

2. В диалоговом окне **Контроль учетных записей** нажмите кнопку **Да**.

3. В диспетчере конфигурации SQL Server на панели слева выберите **Службы SQL Server**.

4. На панели результатов щелкните правой кнопкой мыши **Обозреватель SQL Server**, **Агент SQL Server (MSSQLServer)** или **Агент SQL Server (<имя_экземпляра>)** для именованного экземпляра, а затем выберите **Пуск**, **Остановка**, **Пауза**, **Продолжить** или **Перезапуск**.  

5. Нажмите кнопку **ОК**, чтобы закрыть диспетчер конфигурации SQL Server.  

> [!NOTE]
> Агент SQL Server приостановить нельзя.

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> Запуск, остановка, приостановка, возобновление или перезапуск экземпляра ядра СУБД SQL Server

1. В обозревателе объектов подключитесь к экземпляру ядра СУБД, щелкните правой кнопкой мыши экземпляр ядра СУБД, который нужно запустить, и выберите **Пуск**, **Остановка**, **Пауза**, **Продолжить** или **Перезапуск**.

    Либо в разделе "Зарегистрированные серверы" щелкните правой кнопкой мыши экземпляр ядра СУБД, который нужно запустить, наведите указатель на **Управление службами** и выберите **Пуск**, **Остановка**, **Пауза**, **Продолжить** или **Перезапуск**.

2. В диалоговом окне **Контроль учетных записей** нажмите кнопку **Да**.

3. При появлении запроса на выполнение действия нажмите кнопку **Да**.  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>Запуск, остановка или перезапуск экземпляра агента SQL Server

1. В обозревателе объектов подключитесь к экземпляру ядра СУБД, щелкните правой кнопкой мыши **Агент SQL Server** и выберите **Пуск**, **Остановка** или **Перезапустить**.

2. В диалоговом окне **Контроль учетных записей** нажмите кнопку **Да**.

3. При появлении запроса на выполнение действия нажмите кнопку **Да**.

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> Окно командной строки с помощью команд NET

Службы Microsoft SQL Server можно запустить, остановить или приостановить с помощью команд **net** Microsoft Windows.

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> Запуск экземпляра ядра СУБД по умолчанию

- В командной строке введите одну из следующих команд:  
  
    **net start "SQL Server (MSSQLSERVER)"**

   -или-  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> Запуск именованного экземпляра ядра СУБД

- В командной строке введите одну из следующих команд: Замените *\<instancename>* именем экземпляра, которым необходимо управлять.  
  
    **net start "SQL Server (** *имя_экземпляра* **)"**
  
   -или-  
  
    **net start MSSQL$** *имя_экземпляра*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> Запуск ядра СУБД с параметрами запуска  

- Укажите разделенные пробелами параметры запуска в конце команды **net start "SQL Server (MSSQLSERVER)"** . При запуске с помощью команды **net start** в параметрах запуска используется косая черта (/), а не дефис (-).  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   -или-  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  Дополнительные сведения о параметрах запуска см. в разделе [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> Запуск агента SQL Server в экземпляре SQL Server по умолчанию
  
- В командной строке введите одну из следующих команд:  
  
    **net start "SQL Server Agent (MSSQLSERVER)"**
  
   -или-  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> Запуск агента SQL Server в именованном экземпляре SQL Server  
  
- В командной строке введите одну из следующих команд: Замените *имя_экземпляра* именем экземпляра, которым необходимо управлять.  
  
    **net start "SQL Server Agent(** *имя_экземпляра* **)"**
  
   -или-  
  
    **net start SQLAgent$** *имя_экземпляра*  
  
 Сведения о запуске агента SQL Server в подробном режиме для устранения неполадок см. в статье [Приложение sqlagent90](../../tools/sqlagent90-application.md).  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> Запуск обозревателя SQL Server  

- В командной строке введите одну из следующих команд:  
  
    **net start "SQL Server Browser"**
  
   -или-  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> Приостановка или остановка служб из окна командной строки  

- Чтобы приостановить или остановить службы, измените команды следующими способами.  

- Чтобы приостановить службу, вместо **net start** введите **net pause**.  

- Чтобы остановить службу, вместо **net start** введите **net stop**.  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

Ядро СУБД можно остановить с помощью инструкции **SHUTDOWN**.  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>Остановка ядра СУБД с помощью Transact-SQL

- Чтобы дождаться завершения запущенных в настоящий момент инструкций и хранимых процедур Transact-SQL с последующей остановкой ядра СУБД, выполните следующую инструкцию.  
  
    ```sql
    SHUTDOWN;
    ```
  
- Чтобы остановить ядро СУБД немедленно, выполните следующую инструкцию.  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

Дополнительные сведения об инструкции **SHUTDOWN** см. в статье [SHUTDOWN (Transact-SQL)](../../t-sql/language-elements/shutdown-transact-sql.md).  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>Запуск и остановка служб ядра СУБД

1. В окне командной строки запустите SQL Server PowerShell с помощью следующей команды.  

    ```cmd
    sqlps
    ```

2. В окне командной строки SQL Server PowerShell путем выполнения следующей команды. Замените `computername` именем нужного компьютера.  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. Определите службу, которую нужно остановить или запустить. Выберите одну из следующих строк. Замените `instancename` именем именованного экземпляра.

    - Получение ссылки на экземпляр ядра СУБД по умолчанию.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - Получение ссылки на именованный экземпляр ядра СУБД.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - Получение ссылки на службу агента SQL Server в экземпляре ядра СУБД по умолчанию.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - Получение ссылки на службу агента SQL Server в именованном экземпляре ядра СУБД.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - Получение ссылки на службу обозревателя SQL Server.

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. Завершите пример, чтобы запустить и затем остановить выбранную службу.
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> Использование класса контроллера службы

Класс ServiceController можно использовать для управления службой SQL Server или любой другой службой Windows. Пример того, как это сделать, см. в разделе [Класс ServiceController](/dotnet/api/system.serviceprocess.servicecontroller).

## <a name="manage-the-sql-server-service-on-linux"></a>Управление службой SQL Server в Linux

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>Запуск, остановка или перезапуск экземпляра ядра СУБД SQL Server

Ниже показано, как запустить, остановить, перезапустить [службу SQL Server в Linux](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service) и проверить ее состояние.

Проверьте состояние службы SQL Server с помощью следующей команды.

   ```bash
   sudo systemctl status mssql-server
   ```

Вы можете останавливать, запускать или перезапускать службу SQL Server по мере необходимости, используя следующие команды.

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>Дальнейшие действия

- [Обзор документации по установке SQL Server](../install-windows/install-sql-server.md)
- [Просмотр и чтение файлов журналов программы установки SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Диспетчер конфигурации SQL Server](../../relational-databases/sql-server-configuration-manager.md)
- [Запустите SQL Server с минимальной конфигурацией](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [Features Supported by the Editions of SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)