---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40201cb1fed3938da075b3fd5e97380eb0dddfc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093866"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает записи журнала служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] (sysssislog), совпадающие с идентификатором package_execution_id для данного пакета. Таблица содержит одну строку для каждой записи журнала, сформированной пакетами или их задачами и контейнерами во время выполнения.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>Аргументы  
 *log_id*  
 Локальный уникальный идентификатор для журнала выполнения. *log_id* имеет **тип int**.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|идентификатор|**int**|Уникальный идентификатор для записи журнала.|  
|event|**sysname**|Имя события, которое сформировало запись журнала.|  
|computer|**nvarchar**|Компьютер, на котором был запущен пакет, когда была сформирована запись журнала.|  
|оператор|**nvarchar**|Имя пользователя или агента, запустившего пакет, который сформировал запись журнала.|  
|source|**nvarchar**|Имя исполняемого объекта, который сформировал запись журнала.|  
|sourceid|**uniqueidentifier**|Идентификатор GUID исполняемого объекта, который сформировал запись журнала.|  
|executionid|**uniqueidentifier**|Идентификатор GUID экземпляра выполнения исполняемого объекта, который сформировал запись журнала.|  
|starttime|**datetime**|Время начала выполнения пакета.|  
|endtime|**datetime**|Время завершения выполнения пакета.|  
|datacode|**int**|Целочисленное значение, которое указывает на событие, связанное с записью журнала. Значение 0 указывает, что событие не передало идентификатор.|  
|databytes|**image**|Массив байтов, который указывает на возвращаемое значение.|  
|message|**nvarchar**|Описание события и сведения, связанные с событием.|  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение SELECT для **dc_operator**.  
  
## <a name="see-also"></a>См. также:  
 [Включение ведения журнала пакетов в SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [Сбор данных](../../relational-databases/data-collection/data-collection.md)  
  
  
