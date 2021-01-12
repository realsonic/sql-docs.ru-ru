---
description: sys.dm_filestream_non_transacted_handles (Transact-SQL)
title: sys.dm_filestream_non_transacted_handles (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_filestream_non_transacted_handles_TSQL
- dm_filestream_non_transacted_handles
- dm_filestream_non_transacted_handles_TSQL
- sys.dm_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_non_transacted_handles dynamic management view
ms.assetid: 507ec125-67dc-450a-9081-94cde5444a92
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6b5082f5ad2105d91c23d3d1cca3e959b01343a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097653"
---
# <a name="sysdm_filestream_non_transacted_handles-transact-sql"></a>sys.dm_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Показывает открытые в настоящее время нетранзакционные дескрипторы файлов, связанные с данными FileTable.  
  
 Это представление содержит по одной строке на каждый открытый дескриптор файла. Поскольку данные этого представления соответствуют активному внутреннему состоянию сервера, они постоянно изменяются по мере открытия и закрытия дескрипторов. Это представление не содержит данных предыстории.  
  
 Дополнительные сведения см. в статье [Управление таблицами FileTable](../../relational-databases/blob/manage-filetables.md).  
  
|**Столбец**|**Тип**|**Описание**|  
|----------------|--------------|---------------------|  
|database_id|INT|Идентификатор базы данных, связанной с дескриптором.|  
|object_id|INT|Идентификатор объекта таблицы FileTable, с которой связан дескриптор.|  
|handle_id|INT|Уникальный идентификатор контекста дескриптора. Используется [sp_kill_filestream_non_transacted_handlesом &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md) для уничтожения определенного дескриптора.|  
|file_object_type|INT|Тип дескриптора. Он указывает уровень иерархии, для которого был открыт дескриптор, такой как база данных или элемент.|  
|file_object_type_desc|nvarchar(120)|"Undefine",<br />"SERVER_ROOT",<br />"DATABASE_ROOT",<br />"TABLE_ROOT",<br />"TABLE_ITEM"|  
|correlation_process_id|varbinary(8)|Содержит уникальный идентификатор для процесса, отправившего запрос.|  
|correlation_thread_id|varbinary(8)|Содержит уникальный идентификатор для потока, отправившего запрос.|  
|file_context|varbinary(8)|Указатель на файловый объект, используемый данным дескриптором.|  
|Состояние|INT|Текущее состояние дескриптора. Может быть активным, закрытым или уничтоженным.|  
|state_desc|nvarchar(120)|"ACTIVE",<br />"ЗАКРЫТо",<br />ЗАВЕРШЕН|  
|current_workitem_type|INT|Состояние, в котором этот дескриптор обрабатывается в данный момент.|  
|current_workitem_type_desc|nvarchar(120)|"Носетворкитемтипе",<br />"Ффтпрекреатеворкитем",<br />"Ффтжетфисикалфиленамеворкитем",<br />"Ффтпосткреатеворкитем",<br />"Ффтпреклеанупворкитем",<br />"Ффтпостклеанупворкитем",<br />"Ффтпреклосеворкитем",<br />"Ффткуеридиректориворкитем",<br />"Ффткуеринфоворкитем",<br />"Ффткуериволумеинфоворкитем",<br />"Ффтсетинфоворкитем",<br />"Ффтвритекомплетионворкитем"|  
|fcb_id|BIGINT|Идентификатор блока управления файлами FileTable.|  
|item_id|varbinary (892)|Идентификатор элемента для файла или каталога. Может быть NULL для корневых дескрипторов сервера.|  
|is_directory|bit|Представляет ли собой каталог.|  
|item_name|nvarchar(512)|Имя элемента.|  
|opened_file_name|nvarchar(512)|Первоначально запрошенный путь для открытия.|  
|database_directory_name|nvarchar(512)|Часть opened_file_name, которая представляет имя каталога базы данных.|  
|table_directory_name|nvarchar(512)|Часть opened_file_name, которая представляет имя каталога таблицы.|  
|remaining_file_name|nvarchar(512)|Часть opened_file_name, которая представляет остальную часть имени каталога.|  
|open_time|DATETIME|Время открытия дескриптора.|  
|flags|INT|ShareFlagsUpdatedToFcb = 0x1,<br />DeleteOnClose = 0x2,<br />NewFile = 0x4,<br />PostCreateDoneForNewFile = 0x8,<br />StreamFileOverwritten = 0x10,<br />RequestCancelled = 0x20,<br />NewFileCreationRolledBack = 0x40|  
|login_id|INT|Идентификатор участника, открывшего дескриптор.|  
|login_name|nvarchar(512)|Имя участника, открывшего дескриптор.|  
|login_sid|varbinary(85)|Идентификатор безопасности участника, открывшего дескриптор.|  
|read_access|bit|Открыто для чтения.|  
|write_access|bit|Открыто для записи.|  
|delete_access|bit|Открыто для удаления.|  
|share_read|bit|Открыто с разрешением share_read.|  
|share_write|bit|Открыто с разрешением share_write.|  
|share_delete|bit|Открыто с разрешением share_delete.|  
  
## <a name="see-also"></a>См. также:  
 [Управление таблицами FileTable](../../relational-databases/blob/manage-filetables.md)  
  
  
