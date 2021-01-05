---
title: Настройка учетных записей входа (зеркальное отображение и группы доступности)
description: Настройте учетные записи входа для доступа к конечной точке зеркального отображения базы данных или группе доступности Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- logins [SQL Server], database mirroring
ms.assetid: e9f5287b-1325-4cda-88a6-19eaaa52a652
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea62f1f934527ce40e2b5fe2f03b8b6c1b8e49bc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641996"
---
# <a name="set-up-login-accounts---database-mirroring-always-on-availability"></a>Настройка учетных записей входа для зеркального отображения баз данных или групп доступности AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Чтобы два экземпляра сервера могли подключиться к [конечным точкам зеркального отображения базы данных](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) друг друга, именам входа каждого экземпляра требуется доступ к другому экземпляру. Кроме того, каждой учетной записи требуется разрешение на подключение к конечной точке зеркального отображения базы данных на другом экземпляре.  
  
 Влияние этого требования зависит от того, выполняются ли экземпляры сервера под одной и той же учетной записью пользователя домена.  
  
-   Если экземпляры сервера запущены под одной и той же учетной записью домена, правильные имена входа автоматически существуют в обеих базах данных **master** . Это упрощает настройку безопасности для зеркального отображения баз данных и групп доступности AlwaysOn.  
  
-   Если экземпляры серверов запущены под разными учетными записями, то имена входа на экземпляре сервера, на котором расположен основной сервер или первичная реплика, необходимо вручную воспроизвести на экземпляре сервера, на котором размещен сервер зеркального отображения, или на всех экземплярах серверов, на которых размещены вторичные реплики. Дополнительные сведения см. в подразделах [Создание имени входа для другой учетной записи](#CreateLogin) и [Предоставление разрешения на подключение](#GrantConnect)далее в этом разделе.  
  
    > [!IMPORTANT]  
    >  Для создания более безопасной среды рассмотрите возможность использования отдельных учетных записей домена для каждого экземпляра сервера.  
  
##  <a name="create-a-login-for-a-different-account"></a><a name="CreateLogin"></a> Создание имени входа для другой учетной записи  
 Если два экземпляра сервера запущены под разными учетными записями, системный администратор должен использовать инструкцию CREATE LOGIN [!INCLUDE[tsql](../../includes/tsql-md.md)] для создания имени входа для учетной записи службы запуска удаленного экземпляра для каждого экземпляра сервера. Дополнительные сведения см. в разделе [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md).  
  
> [!IMPORTANT]  
>  Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запущен под учетной записью, не относящейся к домену, следует использовать сертификаты. Дополнительные сведения см. в разделе [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
 Например, для подключения экземпляра сервера sqlA, запущенного под именем loginA к экземпляру сервера sqlB, запущенному под именем loginB, необходимо, чтобы имя loginA существовало на сервере sqlB, а имя loginB — на сервере sqlA. Кроме того, если в сеансе зеркального отображения базы данных участвует экземпляр следящего сервера (sqlC) и если все три экземпляра сервера запущены под разными учетными записями домена, то необходимо создать следующие учетные записи для зеркального отображения:  
  
|На экземпляре...|Необходимо создать имена входа и предоставить разрешение на соединение с...|  
|--------------------|--------------------------------------------------------------|  
|sqlA|sqlB и sqlC|  
|sqlB|sqlA и sqlC|  
|sqlC|sqlA и sqlB|  
  
> [!NOTE]  
>  Можно подключиться, используя учетную запись сетевой службы, если вместо пользователя домена указать учетную запись компьютера. При использовании учетной записи компьютера ее необходимо добавить в качестве пользователя на другом экземпляре сервера.  
  
##  <a name="grant-connect-permission"></a><a name="GrantConnect"></a> Предоставление разрешения на подключение  
 После создания имени входа на экземпляре сервера имени входа должно быть предоставлено разрешение на соединение с конечной точкой зеркального отображения базы данных на другом экземпляре. Системный администратор предоставляет разрешение на подключение с помощью инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] GRANT. Дополнительные сведения см. в статье [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание имени входа](../../relational-databases/security/authentication-access/create-a-login.md)  
  
-   [Разрешение сетевого доступа к конечной точке зеркального отображения базы данных с использованием проверки подлинности Windows (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Поиск и устранение неисправностей конфигурации групп доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
