---
description: Службы Integration Services (службы SSIS)
title: Службы Integration Services (службы SSIS) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssiseditserverregistration.connectionproperties.f1
- sql13.swb.connecttodts.connectionproperties.f1
- sql13.swb.connection.login.dtsserver.f1
- sql13.swb.connecttodts.login.f1
- sql13.swb.connecttodtsserver.login.f1
helpviewer_keywords:
- Integration Services service, about Integration Services service
- SQL Server Integration Services service
- service [Integration Services],about Integration Services service
- service [Integration Services]
- SQL Server Integration Services, service
ms.assetid: 2c785b3b-4a0c-4df7-b5cd-23756dc87842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a665d78d180d87fe0d412b025bcce75d509285d4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171206"
---
# <a name="integration-services-service-ssis-service"></a>Службы Integration Services (службы SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  В подразделах этого раздела описывается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] — служба Windows для управления пакетами служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Эта служба не требуется для создания, сохранения и выполнения пакетов служб Integration Services. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] поддерживает службу [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] для обеспечения обратной совместимости с более ранними версиями служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] сохраняют объекты, настройки и рабочие данные в базе данных **SSISDB** для проектов, развернутых на сервере служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] с помощью модели развертывания проектов. На сервере служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , который является экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ядра СУБД, размещается база данных. Дополнительные сведения о базе данных см. в разделе [Каталог служб SSIS](../../integration-services/catalog/ssis-catalog.md). Дополнительные сведения о развертывании проектов на сервере служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] см. в разделе [Развертывание проектов и пакетов служб Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
## <a name="management-capabilities"></a>Функции управления  
 Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] является службой Windows для управления пакетами служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] доступна только в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Использование службы Windows служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] дает следующие возможности управления:  
  
-   запуск удаленных и локально хранимых пакетов;  
  
-   остановка удаленных и локально запущенных пакетов;  
  
-   наблюдение за работой удаленных и локальных пакетов;  
  
-   импорт и экспорт пакетов;  
  
-   управление хранилищем пакетов;  
  
-   настройка папок хранения;  
  
-   остановка запущенных пакетов при остановке службы;  
  
-   просмотр журнала событий Windows;  
  
-   соединение с несколькими серверами служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
## <a name="startup-type"></a>Тип запуска
 Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] устанавливается при установке компонента [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. По умолчанию запускается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] и устанавливается ее автоматический запуск. Для наблюдения за пакетами, хранящимися в хранилище пакетов служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , эта служба должна быть запущена. Хранилищем пакетов служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] может быть как база данных msdb в экземпляре служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , так и назначенные папки файловой системы.  
  
 Запуск службы Windows служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] не требуется, если необходимо только создавать и выполнять пакеты служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Однако эта служба необходима для перечисления и монитора пакетов, использующих среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

