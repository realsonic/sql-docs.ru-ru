---
title: Установка SQL Server в Server Core | Документация Майкрософт
description: Можно установить SQL Server в установке Server Core. Вариант установки Server Core предусматривает наличие среды, минимально необходимой для запуска конкретных ролей сервера.
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1dd294cc-5b69-4d0c-9005-3e307b75678b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ab5a2352358fdb678241425e0ca5e8f999d8a2f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170176"
---
# <a name="install-sql-server-on-server-core"></a>Установка SQL Server в Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Можно установить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в установке Server Core.   
  
Вариант установки Server Core предусматривает наличие среды, минимально необходимой для запуска конкретных ролей сервера. Это дает возможность снизить требования к обслуживанию и управлению и уменьшить уязвимость для атак со стороны этих ролей сервера. Дополнительные сведения о Server Core см. в разделе [Установка Server Core](/windows-server/get-started/getting-started-with-server-core). Дополнительные сведения о реализации Server Core в операционной системе [!INCLUDE[win8srv](../../includes/win8srv-md.md)] см. в разделе [Server Core для Windows Server 2012](https://msdn.microsoft.com/library/hh846323\(VS.85\).aspx) (https://msdn.microsoft.com/library/hh846323(VS.85).aspx).  
  
 Список текущих поддерживаемых операционных систем см. в разделе [Требования к оборудованию и программному обеспечению для установки SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="prerequisites"></a>Предварительные требования  
  
|Требование|Как установить|  
|-----------------|--------------------|  
|[!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 |Для всех выпусков [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], кроме [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], программе установки требуется [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 Server Core Profile. Программа установки SQL Server автоматически установит это ПО, если оно еще не установлено. Для установки требуется перезагрузка. Чтобы избежать перезагрузки, можно установить [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] перед запуском программы установки.|  
|Установщик Windows 4.5|Поставляется с установкой Server Core.|  
|Windows PowerShell|Поставляется с установкой Server Core.|  
|Среда выполнения Java |Чтобы использовать PolyBase, необходимо установить соответствующую среду выполнения Java. Дополнительные сведения см. в разделе [Установка PolyBase](../../relational-databases/polybase/polybase-installation.md).|
  
##  <a name="supported-features"></a><a name="BK_SupportedFeatures"></a> Поддерживаемые компоненты  
 В следующей таблице можно найти компоненты, которые поддерживаются в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] в установке Server Core.  
  
|Компонент|Поддерживается|Дополнительные сведения|  
|-------------|---------------|----------------------------|  
|Службы[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Да||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Репликация|Да||  
|Полнотекстовый поиск|Да||  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Да||  
|[!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]|Да||  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|Нет||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Tools (SSDT)|Нет||  
|Средства связи клиентских средств|Да||  
|Сервер служб Integration Services|Да||  
|Обратная совместимость клиентских средств|Нет||  
|Пакет SDK клиентских средств|Нет||  
|Электронная документация по[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Нет||  
|Основные средства управления|Только удаленные|Установка этих компонентов на Server Core не поддерживается. Эти компоненты могут быть установлены на сервере, отличном от Server Core, и подключены к службам [!INCLUDE[ssDE](../../includes/ssde-md.md)], установленным в Server Core.|  
|Средства управления — полный набор|Только удаленные|Установка этих компонентов на Server Core не поддерживается. Эти компоненты могут быть установлены на сервере, отличном от Server Core, и подключены к службам [!INCLUDE[ssDE](../../includes/ssde-md.md)], установленным в Server Core.|  
|Контроллер распределенного воспроизведения|Нет||  
|Клиент распределенного воспроизведения|Только удаленные|Установка этих компонентов на Server Core не поддерживается. Эти компоненты могут быть установлены на сервере, отличном от Server Core, и подключены к службам [!INCLUDE[ssDE](../../includes/ssde-md.md)], установленным в Server Core.|  
|Пакет SDK для подключения клиентов SQL|Да||  
|Microsoft Sync Framework|Да|Платформа Microsoft Sync Framework не входит в установочный пакет [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] . Вы можете скачать соответствующую версию Sync Framework в [Центре загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkId=221788) (https://go.microsoft.com/fwlink/?LinkId=221788) и установить ее на компьютер, где работает установка Server Core.|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|Нет||  
|[!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)]|Нет||  
  
## <a name="supported-scenarios"></a>Поддерживаемые сценарии  
 В следующей таблице показана матрица поддерживаемых сценариев для установки [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] в Server Core.  
  
| Установка | Допустимый целевой объект |  
|-|-|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выпуски|Все 64-разрядные версии [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] |  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , язык|Все языки|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , язык в языке ОС-локали (сочетание)|ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для JPN (японский) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для GER (немецкий) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для CHS (китайский — Китай) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для ARA (арабский (SA)) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для THA (тайский) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для TRK (турецкий) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для pt-PT (португальский, Португалия) Windows<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для ENG (английский) Windows|  
|Выпуск Windows|Windows Server 2019 Datacenter <br/><br/> Windows Server 2019 Standard <br /><br />  [!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
  
## <a name="upgrade"></a>Обновление 
 В установках Server Core поддерживается обновление с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .  
  
## <a name="install"></a>Установка  
 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] не поддерживает установку с помощью мастера установки в операционной системе Server Core. При установке на Server Core программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает полный тихий режим без вывода сообщений с использованием параметра /Q или простой режим без вывода сообщений с использованием параметра /QS. Дополнительные сведения см. в разделе [Установка SQL Server из командной строки](./install-sql-server-from-the-command-prompt.md).  
  
 Независимо от метода установки, необходимо подтвердить свое согласие с условиями лицензии на использование пакета программ как физического лица или от имени организации, если на используемое программное обеспечение не распространяется отдельное соглашение [!INCLUDE[msCoName](../../includes/msconame-md.md)] , такое как соглашение о корпоративном лицензировании Майкрософт или отдельное соглашение с независимым поставщиком программного обеспечения или изготовителем оборудования (OEM).  
  
 Условия лицензионного соглашения отображаются для ознакомления и принятия в пользовательском интерфейсе программы установки. Автоматические установки (с использованием параметров /Q или /QS) должны включать параметр /IACCEPTSQLSERVERLICENSETERMS. Ознакомиться с условиями лицензии можно на странице [Условия лицензионного соглашения о программном обеспечении Майкрософт](https://go.microsoft.com/fwlink/?LinkId=148209).  
  
> [!NOTE]  
>  В зависимости от способа получения ПО (например, по [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), на его использование могут распространяться дополнительные условия.  
  
 Для установки отдельных компонентов используйте параметр /FEATURES и укажите значения родительского компонента или компонентов. Дополнительные сведения о параметрах компонентов и их использовании см. в следующих подразделах.  
  
### <a name="feature-parameters"></a>Параметры компонентов  
  
|Параметр компонента|Описание|  
|-----------------------|-----------------|  
|SQLENGINE|Устанавливает только компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|РЕПЛИКАЦИЯ|Устанавливает компонент репликации вместе с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|FULLTEXT|Устанавливает компонент FullText вместе с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|AS|Устанавливает все компоненты служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|IS|Устанавливает все компоненты служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|CONN|Устанавливает компоненты подключения к данным.| 
|ADVANCEDANALYTICS |Устанавливает службы R Services; требуется ядро СУБД. Для автоматической установки требуется параметр /IACCEPTROPENLICENSETERMS.  |


 В следующих примерах показано использование параметров компонентов.  
  
|Параметр и значения|Описание|  
|--------------------------|-----------------|  
|/FEATURES=SQLEngine|Устанавливает только компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|/FEATURES=SQLEngine, FullText|Устанавливает компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] с компонентом Full-Text Search.|  
|/FEATURES=SQLEngine, Conn|Устанавливает компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] и компоненты подключения к данным.|  
|/FEATURES=SQLEngine, AS, IS, Conn|Устанавливает компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)], службы [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]и компоненты подключения к данным.|  
|/FEATURES=SQLENGINE,ADVANCEDANALYTICS /IACCEPTROPENLICENSETERMS |Устанавливает  [!INCLUDE[ssDE](../../includes/ssde-md.md)] и [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)].|  

  
### <a name="installation-options"></a>Варианты установки  
 Программа установки поддерживает следующие варианты установки [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] в операционной системе Server Core.  
  
1.  **Установка из командной строки**  
  
     Чтобы установить конкретные компоненты с помощью командной строки, необходимо использовать параметр /FEATURES и указать родительский компонент или конкретные компоненты. Ниже приведен пример указания параметров в командной строке.  
  
    ```console
    Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,Replication /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /TCPENABLED=1 /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
2.  **Установка с помощью файла конфигурации**  
  
     Программа установки поддерживает использование файлов конфигурации только через командную строку. Файл конфигурации — это текстовый файл, содержащий параметры (пара «имя-значение») и комментарии с описанием. Файл конфигурации, который указывается в командной строке, должен иметь расширение INI. Ниже приведены примеры файла ConfigurationFile.ini.  
  
    - Установка [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 
    
    В следующем примере показано, как установить новый автономный экземпляр, который включает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)].  

    ```console
    ; SQL Server Configuration File  
    [OPTIONS]  
    
    ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
    
    ACTION="Install"  
    
    ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
    
    FEATURES=SQLENGINE  
    
    ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine, and Analysis Services (AS).  
    
    INSTANCENAME="MSSQLSERVER"  
    
    ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
    
    INSTANCEID="MSSQLSERVER"  
    
    ; Account for ssNoVersion service: Domain\User or system account.   
    
    SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
    
    ; Windows account(s) to provision as ssNoVersion system administrators.   
    
    SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
    
    ; Accept the License agreement to continue with Installation  
    
    IAcceptSQLServerLicenseTerms="True"  
    
    ```

    -   Установка компонентов подключения к данным. Следующий пример показывает, как установить компоненты подключения к данным:  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=Conn  
  
        ; Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True  
  
        ```  
  
    -   Установка всех поддерживаемых компонентов  
  
        Следующий пример показывает, как установить все поддерживаемые компоненты [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] в Server Core:  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=SQLENGINE,FullText,Replication,AS,IS,Conn  
  
        ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine (SQL), or Analysis Services (AS).  
  
        INSTANCENAME="MSSQLSERVER"  
  
        ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
  
        INSTANCEID="MSSQLSERVER"  
  
        ; Account for ssNoVersion service: Domain\User or system account.   
  
        SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
  
        ; Windows account(s) to provision as ssNoVersion system administrators.   
  
        SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; The name of the account that the Analysis Services service runs under.   
  
        ASSVCACCOUNT= "NT Service\MSSQLServerOLAPService"  
  
        ; Specifies the list of administrator accounts that need to be provisioned.   
  
        ASSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; Specifies the server mode of the Analysis Services instance. Valid values are MULTIDIMENSIONAL, POWERPIVOT or TABULAR. ASSERVERMODE is case-sensitive. All values must be expressed in upper case.   
  
        ASSERVERMODE="MULTIDIMENSIONAL"  
  
        ; Optional value, which specifies the state of the TCP protocol for the ssNoVersion service. Supported values are: 0 to disable the TCP protocol, and 1 to enable the TCP protocol.  
  
        TCPENABLED=1  
  
        ;Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True"  
        ```  
  
     Далее показан процесс запуска установки с использованием пользовательского или стандартного файла конфигурации.  
  
    -   Запуск установки с использованием пользовательского файла конфигурации.  
  
         Указание файла конфигурации в командной строке:  
  
        ```console
        Setup.exe /QS /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
         Указание паролей в командной строке, а не в файле конфигурации:  
  
        ```console
        Setup.exe /QS /SQLSVCPASSWORD="************" /ASSVCPASSWORD="************"  /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
    -   Запуск установки с использованием DefaultSetup.ini.  
  
         Если файл DefaultSetup.ini находится в папках \x86 и \x64 в корневой папке исходного носителя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , откройте этот файл и добавьте в него параметр *Features* .  
  
         Если файл DefaultSetup.ini не существует, создайте его и скопируйте в папки \x86 и \x64 корневой папки исходного носителя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="configure-remote-access-of-ssnoversion-on-server-core"></a>Настройка удаленного доступа для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при работе в Server Core  
 Чтобы настроить удаленный доступ к экземпляру [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], который запускается в Server Core, выполните описанные ниже действия.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Включение удаленных подключений на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Чтобы разрешить удаленные соединения, выполните следующие инструкции для экземпляра Server Core в локальной программе SQLCMD.exe.  

```sql
EXEC sys.sp_configure N'remote access', N'1'  
GO
RECONFIGURE WITH OVERRIDE
GO
```  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Включите и запустите службу браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] browser service  
 По умолчанию эта служба отключена.  Если она отключена на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , работающем на Server Core, то для ее включения выполните следующую команду из командной строки:  
  
 `sc config SQLBROWSER start= auto`  
  
 После включения службы выполните следующую команду из командной строки, чтобы запустить службу:  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Создание исключений в брандмауэре Windows  
 Чтобы создать исключения в брандмауэре Windows для доступа к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполните действия, описанные в разделе [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Включите поддержку TCP/IP на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Протокол TCP/IP для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Server Core можно включить через Windows PowerShell. Выполните следующие действия.  
  
1.  В PowerShell: Import-Module SQLPS.  
  
2.  В окне **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** выполните следующий скрипт, чтобы включить протокол TCP/IP:  
  
```powershell  
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
## <a name="uninstall"></a>Удаление

 После входа на компьютер, где работает Server Core, вы получите доступ к ограниченной среде с командной строкой администратора. Командную строку можно использовать для запуска удаления [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Чтобы удалить экземпляр [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], запустите удаление из командной строки — либо в полностью тихом режиме (параметр /Q), либо в простом тихом режиме (параметр /QS). Если указан параметр /QS, то ход выполнения будет отображаться в пользовательском интерфейсе, но не потребует ввода. Параметр /Q запускает тихий режим без пользовательского интерфейса.  
  
 Удаление существующего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```console
Setup.exe /Q /Action=Uninstall /FEATURES=SQLEngine,AS,IS /INSTANCENAME=MSSQLSERVER  
```  
  
 Чтобы удалить именованный экземпляр, укажите его имя вместо имени `MSSQLSERVER`, указанного в предыдущем примере.  
  
## <a name="start-a-new-command-prompt"></a>Открытие нового окна командной строки

Если окно командной строки было случайно закрыто, то его можно открыть снова, выполнив следующие действия.  
 
1.  Нажмите CTRL+SHIFT+ESC, чтобы отобразить диспетчер задач.  
2.  На вкладке **Приложения** нажмите **Создать задачу**.  
3.  В диалоговом окне **Создание новой задачи** введите **cmd** в поле **Открыть** [!INCLUDE[clickOK](../../includes/clickok-md.md)], а затем.  
  
## <a name="see-also"></a>См. также раздел  
 [Установка SQL Server с помощью файла конфигурации](./install-sql-server-using-a-configuration-file.md)   
 [Установка SQL Server из командной строки](./install-sql-server-from-the-command-prompt.md)   
 [Выпуски и поддерживаемые функции SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)   
 [Установка Server Core](/windows-server/get-started/getting-started-with-server-core)   
 [Настройка установки Server Core системы Windows Server 2016 с помощью Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)   
 [Командлеты отказоустойчивого кластера в Windows PowerShell](/powershell/module/failoverclusters/)

  
