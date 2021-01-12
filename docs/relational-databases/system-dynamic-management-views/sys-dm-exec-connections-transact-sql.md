---
description: sys.dm_exec_connections (Transact-SQL)
title: sys.dm_exec_connections (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_connections_TSQL
- sys.dm_exec_connections_TSQL
- sys.dm_exec_connections
- dm_exec_connections
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_connections dynamic management view
ms.assetid: 6bd46fe1-417d-452d-a9e6-5375ee8690d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 89d36c36c0d1e32bd8da68db6847dde61dc6a7a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101626"
---
# <a name="sysdm_exec_connections-transact-sql"></a>sys.dm_exec_connections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения о соединениях, установленных с данным экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и подробные сведения о каждом соединении. Возвращает сведения о подключении к серверу для SQL Server. Возвращает сведения о текущем подключении базы данных для базы данных SQL.  
  
> [!NOTE]
> Чтобы вызвать этот метод из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте [sys.dm_pdw_exec_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Идентифицирует сеанс, связанный с данным соединением. Допускает значение NULL.|  
|most_recent_session_id|**int**|Представляет собой идентификатор сеанса самого последнего запроса, связанного с данным соединением. (SOAP-соединения могут повторно использоваться другим сеансом.) Допускает значение null.|  
|connect_time|**datetime**|Метка времени установления соединения. Не допускает значение NULL.|  
|net_transport|**nvarchar(40)**|Всегда возвращает **Session** , если для соединения включено несколько активных результирующих НАБОРОВ (MARS).<br /><br /> **Примечание.** Описывает физический транспортный протокол, используемый этим соединением. Не допускает значение NULL.|  
|protocol_type|**nvarchar(40)**|Указывает тип протокола передачи полезных данных. В настоящее время различаются протоколы TDS (TSQL) и SOAP. Допускает значение NULL.|  
|protocol_version|**int**|Версия протокола доступа к данным, связанного с данным соединением. Допускает значение NULL.|  
|endpoint_id|**int**|Идентификатор, описывающий тип соединения. Этот идентификатор endpoint_id может использоваться для запросов к представлению sys.endpoints. Допускает значение NULL.|  
|encrypt_option|**nvarchar(40)**|Логическое значение, указывающее, разрешено ли шифрование для данного соединения. Не допускает значение NULL.|  
|auth_scheme|**nvarchar(40)**|Указывает схему проверки подлинности ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или Windows), используемую с данным соединением. Не допускает значение NULL.|  
|node_affinity|**smallint**|Идентифицирует узел памяти, которому соответствует данное соединение. Не допускает значение NULL.|  
|num_reads|**int**|Число операций чтения байтов, произошедших через это соединение. Допускает значение NULL.|  
|num_writes|**int**|Число байтов операций записи, произошедших через это соединение. Допускает значение NULL.|  
|last_read|**datetime**|Метка времени о последнем полученном пакете данных. Допускает значение NULL.|  
|last_write|**datetime**|Метка времени о последнем отправленном пакете данных. Не допускает значения NULL.|  
|net_packet_size|**int**|Размер сетевого пакета, используемый для передачи данных. Допускает значение NULL.|  
|client_net_address|**varchar(48)**|Сетевой адрес удаленного клиента. Допускает значение NULL.<br /><br /> До версии 12 в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] этот столбец всегда возвращает значение NULL.|  
|client_tcp_port|**int**|Номер порта на клиентском компьютере, который используется при осуществлении соединения. Допускает значение NULL.<br /><br /> В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] этот столбец всегда возвращает NULL.|  
|local_net_address|**varchar(48)**|IP-адрес сервера, с которым установлено данное соединение. Доступен только для соединений, которые в качестве транспорта данных используют протокол TCP. Допускает значение NULL.<br /><br /> В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] этот столбец всегда возвращает NULL.|  
|local_tcp_port|**int**|TCP-порт сервера, если соединение использует протокол TCP. Допускает значение NULL.<br /><br /> В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] этот столбец всегда возвращает NULL.|  
|connection_id|**uniqueidentifier**|Однозначно определяет каждое соединение. Не допускает значение NULL.|  
|parent_connection_id|**uniqueidentifier**|Идентифицирует первичное соединение, используемое в сеансе режима MARS. Допускает значение NULL.|  
|most_recent_sql_handle|**varbinary (64)**|Дескриптор последнего запроса SQL, выполненного с помощью данного соединения. Постоянно проводится синхронизация между столбцом most_recent_sql_handle и столбцом most_recent_session_id. Допускает значение NULL.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="physical-joins"></a>Физические соединения  
 ![Соединения для sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-exec-connections-1.gif "Соединения для sys.dm_exec_connections")  
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
| Первый элемент | Второй элемент | Relationship |
| --------------| -------------- | ------------ |  
|dm_exec_sessions.session_id|dm_exec_connections.session_id|"Одна к одной"|  
|dm_exec_requests.connection_id|dm_exec_connections.connection_id|Многие к одному|  
|dm_broker_connections.connection_id|dm_exec_connections.connection_id|Один к одному|  
  
## <a name="examples"></a>Примеры  
 Типичный запрос для сбора сведений о собственном соединении запросов.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>См. также:  

 [Динамические административные представления и функции, связанные с выполнением (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


