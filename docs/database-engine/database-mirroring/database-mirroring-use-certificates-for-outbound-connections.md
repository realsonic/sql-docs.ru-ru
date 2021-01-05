---
title: Использование сертификатов для исходящих соединений при зеркальном отображении базы данных | Документы Майкрософт
description: Сведения о настройке экземпляров сервера для использования сертификатов проверки подлинности входящих соединений при зеркальном отображении базы данных после настройки исходящих соединений.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- outbound connections [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 464c9096-10d6-4c5e-8bb1-19acba27ad9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 163b7af8795b663645133307317e39646deee6e5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637575"
---
# <a name="database-mirroring---use-certificates-for-outbound-connections"></a>Использование сертификатов для исходящих соединений при зеркальном отображении базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом подразделе описаны этапы настройки экземпляров сервера для использования сертификатов проверки подлинности исходящих соединений при зеркальном отображении базы данных. Конфигурацию исходящих соединений необходимо выполнить до настройки входящих соединений.  
  
> [!NOTE]  
>  Все соединения зеркального отображения экземпляра сервера используют одну и ту же конечную точку зеркального отображения базы данных, и при ее создании необходимо задать метод проверки подлинности экземпляра сервера.  
  
 Конфигурирование исходящих соединений включает следующие шаги.  
  
1.  В базе данных **master** создайте главный ключ базы данных.  
  
2.  В базе данных **master** создайте зашифрованный сертификат для экземпляра сервера.  
  
3.  Создайте конечную точку для экземпляра сервера при использовании его сертификата.  
  
4.  Создайте резервную копию сертификата в файле и защищенным образом скопируйте этот файл на другие системы.  
  
 Все эти этапы необходимо выполнить для каждого из участников, а также для следящего сервера, если он есть.  
  
 Ниже эти шаги описаны подробно. Для каждого шага приведен пример настройки экземпляра сервера в системе с именем HOST_A. В соответствующем разделе «Пример» показаны те же шаги для другого экземпляра сервера в системе с именем HOST_B.  
  
## <a name="procedure"></a>Процедура  
  
#### <a name="to-configure-server-instances-for-outbound-mirroring-connections-on-host_a"></a>Настройка экземпляров сервера на исходящие соединения зеркального отображения (на HOST_A)  
  
1.  В базе данных **master** создайте главный ключ базы данных, если он еще не создан. Для просмотра существующих ключей базы данных предназначено представление каталога [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) .  
  
     Создание главного ключа базы данных производится следующей инструкцией [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
     Используйте уникальный надежный пароль, запишите его и храните в надежном месте.  
  
     Дополнительные сведения см. в разделах [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md) и [Создание главного ключа базы данных](../../relational-databases/security/encryption/create-a-database-master-key.md).  
  
2.  В базе данных **master** создайте зашифрованный сертификат экземпляра сервера, он будет использоваться для исходящих соединений этого экземпляра при зеркальном отображении базы данных.  
  
     Например, чтобы создать сертификат для системы HOST_A.  
  
    > [!IMPORTANT]  
    >  Если планируется использование сертификата в течение срока, превышающего один год, укажите дату истечения срока действия в формате UTC с помощью параметра EXPIRY_DATE инструкции CREATE CERTIFICATE. Кроме того, рекомендуется использовать среду SQL Server Management Studio для создания правила управления на основе политик, чтобы получать предупреждения при завершении срока действия сертификатов. В диалоговом окне **Создание нового условия** создайте это правило в поле **\@ExpirationDate** аспекта **Certificate**. Дополнительные сведения см. в разделах [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) и [Обеспечение безопасности SQL Server](../../relational-databases/security/securing-sql-server.md).  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate for database mirroring',   
       EXPIRY_DATE = '11/30/2013';  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Просмотр сертификатов в базе данных **master** можно выполнить с помощью следующих инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    USE master;  
    SELECT * FROM sys.certificates;  
    ```  
  
     Дополнительные сведения см. в разделе [sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
3.  Убедитесь, что в каждом экземпляре сервера существует конечная точка зеркального отображения базы данных.  
  
     Если конечная точка уже имеется для данного экземпляра сервера, она используется для всех сеансов, устанавливаемых для этого экземпляра. Чтобы определить, существует ли конечная точка зеркального отображения базы данных в экземпляре сервера и просмотреть ее конфигурацию, выполните следующую инструкцию:  
  
    ```  
    SELECT name, role_desc, state_desc, connection_auth_desc, encryption_algorithm_desc   
       FROM sys.database_mirroring_endpoints;  
    ```  
  
     Если конечная точка не существует, создайте конечную точку, которая использует этот сертификат для исходящих соединений и его учетные данные для проверки на других системах. Это конечная точка на уровне сервера, используемая всеми зеркальными сеансами, в которых участвует экземпляр сервера.  
  
     Например, чтобы создать конечную точку зеркального отображения для экземпляра сервера HOST_A.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
4.  Создайте резервную копию сертификата и скопируйте ее на другую систему или системы. Она понадобится при конфигурировании на них входящих соединений.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
     Скопируйте этот сертификат любым безопасным способом. Отнеситесь с особым вниманием к хранению сертификатов в безопасном месте.  
  
 Программный код примеров на предыдущих шагах конфигурирует исходящие соединения на HOST_A.  
  
 Необходимо выполнить аналогичные шаги для настройки исходящего соединения для HOST_B. Они показаны в следующем разделе примеров.  
  
## <a name="example"></a>Пример  
 В следующем примере показано конфигурирование исходящих соединений на HOST_Б.  
  
```  
USE master;  
--Create the database Master Key, if needed.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
GO  
-- Make a certifcate on HOST_B server instance.  
CREATE CERTIFICATE HOST_B_cert   
   WITH SUBJECT = 'HOST_B certificate for database mirroring',   
   EXPIRY_DATE = '11/30/2013';  
GO  
--Create a mirroring endpoint for the server instance on HOST_B.  
CREATE ENDPOINT Endpoint_Mirroring  
   STATE = STARTED  
   AS TCP (  
      LISTENER_PORT=7024  
      , LISTENER_IP = ALL  
   )   
   FOR DATABASE_MIRRORING (   
      AUTHENTICATION = CERTIFICATE HOST_B_cert  
      , ENCRYPTION = REQUIRED ALGORITHM AES  
      , ROLE = ALL  
   );  
GO  
--Backup HOST_B certificate.  
BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
GO   
--Using any secure copy method, copy C:\HOST_B_cert.cer to HOST_A.  
```  
  
 Скопируйте этот сертификат в другую систему любым безопасным способом. Отнеситесь с особым вниманием к хранению сертификатов в безопасном месте.  
  
> [!IMPORTANT]  
>  После настройки исходящих соединений необходимо настроить входящие соединения на каждом экземпляре сервера для остальных экземпляров сервера. Дополнительные сведения см. в разделе [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 Дополнительные сведения о создании зеркальной базы данных, содержащей пример Transact-SQL, см. в разделе [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Пример Transact-SQL, устанавливающий сеанс в режиме высокой производительности, см. в разделе [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Безопасность .NET Framework  
 За исключением случаев, когда сеть гарантированно защищена, рекомендуется для соединений зеркального отображения базы данных применять шифрование.  
  
 При копировании сертификата на другую систему используйте безопасный метод копирования.  
  
## <a name="see-also"></a>См. также:  
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Настройка зашифрованной зеркальной базы данных](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
