---
description: sys.database_mirroring_endpoints (Transact-SQL)
title: sys.database_mirroring_endpoints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e70bd703e258d8605563119e41617985bb0b961
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093208"
---
# <a name="sysdatabase_mirroring_endpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой конечной точки зеркального отображения базы данных экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Конечная точка зеркального отображения базы данных поддерживает как сеансы между участниками зеркального отображения базы данных, так и следящих серверов и сеансы между первичной репликой группы доступности Always On и ее вторичными репликами.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|-|Наследует столбцы из представления **sys. Endpoints** (Дополнительные сведения см [. в разделе sys. Endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)).|  
|**role**|**tinyint**|Роль в зеркальном отображении, одна из следующих:<br /><br /> **0** = нет<br /><br /> **1** = партнер<br /><br /> **2** = следящий сервер<br /><br /> **3** = все<br /><br /> Примечание. это значение относится только к зеркальному отображению базы данных.|  
|**role_desc**|**nvarchar(60)**|Описание роли в зеркальном отображении, одно из следующих.<br /><br /> **NONE**<br /><br /> **Майкрософт**<br /><br /> **-**<br /><br /> **ALL**<br /><br /> Примечание. это значение относится только к зеркальному отображению базы данных.|  
|**is_encryption_enabled**|**bit**|**1** означает, что шифрование включено.<br /><br /> значение **0** означает, что шифрование отключено.|  
|**connection_auth**|**tinyint**|Тип проверки подлинности соединения, требуемый при подключении к этой конечной точке, один из следующих.<br /><br /> **1** — NTLM<br /><br /> **2** — Kerberos<br /><br /> **3** . согласование<br /><br /> **4** . сертификат<br /><br /> **5** — NTLM, сертификат<br /><br /> **6** — Kerberos, сертификат<br /><br /> **7** . согласование, сертификат<br /><br /> **8** — сертификат, NTLM<br /><br /> **9** — сертификат, Kerberos<br /><br /> **10** — сертификат, согласование|  
|**connection_auth_desc**|**Nvarchar (60)**|Описание типа проверки подлинности, необходимого для подключения к конечной точке. Это один из следующих типов.<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE|  
|**certificate_id**|**int**|Идентификатор сертификата, используемого для проверки подлинности (если таковой имеется).<br /><br /> 0 = используется проверка подлинности Windows.|  
|**encryption_algorithm**|**tinyint**|Алгоритм шифрования. Это один из следующих алгоритмов.<br /><br /> **0** — нет<br /><br /> **1** — RC4<br /><br /> **2** — AES<br /><br /> **3** — нет, RC4<br /><br /> **4** — нет, AES<br /><br /> **5** — RC4, AES<br /><br /> **6** — AES, RC4<br /><br /> **7** — нет, RC4, AES<br /><br /> **8** — нет, AES, RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Описание алгоритма шифрования. Это один из следующих алгоритмов.<br /><br /> NONE<br /><br /> RC4;<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>Комментарии  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Укажите URL-адрес конечной точки при добавлении или изменении &#40;реплики доступности SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
