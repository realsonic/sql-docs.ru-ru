---
description: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)
title: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 6794e073-0895-4507-aba3-c3545acc843f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a2a23c3fffb8bfffa1cd9343d36dfd4c12bf5f1f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094975"
---
# <a name="sysfn_stmt_sql_handle_from_sql_stmt-transact-sql"></a>sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Возвращает **stmt_sql_handle** для [!INCLUDE[tsql](../../includes/tsql-md.md)] оператора с данным типом параметризации (Simple или forced). Это позволяет обращаться к запросам, хранящимся в хранилище запросов, с помощью их **stmt_sql_handle** , если известно их текстовое значение.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sys.fn_stmt_sql_handle_from_sql_stmt   
(  
    'query_sql_text',   
    [ query_param_type   
) [;]  
```  
  
## <a name="arguments"></a>Аргументы  
 *query_sql_text*  
 Текст запроса в хранилище запросов, для которого требуется создать маркер. *query_sql_text* имеет тип **nvarchar (max)** и не имеет значения по умолчанию.  
  
 *query_param_type*  
 Тип параметра запроса. *query_param_type* является **tinyint**. Возможны следующие значения:  
  
-   NULL — значение по умолчанию 0  
  
-   0 - нет  
  
-   1 — пользователь  
  
-   2 — простой  
  
-   3 — принудительно  
  
## <a name="columns-returned"></a>Возвращаемые столбцы  
 В следующей таблице перечислены столбцы, возвращаемые sys.fn_stmt_sql_handle_from_sql_stmt.  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|**statement_sql_handle**|**varbinary (64)**|Маркер SQL.|  
|**query_sql_text**|**nvarchar(max)**|Текст [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции.|  
|**query_parameterization_type**|**tinyint**|Тип параметризации запроса.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Комментарии  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **EXECUTE** на базу данных и разрешение **Delete** для представлений каталога хранилища запросов.  
  
## <a name="examples"></a>Примеры  
 В следующем примере выполняется инструкция, а затем используется функция `sys.fn_stmt_sql_handle_from_sql_stmt` для возврата SQL-маркера этой инструкции.  
  
```  
SELECT * FROM sys.databases;   
SELECT * FROM sys.fn_stmt_sql_handle_from_sql_stmt('SELECT * FROM sys.databases', NULL);  
```  
  
 Используйте функцию для корреляции данных хранилища запросов с другими динамическими административными представлениями. Следующий пример:  
  
```  
SELECT qt.query_text_id, q.query_id, qt.query_sql_text, qt.statement_sql_handle,  
q.context_settings_id, qs.statement_context_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_id  
CROSS APPLY sys.fn_stmt_sql_handle_from_sql_stmt (qt.query_sql_text, null) AS fn_handle_from_stmt  
JOIN sys.dm_exec_query_stats AS qs   
    ON fn_handle_from_stmt.statement_sql_handle = qs.statement_sql_handle;  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [Представления каталога хранилища запросов &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
