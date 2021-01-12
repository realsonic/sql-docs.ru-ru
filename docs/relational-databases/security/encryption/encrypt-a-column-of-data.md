---
title: Шифрование столбца данных | Документация Майкрософт
description: Сведения о том, как зашифровать столбец данных с помощью симметричного шифрования в SQL Server, используя Transact-SQL, что иногда называют шифрованием на уровне столбцов или ячеек.
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: c09f7f91edf2cada0464b6cfbc1922664a86866f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641135"
---
# <a name="encrypt-a-column-of-data"></a>Шифрование столбца данных

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

В этой статье описывается шифрование столбца данных симметричным шифрованием в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Иногда это называется шифрованием на уровне столбца или ячейки. Эта функция доступна в виде предварительной версии для Azure Synapse Analytics.

Примеры в этой статье проверены с помощью AdventureWorks2017. Чтобы получить примеры баз данных, см. статью [Примеры баз данных AdventureWorks](../../../samples/adventureworks-install-configure.md).

## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  

Для выполнения приведенных ниже шагов необходимы следующие разрешения.  
  
- Разрешение `CONTROL` в базе данных.  
- Разрешение `CREATE CERTIFICATE` в базе данных. Сертификаты могут принадлежать только именам входа Windows, именам входа SQL Server и ролям приложений. Сертификаты не могут принадлежать группам и ролям.  
- Разрешение `ALTER` в базе данных.  
- Некоторые разрешения применяются к ключу; также не должно быть запрета на разрешение `VIEW DEFINITION`.  
  
## <a name="create-database-master-key"></a>Создание главного ключа базы данных  

Для применения приведенных ниже примеров требуется главный ключ базы данных. Если в базе данных еще нет главного ключа базы данных, создайте его. Для этого подключитесь к базе данных и выполните следующий скрипт. Обязательно используйте сложный пароль.

Скопируйте и вставьте следующий пример в окно запроса, подключенное к примеру базы данных AdventureWorks. Нажмите кнопку **Выполнить**.  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

Всегда создавайте резервную копию главного ключа базы данных. Дополнительные сведения о создании главных ключей баз данных см. в статье [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>Пример Шифрование с помощью симметричного шифрования и структуры проверки подлинности
  
1. В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. На стандартной панели выберите пункт **Создать запрос**.  
  
3. Скопируйте и вставьте следующий пример в окно запроса, подключенное к примеру базы данных AdventureWorks. Нажмите кнопку **Выполнить**.

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>Шифрование с помощью простого симметричного шифрования  

1. В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. На стандартной панели выберите пункт **Создать запрос**.  
  
3. Скопируйте и вставьте следующий пример в окно запроса, подключенное к примеру базы данных AdventureWorks. Нажмите кнопку **Выполнить**.  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 Дополнительные сведения см. в следующих разделах:  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
