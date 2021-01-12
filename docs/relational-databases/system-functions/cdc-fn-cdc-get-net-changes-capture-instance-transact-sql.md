---
description: '&lt;capture_instance CDC.fn_cdc_get_net_changes_ &gt; (TRANSACT-SQL)'
title: cdc.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (TRANSACT-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_net_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_net_changes_<capture_instance>
ms.assetid: 43ab0d1b-ead4-471c-85f3-f6c4b9372aab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dee462b6b4a66b61d790bb06673d36f0907ef73
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099711"
---
# <a name="cdcfn_cdc_get_net_changes_ltcapture_instancegt-transact-sql"></a>&lt;capture_instance CDC.fn_cdc_get_net_changes_ &gt; (TRANSACT-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку "чистое изменение" для каждой исходной строки, измененной в пределах указанного диапазона номеров LSN.  
  
 **Подождите, что такое номер LSN?** Каждая запись в [SQL Server журнал транзакций](../logs/the-transaction-log-sql-server.md) уникально идентифицируется регистрационным номером транзакции в журнале (LSN). Номера LSN упорядочиваются таким, что если LSN2 больше LSN1, то изменение, описываемое записью журнала, на которое ссылается LSN2, произошло **после** изменения, ОПИСЫВАЕМОГО номером LSN записи журнала.  
  
 Номер LSN записи журнала, в которой произошло значительное событие, может быть полезен для построения правильных последовательностей восстановления. Так как регистрационные номера LSN упорядочены, их можно сравнить на равенство и неравенство (то есть, \<, > =, \<=, > =). Такие сравнения полезны при построении последовательностей восстановления.  
  
 Когда исходная строка имеет несколько изменений во время диапазона номеров LSN, одна строка, отражающая окончательное содержимое строки, возвращается функцией перечисления, описанной ниже. Например, если транзакция вставляет строку в исходную таблицу, а следующая транзакция в диапазоне LSN обновляет один или несколько столбцов в этой строке, функция возвращает только **одну** строку, которая включает обновленные значения столбца.  
  
 Эта функция перечисления создается, когда в исходной таблице включается система отслеживания измененных данных с указанием сетевого отслеживания. Чтобы включить сетевое отслеживание, исходная таблица должна иметь первичный ключ или уникальный индекс. Имя функции является производным и использует формат cdc.fn_cdc_get_net_changes_ *capture_instance*, где *capture_instance* — значение, заданное для экземпляра системы отслеживания, когда в исходной таблице была включена система отслеживания измененных данных. Дополнительные сведения см. в разделе [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
cdc.fn_cdc_get_net_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all with mask  
 | all with merge  
}  
```  
  
## <a name="arguments"></a>Аргументы  
 *from_lsn*  
 Номер LSN, представляющий нижнюю конечную точку диапазона LSN, включаемую в результирующий набор. *from_lsn* является **двоичным (10)**.  
  
 В результирующий набор включаются только строки в таблице [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) таблицы изменений со значением __ $ start_lsn больше или равно *from_lsn* .  
  
 *to_lsn*  
 Номер LSN, представляющий верхнюю конечную точку диапазона LSN, включаемую в результирующий набор. *to_lsn* является **двоичным (10)**.  
  
 В результирующий набор включаются только строки в таблице [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) таблицы изменений со значением __ $ start_lsn, меньшим или равным *from_lsn* , или равным *to_lsn* .  
  
 *<row_filter_option>* :: = {все | все с маской | все с помощью Merge}  
 Параметр, управляющий содержимым столбцов метаданных, а также строк, возвращаемых в результирующем наборе. Может быть одним из следующих:  
  
 все  
 Возвращает номер LSN последнего изменения в строке и операцию, необходимую для применения строки в столбцах метаданных __ $ start_lsn и \_ \_ $Operation. Столбец \_ \_ $Update _Mask всегда имеет значение null.  
  
 all with mask  
 Возвращает номер LSN последнего изменения в строке и операцию, необходимую для применения строки в столбцах метаданных __ $ start_lsn и \_ \_ $Operation. Кроме того, когда операция обновления возвращает ( \_ \_ $Operation = 4), захваченные столбцы, измененные в обновлении, помечаются в значении, возвращаемом в \_ \_ $Update _Mask.  
  
 all with merge  
 Возвращает номер LSN окончательного изменения строки в столбцах метаданных __$start_lsn. Столбец \_ \_ $Operation будет иметь одно из двух значений: 1 для DELETE и 5, чтобы указать, что операция, необходимая для применения изменения, является либо инструкцией INSERT, либо обновлением. Столбец \_ \_ $Update _Mask всегда имеет значение null.  
  
 Поскольку логика определения точной операции для конкретного изменения дополнительно усложняет запрос, эта операция предназначена для увеличения производительности запроса, когда достаточно указать, что операция изменения является вставкой или обновлением, но не требуется явно выбирать тип операции. Этот параметр особенно полезен в целевых средах, в которых операция слияния доступна непосредственно, например в среде [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|__$start_lsn|**binary(10)**|Номер LSN, связанный с фиксацией транзакции изменения.<br /><br /> Все изменения, зафиксированные в одной транзакции, имеют общий номер LSN фиксации. Например, если операция обновления в исходной таблице изменяет два столбца в двух строках, таблица изменений будет содержать четыре строки, каждая с одним и тем же __ $ start_lsnvalue.|  
|__$operation|**int**|Определяет операцию языка обработки данных (DML), необходимую для применения строки информации об изменениях к целевому источнику данных.<br /><br /> Если параметр row_filter_option имеет значение all или all with mask, то значение в этом столбце может быть одним из следующих:<br /><br /> 1 = удаление<br /><br /> 2 = вставка<br /><br /> 4 = обновление<br /><br /> Если параметр row_filter_option имеет значение all with merge, то значение в этом столбце может быть одним из следующих:<br /><br /> 1 = удаление|  
|__$update_mask|**varbinary(128)**|Битовая маска, в которой каждому отслеживаемому столбцу, определенному для экземпляра отслеживания, соответствует один бит. В этом значении все определенные биты установлены в значение 1, если__$operation = 1 или 2. Если \_ \_ $Operation = 3 или 4, то только биты, соответствующие измененным столбцам, устанавливаются в 1.|  
|*\<captured source table columns>*|непостоянно|Остальные столбцы, возвращенные функцией, — это столбцы из исходной таблицы, определенные как отслеживаемые при создании экземпляра отслеживания. Если в списке отслеживаемых столбцов не указано ни одного столбца, возвращаются все столбцы в исходной таблице.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в предопределенной роли сервера sysadmin или предопределенной роли базы данных db_owner. Всем остальным пользователям необходимо разрешение SELECT для всех отслеживаемых столбцов в исходной таблице. Кроме того, если для экземпляра отслеживания была определена шлюзовая роль, требуется членство в этой роли базы данных. Если вызывающий объект не располагает разрешением на просмотр исходных данных, функция возвращает ошибку 208 (недопустимое имя объекта).  
  
## <a name="remarks"></a>Комментарии  
 Если указанный диапазон номеров LSN выходит за пределы временной шкалы для отслеживания изменений в экземпляре отслеживания, функция возвращает ошибку 208 (недопустимое имя объекта).

 Изменения в уникальном идентификаторе строки приведут к тому, что в fn_cdc_get_net_changes будет отображаться команда начального обновления с помощью команды DELETE, а затем вставить.  Это поведение необходимо для отслеживания ключа как до, так и после изменения.
  
## <a name="examples"></a>Примеры  
 В следующем примере функция используется `cdc.fn_cdc_get_net_changes_HR_Department` для сообщения об изменениях, внесенных в исходную таблицу в `HumanResources.Department` течение определенного интервала времени.  
  
 Вначале используется функция `GETDATE`, чтобы отметить начало интервала времени. После того как к исходной таблице применяются несколько инструкций DML, функция `GETDATE` вызывается вновь, чтобы отметить конец интервала времени. Функция [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) используется для отображения интервала времени в диапазоне запросов системы отслеживания измененных данных, ограниченного значениями LSN. Наконец, к функции `cdc.fn_cdc_get_net_changes_HR_Department` выполняется запрос, чтобы получить сетевые изменения в исходной таблице за этот интервал времени. Обратите внимание, что вставленная, а затем удаленная строка не появляется в результирующем наборе функции. Это происходит потому, что вставленная, а затем удаленная строка в окне запроса не выполняет сетевых изменений в исходной таблице за этот период времени. Перед запуском этого примера необходимо сначала запустить пример б в [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @begin_time datetime, @end_time datetime, @from_lsn binary(10), @to_lsn binary(10);  
-- Obtain the beginning of the time interval.  
SET @begin_time = GETDATE() -1;  
-- DML statements to produce changes in the HumanResources.Department table.  
INSERT INTO HumanResources.Department (Name, GroupName)  
VALUES (N'MyDept', N'MyNewGroup');  
  
UPDATE HumanResources.Department  
SET GroupName = N'Resource Control'  
WHERE GroupName = N'Inventory Management';  
  
DELETE FROM HumanResources.Department  
WHERE Name = N'MyDept';  
  
-- Obtain the end of the time interval.  
SET @end_time = GETDATE();  
-- Map the time interval to a change data capture query range.  
SET @from_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than or equal', @begin_time);  
SET @to_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);  
  
-- Return the net changes occurring within the query window.  
SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@from_lsn, @to_lsn, 'all');  
```  
  
## <a name="see-also"></a>См. также:  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Об отслеживании измененных данных (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
