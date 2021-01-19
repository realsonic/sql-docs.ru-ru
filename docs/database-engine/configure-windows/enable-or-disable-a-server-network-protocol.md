---
title: Включение или отключение сетевого протокола сервера | Документы Майкрософт
description: Сведения о включении или отключении сетевого протокола сервера. Информация о том, как использовать диспетчер конфигурации SQL Server или PowerShell для выполнения этой задачи.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- network protocols [SQL Server], disabling
- remote connections [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], disabling using Configuration Manager
- disabling network protocols, Configuration Manager
- network protocols [SQL Server], enabling
- enabling network protocols, Configuration Manager
- surface area configuration [SQL Server], connection protocols
- connections [SQL Server], enabling remote using Configuration Manager
ms.assetid: ec5ccb69-61c9-4576-8843-014b976fd46e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b5f15490744ccfe8bfd47a69892f7033ddae361
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171006"
---
# <a name="enable-or-disable-a-server-network-protocol"></a>Включение или отключение сетевого протокола сервера
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Все сетевые протоколы устанавливаются программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , но некоторые могут быть включены, а некоторые — нет. В этом разделе описано, как включить или отключить сетевой протокол сервера в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или PowerShell. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] необходимо остановить и перезапустить, чтобы изменения вступили в силу.  
  
> [!IMPORTANT]  
>  Во время установки [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] для группы BUILTIN\Users добавляется имя входа. Благодаря этому все прошедшие проверку подлинности пользователи компьютера получают доступ к экземпляру [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] как члены роли public. Имя входа группы BUILTIN\Users можно удалить, чтобы ограничить доступ к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] только пользователям компьютера, у которых есть отдельные имена входа, или членам других групп Windows с именами входа.  
  
> [!WARNING]  
>  Поставщики данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[msCoName](../../includes/msconame-md.md)] для версий с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по [!INCLUDE[sssql14](../../includes/sssql14-md.md)] по умолчанию поддерживают только TLS 1.0 и SSL 3.0. Если применить другой протокол (например, TLS 1.1 или TLS 1.2), изменив протокол на уровне операционной системы SChannel, то подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может завершиться ошибкой, если не установлено соответствующее обновление для поддержки TLS 1.1 и 1.2 в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое указано <a href="https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server">здесь</a>. Начиная с [!INCLUDE[sssql15](../../includes/sssql16-md.md)] все выпуски SQL Server поддерживают TLS 1.2 без установки дополнительных обновлений.
  
 **В этом разделе**  
  
-   **Включение или отключение сетевого протокола сервера с использованием следующего.**  
  
     [Диспетчер конфигурации SQL Server](#SSMSProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Использование диспетчера конфигурации SQL Server  
  
#### <a name="to-enable-a-server-network-protocol"></a>Включение протокола SNP  
  
1.  В диспетчере конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на панели консоли раскройте **Сетевая конфигурация SQL Server**.  
  
2.  В области консоли щелкните **Протоколы для** _\<instance name>_ .  
  
3.  В области сведений щелкните правой кнопкой мыши протокол, который необходимо переключить, затем выберите **Включить** или **Отключить**.  
  
4.  В области консоли выберите **Службы SQL Server**.  
  
5.  В области сведений щелкните правой кнопкой мыши **SQL Server (** _\<instance name>_ **)** и выберите команду **Перезапустить**, чтобы остановить и снова запустить службу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-sql-server-powershell"></a><a name="PowerShellProcedure"></a> Использование SQL Server PowerShell  
  
#### <a name="to-enable-a-server-network-protocol-using-powershell"></a>Включение сетевого протокола сервера с использованием PowerShell  
  
1.  Откройте командную строку с использованием разрешений администратора.  
  
2.  Запустите Windows PowerShell из панели задач или нажмите кнопку "Пуск", а затем последовательно выберите "Все программы", "Стандартные", "Windows PowerShell" и "Windows PowerShell".  
  
3.  Импортируйте модуль **sqlps**, введя команду **Import-Module "sqlps"** .  
  
4.  Выполните следующие инструкции, чтобы включить протокол TCP и протокол именованных каналов. Замените `<computer_name>` именем компьютера, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если настраивается именованный экземпляр, замените `MSSQLSERVER` именем экземпляра.  
  
     Чтобы отключить протоколы, установите для свойства `IsEnabled` значение `$false`.  
  
    ```  
    $smo = 'Microsoft.SqlServer.Management.Smo.'  
    $wmi = new-object ($smo + 'Wmi.ManagedComputer').  
  
    # List the object properties, including the instance names.  
    $Wmi  
  
    # Enable the TCP protocol on the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    $Tcp = $wmi.GetSmoObject($uri)  
    $Tcp.IsEnabled = $true  
    $Tcp.Alter()  
    $Tcp  
  
    # Enable the named pipes protocol for the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Np']"  
    $Np = $wmi.GetSmoObject($uri)  
    $Np.IsEnabled = $true  
    $Np.Alter()  
    $Np  
    ```  
  
#### <a name="to-configure-the-protocols-for-the-local-computer"></a>Настройка протоколов на локальном компьютере  
  
-   Если скрипт запускается локально и настраивает локальный компьютер, с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell можно сделать скрипт более гибким, динамически определяя имя локального компьютера. Для получения имени локального компьютера замените строку, устанавливающую переменную `$uri` , следующей строкой.  
  
    ```  
    $uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    ```  
  
#### <a name="to-restart-the-database-engine-by-using-sql-server-powershell"></a>Перезапуск компонента Database Engine с использованием SQL Server PowerShell  
  
-   После включения или отключения протоколов необходимо остановить и перезапустить компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] , чтобы эти изменения вступили в действие. Выполните следующие инструкции, чтобы остановить и запустить экземпляр по умолчанию с использованием [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Чтобы остановить и запустить именованный экземпляр, замените `'MSSQLSERVER'` на `'MSSQL$<instance_name>'`.  
  
    ```  
    # Get a reference to the ManagedComputer class.  
    CD SQLSERVER:\SQL\<computer_name>  
    $Wmi = (get-item .).ManagedComputer  
    # Get a reference to the default instance of the Database Engine.  
    $DfltInstance = $Wmi.Services['MSSQLSERVER']  
    # Display the state of the service.  
    $DfltInstance  
    # Stop the service.  
    $DfltInstance.Stop();  
    # Wait until the service has time to stop.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    # Start the service again.  
    $DfltInstance.Start();  
    # Wait until the service has time to start.  
    # Refresh the cache and display the state of the service.  
    $DfltInstance.Refresh(); $DfltInstance  
    ```  
  
  
