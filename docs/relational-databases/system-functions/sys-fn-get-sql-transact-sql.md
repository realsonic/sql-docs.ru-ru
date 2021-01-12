---
description: sys.fn_get_sql (Transact-SQL)
title: sys.fn_get_sql (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4b3e28e4c66d45f28c6239431e8e6d5440d5d4a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093824"
---
# <a name="sysfn_get_sql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает текст инструкции SQL для указанного дескриптора SQL.  
  
> [!IMPORTANT]  
>  В будущей версии Microsoft SQL Server этот компонент будет удален. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Вместо нее используйте sys.dm_exec_sql_text. Дополнительные сведения см. в разделе [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Аргументы  
 *SqlHandle*  
 Значение дескриптора. *SqlHandle* имеет тип **varbinary (64)** и не имеет значения по умолчанию.  
  
## <a name="tables-returned"></a>Возвращаемые таблицы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|Идентификатор базы данных. Для нерегламентированных и подготовленных инструкций SQL это идентификатор базы данных, в которой происходила компиляция инструкции.|  
|objectid|**int**|Идентификатор объекта базы данных. Имеет значение NULL для нерегламентированных инструкций SQL.|  
|number|**smallint**|Указывает на номер группы, если процедуры сгруппированы.<br /><br /> 0 = записи не являются процедурами.<br /><br /> NULL = нерегламентированные инструкции SQL.|  
|encrypted|**bit**|Указывает, зашифрован ли объект.<br /><br /> 0 = не зашифрована<br /><br /> 1 = зашифрована|  
|текст|**text**|Текст инструкции SQL. Имеет значение NULL для зашифрованных объектов.|  
  
## <a name="remarks"></a>Комментарии  
 Допустимый обработчик SQL можно получить из столбца sql_handle sys.dm_exec_requests &#40;динамическое административное представление [Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) .  
  
 Если передается обработчик, который больше не существует в кэше, fn_get_sq **l** возвращает пустой результирующий набор. Если передается недопустимый дескриптор, выполнение пакета прекращается и возвращается сообщение об ошибке.  
  
 Инструкция [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] не может кэшировать некоторые [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции, такие как инструкции и инструкции инструкций Copy с строковыми литералами, размер которых ПРЕВЫШАЕТ 8 КБ. Дескрипторы этих инструкций не могут быть извлечены при помощи функции fn_get_sql.  
  
 Столбец **Text** результирующего набора фильтруется по тексту, который может содержать пароли. Дополнительные сведения о хранимых процедурах, связанных с безопасностью, которые не отслеживаются, см. [в разделе Фильтрация трассировки](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 Функция fn_get_sql возвращает сведения, аналогичные команде [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) . Ниже приведены случаи, когда функцию fn_get_sql использовать можно, а команду DBCC INPUTBUFFER — нельзя:  
  
-   если событие содержит более чем 255 символов;  
  
-   если необходимо получить наивысший из текущих уровней вложенности хранимой процедуры. Например, есть две хранимые процедуры с именами sp_1 и sp_2. Если процедура sp_1 вызывает процедуру sp_2 и вы получаете дескриптор из динамического административного представления sys.dm_exec_requests во время выполнения процедуры sp_2, функция fn_get_sql возвращает сведения о процедуре sp_2. Кроме того, функция fn_get_sql возвращает полный текст хранимой процедуры на высшем текущем уровне вложенности.  
  
## <a name="permissions"></a>Разрешения  
 Пользователю необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
 Администраторы базы данных могут использовать функцию fn_get_sql, как показано в следующем примере, для облегчения диагностики проблемных процессов. После того как администратор находит проблемный идентификатор сеанса, он может получить дескриптор SQL для идентификатора сеанса, вызвать функцию n_get_sql, используя дескриптор, и после этого определить текст SQL проблемного идентификатора сеанса.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [DBCC INPUTBUFFER &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [sys.sysные процессы &#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
