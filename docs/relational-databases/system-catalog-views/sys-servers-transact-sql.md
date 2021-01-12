---
description: sys.servers (Transact-SQL)
title: sys. Servers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- servers_TSQL
- sys.servers_TSQL
- servers
- sys.servers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.servers catalog view
ms.assetid: 4e774ed9-4e83-4726-9f1d-8efde8f9feff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 542db6b144471e2942bb160f1b91d7e2628ace69
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097918"
---
# <a name="sysservers-transact-sql"></a>sys.servers (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Содержит строку для каждого зарегистрированного или удаленного сервера и строку для локального сервера, имеющего **server_id** = 0.  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Локальный идентификатор связанного сервера.|  
|**name**|**sysname**|Если **server_id** = 0, возвращаемое значение является именем сервера.<br /><br /> Если **server_id** > 0, возвращаемое значение является локальным именем связанного сервера.|  
|**product**|**sysname**|Имя продукта связанного сервера. Значение "SQL Server" указывает на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**поставщики**|**sysname**|Имя поставщика OLE DB для соединения со связанным сервером.<br /><br />Начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] , значение SQLNCLI сопоставляется с [драйвером Microsoft OLE DB для SQL Server (мсоледбскл)](../../connect/oledb/oledb-driver-for-sql-server.md) по умолчанию. В более ранних версиях значение SQLNCLI сопоставляется с [поставщиком SQL Server Native Client OLE DB (SQLNCLI11)](../../relational-databases/native-client/sql-server-native-client.md).|  
|**data_source**|**nvarchar(4000)**|Свойство соединения источника данных OLE DB.|  
|**расположение**|**nvarchar(4000)**|Свойство соединения местоположения OLE DB. NULL — если нет.|  
|**provider_string**|**nvarchar(4000)**|Свойство соединения строки поставщика OLE DB.<br /><br /> Имеет значение NULL, если у вызывающего объекта нет `ALTER ANY LINKED SERVER` разрешения.|  
|**catalog**|**sysname**|Свойство подключения к каталогу OLE DB. NULL — если нет.|  
|**connect_timeout**|**int**|Время ожидания соединения в секундах; 0 — не указано.|  
|**query_timeout**|**int**|Время ожидания запроса в секундах; 0 — не указано.|  
|**is_linked**|**bit**|0 = это сервер со старым стилем, добавленный с помощью **sp_addserver**, с различными поведениями RPC и распределенных транзакций.<br /><br /> 1 = стандартный связанный сервер.|  
|**is_remote_login_enabled**|**bit**|Параметр RPC установлен на включение входящих удаленных имен входа для этого сервера.|  
|**is_rpc_out_enabled**|**bit**|Исходящие (от этого сервера) RPC включены.|  
|**is_data_access_enabled**|**bit**|Сервер включен для распределенных запросов.|  
|**is_collation_compatible**|**bit**|Параметры сортировки удаленных данных рассматриваются как совместимые с локальными данными, если нет доступных сведений о параметрах сортировки.|  
|**uses_remote_collation**|**bit**|При значении 1 используйте параметры сортировки, переданные удаленным сервером; в ином случае используйте параметры сортировки, указанные следующим столбцом.|  
|**collation_name**|**sysname**|Имя параметров сортировки, которые должны быть использованы, или NULL, если следует использовать локальные параметры сортировки.|  
|**lazy_schema_validation**|**bit**|При значении 1 проверка правильности схемы при запуске запроса не производится.|  
|**is_system**|**bit**|Доступ на этот сервер может получить только внутренняя система.|  
|**is_publisher**|**bit**|Сервер является издателем репликации.|  
|**is_subscriber**|**bit**|Сервер является подписчиком репликации.|  
|**is_distributor**|**bit**|Сервер является распространителем репликации.|  
|**is_nonsql_subscriber**|**bit**|Сервер является подписчиком репликации, отличным от SQL Server.|  
|**is_remote_proc_transaction_promotion_enabled**|**bit**|Если 1, вызов удаленной хранимой процедуры приводит к запуску распределенной транзакции и привлекает к выполнению транзакции MS DTC. Дополнительные сведения см. в статье [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md).|  
|**modify_date**|**datetime**|Дата последнего изменения сведений о сервере.|  
|**is_rda_server**|**bit**|**Применимо к:** Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] .<br /><br />Сервер является удаленным архивом данных включить (Stretch-Enabled). Дополнительные сведения см. в разделе [включение Stretch Database на сервере](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).|
  
## <a name="permissions"></a>Разрешения  
 Значение в **provider_string** всегда равно null, если у вызывающего объекта нет разрешения ALTER ANY Linked Server.  
  
 Для просмотра локального сервера разрешения не требуются (**server_id** = 0).  
  
 При создании связанного или удаленного сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает сопоставление имени входа по умолчанию с ролью сервера **Public** . Сопоставление имен входа по умолчанию означает, что все имена входа могут просматривать все связанные и удаленные серверы. Чтобы ограничить видимость этих серверов, удалите сопоставление имен входа по умолчанию, выполнив [sp_droplinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md) и УКАЗАВ значение NULL для параметра *локаллогин* .  
  
 Если сопоставление удалено, только те пользователи, которые добавлены явно со связанным или удаленным именем входа, могут просматривать связанные или удаленные сервера соответственно.  Следующие разрешения необходимы для просмотра всех связанных и удаленных серверов после сопоставления имени входа по умолчанию:  
  
- `ALTER ANY LINKED SERVER` либо `ALTER ANY LOGIN ON SERVER`  
- Членство в предопределенных ролях сервера **setupadmin** или **sysadmin**  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога связанных серверов &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addremotelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)  
  
