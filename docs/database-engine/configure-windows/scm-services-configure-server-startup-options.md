---
title: Настройка параметров запуска сервера (диспетчер конфигурации SQL Server) | Документы Майкрософт
description: Узнайте, как задать параметры, которые будут использоваться при запуске ядра СУБД SQL Server. Просмотрите ограничения на внесение изменений в параметры запуска.
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], startup options
- SQL Server, startup options
- SQL Server, startup parameters
- single-user mode [SQL Server], starting in
- startup options [SQL Server]
- startup parameters [SQL Server]
- SQL Server services, setting startup options
- SQL Server services, setting startup parameters
ms.assetid: 7a94643c-6460-4baf-bb31-0cb99eaf970d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 270083370770cf81e8c714c1230ec2d2e5f561ef
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170966"
---
# <a name="scm-services---configure-server-startup-options"></a>Службы SCM. Настройка параметров запуска сервера
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как настроить параметры запуска, которые будут использоваться при каждом запуске компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], с помощью Configuration Manager [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Список параметров запуска см. в разделе [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
### <a name="limitations-and-restrictions"></a>Ограничения  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] записывает параметры запуска в реестр. Они вступают в силу при следующем запуске компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 В кластере изменения должны вноситься на активном сервере, пока [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] находится в режиме «в сети», и вступают в силу при перезапуске компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] . При следующей отработке отказа произойдет обновление реестра другого узла.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Настраивать параметры запуска сервера могут только пользователи, уполномоченные изменять соответствующие записи в реестре. Это следующие пользователи.  
  
-   Члены локальной группы администраторов.  
  
-   Учетная запись домена, используемая [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] настроен для работы под определенной учетной записью домена.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Использование диспетчера конфигурации SQL Server  
  
#### <a name="to-configure-startup-options"></a>Настройка параметров запуска  
  
1.  Нажмите кнопку **Пуск** , укажите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
    > [!NOTE]  
    >  Поскольку диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] является оснасткой консоли управления ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), а не изолированной программой, при работе в более новых версиях Windows диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не отображается как приложение.  
    >   
    >  -   **Windows 10**:  
    >          чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , введите на **начальной странице** SQLServerManager13.msc (для [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). Для предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] замените 13 на меньшее число. Если щелкнуть SQLServerManager13.msc, откроется диспетчер конфигурации. Чтобы закрепить диспетчер конфигурации на начальной странице или панели задач, щелкните правой кнопкой мыши SQLServerManager13.msc и выберите пункт **Открыть папку с файлом**. В проводнике щелкните правой кнопкой мыши SQLServerManager13.msc, а затем выберите команду **Закрепить на начальном экране** или **Закрепить на панели задач**.  
    >  -   **Windows 8**:  
    >          Чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с помощью чудо-кнопки **Поиск** на вкладке **Приложения** введите **SQLServerManager\<version>.msc** (например, **SQLServerManager13.msc**) и нажмите клавишу **ВВОД**.  
  
2.  В диспетчере конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выберите пункт **Службы SQL Server**.  
  
3.  На правой панели щелкните правой кнопкой мыши элемент **SQL Server (** _<имя_экземпляра>_ **)** и выберите пункт **Свойства**.  
  
4.  На вкладке **Параметры запуска** в поле **Укажите параметр запуска** введите параметр и нажмите кнопку **Добавить**.  
  
     Например, для запуска в однопользовательском режиме введите **-m** в поле **Укажите параметр запуска** и нажмите кнопку **Добавить**. (Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перезапускается в однопользовательском режиме, остановите агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В противном случае агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может установить соединение первым, что не позволит подключиться второму пользователю.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Перезапустите компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
    > [!WARNING]  
    >  После завершения работы в однопользовательском режиме выберите в окне "Параметры запуска" в поле **Существующие параметры** параметр **-m** и нажмите кнопку **Удалить**. Чтобы вернуться к обычному многопользовательскому режиму [!INCLUDE[ssDE](../../includes/ssde-md.md)] , перезапустите компонент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Запуск SQL Server в однопользовательском режиме](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)   
 [Подключение к SQL Server в случае, если доступ системных администраторов заблокирован](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)   
 [Запуск, остановка или приостановка службы агента SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
 [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md) 
  
