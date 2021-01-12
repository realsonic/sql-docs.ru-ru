---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d8e8a6590dedf9c43da14147c503d1e62423e595
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096710"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Это представление каталога содержит по одной строке на каждую конечную точку компонента Service Broker. Для каждой строки в этом представлении имеется соответствующая строка с тем же **endpoint_id** в представлении **sys.tcp_endpoints** , которое содержит метаданные конфигурации TCP. TCP — единственный разрешенный протокол для компонента Service Broker.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Наследует столбцы из представления [sys. endpoints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_message_forwarding_enabled**|**bit**|Осуществляет пересылку сообщений поддержки конечных точек. Изначально это значение равно **0** (отключено). Не допускает значения NULL.|  
|**message_forwarding_size**|**int**|Максимальное число мегабайт пространства **tempdb** , которое может использоваться для перенаправляемых сообщений. Изначально это значение равно **10**. Не допускает значения NULL.|  
|**connection_auth**|**tinyint**|Тип проверки подлинности соединения, требуемый при подключении к этой конечной точке, один из следующих.<br /><br /> **1** — NTLM<br /><br /> **2** — Kerberos<br /><br /> **3** . согласование<br /><br /> **4** . сертификат<br /><br /> **5** — NTLM, сертификат<br /><br /> **6** — Kerberos, сертификат<br /><br /> **7** . согласование, сертификат<br /><br /> **8** — сертификат, NTLM<br /><br /> **9** — сертификат, Kerberos<br /><br /> **10** — сертификат, согласование<br /><br /> Не допускает значения NULL.|  
|**connection_auth_desc**|**nvarchar(60)**|Описание типа проверки подлинности, необходимого для соединения с данной конечной точкой, одно из следующих:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE<br /><br /> Допускает значение NULL.|  
|**certificate_id**|**int**|Идентификатор сертификата, используемого для проверки подлинности (если таковой имеется).<br /><br /> 0 = используется проверка подлинности Windows.|  
|**encryption_algorithm**|**tinyint**|Алгоритм шифрования. Ниже приведены возможные значения с описаниями и соответствующими параметрами DDL.<br /><br /> **0** : нет. Соответствующий параметр DDL: отключен.<br /><br /> **1** : RC4. Соответствующий параметр DDL: {required &#124; требуемый алгоритм RC4}.<br /><br /> **2** : AES. Соответствующий параметр DDL: требуется алгоритм AES.<br /><br /> **3** : нет, RC4. Соответствующий параметр DDL: {Supported &#124; поддерживаемый алгоритм RC4}.<br /><br /> **4** : нет, AES. Соответствующий параметр DDL: поддерживаемый алгоритм AES.<br /><br /> **5** : RC4, AES. Соответствующий параметр DDL: требуется алгоритм RC4 AES.<br /><br /> **6** : AES, RC4. Соответствующий параметр DDL: требуется алгоритм AES RC4.<br /><br /> **7** : нет, RC4, AES. Соответствующий параметр DDL: поддерживаемый алгоритм RC4 AES.<br /><br /> **8** : нет, AES, RC4. Соответствующий параметр DDL: поддерживаемый алгоритм AES RC4.<br /><br /> Не допускает значения NULL.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Описание алгоритма шифрования. Ниже перечислены возможные значения и соответствующие параметры DDL.<br /><br /> Нет: отключено<br /><br /> RC4: {требуется &#124; требуемый алгоритм RC4}<br /><br /> AES: требуемый алгоритм AES<br /><br /> НЕТ, RC4: {Supported &#124; поддерживаемый алгоритм RC4}<br /><br /> НЕТ, AES: поддерживаемый алгоритм AES<br /><br /> RC4, AES: требуемый алгоритм RC4 AES<br /><br /> AES, RC4: требуемый алгоритм AES RC4<br /><br /> НЕТ, RC4, AES: поддерживаемый алгоритм RC4 AES<br /><br /> NONE, AES, RC4: поддерживаемый алгоритм AES RC4<br /><br /> Допускает значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
