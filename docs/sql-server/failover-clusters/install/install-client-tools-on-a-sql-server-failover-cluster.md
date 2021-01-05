---
title: Установка клиентских средств. Отказоустойчивый кластер
description: Узнайте, как установить клиентские средства, например SQL Server Management Studio, на отказоустойчивом экземпляре SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3c82d510-9798-46be-bebb-cac9bef56936
author: cawrites
ms.author: chadam
ms.openlocfilehash: a596212e17e6fb273b9a6c4f5a2548aff1bc1e7f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642852"
---
# <a name="install-client-tools-on-a-sql-server-failover-cluster"></a>Установка клиентских средств на отказоустойчивом кластере SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Такие клиентские средства, как [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , являются общими компонентами для всех экземпляров на одном компьютере. Эти компоненты имеют обратную совместимость с поддержкой версий [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , которые могут быть установлены параллельно. На узле единовременно может существовать только одна версия клиентского средства.  
  
 Если клиентские средства [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] устанавливаются по время настройки на первом узле кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , они автоматически добавляются ко всем узлам, которые могут быть позднее добавлены к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью функции добавления узлов.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не добавляется автоматически к дополнительным узлам, которые добавляются к кластеру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью функции добавления узлов. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] можно установить вручную на узлах, на которых должна быть локальная копия электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Если клиентские средства [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не установлены при первоначальной установке кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , то их можно установить позже, как описано в следующих процедурах.  
  
## <a name="installation-procedures"></a>Процедуры установки  
  
#### <a name="installing-ssnoversion-client-tools-using-the-setup-user-interface"></a>Установка клиентских средств [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью пользовательского интерфейса программы установки  
  
1.  Вставьте установочный носитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . В корневом каталоге установки дважды щелкните файл Setup.exe. Чтобы выполнить установку из общего сетевого ресурса, перейдите в корневой каталог общего сетевого ресурса и дважды щелкните файл Setup.exe.  
  
2.  На странице **Установка** щелкните **Новая установка изолированного экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или добавление компонентов к существующей установке**. Не нажимайте **Новая установка отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** .  
  
3.  Средство проверки конфигурации проверяет состояние системы компьютера, после чего программа установки продолжает выполнение.  
  
4.  На странице **Тип установки** выберите **Выполнить новую установку [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** .  
  
5.  На странице **Выбор компонентов** выберите средства, которые необходимо установить, и выполните остальные процедуры установки.  
  
#### <a name="installing-ssnoversion-client-tools-at-the-command-prompt"></a>Установка клиентских средств [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в командной строке  
  
1.  Чтобы установить клиентские средства [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и электронную документацию по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], выполните следующую команду: Setup.exe/q/Action=Install /Features=Tools  
  
2.  Чтобы установить только базовую версию средств управления [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], выполните следующую команду: Setup.exe/q/Action=Install Features=SSMS. Будет выполнена установка поддержки [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] для [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)], [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)], программы sqlcmd и поставщика [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell.  
  
3.  Чтобы установить полную версию средств управления [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], выполните следующую команду: Setup.exe/q/Action=Install /Features=ADV_SSMS. Дополнительные сведения о значениях параметров компонентов см. в разделе [Установка SQL Server 2016 из командной строки](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="uninstalling-ssnoversion-client-tools"></a>Удаление клиентских средств [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 Они отображаются в оснастке "Установка и удаление программ" на панели управления под названием **[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** , в ней же их можно и удалить. При удалении экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] из отказоустойчивого кластера с использованием функции удаления узлов клиентские компоненты не удаляются.  
  
## <a name="see-also"></a>См. также:  
 [Просмотр и чтение файлов журналов программы установки SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