## <a name="manage-the-service"></a>Управление службой
  
 При установке компонента служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]также устанавливается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . По умолчанию запускается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] и устанавливается ее автоматический запуск. Однако необходимо также установить среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , чтобы использовать эту службу для управления хранимыми и эксплуатируемыми пакетами [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
> [!NOTE]
> Для соединения непосредственно с экземпляром устаревшей версии службы Integration Service необходимо использовать версию SQL Server Management Studio (SSMS), соответствующую версии SQL Server, где выполняются службы Integration Services. Например, чтобы соединиться с устаревшей версией служб Integration Services, выполняющихся на экземпляре SQL Server 2016, необходимо использовать версию SSMS, выпущенную для SQL Server 2016. [Скачайте SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>   В диалоговом окне SSMS **Соединение с сервером** нельзя вводить имя сервера, на котором работает более ранняя версия службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Однако чтобы управлять пакетами, которые хранятся на удаленном сервере, не нужно соединяться с экземпляром службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на этом удаленном сервере. Вместо этого измените файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] таким образом, чтобы среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] отображала пакеты, хранимые на удаленном сервере.   
  
 Предусмотрена возможность установить только единственный экземпляр службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на отдельном компьютере. Эта служба не относится к конкретному экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Подключение к этой службе осуществляется с использованием имени компьютера, на котором она эксплуатируется.  
  
 Для управления службой [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] может применяться одна из следующих оснасток консоли управления (MMC): "Диспетчер конфигурации SQL Server" или "Службы". Прежде чем появится возможность управлять пакетами в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], необходимо убедиться, что служба запущена.  
  
 По умолчанию служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] настроена для управления пакетами в базе данных msdb экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], который установлен одновременно со службами [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Если экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] не установлен в то же время, служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] будет настроена для управления пакетами базы данных msdb локального экземпляра по умолчанию компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Чтобы управлять пакетами, которые хранятся в именованном или удаленном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]либо в нескольких экземплярах компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], необходимо изменить файл конфигурации для службы.
  
 По умолчанию служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] настроена таким образом, чтобы прекращать выполнение пакетов по завершении работы службы. Однако служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] не ожидает прекращения выполнения пакетов, и некоторые пакеты могут продолжать выполняться по завершении работы службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Если работа службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] остановлена, можно продолжить выполнение пакетов с помощью мастера импорта и экспорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , конструктора служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , программы выполнения пакетов и программы командной строки **dtexec** (dtexec.exe). Однако контролировать выполнение пакетов невозможно.  
  
 По умолчанию служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] выполняется в контексте учетной записи NETWORK SERVICE. Рекомендуется запускать службу SQL Server Integration Services под учетной записью с ограниченными разрешениями, такими как учетная запись NETWORK SERVICE. Запуск службы SQL Server Integration Services в учетной записи с большими привилегиями представляет потенциальную угрозу безопасности. 
  
 Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] вносит записи в журнал событий Windows. Также можно просмотреть события службы в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Можно также просматривать события службы с использованием программы просмотра событий.  
  
## <a name="set-the-properties-of-the-service"></a>Настройка свойств службы
  
 Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] управляет и отслеживает пакеты в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. После первой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] запускается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], а тип запуска для нее устанавливается на автоматический.  
  
 После установки службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] можно задавать свойства службы, используя диспетчер конфигурации SQL Server или оснастку Services консоли MMC.  
  
 Чтобы настроить другие важные функции службы, включая местоположения, в которых осуществляется хранение и управление пакетами, необходимо внести изменения в файл конфигурации службы.
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-sql-server-configuration-manager"></a>Определение свойств службы Integration Services с использованием диспетчера конфигурации SQL Server  
  
1.  В меню **Пуск** укажите **Все программы**, **Microsoft SQL Server**, **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
2.  В оснастке **Диспетчер конфигурации SQL Server** в списке служб найдите **Службы SQL Server Integration Services** , щелкните правой кнопкой мыши **Службы SQL Server Integration Services** и выберите **Свойства**.  
  
3.  В диалоговом окне **Свойства служб SQL Server Integration Services** можно выполнить следующие действия.  
  
    -   Перейдите на вкладку **Вход** , чтобы просмотреть учетные данные, такие как имя учетной записи.  
  
    -   Перейдите на вкладку **Служба** , чтобы просмотреть сведения о службе, такие как имя главного компьютера, и указать режим запуска службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
        > [!NOTE]  
        >  На вкладке **Дополнительно** сведений о службе [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] не содержится.  
  
4.  Нажмите кнопку **ОК**.  
  
5.  Чтобы закрыть оснастку **Диспетчер конфигурации SQL Server** , в меню **Файл** выберите пункт **Выход** .  
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-services"></a>Определение свойств службы Integration Services с использованием оснастки Services  
  
1.  При использовании классического вида **панели управления** щелкните **Администрирование**; если используется вид по категориям, щелкните **Производительность и обслуживание** , а затем **Администрирование**.  
  
2.  Щелкните **Службы**.  
  
3.  В оснастке **Службы** в списке служб найдите **SQL Server Integration Services** , щелкните правой кнопкой мыши **SQL Server Integration Services** и выберите пункт **Свойства**.  
  
