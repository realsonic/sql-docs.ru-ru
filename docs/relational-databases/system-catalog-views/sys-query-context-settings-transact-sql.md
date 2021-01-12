---
description: sys.query_context_settings (Transact-SQL)
title: sys.query_context_settings (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 510f508b8d0048b1e13aa544a3c07f20f3a1d44c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098350"
---
# <a name="sysquery_context_settings-transact-sql"></a>sys.query_context_settings (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Содержит сведения о семантике, влияющей на параметры контекста, связанные с запросом. Существует несколько доступных контекстных параметров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые влияют на семантику запроса (определяя правильный результат запроса). Тот же текст запроса, скомпилированный с разными параметрами, может давать разные результаты (в зависимости от базовых данных).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|Первичный ключ. Это значение предоставляется в инструкциях Showplan XML для запросов.|  
|**set_options**|**varbinary(8)**|Битовая маска, отражающая состояние нескольких параметров SET. Дополнительные сведения см. в разделе [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).|  
|**language_id**|**smallint**|Идентификатор языка. Дополнительные сведения см. в разделе [ языкиsys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|**date_format**|**smallint**|Формат даты. Дополнительные сведения см. в разделе [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|Первое значение даты. Дополнительные сведения см. в разделе [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**status**|**varbinary (2)**|Поле битовой маски, указывающее тип запроса или контекста, в котором был выполнен запрос. <br />Значение столбца может быть сочетанием нескольких флагов (выраженное в шестнадцатеричном формате):<br /><br /> 0x0 — обычный запрос (без специальных флагов)<br /><br /> 0x1 — запрос, выполненный с помощью одной из хранимых процедур API курсоров<br /><br /> 0x2 — запрос на уведомление<br /><br /> 0x4 — внутренний запрос<br /><br /> 0x8 — автоматический параметризованный запрос без универсальной параметризации<br /><br /> 0x10 — запрос на обновление выборки курсора<br /><br /> 0x20 — запрос, используемый в запросах на обновление курсора<br /><br /> 0x40 — начальный результирующий набор возвращается при открытии курсора (автоматическая выборка курсора)<br /><br /> 0x80-зашифрованный запрос<br /><br /> 0x100 — запрос в контексте предиката безопасности на уровне строк|  
|**required_cursor_options**|**int**|Параметры курсора, указанные пользователем, такие как тип курсора.|  
|**acceptable_cursor_options**|**int**|Параметры курсора, которые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может неявно преобразовывать для поддержания выполнения инструкции.|  
|**merge_action_type**|**smallint**|Тип плана выполнения триггера, используемый в качестве результата инструкции **Merge** .<br /><br /> 0 указывает на нетриггерный план, план триггера, который не выполняется в результате выполнения инструкции **Merge** , или план триггера, который выполняется в результате выполнения инструкции **Merge** , которая только указывает действие **Delete** .<br /><br /> 1 указывает план триггера **INSERT** , который выполняется в результате выполнения инструкции **Merge** .<br /><br /> 2 указывает план триггера **обновления** , который выполняется в результате выполнения инструкции **Merge** .<br /><br /> 3 указывает план триггера **Delete** , который выполняется в результате инструкции **Merge** , содержащей соответствующее действие **INSERT** или **Update** .<br /><br /> <br /><br /> Для вложенных триггеров, выполняемых каскадными действиями, это значение является действием инструкции **Merge** , которая вызвала каскадную операцию.|  
|**default_schema_id**|**int**|Идентификатор схемы по умолчанию, которая используется для разрешения имен, которые не являются полными.|  
|**is_replication_specific**|**bit**|Используется для репликации.|  
|**is_contained**|**varbinary (1)**|1 указывает на автономную базу данных.|  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **View Database State** .  
  
## <a name="see-also"></a>См. также:  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Query Store Stored Procedures (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  (Хранимые процедуры хранилища запросов (Transact-SQL))  
 [sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
