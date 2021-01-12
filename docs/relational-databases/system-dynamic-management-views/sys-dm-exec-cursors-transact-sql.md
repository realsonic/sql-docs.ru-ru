---
description: sys.dm_exec_cursors (Transact-SQL)
title: sys.dm_exec_cursors (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_cursors_TSQL
- dm_exec_cursors
- dm_exec_cursors_TSQL
- sys.dm_exec_cursors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cursors dynamic management function
ms.assetid: f520b63c-36af-40f1-bf71-6901d6331d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8ab1ed9e722f2363c0568932e7eb337bc09dbe41
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099013"
---
# <a name="sysdm_exec_cursors-transact-sql"></a>sys.dm_exec_cursors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о курсорах, открытых в различных базах данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dm_exec_cursors (session_id | 0 )  
```  
  
## <a name="arguments"></a>Аргументы  
 *session_id* | 0,0  
 Идентификатор сеанса. Если указан параметр *session_id* , эта функция возвращает сведения о курсорах в указанном сеансе.  
  
 Если указано значение 0, функция возвращает сведения обо всех курсорах для всех сеансов.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Идентификатор сеанса, содержащего курсор.|  
|**cursor_id**|**int**|Идентификатор курсора.|  
|**name**|**nvarchar(256)**|Имя курсора, определенное пользователем.|  
|**properties**|**nvarchar(256)**|Указывает свойства курсора. Для образования значения этого столбца объединены значения следующих свойств:<br />интерфейс объявления;<br />тип курсора; <br />параллелизм курсора;<br />область курсора;<br />уровень вложенности курсора.<br /><br /> Например, значение, возвращаемое в этом столбце, может быть "TSQL &#124; Dynamic &#124; Оптимистическая &#124; Global (0)".|  
|**sql_handle**|**varbinary (64)**|Дескриптор текста пакета, в котором объявлен курсор.|  
|**statement_start_offset**|**int**|Количество символов в выполняемом в настоящий момент пакете или хранимой процедуре, в которой запущена текущая инструкция. Можно использовать вместе с **sql_handle**, **statement_end_offset** и функцией динамического управления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) , чтобы получить текущую выполняемую инструкцию для запроса.|  
|**statement_end_offset**|**int**|Количество символов в выполняемом в настоящий момент пакете или хранимой процедуре, в которой завершилась текущая инструкция. Можно использовать вместе с **sql_handle**, **statement_start_offset** и функцией динамического управления **sys.dm_exec_sql_text** , чтобы получить текущую выполняемую инструкцию для запроса.|  
|**plan_generation_num**|**bigint**|Порядковый номер, который может использоваться для различия экземпляров планов после рекомпиляции.|  
|**creation_time**|**datetime**|Отметка времени создания данного курсора.|  
|**is_open**|**bit**|Указывает, является ли курсор открытым.|  
|**is_async_population**|**bit**|Указывает, выполняется ли до сих пор асинхронное заполнение курсора KEYSET или STATIC фоновым потоком.|  
|**is_close_on_commit**|**bit**|Определяет, был ли курсор объявлен с помощью ключевого слова CURSOR_CLOSE_ON_COMMIT.<br /><br /> 1 = курсор будет закрыт по завершении транзакции.|  
|**fetch_status**|**int**|Возвращает состояние последней выборки курсора. Это последнее возвращенное значение @ @FETCH_STATUS value.|  
|**fetch_buffer_size**|**int**|Возвращает сведения о размере буфера выборки.<br /><br /> 1 = курсоры языка Transact-SQL. Для курсоров API это значение может быть больше.|  
|**fetch_buffer_start**|**int**|Для курсоров типа FAST_FORWARD и DYNAMIC возвращается значение 0, если курсор не отрыт или установлен перед первой строкой. Иначе возвращается значение -1.<br /><br /> Для курсоров типа STATIC и KEYSET возвращается значение 0, если курсор не открыт, и значение -1, если курсор установлен за последней строкой.<br /><br /> В противном случае возвращается номер строки, в которой установлен курсор.|  
|**ansi_position**|**int**|Позиция курсора внутри буфера выборки.|  
|**worker_time**|**bigint**|Время в миллисекундах, потраченное на обработку данного курсора.|  
|**буферизует**|**bigint**|Количество операций чтения, выполненных курсором.|  
|**дает**|**bigint**|Количество операций записи, выполненных курсором.|  
|**dormant_duration**|**bigint**|Количество миллисекунд, прошедшее с момента запуска последнего запроса на открытие или на выборку.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="remarks"></a>Комментарии  
 Следующая таблица содержит сведения об интерфейсе объявления курсора и возможные значения для столбца свойств.  
  
|Свойство|Описание|  
|--------------|-----------------|  
|API|Курсор был объявлен с помощью одной из API-функций для доступа к данным (ODBC, OLEDB).|  
|TSQL|Курсор был объявлен с помощью инструкции DECLARE_CURSOR языка Transact-SQL.|  
  
 Следующая таблица содержит сведения о типе курсора и возможные значения для столбца свойств.  
  
|Type|Описание|  
|----------|-----------------|  
|Keyset|Курсор был объявлен с типом Keyset.|  
|Динамический|Курсор был объявлен с типом Dynamic.|  
|Моментальный снимок|Курсор был объявлен с типом Snapshot или Static.|  
|Fast_Forward|Курсор был объявлен с типом Fast Forward.|  
  
 Следующая таблица содержит сведения о параллелизме курсоров и возможные значения для столбца свойств.  
  
|Параллелизм|Описание|  
|-----------------|-----------------|  
|Только для чтения|Курсор был объявлен в режиме только для чтения.|  
|Scroll Locks|Курсор использует блокирование прокрутки.|  
|Optimistic|Курсор использует управление оптимистичным параллелизмом.|  
  
 Следующая таблица содержит сведения об области курсоров и возможные значения для столбца свойств.  
  
|Область|Описание|  
|-----------|-----------------|  
|Локальная|Указывает, что курсор является локальным по отношению к пакету, хранимой процедуре или триггеру, в котором он был создан.|  
|Глобальная|Указывает, что курсор является глобальным по отношению к соединению.|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-detecting-old-cursors"></a>A. Обнаружение старых курсоров  
 В этом примере возвращаются сведения о курсорах, которые открыты на сервере дольше указанного времени, составляющего 36 часов.  
  
```  
SELECT creation_time, cursor_id, name, c.session_id, login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s ON c.session_id = s.session_id   
WHERE DATEDIFF(hh, c.creation_time, GETDATE()) > 36;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
  

