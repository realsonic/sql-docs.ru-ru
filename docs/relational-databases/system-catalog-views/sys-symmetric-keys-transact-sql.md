---
description: sys.symmetric_keys (Transact-SQL)
title: sys.symmetric_keys (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- symmetric_keys
- sys.symmetric_keys
- sys.symmetric_keys_TSQL
- symmetric_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.symmetric_keys catalog view
ms.assetid: d410eae1-3a52-45de-b9a1-52d2bd93a8eb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: de9933199a37f3b5acd071fbfbe9354173247885
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093047"
---
# <a name="syssymmetric_keys-transact-sql"></a>sys.symmetric_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждого симметричного ключа, созданного инструкцией CREATE SYMMETRIC KEY.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя ключа. Уникально в пределах базы данных.|  
|**principal_id**|**int**|Идентификатор участника базы данных, который владеет ключом.|  
|**symmetric_key_id**|**int**|Идентификатор ключа. Уникально в пределах базы данных.|  
|**key_length**|**int**|Длина ключа в битах.|  
|**key_algorithm**|**char(2)**|Алгоритм, используемый с ключом:<br /><br /> R2 = RC2<br /><br /> R4 = RC4<br /><br /> D = DES<br /><br /> D3 = тройной DES<br /><br /> DT = TRIPLE_DES_3KEY<br /><br /> DX = DESX<br /><br /> A1 = AES 128<br /><br /> A2 = AES 192<br /><br /> A3 = AES 256<br /><br /> NA = EKM Key|  
|**algorithm_desc**|**nvarchar(60)**|Описание алгоритма, используемого с ключом:<br /><br /> RC2<br /><br /> RC4;<br /><br /> DES<br /><br /> Triple_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256<br /><br /> NULL (только алгоритмы расширенного управления ключами)|  
|**create_date**|**datetime**|Дата создания ключа.|  
|**modify_date**|**datetime**|Дата изменения ключа.|  
|**key_guid**|**uniqueidentifier**|Глобальный уникальный идентификатор (GUID), ассоциированный с ключом. Он формируется автоматически для материализованного ключа. Идентификаторы GUID для временных ключей наследуются из указанной пользователем парольной фразы.|  
|**key_thumbprint**|**sql_variant**|Хэш ключа SHA-1. Хэш глобально уникален. Для ключей, не относящихся к системе расширенного управления ключами, это значение будет равно NULL.|  
|**provider_type**|**nvarchar(120)**|Тип поставщика служб шифрования.<br /><br /> CRYPTOGRAPHIC PROVIDER = ключи системы расширенного управления ключами<br /><br /> NULL = ключи, не относящиеся к расширенному управлению ключами|  
|**cryptographic_provider_guid**|**uniqueidentifier**|Идентификатор GUID поставщика служб шифрования. Для ключей, не относящихся к системе расширенного управления ключами, это значение будет равно NULL.|  
|**cryptographic_provider_algid**|**sql_variant**|Идентификатор алгоритма для поставщика служб шифрования. Для ключей, не относящихся к системе расширенного управления ключами, это значение будет равно NULL.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Комментарии  
 Алгоритм RC4 устарел. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
  
 **Пояснение к алгоритмам DES:**  
  
-   DESX был именован неправильно. Симметричные ключи, созданные с параметром ALGORITHM = DESX, в действительности используют шифр TRIPLE DES с 192-битным ключом. Алгоритм DESX не предоставляется. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES_3KEY, используют шифр TRIPLE DES с 192-битным ключом.  
  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES, используют шифр TRIPLE DES с 128-битным ключом.  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