4.  В диалоговом окне **Свойства служб SQL Server Integration Services** можно выполнить следующие действия.  
  
    -   Перейдите на вкладку **Общие** . Чтобы включить службу, выберите ручной или автоматический запуск. Чтобы выключить службу, выберите «Отключено» в поле **Тип запуска** . Выбор варианта «Отключено» не останавливает службу, если она работает в данный момент.  
  
         Если служба уже включена, можно нажать кнопку **Стоп** для остановки службы или **Пуск** для запуска службы.  
  
    -   Перейдите на вкладку **Вход** , чтобы просмотреть или изменить учетные данные.  
  
    -   Перейдите на вкладку **Восстановление** для просмотра реакции компьютера по умолчанию на ошибку в работе службы. Эти параметры можно изменять в соответствии с требованиями среды.  
  
    -   Перейдите на вкладку **Зависимости** для просмотра перечня зависимых служб. У служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] нет зависимостей.  
  
5.  Нажмите кнопку **ОК**.  
  
6.  При необходимости, если тип запуска ручной или автоматический, можно щелкнуть правой кнопкой мыши службы **SQL Server Integration Services** и выбрать пункт **Пуск, Стоп или Перезапустить**.  
  
7.  Чтобы закрыть оснастку **Службы** , в меню **Файл** выберите пункт **Выход** .  

## <a name="grant-permissions-to-the-service"></a>Предоставление разрешений службе
  В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]по умолчанию при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] все пользователи в группе пользователей имели доступ к службе [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . При установке текущего выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]пользователи не имеют доступа к службе [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . По умолчанию эта служба является защищенной. После завершения установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] администратор должен предоставить доступ к службе.  
  
### <a name="to-grant-access-to-the-integration-services-service"></a>Предоставление доступа к службе Integration Services  
  
1.  Запустите файл Dcomcnfg.exe. Программа Dcomcnfg.exe предоставляет пользовательский интерфейс для изменения определенных параметров в реестре.  
  
2.  В диалоговом окне **Службы и компоненты** последовательно разверните "Службы и компоненты" > "Компьютеры" > "Мой компьютер" > "Настройка DCOM".  
  
3.  Щелкните правой кнопкой мыши **SQL Server Integration Services 13.0** и выберите **Свойства**.  
  
4.  На вкладке **Безопасность** нажмите кнопку **Правка** в области **Разрешение на запуск и активацию** .  
  
5.  Добавьте пользователей и назначьте им соответствующие разрешения, а затем нажмите кнопку «ОК».  
  
6.  Повторите шаги 4 - 5 для назначения разрешений на доступ.  
  
7.  Перезапустите среду SQL Server Management Studio.  
  
8.  Перезапустите службу [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  

### <a name="event-logged-when-permissions-are-missing"></a>Событие, регистрируемое при отсутствии разрешений

Если учетная запись службы агента SQL Server не имеет **[разрешений на запуск и активацию]** модели DCOM служб Integration Services, следующее событие добавляется в журналы системных событий, когда агент SQL Server выполняет пакетные задания служб SQL Server Integration Services:

```
Log Name: System
Source: **Microsoft-Windows-DistributedCOM**
Date: 1/9/2019 5:42:13 PM
Event ID: **10016**
Task Category: None
Level: Error
Keywords: Classic
User: NT SERVICE\SQLSERVERAGENT
Computer: testmachine
Description:
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
and APPID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
to the user NT SERVICE\SQLSERVERAGENT SID (S-1-5-80-344959196-2060754871-2302487193-2804545603-1466107430) from address LocalHost (Using LRPC) running in the application container Unavailable SID (Unavailable). This security permission can be modified using the Component Services administrative tool.
```

## <a name="configure-the-service"></a>Настройка службы
 
При установке служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]процесс установки создает и устанавливает файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Этот файл конфигурации содержит следующие настройки.  
  
-   При остановки службы пакетам посылается команда остановки.  
  
-   Корневыми папками служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] для отображения в обозревателе объектов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] являются папки MSDB и файловой системы.  
  
-   Пакеты файловой системы, которыми управляет служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], расположены в папке %ProgramFiles%\Microsoft SQL Server\130\DTS\Packages.  
  
 В этом файле конфигурации указывается, какая база данных msdb содержит пакеты, которыми будет управлять служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . По умолчанию служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] настроена для управления пакетами в базе данных msdb экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], который установлен одновременно со службами [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Если экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] не установлен в то же время, служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] будет настроена для управления пакетами базы данных msdb локального экземпляра по умолчанию компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="default-configuration-file-example"></a>Пример файла конфигурации по умолчанию  
 В следующем примере показан файл конфигурации по умолчанию, который задает следующие параметры.  
  
