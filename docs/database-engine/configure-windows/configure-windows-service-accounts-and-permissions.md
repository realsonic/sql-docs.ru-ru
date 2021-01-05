---
title: Настройка учетных записей службы Windows и разрешений | Документы Майкрософт
description: Ознакомьтесь с учетными записями служб, которые используются для запуска и выполнения служб в SQL Server. Узнайте, как их настроить и назначить соответствующие разрешения.
ms.custom: contperf-fy20q4
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: reference
helpviewer_keywords:
- startup service states [SQL Server]
- Setup [SQL Server], user accounts
- Windows permissions [SQL Server]
- modifying user accounts
- default accounts
- domains [SQL Server], user accounts
- startup accounts [SQL Server]
- system accounts [SQL Server]
- services [SQL Server], permissions
- ACL (access control list)
- local system accounts [SQL Server]
- instance-aware services [SQL Server]
- permissions [SQL Server], services
- SQL Server Agent service, user accounts
- Windows NT permissions [SQL Server]
- user accounts [SQL Server]
- identifying instance-unaware services [SQL Server]
- installing SQL Server, user accounts
- disabled startup state [SQL Server]
- user accounts [SQL Server], users
- Local Service account [SQL Server]
- SQL Server Installation Wizard
- instance-unaware services [SQL Server]
- services [SQL Server], configuring at installation
- Windows accounts [SQL Server]
- SQL Server services, user accounts
- user accounts [SQL Server], services
- MSSQLServer
- identifying instance-aware services [SQL Server]
- services [SQL Server], accounts
- access control lists
- optional accounts [SQL Server]
- service accounts [SQL Server]
- accounts [SQL Server], services
- built-in system accounts [SQL Server]
- automatic startup state
- domains [SQL Server]
- manual startup state [SQL Server]
- accounts [SQL Server], user
ms.assetid: 309b9dac-0b3a-4617-85ef-c4519ce9d014
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da2fc97ef8e5637dfff48d2070cb652b40ae4d28
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878930"
---
# <a name="configure-windows-service-accounts-and-permissions"></a>Настройка учетных записей службы Windows и разрешений

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Каждая служба в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] представляет собой процесс или набор процессов для управления проверкой подлинности при выполнении операций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в операционной системе Windows. В этом разделе описана конфигурация по умолчанию служб данного выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а также параметры конфигурации служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые можно настроить во время и после установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Этот раздел дает возможность опытным пользователям детальнее ознакомиться с учетными записями служб.

 Большинство служб и их свойств можно настроить с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ниже приведены расположения последних четырех версий этого диспетчера при установке Windows на диск C.

|Версия SQL Server|путь|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|

## <a name="services-installed-by-ssnoversion"></a><a name="Service_Details"></a> Службы, устанавливаемые с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

В зависимости от компонентов, которые выбраны для установки, программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливает следующие службы.

- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Database Services** — служба реляционного компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Путь к исполняемому файлу: \<MSSQLPATH>\MSSQL\Binn\sqlservr.exe.
- **Агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** — предназначен для выполнения заданий, наблюдения за [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], предупреждения о нештатных ситуациях. Кроме того, позволяет автоматизировать некоторые задачи по администрированию. Служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] присутствует, но отключена на экземплярах [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Путь к исполняемому файлу: \<MSSQLPATH>\MSSQL\Binn\sqlagent.exe.
- **[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]**  — предоставляет средства оперативной аналитической обработки (OLAP) и средства интеллектуального анализа данных для приложений бизнес-аналитики. Путь к исполняемому файлу: \<MSSQLPATH>\OLAP\Bin\msmdsrv.exe.
- **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**  — предназначены для выполнения, создания, планирования, доставки отчетов и управления ими. Путь к исполняемому файлу: \<MSSQLPATH>\Reporting Services\ReportServer\Bin\ReportingServicesService.exe.
- **[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]**  — обеспечивают поддержку управления хранением и выполнением пакетов служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Путь к исполняемому файлу: \<MSSQLPATH>\130\DTS\Binn\MsDtsSrvr.exe.

   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] может включать дополнительные службы для развертываний с горизонтальным увеличением масштаба. См. дополнительные сведения в руководствах по [ Настройка развертывания служб Integration Services (SSIS) с горизонтальным увеличением масштаба](../../integration-services/scale-out/walkthrough-set-up-integration-services-scale-out.md).

- **Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** — служба разрешения имен, которая предоставляет сведения о соединении с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] клиентским компьютерам. Путь к исполняемому файлу: c:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe
- **Полнотекстовый поиск** — быстро создает полнотекстовые индексы содержимого и свойства структурированных и полуструктурированных данных, чтобы обеспечить фильтрацию документа и разбиение по словам для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- **Модуль записи SQL** — служит для резервного копирования и восстановления приложений для работы в составе платформы служб теневого копирования томов (VSS).
- **Контроллер распределенного воспроизведения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  — обеспечивает согласование воспроизведения трассировки по нескольким клиентским компьютерам распределенного воспроизведения.
- **Клиент распределенного воспроизведения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  — один или несколько клиентских компьютеров распределенного воспроизведения, работающих вместе с контроллером распределенного воспроизведения для имитации параллельных рабочих нагрузок на экземпляре [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].
- **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**  — доверенная служба, в которой находятся внешние исполняемые файлы, предоставляемые корпорацией Майкрософт, такие как среда выполнения R или Python, установленная как часть служб R Services или служб машинного обучения. Вспомогательные процессы могут запускаться при работе панели запуска, однако они регулируются ресурсами в зависимости от конфигурации отдельного экземпляра. Служба панели запуска выполняется с использованием собственной учетной записи, и каждому вспомогательному процессу для конкретной зарегистрированной среды выполнения присваивается учетная запись пользователя панели запуска. Вспомогательные процессы создаются и удаляются по запросу во время выполнения.

  Панель запуска не может создавать используемые ею учетные записи, если вы установили SQL Server на компьютере, который используется в качестве контроллера домена. Таким образом, программа установки служб R Services (в базе данных) или служб машинного обучения (в базе данных) завершается сбоем на контроллере домена.

- **SQL Server PolyBase Engine** — позволяет применять распределенные запросы к внешним источникам данных.
- **Служба перемещения данных SQL Server PolyBase** — позволяет перемешать данные между SQL Server и внешними источниками данных, а также между узлами SQL в группах горизонтального масштабирования PolyBase.

## <a name="service-properties-and-configuration"></a><a name="Serv_Prop"></a> Свойства и настройка служб

В качестве стартовых учетных записей автоматического запуска, используемых для запуска и выполнения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , могут использоваться [учетные записи пользователей домена](#Domain_User), [учетные записи локальных пользователей](#Local_User), [управляемые учетные записи служб](#MSA), [виртуальные учетные записи](#VA_Desc)или [встроенные системные учетные записи](#Local_Service). Для запуска и выполнения каждая служба [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должна иметь стартовую учетную запись автоматического запуска, настраиваемую во время установки.

В этом разделе описываются учетные записи, которые могут быть настроены для запуска служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], значения, используемые при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию, понятие удостоверений безопасности служб, параметры запуска и настройка брандмауэра.

