---
title: Руководство по установке SQL Server
description: Указатель содержимого, которое поможет установить SQL Server и связанные компоненты с помощью мастера установки, командной строки или sysprep.
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- AdventureWorks sample database
- installing SQL Server, preparing to install
- installation [SQL Server]
ms.assetid: 0300e777-d56b-4d10-9c33-c9ebd2489ee5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1930f2e9d41bb212b01a9417fb7d6c861fda228f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172046"
---
# <a name="sql-server-installation-guide"></a>Руководство по установке SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

В этой статье приводится индекс содержимого с рекомендациями по установке SQL Server в Windows.

Дополнительные сведения о других сценариях развертывания см. в следующих источниках:

- [Linux](../../linux/sql-server-linux-setup.md)
- [Контейнеры Docker](../../linux/sql-server-linux-docker-container-deployment.md)
- [Kubernetes — кластеры больших данных](../../big-data-cluster/deploy-get-started.md)

Начиная с [!INCLUDE[sssql15](../../includes/sssql16-md.md)], [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] доступен только в виде 64-разрядного приложения. Ниже приведены важные подробности о том, как получить и установить SQL Server.

## <a name="getting-started"></a>Начало работы
  
* **Выпуски и компоненты**. Ознакомьтесь с поддерживаемыми компонентами для различных выпусков и версий SQL Server, чтобы определить, какие из них лучше подходят для вашего бизнеса. 
    - [[!INCLUDE[ss2019](../../includes/sssqlv15-md.md)]](~/sql-server/editions-and-components-of-sql-server-version-15.md).  
    - [[!INCLUDE[ss2017](../../includes/sssqlv14-md.md)]](~/sql-server/editions-and-components-of-sql-server-2017.md).  
    - [[!INCLUDE[ss2016](../../includes/sssql16-md.md)]](~/sql-server/editions-and-components-of-sql-server-2016.md).  
    - [[!INCLUDE[ss2014](../../includes/sssql14-md.md)]](https://docs.microsoft.com/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014)

*  **Требования**. Ознакомьтесь с требованиями к оборудованию и программному обеспечению для установки [SQL Server 2016 и 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) или [SQL Server на Linux](../../linux/sql-server-linux-setup.md), а также проверками конфигурации системы и рекомендациями по безопасности в разделе [Планирование установки SQL Server](../../sql-server/install/planning-a-sql-server-installation.md) 


  
* **Образцы баз данных и примеры кода**: 
    * Они не устанавливаются по умолчанию в ходе установки SQL Server, но их можно найти 
    * Чтобы установить их для отличных от Express выпусков SQL Server, перейдите на веб-сайт с [примерами](../../samples/sql-samples-where-are.md)
    

## <a name="installation-media"></a>Установочный носитель

Место, откуда можно скачать [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], зависит от выпуска.

* **Выпуски Enterprise, Standard и Express SQL Server** предназначены для использования в рабочей среде. Чтобы получить установочный носитель с выпуском Enterprise или Standard, обратитесь к своему поставщику программного обеспечения. Сведения о приобретении и каталог партнеров Майкрософт можно найти на [странице лицензирования Майкрософт](https://www.microsoft.com/licensing/product-licensing/sql-server).
* [Бесплатная версия — последняя](https://www.microsoft.com/sql-server/sql-server-downloads)
* [Бесплатная версия — другие](https://www.microsoft.com/evalcenter/evaluate-sql-server)


Другие компоненты SQL Server можно найти здесь: 

* [Все накопительные пакеты обновления](https://sqlserverbuilds.blogspot.com/)
* [Службы SQL Server Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122). 
* [Среда SQL Server Management Studio](https://aka.ms/ssmsfullsetup)
* [Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2109256)


## <a name="considerations"></a>Рекомендации

-   Установку не удастся выполнить, если запустить программу установки через удаленный рабочий стол, но носитель при этом будет расположен на клиенте RDC. Чтобы выполнить установку удаленно, установочный носитель должен быть расположен на общем сетевом ресурсе или в локальной папке физической или виртуальной машины. Установочный носитель[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может быть расположен на общем сетевом ресурсе, сопоставленном диске, локальном диске, или он может быть представлен в виде ISO-образа на виртуальной машине.  
  
  
-   Программа установки[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливает следующие компоненты, необходимые для продукта:  
  
    -   Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]    
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Файлы поддержки программы установки  

## <a name="sql-server-installation"></a>Установка SQL Server


|Статья|Описание|  
|-----------|-----------------|  
|[Мастер установки](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)|Установите SQL Server с помощью графического интерфейса мастера установки, запущенного из файла setup.exe на установочном носителе. |  
|[Командная строка](./install-sql-server-from-the-command-prompt.md)|Пример синтаксиса и параметров установки для запуска установки SQL Server из командной строки. | 
|[Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)|Установите [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] в Windows Server Core.|  
|[Параметры для средства проверки конфигурации системы](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)|Содержит обсуждение функций средства проверки конфигурации (SCC).|   
|[Файл конфигурации](./install-sql-server-using-a-configuration-file.md)|Образцы синтаксиса и параметров установки для запуска программы установки с помощью файла конфигурации.|  
|[SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)|Образцы синтаксиса и параметров установки для запуска программы установки с помощью SysPrep.|
|[Добавление компонентов в экземпляр](./add-features-to-an-instance-of-sql-server-setup.md)|Обновление компонентов существующего экземпляра [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Установка отказоустойчивого кластера SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)| Установка экземпляра отказоустойчивого кластера SQL Server.  | 
|[Исправление неудавшейся установки [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)|Восстановление поврежденной установки [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Переименование компьютера с SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)|Обновите системные метаданные, хранящиеся в sys.servers, после переименования узла компьютера, на котором размещен изолированный экземпляр SQL Server. |  
|[Установка обновлений для обслуживания [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/install-sql-server-servicing-updates.md)|Установка обновлений для [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Файлы журналов установки](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)| Просмотр и чтение ошибок в файлах журналов программы установки SQL Server. |  
|[Проверка установки](../../database-engine/install-windows/validate-a-sql-server-installation.md)|Описано использование отчета обнаружения SQL для проверки того, какая версия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и какие компоненты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] установлены на компьютере.|  
  
  
## <a name="individual-component-installation"></a>Установка отдельных компонентов 
  
|Статья|Описание|  
|-----------|-----------------|  
|[Ядро СУБД SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md)|Установка и настройка [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Репликация SQL Server](../../database-engine/install-windows/install-sql-server-replication.md)|Установка и настройка репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Распределенное воспроизведение](../../tools/distributed-replay/install-distributed-replay-overview.md)|Содержит список статей, связанных с установкой компонента распределенного воспроизведения.|  
|[Средства управления SQL Server со средой SSMS](../../ssms/download-sql-server-management-studio-ssms.md)|Установка и настройка средств управления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[SQL Server PowerShell](../../database-engine/install-windows/install-sql-server-powershell.md)|Рекомендации по установке компонентов PowerShell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  

## <a name="sql-server-configuration"></a>Конфигурация SQL Server 
  
|Статья|Описание|  
|-----------|-----------------|  
|[Настройка брандмауэра Windows (SQL Server)](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|Общие сведения о конфигурации брандмауэра и настройке брандмауэра Windows для предоставления доступа к SQL Server.|  
|[Настройка брандмауэра Windows (SSAS)](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|Настройте порт и брандмауэр, чтобы разрешить доступ к [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] или [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint.|  
|[Настройка многосетевого компьютера](../../sql-server/install/configure-a-multi-homed-computer-for-sql-server-access.md)|Настройте [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и брандмауэр Windows в режиме повышенной безопасности для предоставления сетевого подключения экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в многосетевой среде.|  

 
## <a name="see-also"></a>См. также раздел  

[Обновление [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/upgrade-sql-server.md)   
[Удаление [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/uninstall-sql-server.md)   
[Установка SQL Server Reporting Services (SSRS)](../../reporting-services/install-windows/install-reporting-services.md)   
[Установка служб SQL Server Analysis Services (SSAS)](/analysis-services/instances/install-windows/install-analysis-services)   
[Установка компонентов бизнес-аналитики [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
[Решения высокого уровня доступности &#40;SQL Server&#41;](../sql-server-business-continuity-dr.md)