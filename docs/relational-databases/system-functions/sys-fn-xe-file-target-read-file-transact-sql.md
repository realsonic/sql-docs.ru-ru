---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8af7ee0bc6c899026e51110264f2b28c2f579dd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096367"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Читает файлы, создаваемые асинхронным целевым файловым объектом расширенных событий. Возвращается одно событие в каждой строке в формате XML.  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] принимают результаты трассировки, созданные в формате XEL-и XEM. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Расширенные события поддерживают только результаты трассировки в формате XEL-. Для чтения результатов трассировки в формате XEL рекомендуется использовать SQL Server Management Studio.    
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>Аргументы  
 *путь*  
 Путь к файлам для чтения. *путь* может содержать подстановочные знаки и включает имя файла. *путь* имеет тип **nvarchar (260)**. Значение по умолчанию отсутствует. В контексте базы данных SQL Azure это значение является URL-адресом HTTP для файла в службе хранилища Azure.
  
 *мдпас*  
 Путь к файлу метаданных, который соответствует файлу или файлам, указанным в аргументе *path* . *мдпас* имеет тип **nvarchar (260)**. Значение по умолчанию отсутствует. Начиная с SQL Server 2016, этот параметр может быть задан как null.
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] не требуется параметр *мдпас* . Однако он используется для поддержки обратной совместимости фалов журналов, сформированных в предыдущих версиях SQL Server.  
  
 *initial_file_name*  
 Первый файл для чтения из *пути*. *initial_file_name* имеет тип **nvarchar (260)**. Значение по умолчанию отсутствует. Если в качестве аргумента указано **значение NULL** , считываются все файлы, найденные в *пути* .  
  
> [!NOTE]  
>  *initial_file_name* и *initial_offset* являются парными аргументами. При указании значения для любого из этих аргументов необходимо также указать значение для второго аргумента.  
  
 *initial_offset*  
 Используется для указания последнего считанного ранее смещения и пропуска всех событий до смещения (включительно). Перечисление событий начинается после указанного смещения. *initial_offset* имеет тип **bigint**. Если в качестве аргумента указано **значение NULL** , будет прочитан весь файл.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|Идентификатор GUID модуля событий. Не допускает значение NULL.|  
|package_guid|**uniqueidentifier**|Идентификатор GUID пакета событий. Не допускает значение NULL.|  
|object_name|**nvarchar(256)**|Имя события. Не допускает значение NULL.|  
|event_data|**nvarchar(max)**|Содержимое события в формате XML. Не допускает значение NULL.|  
|file_name|**nvarchar(260)**|Имя файла, содержащего событие. Не допускает значение NULL.|  
|file_offset|**bigint**|Смещение блока в файле, содержащем событие. Не допускает значение NULL.|  
|timestamp_utc|**datetime2**|**Применимо к**: [!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br />Дата и время (часовой пояс UTC) события. Не допускает значение NULL.|  

  
## <a name="remarks"></a>Комментарии  
 Чтение больших результирующих наборов путем исполнения **sys.fn_xe_file_target_read_file** в [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] может привести к ошибке. Используйте **результаты в файловый** режим (**CTRL + SHIFT + F**), чтобы экспортировать большие результирующие наборы в файл и прочитать файл с помощью другого средства.  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-retrieving-data-from-file-targets"></a>A. Извлечение данных из целевых файлов  
 В следующем примере извлекаются все строки из всех файлов. В этом примере целевые файлы и метафайлы расположены в папке трассировки на диске «C:\».  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления расширенных событий](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Представления каталога расширенных событий &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
  
