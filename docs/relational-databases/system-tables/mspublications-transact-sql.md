---
description: MSpublications (Transact-SQL)
title: Мспубликатионс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
author: cawrites
ms.author: chadam
ms.openlocfilehash: 089270c4c95b35e25f44339a7f001b5ac194ec01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098188"
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **мспубликатионс** содержит по одной строке для каждой публикации, реплицируемой издателем. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Идентификатор издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**публикации**|**sysname**|Имя публикации.|  
|**publication_id**|**int**|Идентификатор публикации.|  
|**publication_type**|**int**|Тип публикации:<br /><br /> **0** = транзакционная.<br /><br /> **1** = моментальный снимок.<br /><br /> **2** = слияние.|  
|**thirdparty_flag**|**bit**|Указывает, является ли публикация [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базой данных:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **1** = источник данных, отличный от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**independent_agent**|**bit**|Указывает, имеется ли для данной публикации изолированный агент распространителя.|  
|**immediate_sync**|**bit**|Указывает, создаются ли повторно файлы синхронизации при каждом запуске агента моментальных снимков.|  
|**allow_push**|**bit**|Указывает, могут ли быть созданы для данной публикации принудительные подписки.|  
|**allow_pull**|**bit**|Указывает, могут ли быть созданы для данной публикации подписки по запросу.|  
|**allow_anonymous**|**bit**|Указывает, могут ли быть созданы для данной публикации анонимные подписки.|  
|**description**|**nvarchar(255)**|Описание публикации.|  
|**vendor_name**|**nvarchar (100)**|Имя поставщика, если издатель не является [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базой данных.|  
|**хранения**|**int**|Срок хранения публикации в часах.|  
|**sync_method**|**int**|Метод синхронизации.<br /><br /> **0** = Native (создает в собственном режиме выходные данные полного копирования всех таблиц).<br /><br /> **1** = символ (создает выходные данные полного копирования в символьном режиме для всех таблиц).<br /><br /> **3** = одновременные (создает в собственном режиме выходные данные полного копирования всех таблиц, но не блокирует таблицу во время создания моментального снимка).<br /><br /> **4** = Concurrent_c (создает выходное копирование в символьном режиме для всех таблиц, но не блокирует таблицу во время создания моментального снимка).<br /><br /> Значения **3** и **4** доступны для репликации транзакций и репликации слиянием, но не для репликации моментальных снимков.|  
|**allow_subscription_copy**|**bit**|Разрешает или запрещает возможность копирования баз данных подписки, подписанных на эту публикацию. **0** означает, что копирование отключено и **1** означает, что он включен.|  
|**thirdparty_options**|**int**|Указывает, подавляется ли отображение публикации в папке репликации в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **0** = отобразить разнородную публикацию в папке Replication в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **1** = отключить отображение разнородной публикации в папке Replication в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**allow_queued_tran**|**bit**|Определяет, позволяет ли обновление публикация посредством очередей:<br /><br /> **0 =** Публикация не находится в очереди.<br /><br /> **1** = публикация поставлена в очередь.|  
|**options**|**int**|Сведения для данного выпуска отсутствуют.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
