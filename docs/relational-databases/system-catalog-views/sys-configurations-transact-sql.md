---
description: sys.configurations (Transact-SQL)
title: sys.configуратионс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10956e1c3738c64829ec18d92a05b9f427b1833d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100692"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит одну строку для каждого значения параметра конфигурации сервера в системе.  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|Уникальный идентификатор значения конфигурации.|  
|**name**|**nvarchar(35)**|Имя параметра конфигурации.|  
|**value**|**sql_variant**|Установленное значение параметра.|  
|**minimum**|**sql_variant**|Минимальное значение параметра конфигурации.|  
|**maximum**|**sql_variant**|Максимальное значение параметра конфигурации.|  
|**value_in_use**|**sql_variant**|Текущее значение параметра.|  
|**description**|**nvarchar(255)**|Описание параметра конфигурации.|  
|**is_dynamic**|**bit**|1 = переменная, вступающая в силу после выполнения инструкции RECONFIGURE.|  
|**is_advanced**|**bit**|1 = переменная отображается, только если задан параметр **Показать адванцедоптион** .|  
  
 ## <a name="remarks"></a>Комментарии
  Список всех параметров конфигурации сервера см. в разделе [Параметры конфигурации сервера &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Сведения о параметрах конфигурации уровня базы данных см. в разделе [ALTER DATABASE scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Сведения о настройке программной архитектуры NUMA см. в разделе [Soft-numa &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
 
Представление каталога sys.configуратионс можно использовать для определения config_value (столбец значений), run_value (столбец value_in_use), а также значение, определяющее, является ли параметр конфигурации динамическим (не требует перезапуска серверного ядра или столбца is_dynamic).

> [!NOTE]
> Config_value в результирующем наборе sp_configure эквивалентен столбцу **sys.configуратионс. Value** . **Run_value** эквивалентен столбцу **urations.value_in_usesys.config** .

Следующий запрос можно использовать, чтобы определить, не установлены ли какие либо настроенные значения:

```SQL
select * from sys.configurations where value != value_in_use
```

Если значение соответствует измененному параметру конфигурации, но **value_in_use** не совпадает, то либо команда RECONFIGURE не была выполнена, либо произошел сбой, либо необходимо перезапустить ядро сервера.

Существуют параметры конфигурации, в которых значения и value_in_use могут отличаться, и это ожидаемое поведение. Пример:

"max server memory (МБ)" — значение по умолчанию, равное 0, отображается как **value_in_use** = 2147483647<br>

"min server memory (МБ)" — значение по умолчанию 0 может отображаться как **value_in_use** = 8 (32-разрядная) или 16 (64-разрядная). В некоторых случаях **value_in_use** имеет значение 0. В этом случае "true" **value_in_use** имеет значение 8 (32-разрядное) или 16 (64-разрядное).


Столбец **is_dynamic** можно использовать, чтобы определить, требует ли параметр конфигурации перезагрузки. is_dynamic = 1 означает, что при выполнении из командной перенастройки (T-SQL) новое значение вступит в силу немедленно (в некоторых случаях ядро сервера может не оценивать новое значение немедленно, но это происходит в обычном процессе выполнения). is_dynamic = 0 означает, что измененное значение конфигурации не вступит в силу, пока сервер не будет перезагружен, даже если была выполнена команда перенастройки (T-SQL).

Для параметра конфигурации, который не является динамическим, не существует способа определить, выполнялась ли команда перенастройки (T-SQL), чтобы выполнить первый шаг установки изменения конфигурации. Перед перезапуском SQL Server для установки изменения конфигурации выполните команду RECONFIGURE (T-SQL), чтобы убедиться, что все изменения конфигурации вступят в силу после перезапуска SQL Server. 
 
 
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога конфигурации на уровне сервера &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
