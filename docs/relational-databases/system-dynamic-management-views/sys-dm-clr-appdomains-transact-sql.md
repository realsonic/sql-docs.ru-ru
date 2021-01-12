---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ae934fc3b9d8f7664e2542450ee1456298548c85
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097784"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку на каждый домен приложения на сервере. Домен приложения (**AppDomain**) — это конструкция в среде CLR [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , которая является единицей изоляции для приложения. Это представление можно использовать для анализа и устранения неполадок объектов интеграции со средой CLR, которые выполняются в [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Существует несколько типов управляемых объектов CLR для баз данных. Общие сведения об этих объектах см. [в разделе Создание объектов базы данных с интеграцией среды CLR](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md). При каждом выполнении этих объектов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает **домен приложения** , в котором он может загрузить и выполнить необходимый код. Уровень изоляции для **AppDomain** — это один **домен приложения** на одну базу данных для каждого владельца. То есть все объекты среды CLR, принадлежащие пользователю, всегда выполняются в одном **домене AppDomain** для каждой базы данных (если пользователь регистрирует объекты базы данных CLR в разных базах данных, объекты базы данных среды CLR будут работать в разных доменах приложений). После завершения выполнения кода **AppDomain** не уничтожается. Вместо этого он кэшируется в памяти для последующего использования. Это повышает производительность.  
  
 Дополнительные сведения см. в разделе [домены приложений](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary(8)**|Адрес **домена приложения**. Все управляемые объекты базы данных, принадлежащие пользователю, всегда загружаются в один **домен AppDomain**. Этот столбец можно использовать для поиска всех сборок, загруженных в данный момент в этот **домен приложения** в **sys.dm_clr_loaded_assemblies**.|  
|**appdomain_id**|**int**|Идентификатор **домена приложения**. Каждый **домен приложения** имеет уникальный идентификатор.|  
|**appdomain_name**|**varchar (386)**|Имя **домена приложений** , назначенное [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**creation_time**|**datetime**|Время создания **AppDomain** . Так как **домены приложений** кэшируются и повторно используются для повышения производительности, **creation_time** не обязательно является временем выполнения кода.|  
|**db_id**|**int**|Идентификатор базы данных, в которой создан этот **AppDomain** . Код, хранящийся в двух разных базах данных, не может совместно использовать один **AppDomain**.|  
|**user_id**|**int**|Идентификатор пользователя, объекты которого могут выполняться в этом **домене приложения**.|  
|**state**|**nvarchar(128)**|Дескриптор текущего состояния **домена приложения**. Домен приложения может быть в различных состояниях — от создания до удаления. Дополнительные сведения см. в подразделе «Примечания».|  
|**strong_refcount**|**int**|Число строгих ссылок на этот **AppDomain**. Это отражает количество выполняемых в настоящий момент пакетов, использующих этот **AppDomain**. Обратите внимание, что при выполнении этого представления будет создан **строгий счетчик ссылок**; даже если в настоящий момент не выполняется код, **strong_refcount** будет иметь значение 1.|  
|**weak_refcount**|**int**|Число слабых ссылок на этот **AppDomain**. Указывает количество кэшированных объектов в **AppDomain** . При выполнении управляемого объекта базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] кэширует его в **AppDomain** для повторного использования в будущем. Это повышает производительность.|  
|**cost**|**int**|Стоимость **AppDomain**. Чем выше стоимость, тем выше вероятность, что этот **AppDomain** будет выгружен при нехватке памяти. Затраты обычно зависят от объема памяти, необходимой для повторного создания этого **AppDomain**.|  
|**value**|**int**|Значение **домена приложения**. Чем ниже значение, тем скорее вероятность, что этот **AppDomain** будет выгружен при нехватке памяти. Значение обычно зависит от того, сколько подключений или пакетов использует этот **AppDomain**.|  
|**total_processor_time_ms**|**bigint**|Суммарное время процессора в миллисекундах, использованное всеми потоками при выполнении в текущем домене приложения с момента запуска процесса. Это эквивалентно **System. AppDomain. мониторингтоталпроцессортиме**.|  
|**total_allocated_memory_kb**|**bigint**|Общий размер (в килобайтах) всей выделенной памяти, которая была выделена в домене приложения с момента его создания (без вычета освобожденной и собранной памяти). Это эквивалентно **System. AppDomain. мониторингтоталаллокатедмеморисизе**.|  
|**survived_memory_kb**|**bigint**|Количество килобайт, сохранившихся после последней полной, блокирующей сборки мусора, на которые заведомо существуют ссылки из текущего домена приложения. Это эквивалентно **System. AppDomain. мониторингсурвиведмеморисизе**.|  
  
## <a name="remarks"></a>Комментарии  
 Существует связь "один к одному" между **dm_clr_appdomains. appdomain_address** и **dm_clr_loaded_assemblies. appdomain_address**.  
  
 В следующих таблицах перечислены возможные значения **состояния** , их описания и время их возникновения в жизненном цикле **AppDomain** . Эти сведения можно использовать, чтобы отслеживать Lifecyle **AppDomain** и отслеживать подозрительные или повторяющиеся экземпляры **AppDomain** , не анализируя журнал событий Windows.  
  
## <a name="appdomain-initialization"></a>Инициализация домена приложений  
  
|Состояние|Описание|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|Создается **AppDomain** .|  
  
## <a name="appdomain-usage"></a>Использование домена приложений  
  
|Состояние|Описание|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|**AppDomain** среды выполнения готов к использованию несколькими пользователями.|  
|E_APPDOMAIN_SINGLEUSER|**Домен приложения** готов к использованию в операциях DDL. Это отличается от E_APPDOMAIN_SHARED, в котором используется общий домен приложения для выполнения интеграции CLR в качестве противопоставления операциям DDL. Такие домены приложения изолированы от остальных текущих операций.|  
|E_APPDOMAIN_DOOMED|Приложение **AppDomain** запланировано к выгрузке, но в настоящий момент в нем выполняются потоки.|  
  
## <a name="appdomain-cleanup"></a>Очистка домена приложений  
  
|Состояние|Описание|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запросил, чтобы среда CLR выгрузить **AppDomain**, обычно потому, что сборка, содержащая объекты управляемой базы данных, была изменена или удалена.|  
|E_APPDOMAIN_UNLOADED|Среда CLR выгрузила **AppDomain**. Обычно это результат процедуры эскалации из-за **ThreadAbort**, **OutOfMemory** или необработанного исключения в пользовательском коде.|  
|E_APPDOMAIN_ENQUEUE_DESTROY|**AppDomain** был выгружен в среде CLR и настроен для уничтожения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_DESTROY|**AppDomain** находится в процессе уничтожения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_ZOMBIE|**AppDomain** был уничтожен, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] но не все ссылки на **домен приложения** были очищены.|  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW SERVER STATE на базу данных.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как просмотреть сведения о **домене приложения** для данной сборки:  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 В следующем примере показано, как просмотреть все сборки в заданном **AppDomain**:  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_clr_loaded_assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [Динамические административные представления, связанные со средой CLR &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
