---
description: sys.dm_db_objects_impacted_on_version_change (база данных SQL Azure)
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: f7830e874026b9d5af250fde294c8c72294d013d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097704"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Это системное представление уровня базы данных предназначено для отображения ранних предупреждений, позволяющих определить объекты, которые затрагиваются при обновлении выпуска в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Можно использовать это представление до или после обновления для получения полного перечисления затронутых объектов. Чтобы запросить полный отчет для всего сервера, потребуется запросить это представление в каждой базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|class|**тип int** НЕ NULL|Класс объекта, который будет затронут:<br /><br /> **1** = ограничение<br /><br /> **7** = кучи и индексы|  
|class_desc|**nvarchar (60)** НЕ NULL|Описание класса:<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|**тип int** НЕ NULL|Код объекта ограничения или код объекта таблицы, содержащей индекс или кучу.|  
|minor_id|**тип int** ЗАКАНЧИВАЮЩ|значение **NULL** для ограничений<br /><br /> Index_id для индексов и куч|  
|dependency|**nvarchar (60)** НЕ NULL|Описание зависимости, которая вызывает применение затрагиваемого ограничения или индекса. Такое же значение используется для предупреждений, созданных во время обновления.<br /><br /> Примеры:<br /><br /> Значение **space** (для встроенной функции)<br /><br /> **geometry** (для системного, определяемого пользователем типа)<br /><br /> **geography::Parse** (для системного, определяемого пользователем метода)|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE.  
  
## <a name="example"></a>Пример  
 В следующем примере показан запрос к представлению **sys.dm_db_objects_impacted_on_version_change**, применяемый для поиска объектов, подверженных влиянию обновления с переходом на следующую основную версию сервера  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>Комментарии  
  
### <a name="how-to-update-impacted-objects"></a>Обновление затрагиваемых объектов  
 Далее описывается порядок действий по исправлению после обновления набора исправлений, которое будет доступно в июне.  
  
|Порядок|Затрагиваемый объект|Действие по исправлению|  
|-----------|---------------------|-----------------------|  
|1|**Индексы**|Перестройте любой индекс, определенный **sys.dm_db_objects_impacted_on_version_change** например:  `ALTER INDEX ALL ON <table> REBUILD`<br />или диспетчер конфигурации служб<br />`ALTER TABLE <table> REBUILD`|  
|2|**Объект**|Все ограничения, обозначенные как **sys.dm_db_objects_impacted_on_version_change**, должны быть еще раз проверены после повторного вычисления данных типа Geometry и Geography в базовой таблице. Для ограничений выполните проверку с помощью инструкции ALTER TABLE. <br />Пример: <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />или диспетчер конфигурации служб<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  
