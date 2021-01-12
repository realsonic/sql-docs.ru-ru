---
description: sys.server_event_sessions (Transact-SQL)
title: sys.server_event_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_event_sessions
- server_event_sessions_TSQL
- sys.server_event_sessions_TSQL
- sys.server_event_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_sessions catalog view
- xe
ms.assetid: 796f3093-6a3e-4d67-8da6-b9810ae9ef5b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9addba51e1a23fa9cf430d91aced04126a7f03e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093067"
---
# <a name="sysserver_event_sessions-transact-sql"></a>sys.server_event_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Перечисляет все определения сеанса событий, которые существуют в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Уникальный идентификатор сеанса событий. Не допускает значение NULL.|  
|name|**sysname**|Определяемое пользователем имя, идентифицирующее сеанс событий. имя является уникальным. Не допускает значение NULL.|  
|event_retention_mode|**nchar (1)**|Определяет способ обработки потери события. Значение по умолчанию — S. Не допускает значения NULL. Принимает одно из следующих значений.<br /><br /> S. Сопоставляется event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> Н. Сопоставляется event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> О. Сопоставляется event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Описывает способ обработки потери события. Значение по умолчанию ALLOW_SINGLE_EVENT_LOSS. Не допускает значение NULL. Принимает одно из следующих значений.<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Возможна потеря событий в сеансе. Одиночные события удаляются только в том случае, если все буферы событий полны. Потеря одиночных событий при заполнении буферов событий обеспечивает приемлемые характеристики производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], одновременно уменьшая до минимума потери данных в потоке обработанных событий.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. Возможна потеря полных буферов событий в сеансе. Число потерянных событий зависит от размера памяти, выделенной для сеанса, способа секционирования памяти и размера событий в буфере. Этот параметр уменьшает влияние быстрого заполнения буферов событий на производительность сервера. Однако возможна потеря большого числа событий в сеансе.<br /><br /> NO_EVENT_LOSS. Потеря событий не разрешена. Этот параметр обеспечивает сохранение всех произошедших событий. При использовании этого параметра все задачи, которые инициируют события, должны ждать освобождения пространства в буфере событий. Это может привести к заметному снижению производительности во время активного сеанса событий.|  
|max_dispatch_latency|**int**|Промежуток времени в миллисекундах, в течение которого события находятся в буферной памяти перед отправкой целям сеанса. Допустимые значения: от 0 до 2147483648 и 0. Значение 0 указывает, что задержка диспетчеризации бесконечно. Допускает значение NULL.|  
|max_memory|**int**|Объем памяти, выделенной в сеансе для буферов событий. Значение по умолчанию — 4 МБ. Допускает значение NULL.|  
|max_event_size|**int**|Объем памяти, выделенной для событий, которые не могут уместиться в буферах сеанса событий. Если значение max_event_size превышает расчетный размер буфера, два дополнительных буфера размера max_event_size выделяются для сеанса событий. Допускает значение NULL.|  
|memory_partition_mode|**nchar (1)**|Местоположение в памяти, в котором созданы буферы событий. Режим раздела памяти по умолчанию — G. Не допускает значения NULL. memory_partition_mode является одним из следующих:<br /><br /> G — NONE;<br /><br /> C — PER_CPU;<br /><br /> N — PER_NODE.|  
|memory_partition_mode_desc|**sysname**|По умолчанию значение установлено в NONE. Не допускает значение NULL. Принимает одно из следующих значений.<br /><br /> NONE. Внутри экземпляра SQL Server создается один набор буферов.<br /><br /> PER_CPU. Набор буферов создается для каждого ЦП.<br /><br /> PER_NODE. Набор буферов создается для каждого узла неоднородного доступа к памяти (NUMA).|  
|track_causality|**bit**|Включает или отключает отслеживание причинности. Если установлено значение 1 (ВКЛ.), то отслеживание включено и можно установить соответствие между связанными событиями в различных серверных соединениях. Значение по умолчанию — 0 (ВЫКЛ.). Не допускает значение NULL.|  
|startup_state|**bit**|Значение определяет, запускается ли сеанс автоматически при запуске сервера. Значение по умолчанию — 0. Не допускает значение NULL. Может принимать одно из следующих значений:<br /><br /> 0 (ВЫКЛ.). Сеанс не запускается автоматически при запуске сервера.<br /><br /> 1 (ВКЛ.). Сеанс событий запускается при запуске сервера.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога расширенных событий &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
  
