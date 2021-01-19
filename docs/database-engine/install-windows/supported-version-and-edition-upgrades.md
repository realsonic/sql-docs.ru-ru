---
title: Поддерживаемые обновления версий и выпусков (SQL Server 2016)
description: Поддерживаемые обновления версий и выпусков SQL Server 2016.
ms.custom: ''
ms.date: 06/27/2016
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
ms.openlocfilehash: 8bf008b28a130c705eabaebf986b28d7da8cd669
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172236"
---
# <a name="supported-version--edition-upgrades-sql-server-2016"></a>Поддерживаемые обновления версий и выпусков (SQL Server 2016)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Можно обновить версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]и [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. В этой статье приведены поддерживаемые пути обновления данных версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и поддерживаемые обновления выпусков [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
## <a name="pre-upgrade-checklist"></a>Контрольный список действий перед обновлением  
  
-   Перед началом обновления выпуска [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] до новой версии проверьте, поддерживаются ли используемые функции в версии, до которой производится обновление.  
  
-   Перед обновлением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]включите проверку подлинности Windows для агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и проверьте необходимые настройки конфигурации по умолчанию: является ли учетная запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] членом группы системных администраторов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Для обновления до версии [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]необходимо наличие поддерживаемой операционной системы. Дополнительные сведения см. в разделе [Требования к оборудованию и программному обеспечению для установки SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Обновление будет заблокировано, если компьютер ожидает перезагрузки.  
  
-   Обновление будет заблокировано, если не запущена служба установщика Windows.  
  
## <a name="unsupported-scenarios"></a>Неподдерживаемые сценарии  
  
-   Многоверсионные экземпляры в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] не поддерживаются. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)], а также службы [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]и [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] должны иметь один и тот же номер версии для одного экземпляра [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] доступен только для 64-разрядных платформ. Межплатформенное обновление не поддерживается. Невозможно обновить 32-разрядный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до стандартного 64-разрядного при помощи программы установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Но можно создать резервную копию или отсоединить базы данных от 32-разрядного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем восстановить их или присоединить к новому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64-разрядная версия), если базы данных не опубликованы в репликации. Необходимо повторно создать имена входа и другие объекты пользователя в системных базах данных master, msdb и model.  
  
-   Добавить новые компоненты в процессе обновления существующего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]нельзя. После обновления экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] добавить новые компоненты можно при помощи программы установки [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]. Дополнительные сведения см. в разделе [Добавление компонентов в экземпляр SQL Server 2016 (программа установки)](./add-features-to-an-instance-of-sql-server-setup.md).  
 
-   Кластеры отработки отказа не поддерживаются в режиме WOW.  
  
-   Обновление с выпуска Evaluation предыдущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживается.

-   При обновлении с релиз-кандидата 1 или более ранних версий SQL Server 2016 до релиз-кандидата 3 или более поздних версий необходимо удалить PolyBase до обновления и повторно установить после обновления.
  
## <a name="upgrades-from-earlier-versions-to-sssql15-md"></a>Обновление ранних версий до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]  
 
SQL Server 2016 поддерживает обновление со следующих версий SQL Server:
 
- [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] с пакетом обновления 4 (SP4) или более поздней версии
- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] с пакетом обновления 3 (SP3) или более поздней версии
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]с пакетом обновления 2 (SP2) или более поздней версии
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] или более поздней версии 
 
> [!NOTE]  
> Инструкции по обновлению баз данных в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] см. в разделе [Поддержка версии 2005](#SupportFor2005).  
  
В таблице, представленной ниже, перечислены поддерживаемые сценарии обновления предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
|Исходная версия|Поддерживаемые варианты обновления|  
|------------------|----------------------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Enterprise с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Developer с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Standard с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Small Business с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Web с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Workgroup с пакетом обновления 4 (SP4)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Express с пакетом обновления 4 (SP4) |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Datacenter с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Enterprise с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Developer с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Small Business с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Standard с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Web с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Workgroup с пакетом обновления 3 (SP3)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Express с пакетом обновления 3 (SP3) |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise с пакетом обновления 2 (SP2)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Developer с пакетом обновления 2 (SP2)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Standard с пакетом обновления 2 (SP2)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Web с пакетом обновления 1 (SP1)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Express с пакетом обновления 2 (SP2) |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Business Intelligence с пакетом обновления 2 (SP2)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Evaluation с пакетом обновления 2 (SP2)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Ознакомительная версия <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Ознакомительная версия|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Ознакомительная версия <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] версия-кандидат * |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Developer |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise | 

 \* Предоставляемая корпорацией Майкрософт поддержка обновления с версий-кандидатов предназначена для клиентов, участвовавших в программе адаптации технологий (TAP). 
   
###  <a name="sssql15-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Поддержка [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
В этом разделе описывается поддержка [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] для [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. В [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]вы сможете выполнить следующие действия.  
  
-   Присоединить базу данных служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (файлы MDF и LDF) к экземпляру [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] компонента Database Engine.  
  
-   Восстановить базу данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] в экземпляре [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] компонента Database Engine.  
  
-   Создать резервную копию куба [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] и восстановить его в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
> [!NOTE]  
> Когда база данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] обновляется до версии [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], уровень совместимости базы данных будет изменен с 90 на 100. В [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] допустимыми значениями уровня совместимости базы данных являются 100, 110, 120 и 130. В разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) представлены сведения о том, как изменение уровня совместимости может повлиять на приложения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Все сценарии, не указанные в списке выше, не поддерживаются, включая в том числе следующие:  
  
-   Установка [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] на одном компьютере (параллельно).  
  
-   Использование экземпляра [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] в качестве члена топологии репликации, включающего экземпляр [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] .  
  
-   Зеркальное отображение базы данных между экземплярами [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Резервное копирование журнала транзакций с доставкой журналов между экземплярами [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Настройка связанных серверов между экземплярами [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] и [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Управление экземпляром [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] из среды [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Подключение куба [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Подключение к [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Управление службами [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Поддержка компонентов служб Integration Services [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] сторонних разработчиков, например выполнение и обновление.  
  
## <a name="sssql15-md-edition-upgrade"></a>[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Обновление выпуска  
В следующей таблице перечислены поддерживаемые сценарии обновлений в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
Пошаговые инструкции по обновлению выпуска см. в разделе [Обновление до другого выпуска SQL Server 2016 (программа установки)](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Исходная версия|Обновленная версия|  
|------------------|----------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL и Core) **|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation Enterprise **|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> Обновление с переходом от Evaluation (бесплатного выпуска) на любой из платных выпусков поддерживается для изолированных, но не поддерживается для кластеризованных установок. Это ограничение не применяется к изолированным экземплярам, установленным в отказоустойчивом кластере Windows, входящем в группу доступности.|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard **|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL или Core)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer **|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express *|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL или Core) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
  
Кроме того, можно выполнить обновление выпуска между выпусками [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL) и [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Core).  
  
|Обновление выпуска c|Обновление выпуска до|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL) **|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Core)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Core)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (лицензия Server+CAL)|  
  
 \* Также относится к [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] , экспресс-выпуск с инструментами и [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] , экспресс-выпуск с дополнительными службами.  
  
 ** При смене выпуска для отказоустойчивого кластера [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] действуют определенные ограничения. Следующие сценарии не поддерживаются для кластеров отработки отказа [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] .  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer, Standard или Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard или Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation до [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard.  
  
## <a name="see-also"></a>См. также:  

[Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)     
[Требования к оборудованию и программному обеспечению для установки SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)     
[Обновление до SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)    
  
