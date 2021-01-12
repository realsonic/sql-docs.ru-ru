---
description: sys.dm_database_encryption_keys (Transact-SQL)
title: sys.dm_database_encryption_keys (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_database_encryption_keys
- sys.dm_database_encryption_keys_TSQL
- dm_database_encryption_keys
- dm_database_encryption_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_database_encryption_keys dynamic management view
ms.assetid: 56fee8f3-06eb-4fff-969e-abeaa0c4b8e4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9eb56a2c7f2708a46cc0316e1c2600e2f15f0f8e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092942"
---
# <a name="sysdm_database_encryption_keys-transact-sql"></a>sys.dm_database_encryption_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения о состоянии шифрования базы данных и о связанных ключах шифрования базы данных. Дополнительные сведения о шифровании баз данных см. в статье [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Идентификатор базы данных.|  
|encryption_state|**int**|Указывает, является ли база данных зашифрованной или незашифрованной.<br /><br /> 0 = нет ключа шифрования базы данных, нет шифрования<br /><br /> 1 = не зашифрована<br /><br /> 2 = выполняется шифрование<br /><br /> 3 = зашифрована<br /><br /> 4 = выполняется изменение ключа<br /><br /> 5 = выполняется расшифровка<br /><br /> 6 = производится изменение защиты (изменился сертификат или асимметричный ключ, которым зашифрован ключ шифрования базы данных).|  
|create_date|**datetime**|Отображает дату (в формате UTC) создания ключа шифрования.|  
|regenerate_date|**datetime**|Отображает дату (в формате UTC) повторного создания ключа шифрования.|  
|modify_date|**datetime**|Отображает дату (в формате UTC) изменения ключа шифрования.|  
|set_date|**datetime**|Отображает дату (в формате UTC) того, что ключ шифрования был применен к базе данных.|  
|opened_date|**datetime**|Показывает время последнего открытия ключа базы данных (в формате UTC).|  
|key_algorithm|**nvarchar(32)**|Отображает алгоритм, используемый для ключа.|  
|key_length|**int**|Отображает длину ключа.|  
|encryptor_thumbprint|**varbinary(20)**|Показывает отпечаток шифратора.|  
|encryptor_type|**nvarchar(32)**|**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).<br /><br /> Описывает шифратор.|  
|percent_complete|**real**|Процент выполнения шифрования базы данных. Значение 0, если изменения состояния не было.|
|encryption_state_desc|**nvarchar(32)**|**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий.<br><br> Строка, указывающая, является ли база данных зашифрованной или не зашифрованной.<br><br>NONE<br><br>НЕЗАШИФРОВАННЫЕ<br><br>Шифрование<br><br>DECRYPTION_IN_PROGRESS<br><br>ENCRYPTION_IN_PROGRESS<br><br>KEY_CHANGE_IN_PROGRESS<br><br>PROTECTION_CHANGE_IN_PROGRESS|
|encryption_scan_state|**int**|**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий.<br><br>Указывает текущее состояние сканирования шифрования. <br><br>0 = проверка не инициирована, TDE не включен<br><br>1 = выполняется сканирование.<br><br>2 = Проверка выполняется, но приостановлена, пользователь может возобновить работу.<br><br>3 = сканирование было прервано по какой-то причине, требуется вмешательство вручную. Для получения дополнительной помощи обратитесь в служба поддержки Майкрософт.<br><br>4 = сканирование успешно завершено, TDE включен и шифрование завершено.|
|encryption_scan_state_desc|**nvarchar(32)**|**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий.<br><br>Строка, указывающая текущее состояние сканирования шифрования.<br><br> NONE<br><br>RUNNING<br><br>SUSPENDED<br><br>ABORTED<br><br>ЗАВЕРШЕНИЯ|
|encryption_scan_modify_date|**datetime**|**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий.<br><br> Отображает дату (в формате UTC) последнего изменения состояния проверки шифрования.|
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  

 [Динамические административные представления и функции, связанные с безопасностью (Transact-SQL)](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [Шифрование SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Ключи шифрования базы данных и SQL Server (ядро СУБД)](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)  
  