-   Выполнение пакетов прекращается, если останавливается служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Корневыми папками для хранилища пакетов в службах [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] являются MSDB и File System.  
  
-   Эта служба управляет пакетами, хранящимися в базе данных msdb локального экземпляра по умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Службы управляют пакетами, хранящимися в папке Packages файловой системы.  
  
 **Пример стандартного файла конфигурации**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>.</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file"></a>Измените файл конфигурации.  
 Можно изменить файл конфигурации, чтобы продолжить выполнение пакетов при остановке службы, отображать дополнительные корневые папки в обозревателе объектов или указать другую папку или дополнительные папки файловой системы, которые будут управляться службой [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Например, можно создать дополнительные корневые папки типа **SqlServerFolder**, чтобы управлять пакетами в базах данных msdb дополнительных экземпляров компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
> [!NOTE]  
>  Некоторые символы в именах папок являются недопустимыми. Допустимые знаки в именах папок определяются классом [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]**System.IO.Path** и полем **GetInvalidFilenameChars** . Поле **GetInvalidFilenameChars** содержит специфический для платформы набор знаков, которые не могут быть использованы в аргументах, содержащих строки пути и передаваемых элементам класса **Path** . Набор недопустимых символов меняется в зависимости от файловой системы. Обычно недопустимые символы включают кавычки ("), знак «меньше» (<) и вертикальную черту (|).  
  
 Однако чтобы управлять пакетами, хранящимися в именованном или удаленном экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], необходимо изменить файл конфигурации. Если не обновить файл конфигурации, в среде **нельзя будет использовать** обозреватель объектов [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , чтобы просмотреть пакеты, хранящиеся в базе данных msdb на именованном или удаленном экземпляре. При попытке использовать **обозреватель объектов** для просмотра этих пакетов появляется следующее сообщение об ошибке.  
  
 `Failed to retrieve data for this request. (Microsoft.SqlServer.SmoEnum)`  
  
 `The SQL Server specified in Integration Services service configuration is not present or is not available. This might occur when there is no default instance of SQL Server on the computer. For more information, see the topic "Configuring the Integration Services Service" in SQL Server 2008 Books Online.`  
  
 `Login Timeout Expired`  
  
 `An error has occurred while establishing a connection to the server. When connecting to SQL Server 2008, this failure may be caused by the fact that under the default settings SQL Server does not allow remote connections.`  
  
 `Named Pipes Provider: Could not open a connection to SQL Server [2]. (MsDtsSvr).`  
  
 Чтобы изменить файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , используется текстовый редактор.  
  
> [!IMPORTANT]  
>  После изменения файла конфигурации службы необходимо перезапустить службы, чтобы они использовали обновленную конфигурацию.  
  
### <a name="modified-configuration-file-example"></a>Пример измененного файла конфигурации  
 В следующем примере показан модифицированный файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Этот файл предназначен для именованного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , называемого `InstanceName` на сервере с именем `ServerName`.  
  
 **Пример модифицированного файла конфигурации для именованного экземпляра SQL Server**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>ServerName\InstanceName</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file-location"></a>Изменение расположения файла конфигурации  
 Раздел реестра **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\ServiceConfigFile** указывает расположение и имя файла конфигурации, используемого службами [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. По умолчанию этот раздел реестра имеет значение **C:\Program Files\Microsoft SQL Server\130\DTS\Binn\MsDtsSrvr.ini.xml**. Можно изменить значение этого раздела реестра, чтобы использовать другое имя и местонахождение файла конфигурации. Обратите внимание на то, что номер версии в пути (120 для SQL Server [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)], 130 для [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и т. д.) зависит от версии SQL Server.
  
> [!CAUTION]  
>  Неправильное изменение реестра может приводить к серьезным проблемам, вплоть до необходимости переустановки операционной системы. [!INCLUDE[msCoName](../../includes/msconame-md.md)] не гарантирует возможность разрешения проблем, возникших в результате неправильного изменения реестра. Перед изменением реестра создайте резервную копию всех необходимых данных. Дополнительные сведения о том, как выполнять создание резервной копии, восстановление и изменение системного реестра, см. в [!INCLUDE[msCoName](../../includes/msconame-md.md)] статье базы знаний [Описание системного реестра Microsoft Windows](https://support.microsoft.com/kb/256986).  
  
 Службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] загружают файл конфигурации при запуске. Все изменения записей реестра требуют перезапуска службы.  

## <a name="connect-to-the-local-service"></a>Подключение к локальной службе
  Для подключения к службе [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] необходимо, чтобы администратор предоставил к ней доступ. 
  
### <a name="to-connect-to-the-integration-services-service"></a>Подключение к службам Integration Services  
  
1.  Откройте среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Щелкните **обозреватель объектов** в меню **Вид** .  
  
3.  На панели инструментов обозревателя объектов нажмите кнопку **Соединить** и выберите **Службы Integration Services**.  
  
4.  В диалоговом окне **Соединение с сервером** введите имя сервера. Для указания локального сервера можно использовать точку (.), (local) или **localhost** .  
  
5.  Нажмите кнопку **Соединить**.  

## <a name="connect-to-a-remote-ssis-server"></a>Подключение к удаленному серверу служб Integration Services
  
 Соединение с экземпляром служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на удаленном сервере из среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или другого управляющего приложения требует определенного набора прав на сервере для пользователей этого приложения.  
  
> [!IMPORTANT]
> Для соединения непосредственно с экземпляром устаревшей версии службы Integration Service необходимо использовать версию SQL Server Management Studio (SSMS), соответствующую версии SQL Server, где выполняются службы Integration Services. Например, чтобы соединиться с устаревшей версией служб Integration Services, выполняющихся на экземпляре SQL Server 2016, необходимо использовать версию SSMS, выпущенную для SQL Server 2016. [Скачайте SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>  Чтобы управлять пакетами, которые хранятся на удаленном сервере, не нужно соединятся с экземпляром службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на этом удаленном сервере. Вместо этого измените файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] таким образом, чтобы среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] отображала пакеты, хранимые на удаленном сервере.
  
### <a name="connecting-to-integration-services-on-a-remote-server"></a>Подключение к службам Integration Services на удаленном сервере  
  
#### <a name="to-connect-to-integration-services-on-a-remote-server"></a>Подключение к службам Integration Services на удаленном сервере  
  
1.  Откройте среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  В меню **Файл** выберите пункт **Подключить к обозревателю объектов** , чтобы вывести диалоговое окно **Подключение к серверу** .  
  
3.  Выберите службы **Integration Services** в списке **Тип сервера** .  
  
4.  Введите имя сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] в текстовом поле **Имя сервера**.  
  
    > [!NOTE]  
    >  Службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] не привязаны к экземпляру. Чтобы подключиться к службам, используется имя компьютера, на котором работают службы Integration Services.  
  
