---
title: Диспетчер конфигурации SQL Server | Документация Майкрософт
description: Использование клиента диспетчера конфигурации SQL Server
ms.custom: ''
ms.date: 12/31/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: vanto
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server], managing
- network protocols [SQL Server], managing
- Client Network Utility
- accounts [SQL Server]
- SQL Server Configuration Manager
- Server Network Utility
- accounts [SQL Server], services
- services [SQL Server], managing
- tools [SQL Server], SQL Server Configuration Manager
- configuration manager [SQL Server]
ms.assetid: e6beaea4-164c-4078-95ae-b9e28b0aefe8
author: rothja
ms.author: jroth
ms.openlocfilehash: fc5fb052e2a02630a78f899e5dcdad615bb00266
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172746"
---
# <a name="sql-server-configuration-manager"></a>Диспетчер конфигурации SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] — это средство, предназначенное для управления службами, связанными с [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], для настройки сетевых протоколов, которые используются [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], а также для управления конфигурацией подключений с клиентских компьютеров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] устанавливается вместе с SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Диспетчер конфигурации [!INCLUDE[msCoName](../includes/msconame-md.md)] представляет собой оснастку консоли управления (ММС), которую можно открыть из меню "Пуск" или добавить в любой экран консоли управления [!INCLUDE[msCoName](../includes/msconame-md.md)] . [!INCLUDE[msCoName](../includes/msconame-md.md)] Консоль управления (**mmc.exe**) использует файл **SQLServerManager\<version>.msc** (например, **SQLServerManager13.msc** для [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]), чтобы открыть диспетчер конфигурации. Для управления конкретной версией [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] потребуется соответствующая версия диспетчера конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Ниже приведены расположения последних пяти версий этого диспетчера при установке Windows на диск C.  
  
|Версия|путь|  
|-|-|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019|C:\Windows\SysWOW64\SQLServerManager15.msc| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|  
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|  
|[!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|
  
> [!NOTE]
>  Поскольку диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] является оснасткой консоли управления ( [!INCLUDE[msCoName](../includes/msconame-md.md)] ), а не изолированной программой, при работе в более новых версиях Windows диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не отображается как приложение.  
> 
>  -   **Windows 10**:  
>          чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , введите на **начальной странице** SQLServerManager13.msc (для [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]). Для других версий [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] замените 13 соответствующим числом. Если щелкнуть SQLServerManager13.msc, откроется диспетчер конфигурации. Чтобы закрепить диспетчер конфигурации на начальной странице или панели задач, щелкните правой кнопкой мыши SQLServerManager13.msc и выберите пункт **Открыть папку с файлом**. В проводнике щелкните правой кнопкой мыши SQLServerManager13.msc, а затем выберите команду **Закрепить на начальном экране** или **Закрепить на панели задач**.  
> -   **Windows 8**:  
>          Чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], с помощью чудо-кнопки **Поиск** на вкладке **Приложения** введите **SQLServerManager\<version>.msc** (например, **SQLServerManager13.msc**) и нажмите клавишу **ВВОД**.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Диспетчер конфигурации и среда SQL Server Management Studio используют инструментарий WMI для просмотра и изменения некоторых параметров сервера. Инструментарий WMI обеспечивает единообразный интерфейс с API-вызовами, которые управляют операциями с реестром, запрашивающими средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , а также улучшенный контроль и управление выбранными SQL-службами оснастки «Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ». Сведения о настройке разрешений, связанных с WMI, см. в разделе [Настройка инструментария WMI для отображения состояния сервера в инструментальных средствах SQL Server](../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md).  
  
 Сведения о запуске, остановке, приостановке, возобновлении и настройке служб на другом компьютере с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] см. в разделе [Подключение к другому компьютеру (диспетчер конфигурации SQL Server)](../database-engine/configure-windows/scm-services-connect-to-another-computer.md).  
  
