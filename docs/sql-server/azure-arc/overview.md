---
title: SQL Server с поддержкой Azure Arc
titleSuffix: ''
description: Управление экземплярами SQL Server с помощью SQL Server с поддержкой Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: c1ba7f7552b5050e3c1fa7bc765acfa431f3df30
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103149"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>SQL Server с поддержкой Azure Arc (предварительная версия)

SQL Server с поддержкой Azure Arc является частью службы Azure Arc для серверов. Он распространяет службы Azure до экземпляров SQL Server, размещенных за пределами Azure в центре обработки данных клиента, в пограничной зоне или в среде с несколькими облаками.

Для включения служб Azure выполняющийся экземпляр SQL Server должен быть зарегистрирован в службе Azure Arc с помощью портала Azure и скрипта регистрации. После регистрации экземпляр будет представлен в Azure как ресурс __SQL Server — Azure Arc__. Свойства этого ресурса представляют собой подмножество параметров конфигурации SQL Server.

SQL Server можно установить на виртуальном или физическом компьютере под управлением Windows или Linux, подключенном к службе Azure Arc через агент Azure Connected Machine. Агент устанавливается, и компьютер автоматически регистрируется в процессе регистрации экземпляра SQL Server. Агент Connected Machine для Linux и Windows обменивается исходящим трафиком со службой Azure Arc через TCP-порт 443. Если компьютер подключается через брандмауэр или прокси-сервер для взаимодействия по Интернету, ознакомьтесь с [требованиями к конфигурации сети для агента Connected Machine](/azure/azure-arc/servers/agent-overview#prerequisites).

Общедоступная предварительная версия SQL Server с поддержкой Azure Arc поддерживает набор решений, которым требуется установка серверного расширения Microsoft Monitoring Agent (MMA) и подключение к рабочей области Azure Log Analytics для сбора данных и создания отчетов. Эти решения включают в себя расширенную защиту данных с помощью Центра безопасности Azure и Azure Sentinel, а также проверки работоспособности среды SQL с помощью функции "Оценка SQL по требованию".

Архитектура SQL Server с поддержкой Azure Arc показана на следующей схеме.

![Архитектура общедоступной предварительной версии](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>Предварительные требования

### <a name="supported-sql-versions-and-operating-systems"></a>Поддерживаемые версии SQL и операционные системы

SQL Server с поддержкой Azure Arc поддерживает SQL Server 2012 или более поздней версии, работающий в одной из следующих версий операционных систем Windows или Linux:

- Windows Server 2012 R2 и более поздних версий;
- Ubuntu 16.04 и 18.04 (x64);
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64).

### <a name="required-permissions"></a>Необходимые разрешения

Для подключения экземпляров SQL Server и хост-компьютера к службе Azure Arc вам потребуется учетная запись с правами на выполнение следующих действий:
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

Для обеспечения оптимальной безопасности рекомендуется создать в Azure настраиваемую роль с перечисленными выше минимальными разрешениями. Сведения о том, как создать настраиваемую роль в Azure с этими разрешениями, см. в разделе [Общие сведения о настраиваемых ролях](/azure/active-directory/users-groups-roles/roles-custom-overview). Чтобы добавить назначение ролей, ознакомьтесь с разделом [Добавление или удаление назначений ролей Azure с помощью портала Azure](/azure/role-based-access-control/role-assignments-portal) или [Добавление или удаление назначений ролей с помощью Azure RBAC и Azure CLI](/azure/role-based-access-control/role-assignments-cli).

### <a name="azure-subscription-and-service-limits"></a>Ограничения подписки и служб Azure

Прежде чем настраивать компьютеры и экземпляры SQL Server для работы с Azure Arc, ознакомьтесь с [ограничениями подписки](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits) и [ограничениями группы ресурсов](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits), чтобы можно было планировать количество компьютеров для подключения.

### <a name="networking-configuration-and-resource-providers"></a>Конфигурация сети и поставщики ресурсов

Проверьте [конфигурацию сети, протокол TLS и поставщики ресурсов](/azure/azure-arc/servers/agent-overview#prerequisites), необходимые для агента Connected Machine.

Для подключения экземпляров SQL Server к Azure Arc требуется поставщик ресурсов `Microsoft.AzureArcData`. Инструкции по регистрации поставщика ресурсов приводятся в разделе [Предварительные требования](connect.md#prerequisites).

Если у вас уже есть экземпляры SQL Server, подключенные к службе Azure Arc, выполните следующие действия, чтобы перенести существующие ресурсы **SQL Server — Azure Arc** в новое пространство имен.

### <a name="supported-azure-regions"></a>Поддерживаемые регионы Azure

Эта общедоступная предварительная версия доступна в следующих регионах:
- Восточная часть США
- восточная часть США 2
- Западная часть США 2
- Восточная Австралия
- Юго-Восточная Азия
- Северная Европа
- Западная Европа
- южная часть Соединенного Королевства

## <a name="next-steps"></a>Дальнейшие действия

- [Подключение SQL Server к Azure Arc](connect.md)
- [Настройка экземпляра SQL Server для периодической проверки работоспособности среды с помощью оценки SQL по запросу](assess.md)
- [Настройка расширенной защиты данных для экземпляра SQL Server](configure-advanced-data-security.md)