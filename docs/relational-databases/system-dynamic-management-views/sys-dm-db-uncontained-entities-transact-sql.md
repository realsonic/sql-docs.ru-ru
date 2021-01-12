---
description: sys.dm_db_uncontained_entities (Transact-SQL)
title: sys.dm_db_uncontained_entities (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_uncontained_entities
- dm_db_uncontained_entities_TSQL
- sys.dm_db_uncontained_entities_TSQL
- dm_db_uncontained_entities
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_uncontained_entities dynamic management view
ms.assetid: f417efd4-8c71-4f81-bc9c-af13bb4b88ad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 54b087c98071ea550fcdff630a93d8049188ea91
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099906"
---
# <a name="sysdm_db_uncontained_entities-transact-sql"></a>sys.dm_db_uncontained_entities (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Показывает все неавтономные объекты, которые используются в базе данных. Неавтономные объекты пересекают границу базы данных в автономной базе данных. Представление доступно как из автономной базы данных, так и из неавтономной базы данных. Если представление sys.dm_db_uncontained_entities пусто, база данных не использует неавтономные сущности.  
  
 Если модуль пересекает границу базы данных несколько раз, то в отчете указывается только первое пересечение границы.  
  
||||  
|-|-|-|  
|**Имя столбца**|**Тип**|**Описание**|  
|*class*|**int**|1 = объект или столбец (включая модули, XP, представления, синонимы и таблицы).<br /><br /> 4 = Участник базы данных<br /><br /> 5 = Сборка<br /><br /> 6 = Тип<br /><br /> 7 = Индекс (полнотекстовый индекс)<br /><br /> 12 = Триггер DDL базы данных<br /><br /> 19 = Маршрут<br /><br /> 30 = Спецификация аудита|  
|*class_desc*|**nvarchar(120)**|Описание класса сущности. Один из следующих элементов для сопоставления с классом:<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **DATABASE_PRINCIPAL**<br /><br /> **СБОРОК**<br /><br /> **TYPE**<br /><br /> **INDEX**<br /><br /> **DATABASE_DDL_TRIGGER**<br /><br /> **НАПРАВЛЕНЫ**<br /><br /> **AUDIT_SPECIFICATION**|  
|*major_id*|**int**|Идентификатор сущности.<br /><br /> Если *класс* = 1, то object_id<br /><br /> Если *класс* = 4, то sys. database_principals. principal_id.<br /><br /> Если *класс* = 5, то sys.assemblies.assembly_id.<br /><br /> Если *класс* = 6, то sys.types.user_type_id.<br /><br /> Если *Class* = 7, то sys.indexes.index_id.<br /><br /> Если *Class* = 12, то sys.triggers.object_id.<br /><br /> Если *класс* = 19, то sys.routes.route_id.<br /><br /> Если *Class* = 30, то sys. database_audit_specifications.database_specification_id database_audit_specifications.|  
|*statement_line_number*|**int**|Если класс является модулем, возвращает номер строки, в которой используется неавтономная инструкция.  В противном случае — значение NULL.|  
|*statement_ offset_begin*|**int**|Если класс является модулем, он указывает (в байтах, начиная с 0) положение, откуда начинается неавтономная инструкция. В противном случае возвращается значение null.|  
|*statement_ offset_end*|**int**|Если класс является модулем, он указывает (в байтах, начиная с 0) положение, где заканчивается неавтономная инструкция. Значение -1 обозначает конец модуля. В противном случае возвращается значение null.|  
|*statement_type*|**nvarchar(512)**|Тип инструкции.|  
|*имя feature_*|**nvarchar(256)**|Возвращает внешнее имя объекта.|  
|*feature_type_name*|**nvarchar(256)**|Возвращает тип функции.|  
  
## <a name="remarks"></a>Комментарии  
 sys.dm_db_uncontained_entities показывает эти сущности, которые потенциально могут пересекать границу базы данных. Будут возвращены все сущности пользователей, которые могут использовать объекты за пределами базы данных.  
  
 В отчете указываются следующие типы функций.  
  
-   Неизвестный режим включения (динамическое SQL или отложенное разрешение имени)  
  
-   Команда DBCC  
  
-   Системная хранимая процедура  
  
-   Системная скалярная функция  
  
-   Системная функция, возвращающая табличное значение  
  
-   Встроенная системная функция  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 sys.dm_db_uncontained_entities возвращает только объекты, для которых у пользователя имеются определенные типы разрешения. Чтобы полностью оценить включение базы данных, эта функция должна использоваться привилегированным пользователем, например членом предопределенной роли сервера **sysadmin** или ролью **db_owner** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается процедура с именем P1, а затем отправляется запрос `sys.dm_db_uncontained_entities`. В отчете запроса указывается использование процедурой P1 представления **sys.endpoints** , находящегося за пределами базы данных.  
  
```sql  
CREATE DATABASE Test;  
GO  
  
USE Test;  
GO  
CREATE PROC P1  
AS   
SELECT * FROM sys.endpoints ;  
GO  
SELECT SO.name, UE.* FROM sys.dm_db_uncontained_entities AS UE  
LEFT JOIN sys.objects AS SO  
    ON UE.major_id = SO.object_id;  
```  
  
## <a name="see-also"></a>См. также:  
 [Автономные базы данных](../../relational-databases/databases/contained-databases.md)  
  
  
