---
title: Удаленное хранилище больших двоичных объектов (SQL Server) | Документация Майкрософт
description: Удаленное хранилище больших двоичных объектов (RBS) SQL Server хранит большие двоичные объекты в стандартном хранилище, а не на основном сервере базы данных. Дополнительные сведения об этом компоненте надстройки.
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bfd234025664e7508d7d9cf942ff81a216b447a2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171756"
---
# <a name="remote-blob-store-rbs-sql-server"></a>Удаленное хранилище больших двоичных объектов (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаленное хранилище больших двоичных объектов (RBS) для[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это дополнительный компонент, позволяющий администраторам баз данных хранить большие двоичные объекты в отдельных хранилищах вместо хранения непосредственно на основном сервере базы данных.  
  
 Хранилище RBS включено в установочный носитель [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , но не устанавливается программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ищите RBS.msi на установочном носителе, чтобы найти файл установки.

 Если у вас нет установочного носителя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно скачать RBS в одном из следующих расположений:

| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version | Расположение скачивания RBS |
|:---|:---|
| [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | [[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Пакет дополнительных компонентов SP2](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [Пакет дополнительных компонентов [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] | [Страница скачивания RBS [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>Для чего используется RBS?  
  
### <a name="optimized-database-storage-and-performance"></a>Оптимизированные производительность и хранилище базы данных  
 Хранение больших двоичных объектов в базе данных подразумевает использование большого объема файлового пространства и дорогостоящих ресурсов серверного оборудования. RBS передает большие двоичные объекты в выбранное пользователем выделенное хранилище. При этом в базе данных сохраняются только ссылки на них. Благодаря этому на сервере освобождается место для хранения структурированных данных, а также высвобождаются ресурсы для выполнения операций над базами данных.  
  
### <a name="efficient-blob-management"></a>Эффективное управление большими двоичными объектами  
 Некоторые функции RBS позволяют управлять хранимыми большими двоичными объектами:  
  
-   Управление большими двоичными объектами осуществляется с помощью ACID-транзакций (атомарность, согласованность, изолированность и устойчивость).  
  
-   Большие двоичные объекты организованы в виде коллекций.  
  
-   Поддерживаются функции сборки мусора, проверки согласованности и другие обслуживающие функции.  
  
### <a name="standardized-api"></a>Стандартизированный API-интерфейс  
 Удаленное хранилище больших двоичных объектов определяет набор API, с помощью которых обеспечивается стандартная модель программирования, которая используется приложениями для доступа и изменения всех хранилищ больших двоичных объектов. Каждое хранилище больших двоичных объектов может иметь собственную библиотеку поставщика, которая подключается к библиотеке клиента удаленного хранилища больших двоичных объектов и указывает способы хранения больших двоичных объектов и доступа к ним.  
  
 Некоторые сторонние поставщики хранилищ разработали средства удаленного хранения больших двоичных объектов, которые соответствуют этим стандартным API-интерфейсам и поддерживают хранилища больших двоичных объектов на разных платформах.  
  
## <a name="rbs-requirements"></a>Требования к удаленным хранилищам больших двоичных объектов  
 - При работе с удаленным хранилищем больших двоичных объектов необходим выпуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise для главного сервера базы данных, на котором хранятся метаданные больших двоичных объектов.  Однако, если используется предоставленный поставщик FILESTREAM, сами большие двоичные объекты можно хранить в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard. Для подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]RBS требуется по меньшей мере версия 11 драйвера ODBC для [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] и версия 13 драйвера ODBC для [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. Драйверы можно найти на странице [скачивания драйвера ODBC для SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).    
  
 Удаленное хранилище больших двоичных объектов включает поставщик FILESTREAM, позволяющий использовать удаленное хранилище больших двоичных объектов для хранения больших двоичных объектов на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы применять RBS для хранения больших двоичных объектов в другом хранилище, необходимо использовать сторонний поставщик RBS, разработанный для этого хранилища, или разработать пользовательский поставщик RBS с помощью API-интерфейса RBS. Образец поставщика, хранящего большие двоичные объекты в файловой системе NTFS, доступен в качестве обучающего ресурса на сайте [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190).  
  
## <a name="rbs-security"></a>Безопасность удаленного хранилища больших двоичных объектов  
 Хорошим источником информации об этой возможности является блог группы разработчиков удаленного хранилища BLOB-объектов SQL. Модель безопасности RBS описана в записи блога [Модель безопасности RBS](/archive/blogs/sqlrbs/rbs-security-model).  
  
### <a name="custom-providers"></a>Настраиваемые поставщики  
 Если вы используете настраиваемых поставщиков для хранения больших двоичных объектов вне [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], обязательно защищайте сохраненные большие двоичные объекты с помощью разрешений и параметров шифрования, подходящих для среды хранения, которая используется настраиваемым поставщиком.  
  
### <a name="credential-store-symmetric-key"></a>Симметричный ключ хранилища учетных данных  
 Если поставщик требует установки и использования секретов, хранимых в хранилище учетных данных, RBS использует симметричный ключ для шифрования секрета поставщика, который клиент может использовать для авторизации в хранилище больших двоичных объектов этого поставщика.  
  
-   RBS 2016 использует симметричный ключ **AES_128** . [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] позволяет создавать новые ключи **TRIPLE_DES** только для обеспечения обратной совместимости. Дополнительные сведения см. в статье [CREATE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
-   RBS 2014 и более ранние версии используют хранилище учетных данных, которое содержит секретные данные, зашифрованные с помощью устаревшего алгоритма симметричного ключа **TRIPLE_DES**. Если вы используете **TRIPLE_DES**[!INCLUDE[msCoName](../../includes/msconame-md.md)] , рекомендуется усилить безопасность и сменить ваш ключ на новый с более надежным методом шифрования, выполнив описанные в этом разделе действия.  
  
 Вы можете задать свойства симметричного ключа для хранилища учетных данных RBS, выполнив следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] в базе данных RBS:   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` Если выходные данные подтвердят, что **TRIPLE_DES** все еще используется, смените этот ключ.  
  
### <a name="rotating-the-symmetric-key"></a>Смена симметричного ключа  
 При использовании RBS следует периодически менять симметричный ключ хранилища учетных данных. Это стандартная рекомендация для соблюдения политик организационной безопасности.  Например, для смены симметричного ключа хранилища учетных данных RBS можно использовать [приведенный ниже сценарий](#Key_rotation) базы данных RBS.  Этот сценарий также можно использовать для повышения уровня надежности шифрования, например для смены алгоритма или увеличения длины ключа. Перед сменой ключей обязательно создайте резервную копию базы данных.  В конце этого сценария предусмотрены несколько шагов проверки.  
Если ваша политика безопасности требует другие параметры шифрования (например, другой алгоритм или длину ключа), вы можете использовать этот скрипт как шаблон. Измените ключевые свойства в двух местах: 1) при создании временного ключа и 2) при создании постоянного ключа.  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> Ресурсы RBS  
  
 **Образцы RBS**  
 Образцы RBS, доступные на сайте [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190) , демонстрируют способ разработки приложения RBS, а также способ разработки и установки пользовательского поставщика хранилища RBS.  
  
 **Блог об RBS**  
 В [блоге по удаленному хранилищу больших двоичных объектов](/archive/blogs/sqlrbs/) содержатся дополнительные сведения, которые помогут лучше понять принципы работы, развертывания и обслуживания удаленных хранилищ больших двоичных объектов.  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> Скрипт смены ключей  
 Сценарий из этого примера создает хранимую процедуру с именем `sp_rotate_rbs_symmetric_credential_key` , которая заменяет используемый симметричный ключ хранилища учетных данных RBS на новый симметричный ключ  
с выбранными вами параметрами.  Этот сценарий будет полезен, если политика безопасности требует   
периодической смены ключей или использования конкретного алгоритма.  
 Эта хранимая процедура заменяет текущий симметричный ключ на ключ с алгоритмом **AES_256** .  После  
замены симметричного ключа следует повторно зашифровать секреты с помощью нового ключа.  Эта хранимая   
процедура выполняет процесс шифрования секретов.  Перед сменой ключей следует создать резервную копию базы данных.  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 Теперь вы можете использовать хранимую процедуру `sp_rotate_rbs_symmetric_credential_key` для смены симметричного ключа хранилища учетных данных RBS, а секреты останутся теми же, что и до смены ключей.  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>См. также:  
[Удаленное хранилище больших двоичных объектов и группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
