---
title: Поддерживаемые обновления версий и выпусков (SQL Server 2017)
description: Поддерживаемые обновления версий и выпусков SQL Server 2017.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 660f0da5aa9fc59c6c742373660e95d3ad0bcf95
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172266"
---
# <a name="supported-version--edition-upgrades-sql-server-2017"></a>Поддерживаемые обновления версий и выпусков (SQL Server 2017)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Можно обновить версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]. В этой статье приведены поддерживаемые пути обновления данных версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и поддерживаемые обновления выпусков [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
  
## <a name="pre-upgrade-checklist"></a>Контрольный список действий перед обновлением  
  
-   Перед началом обновления выпуска [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] до новой версии проверьте, поддерживаются ли используемые функции в версии, до которой производится обновление.  
  
-   Перед обновлением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]включите проверку подлинности Windows для агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и проверьте необходимые настройки конфигурации по умолчанию: является ли учетная запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] членом группы системных администраторов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Для обновления до версии [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]необходимо наличие поддерживаемой операционной системы. Дополнительные сведения см. в разделе [Требования к оборудованию и программному обеспечению для установки SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Обновление будет заблокировано, если компьютер ожидает перезагрузки.  
  
-   Обновление будет заблокировано, если не запущена служба установщика Windows.  
  
## <a name="unsupported-scenarios"></a>Неподдерживаемые сценарии  
  
-   Многоверсионные экземпляры в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] не поддерживаются. Компоненты [!INCLUDE[ssDE](../../includes/ssde-md.md)] должны иметь один и тот же номер версии для одного экземпляра [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] доступен только для 64-разрядных платформ. Межплатформенное обновление не поддерживается. Невозможно обновить 32-разрядный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до стандартного 64-разрядного при помощи программы установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Но можно создать резервную копию или отсоединить базы данных от 32-разрядного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем восстановить их или присоединить к новому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64-разрядная версия), если базы данных не опубликованы в репликации. Необходимо повторно создать имена входа и другие объекты пользователя в системных базах данных master, msdb и model.  
  
-   Добавить новые компоненты в процессе обновления существующего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]нельзя. После обновления экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] добавить новые компоненты можно при помощи программы установки [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]. Дополнительные сведения см. в разделе [Добавление компонентов в экземпляр SQL Server (программа установки)](./add-features-to-an-instance-of-sql-server-setup.md).  
 
-   Кластеры отработки отказа не поддерживаются в режиме WOW.  
    
## <a name="upgrades-from-earlier-versions-to-sssqlv14-md"></a>Обновление ранних версий до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]  
 
[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] поддерживает обновление со следующих версий SQL Server:
 
- SQL Server 2008 с пакетом обновления 4 (SP4) или более поздней версии;
- SQL Server 2008 R2 с пакетом обновления 3 (SP3) или более поздней версии;
- SQL Server 2012 с пакетом обновления 2 (SP2) или более поздней версии;
- SQL Server 2014 или более поздней версии. 
- SQL Server 2016 или более поздней версии.
 

  
> [!NOTE]  
>  Инструкции по обновлению баз данных в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] см. в разделе [Поддержка версии 2005](#SupportFor2005).  
  
 В таблице, представленной ниже, перечислены поддерживаемые сценарии обновления предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
  
|Исходная версия|Поддерживаемые варианты обновления|  
|:------|:------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Enterprise с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Developer с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Standard с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Small Business с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Web с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Workgroup с пакетом обновления 4 (SP4)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Express с пакетом обновления 4 (SP4) |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Datacenter с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Enterprise с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Developer с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Small Business с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Standard с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Web с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Workgroup с пакетом обновления 3 (SP3)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Express с пакетом обновления 3 (SP3) |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise с пакетом обновления 2 (SP2)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Developer с пакетом обновления 2 (SP2)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Standard с пакетом обновления 2 (SP2)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Web с пакетом обновления 1 (SP1)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Express с пакетом обновления 2 (SP2) |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Business Intelligence с пакетом обновления 2 (SP2)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Evaluation с пакетом обновления 2 (SP2)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Ознакомительная версия <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Ознакомительная версия|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Ознакомительная версия <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Business Intelligence|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Ознакомительная версия|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Ознакомительная версия <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] версия-кандидат * |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise |  
|[!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)] Developer |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise | 

 \* Предоставляемая корпорацией Майкрософт поддержка обновления с версий-кандидатов предназначена для клиентов, участвовавших в программе адаптации технологий (TAP). 

   
###  <a name="sssqlv14-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Поддержка [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
 В этом разделе описывается поддержка [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] для [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. В [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]вы сможете выполнить следующие действия.  
  
-   Присоединить базу данных служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (файлы MDF и LDF) к экземпляру [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] компонента Database Engine.  
  
-   Восстановить базу данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] в экземпляре [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] компонента Database Engine.  
  
-   Создать резервную копию куба [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] и восстановить его в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
  
Когда база данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] обновляется до версии [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], уровень совместимости базы данных будет изменен с 90 на 100. (В [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] допустимыми значениями уровня совместимости базы данных являются 100, 110, 120, 130 и 140.) В разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) представлены сведения о том, как изменение уровня совместимости может повлиять на приложения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Все сценарии, не указанные в списке выше, не поддерживаются, включая в том числе следующие:  
  
- Установка [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] на одном компьютере (параллельно).  
  
- Использование экземпляра [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] в качестве члена топологии репликации, включающего экземпляр [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] .  
  
- Зеркальное отображение базы данных между экземплярами [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
- Резервное копирование журнала транзакций с доставкой журналов между экземплярами [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
- Настройка связанных серверов между экземплярами [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
- Управление экземпляром [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] из среды [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Management Studio.  
  
- Подключение куба [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Management Studio.  
  
- Подключение к [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Management Studio.  
  
- Управление службами [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Management Studio.  
  
- Поддержка компонентов служб Integration Services [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] сторонних разработчиков, например выполнение и обновление.  
  
## <a name="sssqlv14-md-edition-upgrade"></a>[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Обновление выпуска  
В следующей таблице перечислены поддерживаемые сценарии обновлений в [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
  
Пошаговые инструкции по обновлению выпуска см. в разделе [Обновление до другого выпуска SQL Server (программа установки)](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Исходная версия|Обновленная версия|  
|------------------|----------------|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL и Core) **|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise |  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation Enterprise **|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> Обновление с переходом от Evaluation (бесплатного выпуска) на любой из платных выпусков поддерживается для изолированных, но не поддерживается для кластеризованных установок. Это ограничение не применяется к изолированным экземплярам, установленным в отказоустойчивом кластере Windows, входящем в группу доступности.|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard **|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL или Core)|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer **|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express *|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
  
 Кроме того, можно выполнить обновление выпуска между выпусками [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL) и [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Core).  
  
|Обновление выпуска c|Обновление выпуска до|  
|--------------------------|------------------------|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL) **|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Core)|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Core)|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise (лицензия Server+CAL)|  
  
 \* Также относится к [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , экспресс-выпуск с инструментами и [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , экспресс-выпуск с дополнительными службами.  
  
 ** При смене выпуска для отказоустойчивого кластера [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] действуют определенные ограничения. Следующие сценарии не поддерживаются для кластеров отработки отказа [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] .  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer, Standard или Evaluation.  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard или Evaluation.  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation.  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation до [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard.  
  
## <a name="see-also"></a>См. также:  

 [Возможности, поддерживаемые различными выпусками SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)
 
 [Требования к оборудованию и программному обеспечению для установки SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 
 [Обновление SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
