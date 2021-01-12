---
description: sys.dm_filestream_file_io_requests (Transact-SQL)
title: sys.dm_filestream_file_io_requests (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_filestream_file_io_requests
- dm_filestream_file_io_requests
- sys.dm_filestream_file_io_requests_TSQL
- dm_filestream_file_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_requests catalog view
ms.assetid: d41e39a5-14d5-4f3d-a2e3-a822b454c1ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 69b34fd2a8ec5adf393994399011cf1b2df6ea03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097676"
---
# <a name="sysdm_filestream_file_io_requests-transact-sql"></a>sys.dm_filestream_file_io_requests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Выводит список запросов ввода-вывода, обрабатываемых в данный момент владельцем пространства имен (NSO).  
  
|Столбец|Type|Описание|  
|------------|----------|-----------------|  
|**request_context_address**|**varbinary(8)**|Показывает внутренний адрес блока памяти NSO, содержащего запрос ввода-вывода от драйвера. Не допускает значение NULL.|  
|**current_spid**|**smallint**|Показывает идентификатор системного процесса (SPID) для соединения текущего SQL Server. Не допускает значение NULL.|  
|**request_type**|**nvarchar(60)**|Показывает тип пакета запроса ввода-вывода (IRP). Возможные типы запросов: REQ_PRE_CREATE, REQ_POST_CREATE, REQ_RESOLVE_VOLUME, REQ_GET_VOLUME_INFO, REQ_GET_LOGICAL_NAME, REQ_GET_PHYSICAL_NAME, REQ_PRE_CLEANUP, REQ_POST_CLEANUP, REQ_CLOSE, REQ_FSCTL, REQ_QUERY_INFO, REQ_SET_INFO, REQ_ENUM_DIRECTORY, REQ_QUERY_SECURITY и REQ_SET_SECURITY. Не допускает значение NULL.|  
|**request_state**|**nvarchar(60)**|Показывает состояние запроса ввода-вывода в NSO. Возможные значения: REQ_STATE_RECEIVED, REQ_STATE_INITIALIZED, REQ_STATE_ENQUEUED, REQ_STATE_PROCESSING, REQ_STATE_FORMATTING_RESPONSE, REQ_STATE_SENDING_RESPONSE, REQ_STATE_COMPLETING и REQ_STATE_COMPLETED. Не допускает значение NULL.|  
|**request_id**|**int**|Показывает уникальный идентификатор, назначенный драйвером этому запросу. Не допускает значение NULL.|  
|**irp_id**|**int**|Показывает уникальный идентификатор IRP. Это удобно для определения всех запросов ввода-вывода, связанных с заданным IRP. Не допускает значение NULL.|  
|**handle_id**|**int**|Показывает идентификатор дескриптора пространства имен. Этот идентификатор зависит от NSO и уникален в пределах экземпляра. Не допускает значение NULL.|  
|**client_thread_id**|**varbinary(8)**|Показывает идентификатор потока клиентского приложения, который является источником запроса.<br /><br /> Предупреждение это имеет смысл только в том случае, если клиентское приложение работает на том же компьютере, что и SQL Server. **\* \* \* \*** Когда клиентское приложение выполняется удаленно, **client_thread_id** показывает идентификатор потока процесса, который работает от имени удаленного клиента.<br /><br /> Допускает значение NULL.|  
|**client_process_id**|**varbinary(8)**|Показывает идентификатор процесса клиентского приложения, если оно работает на одном компьютере с SQL Server. Для удаленного клиента здесь показывается идентификатор системного процесса, который работает от имени клиентского приложения. Допускает значение NULL.|  
|**handle_context_address**|**varbinary(8)**|Показывает адрес внутренней структуры NSO, связанной с маркером клиента. Допускает значение NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Показывает идентификатор транзакции, связанной с заданным дескриптором, и все запросы, связанные с этим дескриптором. Это значение, возвращаемое функцией **GET_FILESTREAM_TRANSACTION_CONTEXT** . Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления FILESTREAM и FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
