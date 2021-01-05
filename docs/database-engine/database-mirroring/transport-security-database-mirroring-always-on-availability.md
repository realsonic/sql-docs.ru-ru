---
title: Защита транспорта. Группы доступности и зеркальное отображение баз данных
description: Узнайте, как защитить сообщения, передаваемые между базами данных, участвующими в группе доступности Always On или в сеансе зеркального отображения базы данных, размещенном на SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- cryptography [SQL Server], database mirroring
- certificates [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- transport security
- database mirroring [SQL Server], security
ms.assetid: 49239d02-964e-47c0-9b7f-2b539151ee1b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8ff60136994d809b78cf09ebd98e4cc36ceb1a02
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641658"
---
# <a name="transport-security---database-mirroring---always-on-availability"></a>Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Защита транспорта подразумевает проверку подлинности и (необязательно) шифрование сообщений, которыми обмениваются базы данных. Для зеркального отображения базы данных и [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]проверка подлинности и шифрование настраиваются в конечной точке зеркального отображения базы данных. Базовые сведения о конечных точках зеркального отображения базы данных см. в разделе [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 **В этом разделе.**  
  
-   [Аутентификация](#Authentication)  
  
-   [Шифрование данных](#DataEncryption)  
  
-   [Связанные задачи](#RelatedTasks)  
  
##  <a name="authentication"></a><a name="Authentication"></a> Проверка подлинности  
 Проверка подлинности — это процесс проверки того, что пользователь является тем, за кого себя выдает. Подключения между конечными точками зеркального отображения базы данных требуют проверки подлинности. Запросы соединения зеркального или следящего сервера должны проходить проверку подлинности.  
  
 Тип проверки подлинности, применяемый экземпляром сервера при зеркальном отображении базы данных или [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , является свойством конечной точки зеркального отображения базы данных. Для конечных точек зеркального отображения базы данных доступны два типа защиты транспорта: проверка подлинности Windows (интерфейс поставщика поддержки безопасности, SSPI) и проверка подлинности на основе сертификата.  
  
### <a name="windows-authentication"></a>Проверка подлинности Windows  
 При проверке подлинности Windows каждый экземпляр сервера входит в систему на зеркальную сторону, используя учетные данные Windows учетной записи пользователя Windows, под которой выполняется процесс. Проверка подлинности Windows может потребовать небольшой настройки вручную учетных записей следующим образом.  
  
-   Если экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняются как службы в одной и той же учетной записи домена, то дополнительная настройка не требуется.  
  
-   Если экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняются как службы под разными учетными записями домена (одного и того же или доверенных доменов), вам нужно создать имя входа для каждой учетной записи в базе данных **master** на каждом экземпляре сервера, а затем предоставить этому имени входа разрешение CONNECT для конечной точки.  
  
-   Если экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняются как учетные записи сетевой службы, вам нужно создать в базе данных _master_ **\\** _на каждом из остальных серверов имя входа для каждой учетной записи главного компьютера (_ имя_домена **имя _ компьютера$** ), а затем предоставить этому имени входа разрешение CONNECT для конечной точки. Это обусловлено тем, что экземпляр сервера, выполняемый под учетной записью Network Service, выполняет проверку подлинности с помощью учетной записи домена главного компьютера.  
  
> [!NOTE]  
>  Пример установки сеанса для зеркального отображения базы данных с помощью проверки подлинности Windows см. в разделе [Пример. Настройка зеркального отображения базы данных с помощью проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
  
### <a name="certificates"></a>Сертификаты  
 В некоторых ситуациях, например, когда экземпляры сервера находятся не в доверенных доменах или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется как локальная служба, проверка подлинности Windows недоступна. В этом случае для проверки подлинности запросов на соединение вместо учетных данных пользователя требуются сертификаты. Зеркальную конечную точку каждого экземпляра сервера необходимо настроить с собственным, локально созданным сертификатом.  
  
 Метод шифрования определяется при создании сертификата. Дополнительные сведения см. в разделе [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Соблюдайте осторожность при управлении действующим сертификатом.  
  
 Чтобы установить идентичность при соединении, экземпляр сервера использует закрытый ключ собственного идентификатора. Экземпляр сервера, который принимает запрос на соединение, для проверки подлинности отправителя использует открытый ключ идентификатора отправителя. Рассмотрим два экземпляра сервера: сервер_A и сервер_В. Сервер_A перед отправкой запроса на подключение к серверу_В использует закрытый ключ для шифрования заголовка соединения. Сервер_В использует открытый ключ сертификата сервера_A для расшифровки заголовка соединения. Если дешифрованный заголовок верен, сервер_B знает, что он был зашифрован сервером_A, и соединение авторизуется. Если дешифрованный заголовок неверен, сервер_B знает, что запрос на соединение не является достоверным, и отказывает в подключении.  
  
##  <a name="data-encryption"></a><a name="DataEncryption"></a> Шифрование данных  
 По умолчанию конечная точка зеркальной базы данных требует шифрования данных, которые отправляются по соединениям зеркальных баз данных. В этом случае конечная точка может подключиться только к конечной точке, которая также использует соединение. Для соединений зеркального отображения базы данных рекомендуется использовать шифрование, за исключением случаев, когда гарантируется безопасность сети. Тем не менее шифрование можно отключить или сделать так, чтобы оно поддерживалось, но не требовалось явно. Если шифрование отключено, данные не шифруются, а конечная точка не может подключаться к конечной точке, требующей шифрования. Если шифрование поддерживается, данные шифруются, только если противоположная конечная точка поддерживает или требует шифрования.  
  
> [!NOTE]  
>  Зеркальные конечные точки, созданные [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , или требуют шифрования, или создаются с отключенным шифрованием. Чтобы изменить параметры шифрования на SUPPORTED, используется инструкция ALTER ENDPOINT [!INCLUDE[tsql](../../includes/tsql-md.md)] . Дополнительные сведения см. в статье [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 При необходимости пользователь обладает возможностью управлять алгоритмом шифрования, который может быть использован конечной точкой, путем определения одного из следующих значений для параметра ALGORITHM в инструкции CREATE ENDPOINT или в инструкции ALTER ENDPOINT:  
  
|Значение параметра ALGORITHM|Описание|  
|---------------------|-----------------|  
|RC4;|Указывает на то, что конечная точка должна использовать алгоритм RC4. Это значение по умолчанию.<br /><br /> <strong>\*\* Предупреждение. \*\*</strong> Алгоритм RC4 использовать не рекомендуется. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется использовать алгоритм AES.|  
|AES|Указывает на то, что конечная точка должна использовать алгоритм AES.|  
|AES RC4|Указывает на то, что две конечные точки будут согласовывать алгоритм шифрования, оставляя приоритет за алгоритмом AES.|  
|RC4 AES|Указывает на то, что две конечные точки будут согласовывать алгоритм шифрования, оставляя приоритет за алгоритмом RC4.|  
  
 Если подсоединяемые конечные точки указывают оба алгоритма, но в разной последовательности, используется тот алгоритм, который указан принимающей стороной.  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритма RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
>   
>  Алгоритм RC4 является относительно слабым, хотя работает значительно быстрее по сравнению с AES, в то время как AES является довольно надежным алгоритмом. Поэтому рекомендуется использовать алгоритм AES.  
  
 Дополнительные сведения о синтаксисе [!INCLUDE[tsql](../../includes/tsql-md.md)] для задания шифрования см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Конфигурация защиты транспорта для конечной точки зеркального отображения базы данных**  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
## <a name="see-also"></a>См. также:  
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [DROP ENDPOINT (Transact-SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [Центр обеспечения безопасности для базы данных Azure SQL и SQL Server Database Engine](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)   
 [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Поиск и устранение неисправностей конфигурации групп доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
