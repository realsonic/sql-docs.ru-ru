---
title: Динамическая статистика запросов | Документация Майкрософт
description: Сведения о том, как просмотреть динамический план выполнения активного запроса в SQL Server Management Studio. Использование статистики выполнения для отладки проблем с производительностью запросов.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee989011b8e5cf6092fdabd9ee0afcd1a06e0e58
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171376"
---
# <a name="live-query-statistics"></a>Динамическая статистика запросов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] позволяет просматривать динамический план выполнения активного запроса. Этот динамический план запроса позволяет анализировать процесс выполнения запроса в режиме реального времени по мере передачи управления от одного [оператора плана запроса](../../relational-databases/showplan-logical-and-physical-operators-reference.md) другому. Динамический план запроса отображает общий ход выполнения запроса и текущую статистику выполнения на уровне оператора, например число полученных строк, затраченное время, ход выполнения оператора и т. д. Так как эти данные доступны в режиме реального времени и, чтобы их увидеть, не нужно дожидаться завершения запроса, такая статистика чрезвычайно полезна для отладки проблем с производительностью запросов. Эта функция доступна с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], но она может работать и с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

> [!NOTE]
> На внутреннем уровне динамическая статистика запросов использует динамическое административное представление [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md).
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
> [!WARNING]  
> Эта функция предназначена в основном для диагностики. Ее использование может значительно снизить общую производительность запроса, особенно в [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Дополнительные сведения см. в разделе [Инфраструктура профилирования запросов](../../relational-databases/performance/query-profiling-infrastructure.md).  
> Эта функция может использоваться с [отладчиком Transact-SQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).  
  
## <a name="to-view-live-query-statistics-for-one-query"></a>Просмотр динамической статистики запросов для одного запроса 
  
1.  Чтобы просмотреть план выполнения запроса в режиме реального времени, в меню "Сервис" выберите значок **Включить динамическую статистику запросов**.  
  
     ![Динамическая кнопка статистики запросов на панели инструментов](../../relational-databases/performance/media/livequerystatstoolbar.png "Динамическая кнопка статистики запросов на панели инструментов")  
  
     Кроме того, динамический план выполнения запроса можно открыть, щелкнув правой кнопкой мыши запрос в [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] и выбрав пункт **Включить статистику активных запросов**.  
  
     ![Динамическая кнопка статистики запросов во всплывающем меню](../../relational-databases/performance/media/livequerystatsmenu.png "Динамическая кнопка статистики запросов во всплывающем меню")  
  
2.  Теперь можно выполнить запрос. В динамическом плане запроса отображается общий ход выполнения запроса и текущая статистика выполнения (например, затраченное время, ход выполнения и т. д.) по операторам плана запроса. Сведения о ходе выполнения запроса и статистика выполнения периодически обновляются во время выполнения запроса. С помощью этих сведений вы сможете в общих чертах понимать ход выполнения запроса, а также отлаживать долго выполняемые запросы, бесконечно выполняемые запросы, запросы, которые приводят к переполнению tempdb, и проблемы с временем ожидания.  
  
     ![Динамическая кнопка статистики запросов в инструкции Showplan](../../relational-databases/performance/media/livequerystatsplan.png "Динамическая кнопка статистики запросов в инструкции Showplan")  
  
## <a name="to-view-live-query-statistics-for-any-query"></a>Просмотр динамической статистики запросов для любого запроса 

Динамический план выполнения можно открыть из **[монитора активности](../../relational-databases/performance-monitor/activity-monitor.md)** . Для этого правой кнопкой мыши щелкните запросы в таблице **Процессы** или **Текущие ресурсоемкие запросы**.  
  
 ![Динамическая кнопка статистики запросов в мониторе активности](../../relational-databases/performance/media/livequerystatsactmon.png "Динамическая кнопка статистики запросов в мониторе активности")  
  
## <a name="remarks"></a>Remarks  
 Чтобы функция динамической статистики запросов могла собирать данные о ходе выполнения, необходимо включить инфраструктуру профиля статистики. В зависимости от версии затраты могут быть значительными. Дополнительные сведения об этих затратах см. в разделе [Инфраструктура профилирования запросов](../../relational-databases/performance/query-profiling-infrastructure.md).
  
## <a name="permissions"></a>Разрешения  
Для заполнения страницы результатов **Динамическая статистика запросов** требуется разрешение `SHOWPLAN` уровня базы данных. Для выполнения запроса требуются все разрешения, необходимые для этого.
На [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для просмотра динамической статистики требуются разрешения уровня сервера `VIEW SERVER STATE`.  
В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] на уровнях Premium для просмотра динамической статистики необходимо разрешение `VIEW DATABASE STATE` в базе данных. В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] на уровнях "Стандартный" и "Базовый" для просмотра динамической статистики требуется учетная запись **администратора сервера** или **администратора Azure Active Directory**.
  
## <a name="see-also"></a>См. также:  
 [Планы выполнения](../../relational-databases/performance/execution-plans.md)    
 [Руководство по архитектуре обработки запросов](../../relational-databases/query-processing-architecture-guide.md)    
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Средства контроля и настройки производительности](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Открытие монитора активности (среда SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Монитор активности](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Справочник по логическим и физическим операторам Showplan](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
 [Инфраструктура профилирования запросов](../../relational-databases/performance/query-profiling-infrastructure.md)