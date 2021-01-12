---
description: sys.key_encryptions (Transact-SQL)
title: sys.key_encryptions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.key_encryptions
- key_encryptions_TSQL
- sys.key_encryptions_TSQL
- key_encryptions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.key_encryptions catalog view
ms.assetid: c39cecf8-af63-40b9-98e5-f84a5bf3ae54
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3210dabc99d3bdd80a6cd274f259e5027fe83dec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094539"
---
# <a name="syskey_encryptions-transact-sql"></a>sys.key_encryptions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает по одной строке для каждого симметричного ключа шифрования, созданного с помощью инструкции CREATE SYMMETRIC KEY с предложением ENCRYPTION BY.  

  
|Имена столбцов|Типы данных|Описание|  
|------------------|----------------|-----------------|  
|**key_id**|**int**|Идентификатор зашифрованного ключа.|  
|**отпечатк**|**varbinary(32)**|Хэш SHA-1 сертификата, с помощью которого был зашифрован ключ, или идентификатор GUID симметричного ключа, которым был зашифрован этот ключ.|  
|**crypt_type**|**char (4)**|Тип шифрования:<br /><br /> ESKS = зашифровано симметричным ключом<br /><br /> ЕСКП, ESP2 или ESP3 = зашифровано паролем<br /><br /> EPUC = зашифровано сертификатом<br /><br /> EPUA = зашифровано асимметричным ключом<br /><br /> ESKM = зашифровано главным ключом|  
|**crypt_type_desc**|**nvarchar(60)**|Описание типа шифрования:<br /><br /> ENCRYPTION BY SYMMETRIC KEY<br /><br /> ENCRYPTION BY PASSWORD <br />(Начиная с [!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)] , включает номер версии для использования в CSS.)<br /><br /> ENCRYPTION BY CERTIFICATE<br /><br /> ENCRYPTION BY ASYMMETRIC KEY<br /><br /> ENCRYPTION BY MASTER KEY<br /><br /> Примечание. Windows DPAPI используется для защиты главного ключа службы.|  
|**crypt_property**|**varbinary(max)**|Подписанные или зашифрованные биты.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