5.  Нажмите кнопку **Соединить**.  
  
> [!NOTE]  
>  В диалоговом окне **Выбор серверов** не отображаются удаленные экземпляры служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Кроме того, параметры, доступные на вкладке **Параметры подключения** диалогового окна **Подключение к серверу** , которое выводится при нажатии кнопки **Параметры** , неприменимо для подключения к службам [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="eliminating-the-access-is-denied-error"></a>Устранение ошибки «Доступ запрещен»  
 Если пользователь без достаточных прав пытается подключиться к экземпляру служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на удаленном сервере, сервер отвечает сообщением об ошибке «Доступ запрещен». Этого сообщения об ошибке можно избежать, убедившись, что пользователи имеют нужные разрешения DCOM.  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-server-2003-or-windows-xp"></a>Настройка прав для удаленных пользователей в Windows Server 2003 или Windows XP  
  
1.  Если пользователь не является членом группы локальных администраторов, добавьте его в группу «Пользователи DCOM». Это можно сделать в оснастке MMC "Управление компьютером" в меню **Администрирование** .  
  
2.  Откройте панель управления, дважды щелкните **Администрирование** и дважды щелкните **Службы компонентов** , чтобы запустить оснастку MMC "Службы компонентов".  
  
3.  Разверните узел **Службы компонентов** в левой части панели консоли. Разверните узел **Компьютеры** , разверните узел **Мой компьютер** и щелкните узел **Настройка DCOM** .  
  
4.  Выберите узел **Настройка DCOM** и в списке приложений, которые можно настроить, выберите SQL Server Integration Services 11.0.  
  
5.  Щелкните правой кнопкой мыши SQL Server Integration Services 11.0, а затем выберите пункт **Свойства**.  
  
6.  В диалоговом окне **Свойства SQL Server Integration Services 11.0** перейдите на вкладку **Безопасность** .  
  
7.  В разделе **Разрешения на запуск и активацию** выберите **Настройка** и щелкните **Изменить** , чтобы открыть диалоговое окно **Запуск разрешений** .  
  
8.  В диалоговом окне **Запуск разрешений** добавьте или удалите пользователей и присвойте соответствующие разрешения нужным пользователям и группам. Доступные разрешения: «Локальный запуск», «Удаленный запуск», «Локальная активация» и «Удаленная активация». Права запуска предоставляют или отказывают в разрешении запускать и останавливать службы, права активации предоставляют или отказывают в разрешении подключаться к службе.  
  
9. Нажмите кнопку «OК», чтобы закрыть диалоговое окно.  
  
10. В разделе **Разрешения доступа** повторите шаги 7 и 8, чтобы назначить соответствующие разрешения пользователям и группам.  
  
11. Закройте оснастку MMC.  
  
12. Перезапустите службу [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-2000-with-the-latest-service-packs"></a>Настройка прав для удаленных пользователей в Windows 2000 с последними пакетами обновления  
  
1.  Запустите программу **dcomcnfg.exe** из командной строки.  
  
2.  На странице **Приложения** диалогового окна **Свойства конфигурации DCOM** выберите приложение SQL Server Integration Services 11.0 и щелкните **Свойства**.  
  
3.  Перейдите на страницу **Безопасность** .  
  
4.  В двух разных диалоговых окнах настройте **Разрешения на доступ** и **Разрешения на запуск**. Нельзя различить удаленный и локальный доступ: права на доступ включают локальный и удаленный доступ, а права на запуск включают локальный и удаленный запуск.  
  
5.  Закройте диалоговые окна и программу **dcomcnfg.exe**.  
  
6.  Перезапустите службу [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="connecting-by-using-a-local-account"></a>Подключение с использованием локальной учетной записи

При работе с локальной учетной записью Windows на клиентском компьютере можно подключиться к службе [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на удаленном компьютере, только если локальная учетная запись имеет то же самое имя пользователя и пароль, а на удаленном компьютере имеются соответствующие права.  
  
### <a name="ssis-windows-service-doesnt-support-delegation"></a>Служба SSIS Windows не поддерживает делегирование

Служба SSIS не поддерживают делегирование учетных данных, иногда называемое двойным прыжком. В этом сценарии вы работаете на клиентском компьютере, служба SSIS установлена на втором компьютере, а SQL Server — на третьем. Хотя среда SSMS успешно отправляет учетные записи с клиентского компьютера на второй компьютер (где работает SSIS), служба SSIS не может передать эти учетные данные со второго компьютера на третий, на котором работает SQL Server.

## <a name="configure-the-firewall"></a>Настройка брандмауэра
  
 Система брандмауэра Windows предотвращает несанкционированный доступ к ресурсам компьютера через сетевое подключение. Чтобы получить доступ к службам [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] через этот брандмауэр, необходимо настроить брандмауэр для разрешения доступа.  
  
> [!IMPORTANT]  
>  Чтобы управлять пакетами, которые хранятся на удаленном сервере, не нужно соединятся с экземпляром службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на этом удаленном сервере. Вместо этого измените файл конфигурации для службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] таким образом, чтобы среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] отображала пакеты, хранимые на удаленном сервере.
  
 Служба [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] использует протокол DCOM.
  
 Существует множество систем брандмауэров. При запуске другого брандмауэра обратитесь к документации по нему.  
  
 Если брандмауэр поддерживает фильтрацию на уровне приложения, то можно использовать пользовательский интерфейс, предоставляемый Windows для указания исключений, которым разрешается доступ через брандмауэр, например программам и службам. Иначе необходимо установить настройки DCOM, ограничивающие количество портов TCP. Ссылка на веб-сайт Майкрософт в прошлом включала сведения о том, как указать TCP-порты для использования.  
  
 Служба Integration Services использует порт 135, который не может быть изменен. Для доступа к диспетчеру управления службами (SCM) необходимо открыть TCP-порт 135. SCM выполняет такие задачи, как запуск и остановка служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , а также передачу управляющих запросов выполняемой службе.  
  
 Сведения в следующем разделе относятся к брандмауэру Windows. Вы можете настроить систему брандмауэра Windows, введя команду в командной строке или задав свойства в диалоговом окне брандмауэра Windows.  
  
 Дополнительные сведения о настройках брандмауэра Windows по умолчанию и описание портов TCP, влияющих на компонент Database Engine, службы Analysis Services, службы Reporting Services и службы Integration Services, см. в разделе [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="configuring-a-windows-firewall"></a>Настройка брандмауэра Windows  
 Можно использовать следующие команды для открытия TCP-порта 135, добавления в список исключения MsDtsSrvr.exe и указания области доступа, предоставляемого брандмауэром.  
  
#### <a name="to-configure-a-windows-firewall-using-the-command-prompt-window"></a>Настройка брандмауэра Windows с помощью окна командной строки  
  
1.  Выполните следующую команду:

    ```dos
    netsh firewall add portopening protocol=TCP port=135 name="RPC (TCP/135)" mode=ENABLE scope=SUBNET
    ```
  
2.  Выполните следующую команду:

    ```dos
    netsh firewall add allowedprogram program="%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn\MsDtsSrvr.exe" name="SSIS Service" scope=SUBNET
    ```
  
    > [!NOTE]  
    >  Чтобы включить брандмауэр для всех компьютеров, в том числе в сети Интернет, замените предложение «scope=SUBNET» на «scope=ALL».  
  
 Следующая процедура описывает, как использовать пользовательский интерфейс Windows для открытия TCP-порта 135, добавления в список исключения MsDtsSrvr.exe и указания области доступа, предоставляемой брандмауэром.  
  
#### <a name="to-configure-a-firewall-using-the-windows-firewall-dialog-box"></a>Настройка брандмауэра Windows с помощью диалогового окна  
  
1.  На панели управления дважды щелкните элемент **Брандмауэр Windows**.  
  
2.  В диалоговом окне **Брандмауэр Windows** перейдите на вкладку **Исключения** , затем нажмите кнопку **Добавить программу**.  
  
3.  В диалоговом окне **Добавление программы** нажмите кнопку **Обзор**, найдите папку Program Files\Microsoft SQL Server\100\DTS\Binn, выберите файл MsDtsSrvr.exe и нажмите кнопку **Открыть**. Нажмите кнопку **ОК** , чтобы закрыть диалоговое окно **Добавить программу** .  
  
4.  На вкладке **Исключения** нажмите кнопку **Добавить порт**.  
  
5.  В диалоговом окне **Добавить порт** введите **RPC(TCP/135)** или другое описательное имя в поле **Имя**, введите **135** в поле **Номер порта** и выберите **TCP**.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] всегда использует порт 135. Другой порт указать нельзя.  
  
6.  В диалоговом окне **Добавить порт** можно нажать кнопку **Изменить область** , чтобы изменить область по умолчанию.  
  
7.  В диалоговом окне **Изменить область** выберите **Только локальная сеть (подсеть)** или введите пользовательский список и нажмите кнопку **ОК**.  
  
8.  Чтобы закрыть диалоговое окно **Добавить порт** , нажмите кнопку **ОК**.  
  
9. Чтобы закрыть диалоговое окно **Брандмауэр Windows** , нажмите кнопку **ОК**.  
  
    > [!NOTE]  
    >  Для настройки брандмауэра Windows в этой процедуре используется элемент **Брандмауэр Windows** на панели управления. Элемент **Брандмауэр Windows** настраивает брандмауэр только для текущего сетевого профиля. Брандмауэр Windows также можно настроить с помощью программы командной строки **netsh** или оснастки консоли управления [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MMC) "Брандмауэр Windows в режиме повышенной безопасности". Дополнительные сведения об этих средствах см. в разделе [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
