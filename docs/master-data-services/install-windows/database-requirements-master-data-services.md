---
title: Требования к базе данных
description: Используйте диспетчер конфигурации Master Data Services, чтобы создать и настроить базу данных Master Data Services, в которой хранятся все основные данные.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: fe731839-c5c4-4884-bb6a-644eca28bb30
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 21224e52ae9109f5404fc6a6ee11ad59e525995b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171176"
---
# <a name="database-requirements-master-data-services"></a>Требования к базе данных (службы Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Все основные данные хранятся в базе данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . На компьютере, на котором размещена эта база данных, должен быть запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Воспользуйтесь средством [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] для создания и настройки базы данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] на локальном или удаленном компьютере. Если база данных перемещается из одной среды в другую, то можно сохранить информацию в новой среде, связав веб-службу [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] и [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] с базой данных в новом расположении.  
  
> [!NOTE]  
>  Любой компьютер, на котором устанавливаются компоненты [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , должен иметь соответствующие лицензии. Дополнительные сведения см. в лицензионном соглашении.  
  
## <a name="requirements"></a>Требования  
 Перед началом создания базы данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] проверьте выполнение следующих требований.  
  
### <a name="sql-server-edition"></a>Выпуск SQL Server  
 База данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] может быть размещена на следующих выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Enterprise x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Developer Edition x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer Edition x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Business Intelligence x64 (64-разрядная версия)  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise (64-разрядная версия) x64 — обновление только с [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Enterprise  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Developer Edition x64 (64-разрядная версия)  
  
-   64-разрядная версия Microsoft SQL Server 2008 R2 Enterprise x64  
  
-   Microsoft SQL Server 2008 R2 Developer x64 (64-разрядная версия)  
  
 Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). 
  
### <a name="operating-system"></a>Операционная система  
 Дополнительные сведения о поддерживаемых операционных системах Windows и других требованиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] см. в статье [Требования к оборудованию и программному обеспечению для установки SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
### <a name="accounts-and-permissions"></a>Учетные записи и разрешения  
  
|Тип|Описание|  
|----------|-----------------|  
|Учетная запись пользователя|В [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]для соединения с экземпляром компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] , на котором размещена база данных служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , можно использовать учетную запись Windows или учетную запись [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Учетная запись пользователя должна принадлежать роли сервера **sysadmin** на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Дополнительные сведения о роли **sysadmin** см. в статье [Роли уровня сервера](../../relational-databases/security/authentication-access/server-level-roles.md).|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] учетная запись администратора|При создании базы данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] необходимо указать учетную запись пользователя домена, который является системным администратором [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Для всех веб-приложений [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], связанных с этой базой данных, пользователь может обновлять все модели и все данные во всех функциональных областях. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md).|  
  
### <a name="database-backup"></a>Резервное копирование базы данных  
 Рекомендуется выполнять резервное копирование базы данных ежедневно в периоды низкой активности, а резервные копии журнала транзакций следует делать чаще в соответствии с требованиями среды. Дополнительные сведения о резервных копиях см. в статье [Общие сведения о резервном копировании (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
## <a name="see-also"></a>См. также:  
 [Установка Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)   
 [Создание базы данных Master Data Services](../../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [База данных Master Data Services](../../master-data-services/master-data-services-database.md)   
 [Диалоговое окно «Подключение к Master Data Services базе данных»](../../master-data-services/connect-to-a-master-data-services-database-dialog-box.md)   
 [Мастер создания базы данных (диспетчер конфигурации служб Master Data Services)](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)  
  
