---
title: Службы SCM. Изменение пароля учетных записей | Документы Майкрософт
description: Узнайте, как изменить пароль учетных записей, используемых ядром СУБД и агентом SQL Server. Узнайте, когда важно изменить пароль.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- expired password [SQL Server], SQL Server Agent
- passwords [SQL Server], SQL Server Agent service
- passwords [SQL Server], changing
- expired password [SQL Server], Database Engine
- passwords [SQL Server], SQL Server service
- Database Engine [SQL Server], passwords
- changing passwords used by SQL Server
- modifying passwords
ms.assetid: 5b6dcc03-6cae-45d3-acef-6f85ca6d615f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6b639f6bc5a8a5bebdca351a5fce2bd713f0b30
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171026"
---
# <a name="scm-services---change-the-password-of-the-accounts-used"></a>Службы SCM. Изменение пароля учетных записей
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как менять пароль для учетных записей, используемых компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] и агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , с помощью диспетчера конфигурации SQL Server. Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускаются на компьютере как службы, с заданными при установке учетными данными. Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускается с учетной записью домена, а пароль для этой учетной записи изменен, то пароль, используемый [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , необходимо обновить. Если этого не сделать, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может потерять доступ к некоторым ресурсам домена и, в случае остановки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , служба не перезапустится до тех пор, пока не будет обновлен пароль.  
  
 Сведения об изменении паролей для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Срок действия пароля истек](../../relational-databases/security/choose-an-authentication-mode.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разработан для изменения параметров служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и обладает для этого всеми правами. Редактирование параметров службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в диспетчере управления службами Windows (**services.msc**) не всегда меняет все необходимые параметры, что может привести к неправильной работе службы. Однако в кластерной среде после изменения пароля на активном узле с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо изменить пароль и на пассивном узле с помощью диспетчера управления службами.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для изменения пароля службы необходимо быть администратором компьютера.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Использование диспетчера конфигурации SQL Server  
  
#### <a name="to-change-the-password-used-by-the-sql-server-database-engine-service"></a>Изменение пароля службы SQL Server (компонент Database Engine)  
  
1.  Нажмите кнопку **Пуск** , укажите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
    > [!NOTE]  
    >  Поскольку диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] является оснасткой консоли управления ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), а не изолированной программой, при работе в более новых версиях Windows диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не отображается как приложение.  
    >   
    >  -   **Windows 10**:  
    >          чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , введите на **начальной странице** SQLServerManager13.msc (для [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). Для предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] замените 13 на меньшее число. Если щелкнуть SQLServerManager13.msc, откроется диспетчер конфигурации. Чтобы закрепить диспетчер конфигурации на начальной странице или панели задач, щелкните правой кнопкой мыши SQLServerManager13.msc и выберите пункт **Открыть папку с файлом**. В проводнике щелкните правой кнопкой мыши SQLServerManager13.msc, а затем выберите команду **Закрепить на начальном экране** или **Закрепить на панели задач**.  
    > -   **Windows 8**:  
    >          Чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с помощью чудо-кнопки **Поиск** на вкладке **Приложения** введите **SQLServerManager\<version>.msc** (например, **SQLServerManager13.msc**) и нажмите клавишу **ВВОД**.  
  
2.  В диспетчере конфигурации SQL Server выберите пункт **Службы SQL Server**.  
  
3.  В области сведений щелкните правой кнопкой мыши **SQL Server (** \<instancename> **)** и выберите пункт **Свойства**.  
  
4.  В диалоговом окне **Свойства SQL Server (** \<instancename> **)** на вкладке "Вход" введите для учетной записи, указанной в поле **Имя учетной записи**, новый пароль в полях **Пароль** и **Подтверждение пароля** и нажмите кнопку **OK**.  
  
     Новый пароль вступает в силу немедленно, без перезагрузки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-change-the-password-used-by-the-sql-server-agent-service"></a>Изменение пароля с помощью службы агента SQL Server  
  
1.  Нажмите кнопку **Пуск** , укажите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
2.  В диспетчере конфигурации SQL Server выберите пункт **Службы SQL Server**.  
  
3.  В области сведений щелкните правой кнопкой мыши **SQL Server Agent (** \<instancename> **)** и выберите пункт **Свойства**.  
  
4.  В диалоговом окне **Свойства SQL Server Agent (** \<instancename> **)** на вкладке "Вход" введите для учетной записи, указанной в поле **Имя учетной записи**, новый пароль в полях **Пароль** и **Подтверждение пароля** и нажмите кнопку **OK**.  
  
     На отдельном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]пароль вступает в силу сразу же, без перезапуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В кластеризованном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может выключить ресурс [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , после чего потребуется перезапуск.  
  
## <a name="see-also"></a>См. также:  
 [Инструкции по управлению службами (диспетчер конфигурации SQL Server)](./scm-services-connect-to-another-computer.md)  
  
