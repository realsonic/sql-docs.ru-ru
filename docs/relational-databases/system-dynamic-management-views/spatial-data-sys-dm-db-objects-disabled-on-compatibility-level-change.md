---
description: sys.dm_db_objects_disabled_on_compatibility_level_change (Transact-SQL)
title: sys.dm_db_objects_disabled_on_compatibility_level_change (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_objects_disabled_on_compatibility_level_change
- dm_db_objects_disabled_on_compatibility_level_change_TSQL
- sys.dm_db_objects_disabled_on_compatibility_level_change
- sys.dm_db_objects_disabled_on_compatibility_level_change_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_objects_disabled_on_compatibility_level_change catalog view
ms.assetid: a5d70064-0330-48b9-b853-01eba50755d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4e5092468b8a6beebb9d5149d215b39f47db0818
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100044"
---
# <a name="spatial-data---sysdm_db_objects_disabled_on_compatibility_level_change"></a>Пространственные данные — sys.dm_db_objects_disabled_on_compatibility_level_change
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Перечисляет индексы и ограничения, которые будут отключены в результате изменения уровня совместимости в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Индексы и ограничения, содержащие материализованные вычисляемые столбцы, в выражениях которых используются пространственные определяемые пользователем типы, будут отключены после обновления или изменения уровня совместимости. Данная функция динамического управления используется для определения влияния изменений уровня совместимости.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
sys.dm_db_objects_disabled_on_compatibility_level_change ( compatibility_level )   
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 *compatibility_level*  
 **целое** число, определяющее уровень совместимости, который вы планируете задать.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**class**|**int**|1 = ограничения<br /><br /> 7 = индексы и кучи|  
|**class_desc**|**nvarchar(60)**|OBJECT или COLUMN для ограничений<br /><br /> INDEX для индексов и куч|  
|**major_id**|**int**|OBJECT ID ограничений<br /><br /> OBJECT ID таблицы, в которой содержатся индексы и кучи.|  
|**minor_id**|**int**|NULL для ограничений<br /><br /> Index_id для индексов и куч|  
|**зависимостей**|**nvarchar(60)**|Описание зависимости, которая вызывает отключение ограничения или индекса. Эти же значения используются также в предупреждениях, возникающих во время обновления. Вот несколько примеров.<br /><br /> space для встроенных<br /><br /> geometry для системы определяемого пользователем типа<br /><br /> geography::Parse для метода системного определяемого пользователем типа|  
  
## <a name="general-remarks"></a>Общие замечания  
 Материализованные вычисляемые столбцы, использующие некоторые встроенные функции, отключаются при изменении уровня совместимости. Кроме того, материализованные вычисляемые столбцы, использующие геометрический или географический метод, отключаются при обновлении базы данных.  
  
### <a name="which-functions-cause-persisted-computed-columns-to-be-disabled"></a>Использование каких функций приводит к отключению материализованных вычисляемых столбцов?  
 При использовании следующих функций в выражении материализованных вычисляемых столбцов происходит отключение индексов и ограничений, которые ссылаются на данные столбцы, в ходе изменения уровня совместимости с 80 на 90.  
  
-   **IsNumeric**  
  
 При использовании следующих функций в выражении материализованных вычисляемых столбцов происходит отключение индексов и ограничений, которые ссылаются на данные столбцы, в ходе изменения уровня совместимости с 100 на 110 или выше.  
  
-   **SOUNDEX**  
  
-   **Geography:: GeomFromGML**  
  
-   **Geography:: STGeomFromText**  
  
-   **Geography:: STLineFromText**  
  
-   **Geography:: STPolyFromText**  
  
-   **Geography:: STMPointFromText**  
  
-   **Geography:: STMLineFromText**  
  
-   **Geography:: STMPolyFromText**  
  
-   **Geography:: STGeomCollFromText**  
  
-   **Geography:: STGeomFromWKB**  
  
-   **Geography:: STLineFromWKB**  
  
-   **Geography:: STPolyFromWKB**  
  
-   **Geography:: STMPointFromWKB**  
  
-   **Geography:: STMLineFromWKB**  
  
-   **Geography:: STMPolyFromWKB**  
  
-   **Geography:: STUnion**  
  
-   **Geography:: STIntersection**  
  
-   **Geography:: STDifference**  
  
-   **Geography:: STSymDifference**  
  
-   **Geography:: STBuffer**  
  
-   **Geography:: BufferWithTolerance**  
  
-   **География:: Parse**  
  
-   **География:: reduce**  
  
### <a name="behavior-of-the-disabled-objects"></a>Поведение отключенных объектов  
 **Индексы**  
  
 Если кластеризованный индекс отключен или принудительно задан некластеризованный индекс, возникает следующая ошибка: "обработчику запросов не удается создать план, поскольку индекс"%. \* ls "для таблицы или представления"%. \* ls "отключено". Чтобы повторно включить эти объекты, перестройте индексы после обновления, вызвав **инструкцию ALTER INDEX ON... Перестроение**.  
  
 **Кучи**  
  
 При использовании таблицы с отключенной кучей возникает следующая ошибка. Чтобы повторно включить эти объекты, перестройте после обновления, вызвав **инструкцию ALTER INDEX ALL ON... Перестроение**.  
  
```  
// ErrorNumber: 8674  
// ErrorSeverity: EX_USER  
// ErrorFormat: The query processor is unable to produce a plan because the table or view '%.*ls' is disabled.  
// ErrorCause: The table has a disabled heap.   
// ErrorCorrectiveAction: Rebuild the disabled heap to enable it.   
// ErrorInserts: table or view name   
// ErrorOwner: mtintor   
// ErrorFirstProduct: SQL11  
```  
  
 При попытке перестроить кучу во время операции в сети возникает ошибка.  
  
 **Проверочные ограничения и внешние ключи**  
  
 При отключенных проверочных ограничениях и внешних ключах сообщение об ошибке не возникает. Однако ограничения не применяются при изменении строк. Чтобы повторно включить эти объекты, проверьте ограничения после обновления, вызвав **инструкцию ALTER TABLE... ПРОВЕРОЧное ограничение**.  
  
 **Материализованные вычисляемые столбцы**  
  
 При отсутствии возможности отключения отдельного столбца отключается вся таблица посредством отключения кластеризованного индекса или кучи.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE.  
  
## <a name="example"></a>Пример  
 В следующем примере показан запрос к **sys.dm_db_objects_disabled_on_compatibility_level_change** для поиска объектов, на которые влияет изменение уровня совместимости до 120.  
  
```sql  
SELECT * FROM sys.dm_db_objects_disabled_on_compatibility_level_change(120);  
GO  
  
```  
  
  