- [Учетные записи служб по умолчанию](#Default_Accts)
- [Автоматический запуск](#Auto_Start)
- [Настройка типа запуска службы](#Configure_services)
- [Порт брандмауэра](#Firewall)

### <a name="default-service-accounts"></a><a name="Default_Accts"></a> Учетные записи служб по умолчанию

В следующей таблице перечислены учетные записи служб по умолчанию, используемые программой установки при установке всех компонентов. Перечисленные учетные записи по умолчанию являются рекомендуемыми, если не указано иное.

**Изолированный сервер или контроллер домена**

|Компонент|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|Windows 7 и [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 и выше|
|---------------|------------------------------------|----------------------------------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)*|
|Агент[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)*|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)\* \*\*|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)\*|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)\*|
|Контроллер распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)\*|
|Клиент распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)\*|
|Средство запуска FD (полнотекстовый поиск)|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|[Виртуальная учетная запись](#VA_Desc)|
|Браузер[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|
|службы синхронизации контроля версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];|[ЛОКАЛЬНАЯ СИСТЕМА](#Local_System)|[ЛОКАЛЬНАЯ СИСТЕМА](#Local_System)|
|[!INCLUDE[rsql_extensions](../../includes/rsql-extensions-md.md)]|NTSERVICE\MSSQLLaunchpad|NTSERVICE\MSSQLLaunchpad|
|Ядро PolyBase |[СЕТЕВАЯ СЛУЖБА](#Network_Service) |[СЕТЕВАЯ СЛУЖБА](#Network_Service)|
|Служба перемещения данных PolyBase |[СЕТЕВАЯ СЛУЖБА](#Network_Service) |[СЕТЕВАЯ СЛУЖБА](#Network_Service)|

\* Если требуются ресурсы за пределами компьютера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[msCoName](../../includes/msconame-md.md)] рекомендует использовать управляемую учетную запись службы (MSA), которой предоставлены следующие необходимые разрешения.
\*\* При установке на контроллере домена виртуальная учетная запись не поддерживается как учетная запись службы.

**Экземпляр отказоустойчивого кластера SQL Server**

|Компонент|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2|
|---------------|------------------------------------|---------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Нет. Укажите учетную запись [пользователя домена](#Domain_User) .|Укажите учетную запись [пользователя домена](#Domain_User) .|
|Агент[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Нет. Укажите учетную запись [пользователя домена](#Domain_User) .|Укажите учетную запись [пользователя домена](#Domain_User) .|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|Нет. Укажите учетную запись [пользователя домена](#Domain_User) .|Укажите учетную запись [пользователя домена](#Domain_User) .|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[СЕТЕВАЯ СЛУЖБА](#Network_Service)|[Виртуальная учетная запись](#VA_Desc)|
|Средство запуска FD (полнотекстовый поиск)|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|[Виртуальная учетная запись](#VA_Desc)|
|Браузер[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|[ЛОКАЛЬНАЯ СЛУЖБА](#Local_Service)|
|службы синхронизации контроля версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];|[ЛОКАЛЬНАЯ СИСТЕМА](#Local_System)|[ЛОКАЛЬНАЯ СИСТЕМА](#Local_System)|

#### <a name="changing-account-properties"></a><a name="Changing_Accounts"></a> Изменение свойств учетной записи

> [!IMPORTANT]
>
> - Всегда используйте такие средства [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , как диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для изменения учетной записи, используемой службами [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] или агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , либо для изменения пароля учетной записи. В дополнение к изменению имени учетной записи диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет дополнительную настройку, например обновление локального хранилища безопасности Windows, которое защищает главный ключ службы для компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Другие средства, такие как диспетчер управления службами Windows, могут изменить имя учетной записи, но не изменяют все необходимые параметры.
> - Для экземпляров служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , развертываемых на ферме SharePoint, изменение учетных записей сервера для приложений [!INCLUDE[ssGeminiMTS](../../includes/ssgeminimts-md.md)] и [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)]следует всегда выполнять с помощью центра администрирования SharePoint. Связанные настройки и разрешения обновляются для обеспечения возможности использования новых учетных данных при работе с центром администрирования.
> - Чтобы изменить параметры служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , используйте средство настройки служб Reporting Services.

### <a name="managed-service-accounts-group-managed-service-accounts-and-virtual-accounts"></a><a name="New_Accounts"></a> Управляемые учетные записи служб, групповые управляемые учетные записи служб и виртуальные учетные записи

Управляемые учетные записи служб, групповые управляемые учетные записи служб и виртуальные учетные записи разработаны для обеспечения важных приложений, таких как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , с изоляцией собственных учетных записей, устраняя необходимость в ручном администрировании имени участника-службы (SPN) и учетных данных для этих учетных записей. Это намного упрощает долгосрочное управление пользователями учетных записей служб, паролями и именами SPN.

- <a name="MSA"></a> **Управляемые учетные записи служб**

  Управляемая учетная запись службы (MSA) — это тип учетной записи домена, создаваемый и управляемый контроллером домена. Она назначается отдельному компьютеру-участнику для использования при запуске службы. Управление паролем осуществляет автоматически контроллер домена. MSA нельзя использовать для входа на компьютер, но компьютер может использовать MSA для запуска службы Windows. MSA имеет возможность зарегистрировать имя участника-службы (SPN) в Active Directory при наличии разрешений на чтение и запись servicePrincipalName. MSA присваивается имя с суффиксом **$** , например **DOMAIN\ACCOUNTNAME$** . При указании MSA следует оставить поле пароля пустым. Поскольку учетная запись MSA назначается одному компьютеру, ее нельзя использовать на разных узлах кластера Windows.

  > [!NOTE]
  > Учетная запись MSA должна быть создана в Active Directory администратором домена до того, как ее сможет использовать программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- <a name="GMSA"></a> **Групповые управляемые учетные записи служб**

  Групповая управляемая учетная запись службы (gMSA) — это управляемая учетная запись службы для нескольких серверов. Windows управляет такой учетной записью для служб, работающих на группе серверов. Active Directory обновляет пароль групповой управляемой учетной записи службы автоматически, не перезапуская при этом службы. Службы SQL Server можно настроить на использование основного экземпляра групповой управляемой учетной записи службы. Начиная с версии SQL Server 2014, решение поддерживает групповые управляемые учетные записи службы для изолированных экземпляров, а с версии SQL Server 2016 — для экземпляров отказоустойчивого кластера и для групп доступности.

  Для работы с gMSA в SQL Server 2014 или более поздней версии требуется ОС Windows Server 2012 R2 или более поздней версии. Серверам с Windows Server 2012 R2 должно быть выделено [2998082 КБ](https://support.microsoft.com/kb/2998082) , чтобы службы могли выполнять вход после изменения пароля, не нарушая работу системы.

  Дополнительные сведения см. в разделе [Групповые управляемые учетные записи служб](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))

  > [!NOTE]
  > Учетная запись gMSA должна быть создана в Active Directory администратором домена до того, как ее сможет использовать программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- <a name="VA_Desc"></a>**Virtual Accounts**

  Виртуальные учетные записи (начиная с Windows Server 2008 R2 и Windows 7) — это *управляемые локальные учетные записи* , которые предоставляют следующие возможности для упрощения администрирования служб. Управление виртуальной учетной записью осуществляется автоматически, и она может получать доступ к сети в среде домена. Если установка [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется со значением по умолчанию для учетных записей служб, используется виртуальная учетная запись с именем, соответствующим имени экземпляра, в формате **NT SERVICE\\** _\<SERVICENAME>_ . Службы, запускаемые от имени виртуальных учетных записей, получают доступ к сетевым ресурсам с использованием учетных данных учетной записи компьютера в формате *<имя_домена>* __\\__ *<имя_компьютера>* __$__ . При указании виртуальной учетной записи для запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] оставьте поле пароля пустым. Если для виртуальной учетной записи не удалось зарегистрировать имя участника-службы (SPN), выполните регистрацию вручную. Дополнительные сведения о регистрации SPN вручную см. в статье [Регистрация имени участника-службы для соединений Kerberos](register-a-service-principal-name-for-kerberos-connections.md).

  > [!NOTE]
  > Виртуальные учетные записи не могут использоваться для экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , так как у виртуальной учетной записи будет отличаться идентификатор безопасности на каждом узле кластера.

  В следующей таблице перечислены примеры имен виртуальных учетных записей.

  |Служба|Имя виртуальной учетной записи|
  |-------------|--------------------------|
  |Экземпляр по умолчанию службы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]|**NT SERVICE\MSSQLSERVER**|
  |Именованный экземпляр службы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] с именем **PAYROLL**|**NT SERVICE\MSSQL$PAYROLL**|
  |Служба агента[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]по умолчанию|**NT SERVICE\SQLSERVERAGENT**|
  |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с именем **PAYROLL**|**NT SERVICE\SQLAGENT$PAYROLL**|

Дополнительные сведения об управляемых учетных записях служб и виртуальных учетных записях см. в разделе **Основные понятия управляемых учетных записей служб и виртуальных учетных записей**[Пошагового руководства по учетным записям служб](https://technet.microsoft.com/library/dd548356\(WS.10\).aspx) , а также в документе [Вопросы и ответы по управляемым учетным записям служб](https://technet.microsoft.com/library/ff641729\(WS.10\).aspx).

**Примечание по безопасности.** [!INCLUDE[ssNoteLowRights](../../includes/ssnotelowrights-md.md)] По возможности используйте [управляемую учетную запись службы](#MSA), [групповую управляемую учетную запись службы](#GMSA) или [виртуальную учетную запись](#VA_Desc). Если использование управляемых учетных записей служб, групповых управляемых учетных записей служб и виртуальных учетных записей невозможно, используйте специальную учетную запись пользователя с ограниченными правами доступа или учетную запись домена вместо общей учетной записи для служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте отдельные учетные записи для разных служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Не предоставляйте дополнительные разрешения учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или группам службы. Разрешения идентификатору безопасности службы будут предоставлены через членство в группе или напрямую самому идентификатору службы там, где поддерживается идентификатор службы.

### <a name="automatic-startup"></a><a name="Auto_Start"></a> Автоматический запуск

Кроме учетной записи каждая служба имеет три возможных типа запуска, которыми могут управлять пользователи.

- **Отключено** . Служба установлена, но в данный момент не запущена.
- **Вручную** . Служба установлена, но будет запущена тогда, когда потребуется другой службе или приложению.
- **Автоматически** . Служба автоматически запускается операционной системой.

Тип запуска выбирается во время установки. При установке именованного экземпляра службу браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следует настроить на автоматический запуск.

### <a name="configuring-services-during-unattended-installation"></a><a name="Configure_services"></a> Настройка служб во время автоматической установки

В таблице ниже приведены службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые могут быть настроены в ходе установки. Для автоматической установки можно задать параметры в файле конфигурации или командной строке.

|Имя службы SQL Server|Параметры для автоматической установки\*|
|-----------------------------|---------------------------------------------|
|MSSQLSERVER|SQLSVCACCOUNT, SQLSVCPASSWORD, SQLSVCSTARTUPTYPE|
|SQLServerAgent\*\*|AGTSVCACCOUNT, AGTSVCPASSWORD, AGTSVCSTARTUPTYPE|
|MSSQLServerOLAPService|ASSVCACCOUNT, ASSVCPASSWORD, ASSVCSTARTUPTYPE|
|ReportServer|RSSVCACCOUNT, RSSVCPASSWORD, RSSVCSTARTUPTYPE|
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|ISSVCACCOUNT, ISSVCPASSWORD, ISSVCSTARTUPTYPE|
|Контроллер распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|DRU_CTLR, CTLRSVCACCOUNT, CTLRSVCPASSWORD, CTLRSTARTUPTYPE, CTLRUSERS|
|Клиент распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|DRU_CLT, CLTSVCACCOUNT, CLTSVCPASSWORD, CLTSTARTUPTYPE, CLTCTLRNAME, CLTWORKINGDIR, CLTRESULTDIR|
|R Services или службы машинного обучения (Майкрософт)|EXTSVCACCOUNT, EXTSVCPASSWORD, ADVANCEDANALYTICS\*\*\*|
|Ядро PolyBase| PBENGSVCACCOUNT, PBENGSVCPASSWORD, PBENGSVCSTARTUPTYPE, PBDMSSVCACCOUNT,PBDMSSVCPASSWORD, PBDMSSVCSTARTUPTYPE, PBSCALEOUT, PBPORTRANGE

\* Дополнительные сведения и примеры синтаксиса для автоматической установки см. в руководстве по [установке SQL Server 2016 из командной строки](../install-windows/install-sql-server-from-the-command-prompt.md).

\*\*Служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отключена на экземплярах [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] и [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] с дополнительными службами.

\*\*\* Настройка учетной записи для панели запуска с помощью одних только параметров сейчас не поддерживается. Используйте диспетчер конфигурации SQL Server, чтобы изменить учетную запись и другие параметры службы.

### <a name="firewall-port"></a><a name="Firewall"></a> Порт брандмауэра

Обычно при начальной установке компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] к нему можно подключаться с помощью таких средств, как среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , установленных на том же компьютере, что и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не открывает порты брандмауэра Windows. Соединение с другими компьютерами может оказаться невозможным, пока компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] будет настроен для прослушивания TCP-порта, закрытого для соединений в брандмауэре Windows. Дополнительные сведения см. в статье [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).

## <a name="service-permissions"></a><a name="Serv_Perm"></a> Разрешения службы

В этом разделе описаны разрешения, которые настраивает программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для удостоверений безопасности служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- [Настройка служб и управление доступом](#Serv_SID)
- [Права доступа и права Windows](#Windows)
- [Разрешения файловой системы, предоставляемые удостоверениям безопасности служб SQL Server или локальным группам Windows для SQL Server](#Reviewing_ACLs)
- [Разрешения файловой системы, предоставляемые другим учетным записям или группам Windows](#File_System_Other)
- [Разрешения файловой системы, связанные с нестандартными дисковыми расположениями](#Unusual_Locations)
- [Обзор дополнительных вопросов](#Review_additional_considerations)
- [Разрешения для реестра](#Registry)
- [WMI](#WMI)
- [Именованные каналы](#Pipes)

### <a name="service-configuration-and-access-control"></a><a name="Serv_SID"></a> Настройка служб и управление доступом

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] позволяет обеспечить изоляцию и всестороннюю защиту идентификатора безопасности каждой службы. Идентификатор безопасности службы создается на основе имени службы и является уникальным для этой службы. Например, именем идентификатора безопасности службы для именованного экземпляра службы [!INCLUDE[ssDE](../../includes/ssde-md.md)] может быть **NT Service\MSSQL$** _\<InstanceName>_ . Изоляция служб обеспечивает доступ к конкретным объектам без необходимости использования учетной записи с высоким уровнем привилегий или ослабления защиты этих объектов. Используя запись управления доступом, содержащую удостоверение безопасности службы, служба [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может ограничить доступ к своим ресурсам.

> [!NOTE]
> В Windows 7 и [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 (и более поздних версиях) удостоверением безопасности службы может быть виртуальная учетная запись, используемая этой службой.

Для большинства компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настраивает список управления доступом для учетной записи службы непосредственно, поэтому изменение учетной записи службы можно выполнить без необходимости повторения обработки списка управления доступом к ресурсу.

При установке служб [!INCLUDE[ssAS](../../includes/ssas-md.md)]создается удостоверение безопасности для службы [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Создается локальная группа Windows с именем в формате **SQLServerMSASUser$** _имя_компьютера_ **$** _имя_экземпляра_. Удостоверению безопасности службы **NT SERVICE\MSSQLServerOLAPService** предоставляется членство в локальной группе Windows, а локальной группе Windows — соответствующие разрешения в списке управления доступом. В случае изменения учетной записи, используемой для запуска службы [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен изменить некоторые разрешения Windows (например, право на вход в качестве службы), но разрешения, назначенные локальной группе Windows, будут все равно доступны без обновления, так как удостоверение безопасности службы не было изменено. Этот метод позволяет переименовывать службу [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] во время обновлений.

Во время установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает локальные группы Windows для службы [!INCLUDE[ssAS](../../includes/ssas-md.md)] и службы браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Для этих служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настраивает список управления доступом к локальным группам Windows.

В зависимости от конфигурации службы учетная запись службы или ее идентификатор безопасности добавляется как элемент группы служб во время установки или обновления.

### <a name="windows-privileges-and-rights"></a><a name="Windows"></a> Права доступа и права Windows

Учетной записи, назначенной для запуска службы, необходимы **разрешения на запуск, остановку и приостановку** службы. Они автоматически назначаются программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Сначала установите средства администрирования удаленного сервера (RSAT). См. раздел [Средства администрирования удаленного сервера для Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).

В следующей таблице перечислены разрешения, которые запрашивает программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для удостоверений безопасности служб или локальных групп Windows, используемых компонентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|Служба[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Разрешения, предоставляемые программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|
|---------------------------------------|------------------------------------------------------------|
|**[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]:**<br /><br /> (Все права предоставляются удостоверению безопасности службы. Экземпляр по умолчанию: **NT SERVICE\MSSQLSERVER**. Именованный экземпляр: **NT Service\MSSQLServer$** _ИмяЭкземпляра_.)|**Вход в систему в качестве службы** (SeServiceLogonRight)<br /><br /> **Замена токена уровня процесса** (SeAssignPrimaryTokenPrivilege)<br /><br /> **Обход проходной проверки** (SeChangeNotifyPrivilege)<br /><br /> **Назначение квот памяти процессам** (SeIncreaseQuotaPrivilege)<br /><br /> Разрешение на запуск модуля записи SQL Writer<br /><br /> Разрешение на чтение службы журнала событий<br /><br /> Разрешение на чтение службы удаленного вызова процедур (RPC)|

| **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Агент:** \*<br /><br /> (Все права предоставляются удостоверению безопасности службы. Экземпляр по умолчанию: **NT Service\SQLSERVERAGENT**. Именованный экземпляр: **NT Service\SQLAGENT$** _ИмяЭкземпляра_.)|**Вход в систему в качестве службы** (SeServiceLogonRight)<br /><br /> **Замена токена уровня процесса** (SeAssignPrimaryTokenPrivilege)<br /><br /> **Обход проходной проверки** (SeChangeNotifyPrivilege)<br /><br /> **Назначение квот памяти процессам** (SeIncreaseQuotaPrivilege)| | **[!INCLUDE[ssAS](../../includes/ssas-md.md)]:**<br /><br /> (Все права предоставляются локальной группе Windows. Экземпляр по умолчанию: **SQLServerMSASUser$** _имя_компьютера_ **$MSSQLSERVER**. Именованный экземпляр: **SQLServerMSASUser$** _имя_компьютера_ **$** _имя_экземпляра_. Экземпляр [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]: **SQLServerMSASUser$** _ИмяКомпьютера_ **$** _PowerPivot_.)|**Вход в систему в качестве службы** (SeServiceLogonRight)<br /><br /> Только для табличного режима:<br /><br /> **Увеличение рабочего набора процесса** (SeIncreaseWorkingSetPrivilege)<br /><br /> **Назначение квот памяти процессам** (SeIncreaseQuotaPrivilege)<br /><br /> **Блокировка страниц в памяти** (SeLockMemoryPrivilege) — это право доступа требуется только в случае полного отключения функции разбиения по страницам.<br /><br /> Только для установок отказоустойчивого кластера:<br /><br /> **Увеличение приоритета планирования** (SeIncreaseBasePriorityPrivilege)| | **[!INCLUDE[ssRS](../../includes/ssrs.md)]:**<br /><br /> (Все права предоставляются удостоверению безопасности службы. Экземпляр по умолчанию: **NT SERVICE\ReportServer**. Именованный экземпляр: **NT SERVICE\\ReportServer$** _ИмяЭкземпляра_.)|**Вход в систему в качестве службы** (SeServiceLogonRight)| | **[!INCLUDE[ssIS](../../includes/ssis-md.md)]:**<br /><br /> (Все права предоставляются удостоверению безопасности службы. Экземпляр по умолчанию и именованный экземпляр: **NT SERVICE\MsDtsServer130**. В службах [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] нет отдельного процесса для именованного экземпляра.)|**Вход в систему в качестве службы** (SeServiceLogonRight)<br /><br /> Разрешение на запись в журнал событий приложений<br /><br /> **Обход проходной проверки** (SeChangeNotifyPrivilege)<br /><br /> **Олицетворение клиента после проверки подлинности** (SeImpersonatePrivilege)| |**Полнотекстовый поиск:**<br /><br /> (Все права предоставляются удостоверению безопасности службы. Экземпляр по умолчанию: **NT Service\MSSQLFDLauncher**. Именованный экземпляр: **NT Service\ MSSQLFDLauncher$** _Имя экземпляра_.)|**Вход в систему в качестве службы** (SeServiceLogonRight)<br /><br /> **Назначение квот памяти процессам** (SeIncreaseQuotaPrivilege)<br /><br /> **Обход проходной проверки** (SeChangeNotifyPrivilege)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Браузер:**<br /><br /> (Все права предоставляются локальной группе Windows. Именованный экземпляр или экземпляр по умолчанию: **SQLServer2005SQLBrowserUser** _$имя_компьютера_. В браузере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нет отдельного процесса для именованного экземпляра.)|**Вход в систему в качестве службы** (SeServiceLogonRight)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Модуль записи VSS:**<br /><br /> (Все права предоставляются удостоверению безопасности службы. Именованный экземпляр или экземпляр по умолчанию: **NT Service\SQLWriter**. Модуль записи VSS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не имеет отдельного процесса для именованного экземпляра.)|Служба SQLWriter запускается под учетной записью LOCAL SYSTEM, которая имеет все необходимые разрешения. При установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешения для этой службы не проверяются и не предоставляются.| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Контроллер распределенного воспроизведения:** |**Вход в систему в качестве службы** (SeServiceLogonRight)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Клиент распределенного воспроизведения:** |**Вход в систему в качестве службы** (SeServiceLogonRight)| |**Ядро PolyBase и DMS**| **Вход в систему в качестве службы** (SeServiceLogonRight)| |**Панель запуска:** |**Вход в систему в качестве службы** (SeServiceLogonRight) <br /><br /> **Замена токена уровня процесса** (SeAssignPrimaryTokenPrivilege)<br /><br />**Обход проходной проверки** (SeChangeNotifyPrivilege)<br /><br />**Назначение квот памяти процессам** (SeIncreaseQuotaPrivilege)| |**R Services или службы машинного обучения (Майкрософт):** **SQLRUserGroup** (SQL 2016 и 2017) |По умолчанию не имеет разрешения **Локальный вход в систему** | |**Службы машинного обучения** **все пакеты приложений [AppContainer]** (SQL 2019) |**Разрешения READ и EXECUTE** для каталогов Binn, R_Services и PYTHON_Services SQL Server |

 \* Служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отключена на экземплярах [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].

### <a name="file-system-permissions-granted-to-sql-server-per-service-sids-or-local-windows-groups"></a><a name="Reviewing_ACLs"></a> Разрешения файловой системы, предоставляемые для SQL Server согласно SID-идентификаторам служб или локальным группам Windows

Учетные записи служб[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должны иметь доступ к ресурсам. Списки управления доступом задаются для каждого удостоверения безопасности службы или локальной группы Windows.

> [!IMPORTANT]
> В конфигурации с отказоустойчивым кластером ресурсы на общих дисках должны быть включены в список управления доступом для локальной учетной записи.

В следующей таблице показаны списки управления доступом, настраиваемые программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|Учетная запись службы для|Файлы и папки|Доступ|
|-------------------------|-----------------------|------------|
|MSSQLServer|Instid\MSSQL\backup|Полный доступ|
||Instid\MSSQL\binn|Чтение и выполнение|
||Instid\MSSQL\data|Полный доступ|
||Instid\MSSQL\FTData|Полный доступ|
||Instid\MSSQL\Install|Чтение и выполнение|
||Instid\MSSQL\Log|Полный доступ|
||Instid\MSSQL\Repldata|Полный доступ|
||130\shared|Чтение и выполнение|
||Instid\MSSQL\Template Data (только[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] )|Чтение|
|SQLServerAgent\*|Instid\MSSQL\binn|Полный доступ|
||Instid\MSSQL\Log|Чтение, запись, удаление и выполнение|
||130\com|Чтение и выполнение|
||130\shared|Чтение и выполнение|
||130\shared\Errordumps|Чтение и запись|
||ServerName\EventLog|Полный доступ|
|FTS|Instid\MSSQL\FTData|Полный доступ|
||Instid\MSSQL\FTRef|Чтение и выполнение|
||130\shared|Чтение и выполнение|
||130\shared\Errordumps|Чтение и запись|
||Instid\MSSQL\Install|Чтение и выполнение|
||Instid\MSSQL\jobs|Чтение и запись|
|MSSQLServerOLAPService|130\shared\ASConfig|Полный доступ|
||Instid\OLAP|Чтение и выполнение|
||Instid\Olap\Data|Полный доступ|
||Instid\Olap\Log|Чтение и запись|
||Instid\OLAP\Backup|Чтение и запись|
||Instid\OLAP\Temp|Чтение и запись|
||130\shared\Errordumps|Чтение и запись|
|ReportServer|Instid\Reporting Services\Log Files|Чтение, запись и удаление|
||Instid\Reporting Services\ReportServer|Чтение и выполнение|
||Instid\Reporting Services\ReportServer\global.asax|Полный доступ|
||Instid\Reporting Services\ReportServer\rsreportserver.config|Чтение|
||Instid\Reporting Services\RSTempfiles|Чтение, запись, выполнение и удаление|
||Instid\Reporting Services\RSWebApp|Чтение и выполнение|
||130\shared|Чтение и выполнение|
||130\shared\Errordumps|Чтение и запись|
|MSDTSServer100|130\dts\binn\MsDtsSrvr.ini.xml|Чтение|
||130\dts\binn|Чтение и выполнение|
||130\shared|Чтение и выполнение|
||130\shared\Errordumps|Чтение и запись|
|Браузер[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|130\shared\ASConfig|Чтение|
||130\shared|Чтение и выполнение|
||130\shared\Errordumps|Чтение и запись|
|SQLWriter|н/д (запускается с учетной записью локальной системы)||
|Пользователь|Instid\MSSQL\binn|Чтение и выполнение|
||Instid\Reporting Services\ReportServer|Чтение и выполнение, список содержимого папки|
||Instid\Reporting Services\ReportServer\global.asax|Чтение|
||Instid\Reporting Services\RSWebApp|Чтение и выполнение, список содержимого папки|
||130\dts|Чтение и выполнение|
||130\tools|Чтение и выполнение|
||100\tools|Чтение и выполнение|
||90\tools|Чтение и выполнение|
||80\tools|Чтение и выполнение|
||130\sdk|Чтение|
||Microsoft SQL Server\130\Setup Bootstrap|Чтение и выполнение|
|Контроллер распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|\<ToolsDir>\DReplayController\Log\ (пустой каталог)|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayController\DReplayController.exe|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayController\resources\|Чтение, выполнение, просмотр содержимого папки|
||\<ToolsDir>\DReplayController\\{все DLL-библиотеки}|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayController\DReplayController.config|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayController\IRTemplate.tdf|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayController\IRDefinition.xml|Чтение и выполнение, список содержимого папки|
|Клиент распределенного воспроизведения[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|\<ToolsDir>\DReplayClient\Log\|Чтение, выполнение, просмотр содержимого папки|
||\<ToolsDir>\DReplayClient\DReplayClient.exe|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayClient\resources\|Чтение, выполнение, просмотр содержимого папки|
||\<ToolsDir>\DReplayClient\ (все DLL-библиотеки)|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayClient\DReplayClient.config|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayClient\IRTemplate.tdf|Чтение и выполнение, список содержимого папки|
||\<ToolsDir>\DReplayClient\IRDefinition.xml|Чтение и выполнение, список содержимого папки|
|Панель запуска|%binn|Чтение и выполнение|
||ExtensiblilityData|Полный доступ|
||Log\ExtensibilityLog|Полный доступ|

\* Служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отключена на экземплярах [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] и [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] с дополнительными службами.

Когда файлы базы данных хранятся в определяемом пользователем расположении, идентификатору безопасности службы необходимо предоставить доступ к этому расположению. Дополнительные сведения о предоставлении разрешений файловой системы идентификаторам безопасности служб см. в статье [Настройка разрешений файловой системы для доступа к компоненту ядра СУБД](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md).

### <a name="file-system-permissions-granted-to-other-windows-user-accounts-or-groups"></a><a name="File_System_Other"></a> Разрешения файловой системы, предоставляемые другим учетным записям или группам Windows

Некоторые управляющие разрешения на доступ могут быть предоставлены для встроенных и других учетных записей служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В следующей таблице перечислены дополнительные списки управления доступом, настраиваемые программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|Запрашивающий компонент|Учетная запись|Ресурс|Разрешения|
|--------------------------|-------------|--------------|-----------------|
|MSSQLServer|Пользователи журнала производительности|Instid\MSSQL\binn|Просмотр содержимого папки|
||пользователи системного монитора.|Instid\MSSQL\binn|Просмотр содержимого папки|
||Пользователи журнала производительности, пользователи системного монитора|\WINNT\system32\sqlctr130.dll|Чтение и выполнение|
||Только администратор|\\\\.\root\Microsoft\SqlServer\ServerEvents\\<имя_экземпляра_SQL>\*|Полный доступ|
||Администраторы, система|\tools\binn\schemas\sqlserver\2004\07\showplan|Полный доступ|
||Пользователи|\tools\binn\schemas\sqlserver\2004\07\showplan|Чтение и выполнение|
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|Учетная запись службы Windows для сервера отчетов|*\<install>* \Reporting Services\LogFiles|DELETE<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||Учетная запись службы Windows для сервера отчетов|*\<install>* \Reporting Services\ReportServer|Чтение|
||Учетная запись службы Windows для сервера отчетов|*\<install>* \Reporting Services\ReportServer\global.asax|Полное|
||Учетная запись службы Windows для сервера отчетов|*\<install>* \Reporting Services\RSWebApp|Чтение и выполнение|
||Все|*\<install>* \Reporting Services\ReportServer\global.asax|READ_CONTROL<br /><br /> FILE_READ_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_READ_ATTRIBUTES|
||Учетная запись службы Windows для сервера отчетов|*\<install>* \Reporting Services\ReportServer\rsreportserver.config|DELETE<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||Все|Разделы реестра сервера отчетов (куст Instid)|Запрос значения<br /><br /> Перечисление подразделов<br /><br /> Уведомление<br /><br /> Управление чтением|
||Пользователь служб терминала|Разделы реестра сервера отчетов (куст Instid)|Запрос значения<br /><br /> Установка значения<br /><br /> Создание подраздела<br /><br /> Перечисление подразделов<br /><br /> Уведомление<br /><br /> DELETE<br /><br /> Управление чтением|
||Опытные пользователи|Разделы реестра сервера отчетов (куст Instid)|Запрос значения<br /><br /> Установка значения<br /><br /> Создание подраздела<br /><br /> Перечисление подразделов<br /><br /> Уведомление<br /><br /> DELETE<br /><br /> Управление чтением|

\* Это пространство имен поставщика WMI.

### <a name="file-system-permissions-related-to-unusual-disk-locations"></a><a name="Unusual_Locations"></a> Разрешения файловой системы, связанные с нестандартными дисковыми расположениями

Диском по умолчанию для установки является **systemdrive**, обычно это диск C. Этот раздел содержит дополнительные рекомендации, относящиеся к установке базы данных tempdb или пользовательских баз данных в необычных расположениях.

**Диск не по умолчанию**

При установке на локальный диск, который не является диском по умолчанию, удостоверение безопасности службы должно иметь доступ к расположению файлов. Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставит необходимый доступ.

**Общая сетевая папка**

При установке баз данных в общую сетевую папку учетная запись службы должна иметь доступ к расположению файлов пользовательских баз данных и базы данных tempdb. Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может предоставить доступ к общей сетевой папке. Пользователь должен предоставлять учетной записи службы доступ к расположению базы данных tempdb до выполнения установки. Пользователь должен предоставить доступ к расположению пользовательской базы данных перед созданием базы данных.

> [!NOTE]
> Виртуальные учетные записи не могут проходить проверку подлинности в удаленном расположении. Все виртуальные учетные записи используют разрешение локальной учетной записи. Укажите учетную запись компьютера в формате _<имя_домена>_ **\\** _<имя_компьютера>_ **$** .

### <a name="reviewing-additional-considerations"></a><a name="Review_additional_considerations"></a> Обзор дополнительных вопросов

В следующей таблице перечислены разрешения, которые необходимы службам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для поддержки дополнительных функций.

|Служба или приложение|Функциональность|Требуемое разрешение|
|--------------------------|-------------------|-------------------------|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|Запись в почтовый слот с помощью xp_sendmail.|Разрешения на запись по сети.|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|Запуск xp_cmdshell пользователем, не являющимся администратором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Работа в качестве части операционной системы, а также замена токена уровня процесса.|
|Агент[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|Использование функции автоматического перезапуска.|Должен быть членом локальной группы «Администраторы».|
|Помощник по настройке[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Позволяет настраивать базы данных для оптимальной производительности запросов.|При первом запуске приложение должно быть инициализировано пользователем с учетными данными системного администратора. После этого пользователи dbo могут при использовании помощника по настройке компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] настраивать только те таблицы, владельцами которых они являются. Дополнительные сведения см. в статье "Инициализация помощника по настройке [!INCLUDE[ssDE](../../includes/ssde-md.md)] при первом использовании" в электронной документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|

> [!IMPORTANT]
> Перед обновлением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], включите агент SQL Server и проверьте необходимые настройки конфигурации по умолчанию: является ли учетная запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] членом предопределенной роли сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin**.

### <a name="registry-permissions"></a><a name="Registry"></a> Разрешения для реестра

В разделе **HKLM\Software\Microsoft\Microsoft SQL Server\\** _<ИД_экземпляра>_ создается куст реестра для компонентов, привязанных к экземпляру. Например.

- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSASSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL.130**

В реестре также хранится сопоставление идентификаторов экземпляров с именами экземпляров. Сопоставление идентификатора экземпляра с именем экземпляра осуществляется следующим образом:

- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\SQL] "InstanceName"="MSSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\OLAP] "InstanceName"="MSASSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\RS] "InstanceName"="MSRSSQL13"**

### <a name="wmi"></a><a name="WMI"></a> WMI

Инструментарий управления Windows (WMI) должен иметь возможность подключиться к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Для этого компонент **предоставляет удостоверение безопасности службы поставщика WMI (** NT SERVICE\winmgmt [!INCLUDE[ssDE](../../includes/ssde-md.md)]).

Поставщику SQL WMI необходимы следующие минимальные разрешения:

- Членство в предопределенных ролях **db_ddladmin** или **db_owner** базы данных msdb.
- Разрешение **CREATE DDL EVENT NOTIFICATION** на сервере.
- Разрешение **CREATE TRACE EVENT NOTIFICATION** компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].
- Разрешение уровня сервера **VIEW ANY DATABASE** .

  Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает пространство имен SQL WMI и предоставляет разрешение на чтение удостоверению безопасности службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

### <a name="named-pipes"></a><a name="Pipes"></a> Именованные каналы

Во всех видах установки программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляет доступ к компоненту [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] через протокол общей памяти, который является локальным именованным каналом.

## <a name="provisioning"></a><a name="Provisioning"></a> Подготовка

В этом разделе описывается, как учетные записи подготавливаются к работе в разных компонентах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [Подготовка ядра СУБД к работе](#DE_Prov)

  - [Участники Windows](#Win_Principals)
  - [Учетная запись sa](#sa)
  - [Вход на SQL Server и права доступа для удостоверений безопасности служб](#Logins)
  - [Вход и права доступа агента SQL Server](#Agent)
  - [Экземпляр и права доступа HADRON и отказоустойчивого кластера SQL Server](#Hadron)
  - [Модуль записи SQL и права доступа](#Writer)
  - [SQL WMI и права доступа](#SQLWMI)
- [Подготовка служб SSAS](#SSAS)
- [Подготовка служб SSRS](#SSRS)

### <a name="database-engine-provisioning"></a><a name="DE_Prov"></a> Подготовка ядра СУБД к работе

Следующие учетные записи добавляются в качестве имен входа в компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].

#### <a name="windows-principals"></a><a name="Win_Principals"></a> Участники Windows

Во время установки программе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] требуется наличие как минимум одной учетной записи пользователя, являющейся членом предопределенной роли сервера **sysadmin** .

#### <a name="sa-account"></a><a name="sa"></a> Учетная запись sa

Учетная запись **sa** всегда присутствует в качестве имени входа в компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] и является членом предопределенной роли сервера **sysadmin** . Если компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] установлен с использованием только проверки подлинности Windows (то есть проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не включена), имя входа **sa** все равно будет присутствовать, но будет отключено, а пароль будет сложным и случайным. Дополнительные сведения о включении учетной записи **sa** см. в статье [Изменение режима проверки подлинности сервера](../../database-engine/configure-windows/change-server-authentication-mode.md).

#### <a name="sql-server-per-service-sid-login-and-privileges"></a><a name="Logins"></a> Вход на SQL Server и права доступа для удостоверений безопасности служб

Удостоверение безопасности службы (порой также называемое субъектом безопасности службы (SID)) для службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляется как имя входа [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Имя входа удостоверения безопасности службы является членом предопределенной роли сервера **sysadmin** . Сведения об удостоверениях безопасности служб см. в [Использование идентификаторов безопасности для предоставления разрешений службам в SQL Server](../../relational-databases/security/using-service-sids-to-grant-permissions-to-services-in-sql-server.md).

#### <a name="sql-server-agent-login-and-privileges"></a><a name="Agent"></a> Вход и права доступа агента SQL Server

Удостоверение безопасности службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляется в качестве имени входа в компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Имя входа удостоверения безопасности службы является членом предопределенной роли сервера **sysadmin** .

#### <a name="sshadrc-and-sql-failover-cluster-instance-and-privileges"></a><a name="Hadron"></a> [!INCLUDE[ssHADRc](../../includes/sshadrc-md.md)] и отказоустойчивый кластер SQL — привилегии

При установке компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в качестве [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] или отказоустойчивого кластера SQL (SQL FCI) учетная запись **LOCAL SYSTEM** предоставляется компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Имени входа **LOCAL SYSTEM** предоставляется разрешение **ALTER ANY AVAILABILITY GROUP** (для [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]) и разрешение **VIEW SERVER STATE** (для SQL FCI).

#### <a name="sql-writer-and-privileges"></a><a name="Writer"></a> Модуль записи SQL и права доступа

Удостоверение безопасности службы модуля записи VSS в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляется в качестве имени входа в экземпляр [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Имя входа удостоверения безопасности службы является членом предопределенной роли сервера **sysadmin** .

#### <a name="sql-wmi-and-privileges"></a><a name="SQLWMI"></a> SQL WMI и права доступа

Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подготавливает учетную запись **NT SERVICE\Winmgmt** как имя входа [!INCLUDE[ssDE](../../includes/ssde-md.md)] и добавляет ее к фиксированной роли сервера **sysadmin** .

#### <a name="ssrs-provisioning"></a>Подготовка служб SSRS

Учетная запись, указанная во время установки, предоставляется в качестве члена роли базы данных **RSExecRole** . Дополнительные сведения см. в разделе [Настройка учетной записи службы сервера отчетов (диспетчер конфигурации служб SSRS)](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).

### <a name="ssas-provisioning"></a><a name="SSAS"></a> Подготовка служб SSAS

Требования к учетной записи службы[!INCLUDE[ssAS](../../includes/ssas-md.md)] различаются в зависимости от способа развертывания сервера. При установке [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]программе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] требуется, чтобы служба [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] запускалась под учетной записью домена. Учетные записи домена необходимы для поддержки механизма управляемых учетных записей, встроенного в SharePoint. По этой причине программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не предоставляет учетную запись службы по умолчанию, например виртуальную учетную запись, для установки [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Дополнительные сведения о подготовке [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint см. в статье [Настройка учетных записей служб Power Pivot](/analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts).

Для всех других случаев автономной установки служб [!INCLUDE[ssAS](../../includes/ssas-md.md)] можно предоставить службу для запуска под учетной записью домена, встроенной системной учетной записью, управляемой или виртуальной учетной записью. Дополнительные сведения о подготовке учетных записей см. в статье [Настройка учетных записей службы (службы Analysis Services)](/analysis-services/instances/configure-service-accounts-analysis-services).

Для кластерной установки необходимо указать учетную запись домена или встроенную системную учетную запись. Ни управляемые, ни виртуальные учетные записи не поддерживаются для отказоустойчивых кластеров служб [!INCLUDE[ssAS](../../includes/ssas-md.md)] .

Для установки служб [!INCLUDE[ssAS](../../includes/ssas-md.md)] необходимо указать системного администратора экземпляра [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Права администратора предоставляются роли **Сервер** служб Analysis Services.

### <a name="ssrs-provisioning"></a><a name="SSRS"></a> Подготовка служб SSRS

Учетная запись, указанная во время установки, предоставляется компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] в качестве члена роли базы данных **RSExecRole** . Дополнительные сведения см. в разделе [Настройка учетной записи службы сервера отчетов (диспетчер конфигурации служб SSRS)](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).

## <a name="upgrading-from-previous-versions"></a><a name="Upgrade"></a> Обновление с предыдущих версий

В этом разделе описаны изменения, внесенные во время обновления предыдущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] требуется [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 с пакетом обновления 1 (SP1), Windows Server 2012, Windows 8.0 и Windows Server 2012 R2 или Windows 8.1. Для предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , работающих в более низкой версии операционной системы, необходимо обновить операционную систему, прежде чем обновлять службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Во время обновления [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настроит [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следующим образом.

  - Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] запускается в контексте безопасности удостоверения безопасности службы. Удостоверению безопасности службы предоставляется доступ к папкам с файлами экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например, DATA), а также к разделам реестра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  - Удостоверение безопасности службы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] предоставляется компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] в качестве члена предопределенной роли сервера **sysadmin** .
  - Удостоверение безопасности службы добавляется к локальным группам Windows для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не является экземпляром отказоустойчивого кластера.
  - Ресурсы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] остаются предоставленными локальным группам Windows для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  - Локальная группа Windows для служб переименовывается с **SQLServer2005MSSQLUser$** _<имя_компьютера>_ **$** _<имя_экземпляра>_ на **SQLServerMSSQLUser$** _<имя_компьютера>_ **$** _<имя_экземпляра>_ . Расположения файлов перенесенных баз данных будут иметь записи управления доступом (ACE) для локальных групп Windows. Расположения файлов для новых баз данных будут иметь записи ACE для удостоверения безопасности службы.

- Во время обновления с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сохранит записи ACE для удостоверения безопасности службы [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].
- Для экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запись ACE учетной записи домена, настроенной для службы, будет сохранена.

## <a name="appendix"></a><a name="Appendix"></a> Приложение

В данном разделе содержатся дополнительные сведения о службах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [Описание учетных записей служб](#Serv_Accts)
- [Идентификация зависимых и не зависимых от экземпляра служб](#Identify_instance_aware_and_unaware)
- [Локализованные имена служб](#Localized_service_names)

### <a name="description-of-service-accounts"></a><a name="Serv_Accts"></a> Описание учетных записей служб

Учетной записью службы является учетная запись, используемая для запуска службы Windows, например [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. При запуске SQL Server не требуется добавлять учетную запись службы в качестве имени входа для SQL Server в дополнение к идентификатору безопасности службы, который имеется всегда и является членом предопределенной роли сервера **sysadmin**.

#### <a name="accounts-available-with-any-operating-system"></a><a name="Any_OS"></a> Учетные записи, доступные в любой операционной системе

В дополнение к новым [управляемым учетным записям служб](#MSA), [групповым управляемым учетным записям служб](#GMSA) и [виртуальным учетным записям](#VA_Desc), описанным выше, могут использоваться следующие учетные записи.

<a name="Domain_User"></a> **Учетная запись пользователя домена**

Если служба должна взаимодействовать с сетевыми службами, получать доступ к ресурсам домена (например, к общей папке) либо использовать соединения связанного сервера с другими компьютерами, работающими под управлением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используйте учетную запись домена с минимальными правами доступа. Многие операции межсерверного взаимодействия могут быть выполнены только от учетной записи пользователя домена. Эта учетная запись должна быть создана предварительно администрацией домена в используемой среде.

> [!NOTE]
> При настройке SQL Server на использование учетной записи домена можно изолировать права доступа для службы, при этом придется управлять паролями вручную или создать пользовательское решение для управления паролями. Эта стратегия помогает повысить безопасность многих серверных приложений, но повышает сложность и требует дополнительного администрирования. В этих случаях развертывания администраторы служб тратят значительное количество времени на выполнение задач обслуживания, таких как управление паролями служб и именами участников-служб (SPN), необходимыми для проверки подлинности Kerberos. Кроме того, задачи обслуживания могут нарушить работу службы.

<a name="Local_User"></a> **Локальные учетные записи пользователей**

Если компьютер не является частью домена, рекомендуется использовать учетную запись локального пользователя, не имеющую разрешений администратора Windows.

<a name="Local_Service"></a> **Учетная запись локальной службы**

Учетная запись локальной службы является встроенной и имеет тот же уровень доступа к ресурсам и объектам, что и члены группы «Пользователи». Такой ограниченный доступ помогает защитить систему в случае нарушения безопасности отдельных служб или процессов. Службы, запускаемые с учетной записью локальной службы, получают доступ к сетевым ресурсам в качестве нулевого сеанса без использования учетных данных. 
> [!NOTE]
> Учетная запись локальной службы не поддерживается службами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Локальная служба не поддерживается в качестве учетной записи для запуска этих служб, так как это общая служба, а любые другие службы, работающие под учетной записью локальной службы, получили бы доступ с правами администратора к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Фактическое имя этой учетной записи — **NT AUTHORITY\LOCAL SERVICE**.

<a name="Network_Service"></a> **Учетная запись сетевой службы**

Встроенная учетная запись сетевой службы имеет более высокий уровень доступа к ресурсам и объектам, чем члены группы «Пользователи». Службы, запущенные из-под учетной записи сетевой службы, производят доступ к сетевым ресурсам с помощью данных учетной записи компьютера в формате _<имя_домена>_ **\\** _<имя_компьютера>_ **$** . Фактическое имя этой учетной записи — **NT AUTHORITY\NETWORK SERVICE**.

<a name="Local_System"></a> **Учетная запись локальной системы**

Локальная система — это встроенная учетная запись, обладающая очень высокими правами доступа. Она имеет обширные права и выступает в качестве компьютера сети. Фактическое имя этой учетной записи — **NT AUTHORITY\SYSTEM**.

### <a name="identifying-instance-aware-and-instance-unaware-services"></a><a name="Identify_instance_aware_and_unaware"></a> Идентификация зависимых и не зависимых от экземпляра служб

Служба, связанная с экземпляром, относится к конкретному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и ей выделяется отдельный куст реестра. Программа установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет для каждого компонента или службы установить несколько копий служб, привязанных к разным экземплярам. Службы, не связанные с экземплярами, являются общими для всех установленных экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Они устанавливаются всего один раз, их параллельная установка не допускается.

С экземпляром связаны следующие службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- Агент[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

   Необходимо помнить, что служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отключена в экземплярах [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] и [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services.

- [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]\*
- [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
- Полнотекстовый поиск

  В число служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , не связываемых с экземпляром, входят следующие.

- [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]
- Браузер[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- Модуль записи SQL

 \* Службы Analysis Services в режиме интеграции с SharePoint работают в виде единичного именованного экземпляра [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Имя экземпляра фиксировано. Другое имя указать нельзя. На каждом физическом сервере можно установить только один экземпляр служб Analysis Services, работающих как[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].

### <a name="localized-service-names"></a><a name="Localized_service_names"></a> Локализованные имена служб

В следующей таблице показаны имена служб, отображаемые в локализованных версиях Windows.

|Язык|Имя для Local Service|Имя сетевой службы|Имя Local System|Имя группы администраторов|
|--------------|----------------------------|------------------------------|---------------------------|--------------------------|
|Английский<br /><br /> Китайский (упрощенный)<br /><br /> Китайский (традиционный)<br /><br /> Корейский<br /><br /> Японский|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|Немецкий|NT-AUTORITДT\LOKALER DIENST|NT-AUTORITДT\NETZWERKDIENST|NT-AUTORITДT\SYSTEM|VORDEFINIERT\Administratoren|
|Французский|AUTORITE NT\SERVICE LOCAL|AUTORITE NT\SERVICE RЙSEAU|AUTORITE NT\SYSTEM|BUILTIN\Administrators|
|Итальянский|NT AUTHORITY\SERVIZIO LOCALE|NT AUTHORITY\SERVIZIO DI RETE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|Испанский|NT AUTHORITY\SERVICIO LOC|NT AUTHORITY\SERVICIO DE RED|NT AUTHORITY\SYSTEM|BUILTIN\Administradores|
|Русский|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\СИСТЕМА|BUILTIN\Администраторы|

## <a name="related-content"></a>См. также

[Вопросы безопасности при установке SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)

[Расположение файлов для экземпляра по умолчанию и именованных экземпляров SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)

[Установка служб Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)