## <a name="managing-services"></a>Управление службами  
 Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] используется для запуска, приостановки, возобновления и остановки служб, а также для просмотра или изменения свойств служб.  
  
 Используйте диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] для запуска компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] с помощью параметров запуска.  Дополнительные сведения см. в разделе [Настройка параметров запуска сервера (диспетчер конфигурации SQL Server)](../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  
  
## <a name="changing-the-accounts-used-by-the-services"></a>Изменение учетных записей, используемых службами  
 С помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] можно управлять службами [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Всегда используйте такие средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , как диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , для изменения учетной записи, используемой службами [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или агентом [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , либо для изменения пароля учетной записи. Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не только изменяет имя учетной записи, но и выполняет дополнительную настройку, например установку разрешений в реестре Windows, чтобы новая учетная запись могла считывать настройки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Другие средства, такие как диспетчер управления службами Windows, могут изменить имя учетной записи, но не изменяют соответствующие параметры. Если служба не может получить доступ к разделу реестра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , она может запуститься некорректно.  
  
 Дополнительное преимущество диспетчера конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , SMO и инструментария WMI заключается в том, что новые параметры вступают в силу немедленно без перезапуска службы.  
  
## <a name="manage-server--client-network-protocols"></a>Управление серверными и клиентскими сетевыми протоколами  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] позволяет настраивать серверные и клиентские сетевые протоколы, а также параметры подключения. После включения правильных протоколов обычно не нужно менять сетевые подключения сервера. В то же время диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] можно использовать для перенастройки соединений, чтобы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] прослушивал определенный сетевой протокол, порт или канал. Дополнительные сведения о включении протоколов см. в разделе [Включение или отключение сетевого протокола сервера](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md). Сведения о разрешении доступа к протоколам в брандмауэре см в разделе [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Диспетчер конфигурации позволяет управлять серверными и клиентскими сетевыми протоколами, в том числе применять шифрование протокола, просматривать свойства псевдонима, а также включать и отключать протокол.  
  
 Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] позволяет создавать или удалять псевдонимы, изменять порядок использования протоколов и просматривать свойства псевдонима сервера, включая:  
  
-   псевдонимы сервера— псевдонимы сервера, используемый для компьютера, с которым соединяется клиент;  
  
-   протокол — сетевой протокол, используемый для данной конфигурации;  
  
-   параметры соединения — параметры, связанные с адресом соединения для конфигурации сетевого протокола.  
  
 Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] позволяет также просматривать сведения об экземплярах отказоустойчивого кластера, хотя для некоторых действий, например запуска и остановки служб, должен использоваться администратор кластера.  
  
### <a name="available-network-protocols"></a>Доступные сетевые протоколы  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] поддерживает протоколы общей памяти, TCP/IP и именованных каналов. Сведения о выборе сетевых протоколов см. в разделе [Configure Client Protocols](../database-engine/configure-windows/configure-client-protocols.md). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не поддерживает сетевые протоколы VIA, Banyan VINES Sequenced Packet Protocol (SPP), Multiprotocol, AppleTalk и NWLink IPX/SPX. Клиенты, подключенные ранее с помощью этих протоколов, для соединения с [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]должны выбрать другой протокол. Диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] нельзя использовать для настройки прокси-сервера WinSock. Чтобы настроить прокси-сервер WinSock, см. документацию по ISA Server.  
  
## <a name="related-tasks"></a>Связанные задачи  
 [Инструкции по управлению службами (диспетчер конфигурации SQL Server)](../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
  
 [Запуск, остановка, приостановка, возобновление и перезапуск компонента Database Engine, агента SQL и службы браузера SQL Server](../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
 [Запуск, остановка или приостановка службы агента SQL Server](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
 [Настройка автоматического запуска экземпляра SQL Server (диспетчер конфигурации SQL Server)](../database-engine/configure-windows/scm-services-set-an-instance-to-start-automatically.md)  
  
 [Отключение автоматического запуска экземпляра SQL Server (диспетчер конфигурации SQL Server)](../database-engine/configure-windows/scm-services-prevent-automatic-startup-of-an-instance.md)  
  
