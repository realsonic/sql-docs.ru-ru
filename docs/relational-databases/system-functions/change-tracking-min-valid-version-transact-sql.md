---
description: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 81e386ef391283f3a3f3095b7dd321055a676332
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093912"
---
# <a name="change_tracking_min_valid_version-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает минимальную версию клиента, которая является допустимой для использования при получении сведений об отслеживании изменений из указанной таблицы при использовании функции [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
    
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>Аргументы  
 *table_object_id*  
 Объектный идентификатор таблицы. *table_object_id* является типом **int**.  
  
## <a name="return-type"></a>Возвращаемый тип  
 **bigint**  
  
## <a name="remarks"></a>Remarks  
 Эта функция используется для проверки значения параметра *last_sync_version* для CHANGETABLE. Если *last_sync_version* меньше значения, сообщаемого этой функцией, результаты, возвращаемые из последующего вызова CHANGETABLE, могут быть недействительными.  
  
 Функция CHANGE_TRACKING_MIN_VALID_VERSION использует для определения значения возврата следующие сведения.  
  
-   Когда было включено отслеживание изменений для таблицы.  
  
-   Когда была выполнена задача фоновой очистки для удаления данных отслеживания изменений, срок хранения которых превысил срок хранения, указанный для базы данных.  
  
-   Была ли таблица усечена. При этом удаляются все данные отслеживания изменений, связанные с таблицей.  
  
 Функция возвращает значение NULL, если любое из следующих условий верно.  
  
-   Отслеживание изменений для базы данных не включено.  
  
-   Указанный идентификатор объекта таблицы для текущей базы данных недопустим.  
  
-   Недостаточно разрешений для таблицы, указанной идентификатором объекта.  
  
## <a name="examples"></a>Примеры  
 В следующем примере определяется, является ли указанная версия допустимой. В примере получается минимально допустимая версия таблицы `dbo.Employees`, а затем это значение сравнивается со значением переменной `@last_sync_version`. Если значение `@last_sync_version` меньше, чем значение переменной `@min_valid_version`, список измененных строк недопустим.  
  
> [!NOTE]  
>  Обычно это значение получается из таблицы или другого места, в котором хранится номер последней версии, использованной для синхронизации данных.  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции отслеживания изменений (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
