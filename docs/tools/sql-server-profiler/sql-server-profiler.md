---
title: Приложение SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Изучите функции SQL Server Profiler. Получите помощь в устранении неполадок, используя это средство для создания трассировок, а также анализа и воспроизведения результатов трассировки.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 3ad5f33d-559e-41a4-bde6-bb98792f7f1a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 05/01/2020
ms.openlocfilehash: 3169f8fbbc86fc95a62631c0cc93d77b6a46b0a4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643427"
---
# <a name="sql-server-profiler"></a>Приложение SQL Server Profiler

 [!INCLUDE[sql-asdbmi](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] — это интерфейс для создания трассировок и управления ими, а также для анализа и воспроизведения полученных результатов. События сохраняются в файле трассировки, который затем может быть проанализирован или использован для воспроизведения определенных последовательностей шагов для выявления возникших проблем.

> [!IMPORTANT]
> Трассировка SQL и [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] являются устаревшими. Пространство имен *Microsoft.SqlServer.Management.Trace*, которое содержит объекты трассировки Microsoft SQL Server и Replay, также устаревшее.
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> Вместо этого используйте расширенные события. Дополнительные сведения о [расширенных событиях](../../relational-databases/extended-events/extended-events.md) см. в разделе [Быстрое начало. Расширенные события в SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) и [SSMS XEvent Profiler](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] для рабочих нагрузок Analysis Services поддерживается.

> [!NOTE]
> При попытке подключиться к Базе данных SQL Azure из SQL Server Profiler она некорректно выдает сообщение об ошибке, как показано ниже.
>
> - Чтобы выполнить трассировку SQL Server, необходимо быть членом предопределенной роли сервера sysadmin или иметь разрешение ALTER TRACE.
>
> В сообщении должно объясняться, что База данных SQL Azure не поддерживается SQL Server Profiler.

## <a name="where-is-the-profiler"></a>Где находится профилировщик?

В [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] профилировщик можно запустить несколькими способами. [В этом разделе описано, как запустить профилировщик.](start-sql-server-profiler.md)

## <a name="capture-and-replay-trace-data"></a>Захват и воспроизведение данных трассировки

В следующей таблице показаны компоненты, с помощью которых мы рекомендуем выполнять захват и воспроизведение данных трассировки в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .

||||
|-|-|-|
|**Компонент\целевая рабочая нагрузка**|**Реляционный механизм**|**Службы Analysis Services**|  
|**Запись трассировки**|Графический пользовательский интерфейс [расширенных событий](../../relational-databases/extended-events/extended-events.md) в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|  
|**Воспроизведение трассировки**|[Распределенное воспроизведение](../distributed-replay/sql-server-distributed-replay.md)|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|

## <a name="use-sql-server-profiler"></a>Использование SQL Server Profiler

Приложение Microsoft [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] — это графический пользовательский интерфейс для трассировки SQL, с помощью которого можно наблюдать за экземпляром компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] или службами Analysis Services. Приложение позволяет собирать и сохранять данные о каждом событии в файле или в таблице для последующего анализа. Например, с помощью приложения можно следить за производственной средой, чтобы определить, какие хранимые процедуры снижают производительность из-за того, что выполняются слишком медленно. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] используется для таких действий, как:

- пошаговое выполнение проблемных запросов для поиска источника проблемы;

- выявление и диагностика медленно работающих запросов;

- перехват серии инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] , ведущих к проблеме. Сохраненную трассировку затем можно использовать для моделирования и диагностики проблемы на тестовом сервере;

- контроль производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для настройки рабочих нагрузок. Дополнительные сведения о настройке физической структуры базы данных для рабочих нагрузок см. в разделе [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).

- Анализ счетчиков производительности для диагностики проблем.

Приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] поддерживает также аудит действий, выполняемых в экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В ходе аудита ведется запись действий, связанных с безопасностью, для дальнейшего просмотра администратором безопасности.

## <a name="sql-server-profiler-concepts"></a>Основные понятия приложения SQL Server Profiler

Для использования [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]необходимо понимать термины, описывающие работу данного средства.

> [!NOTE]
> Понимание трассировки SQL особенно полезно при работе с [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Дополнительные сведения см. в статье [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

### <a name="event"></a>Событие

Событие — это действие экземпляра [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Примеры:  
  
- подключения пользователей, сбои, отключения;    
- Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], `SELECT`, `INSERT`, `UPDATE` и `DELETE`.
- состояние пакетов вызова удаленных процедур (RPC);  
- запуск или завершение хранимой процедуры;  
- инструкции запуска или завершения в хранимых процедурах;  
- запуск или завершение пакета SQL;  
- запись ошибки в журнал [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
- получение блокировки или освобождение объекта базы данных;  
- открытый курсор;  
- проверки права доступа.  
  
Все данные, создаваемые событием, отображаются в трассировке одной строкой. Эта строка пересекается столбцами данных, подробно описывающими данное событие.  

### <a name="eventclass"></a>EventClass

Класс событий — это тип трассируемого события. Класс событий содержит все данные, которые может сообщить событие. Примеры классов событий

- **SQL:BatchCompleted**
- **Аудит входа в систему**
- **Аудит выхода из системы**
- **Блокировка: получено**
- **Блокировка: удалено**

### <a name="eventcategory"></a>EventCategory

Категория событий определяет способы группировки событий в [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Например, все классы событий блокировок группируются в категории событий **Блокировки** . Однако категории событий существуют только в [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Этот термин не отображает способ группировки событий ядра.

### <a name="datacolumn"></a>DataColumn

Столбец данных — это атрибут классов событий, захваченных при трассировке. Поскольку класс событий определяет тип собираемых данных, не все столбцы данных применимы ко всем классам событий. Например, при трассировке, которая захватывает событие **Блокировка: получено**, столбец данных **BinaryData** содержит значение идентификатора блокированной страницы или строки, а столбец данных **Integer Data** не содержит никаких значений, поскольку он неприменим к захватываемому классу событий.

### <a name="template"></a>Шаблон

Шаблон определяет конфигурацию трассировки по умолчанию. А именно, он включает классы событий, которые нужно контролировать в [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Например, можно создать шаблон, указывающий используемые события, столбцы данных и фильтры. Шаблоны не выполняются, а сохраняются в файлах с расширением TDF. После сохранения шаблон управляет захватом данных, если запускается трассировка, основанная на этом шаблоне.

### <a name="trace"></a>Трассировка

Трассировка захватывает данные на основании выбранных классов событий, столбцов данных и фильтров. Например, можно создать трассировку для контроля ошибок исключений. Для этого выберите класс событий **Exception** и столбцы данных **Error**, **State** и **Severity** . Необходимо собирать данные этих трех столбцов, чтобы результаты трассировки содержали значимые данные. Теперь можно запустить трассировку, настроенную таким образом, и собирать данные обо всех событиях класса **Exception** на данном сервере. Данные трассировки можно сохранить или немедленно проанализировать. Трассировки можно воспроизводить впоследствии, хотя некоторые события, например класса **Исключение** , воспроизвести нельзя. Можно также сохранить трассировку как шаблон для построения аналогичных трассировок в будущем.  

В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предусмотрено два способа трассировки экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: с помощью [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] или системных хранимых процедур.  

### <a name="filter"></a>Filter

При создании трассировки или шаблона можно определить критерии для фильтрации данных, собираемых событием. Чтобы трассировки не становились слишком большими, можно устанавливать фильтры, чтобы собирать только подмножества данных о событиях. Например, в трассировке можно указать определенные имена пользователей Microsoft Windows, тем самым уменьшив объем выходных данных.  

Если фильтр не установлен, то на выход трассировки возвращаются все события выбранных классов событий.

## <a name="sql-server-profiler-tasks"></a>Задачи приложения SQL Server Profiler

|Описание задачи|Раздел|  
|----------------------|-----------|  
|Позволяет получить список предопределенных шаблонов, с помощью которых приложение SQL Server выполняет мониторинг событий определенных типов, а также список разрешений, используемых для воспроизведения трассировок.|[Шаблоны и разрешения приложения SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)|  
|Описание процесса запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler.|[Разрешения, необходимые для запуска приложения SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|  
|Содержит описание процесса создания трассировки.|[Создание трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса указания определенных событий и столбцов данных для файла трассировки.|[Указание столбцов событий и данных для файла трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)|  
|Содержит описание процесса сохранения результатов трассировки в файл.|[Сохранение результатов трассировки в файл (приложение SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)|  
|Содержит описание процесса сохранения результатов трассировки в таблице.|[Сохранение результатов трассировки в таблицу (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)|  
|Содержит описание процесса фильтрации событий в трассировке.|[Фильтрация событий в трассировке (приложение SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса просмотра сведений о фильтре.|[Просмотр сведений о фильтре (приложение SQL Server Profiler)](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)|  
|Содержит описание процесса изменения фильтра.|[Изменение фильтра (приложение SQL Server Profiler)](../../tools/sql-server-profiler/modify-a-filter-sql-server-profiler.md)|  
|Содержит описание процесса установки максимального размера файла для файла трассировки ([!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]).|[Установка максимального размера для файла трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/set-a-maximum-file-size-for-a-trace-file-sql-server-profiler.md)|  
|Содержит описание процесса установки максимального размера для таблицы трассировки.|[Установка максимального размера для таблицы трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/set-a-maximum-table-size-for-a-trace-table-sql-server-profiler.md)|  
|Содержит описание процесса запуска трассировки.|[Запуск трассировки](../../tools/sql-server-profiler/start-a-trace.md)|  
|Содержит описание автоматического процесса запуска трассировки после соединения с сервером.|[Автоматический запуск трассировки после соединения с сервером (приложение SQL Server Profiler)](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)|  
|Содержит описание процесса фильтрации событий по времени начала события.|[Фильтрация событий по времени начала (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-based-on-the-event-start-time-sql-server-profiler.md)|  
|Содержит описание процесса фильтрации событий по времени окончания события.|[Фильтрация событий по времени окончания (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-based-on-the-event-end-time-sql-server-profiler.md)|  
|Содержит описание процесса фильтрации идентификаторов процесса сервера (SPID) в трассировке.|[Фильтрация идентификаторов серверных процессов (SPID) в трассировке (приложение SQL Server Profiler)](../../tools/sql-server-profiler/filter-server-process-ids-spids-in-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса приостановки трассировки.|[Приостановка трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса прекращения трассировки.|[Остановка трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса запуска трассировки после ее приостановки или прекращения.|[Проведение трассировки после паузы или остановки (SQL Server Profiler)](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)|  
|Содержит описание процесса очистки окна трассировки.|[Очистка окна трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)|  
|Содержит описание процесса закрытия окна трассировки.|[Закрытие окна трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/close-a-trace-window-sql-server-profiler.md)|  
|Содержит описание процесса настройки трассировки по умолчанию.|[Установка значений по умолчанию для определения трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md)|  
|Содержит описание процесса настройки отображения трассировки по умолчанию.|[Настройка параметров отображения трассировки по умолчанию (приложение SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)|  
|Содержит описание процесса открытия файла трассировки.|[Открыть файл трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)|  
|Содержит описание процесса открытия таблицы трассировки.|[Открыть таблицу трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)|  
|Содержит описание процесса воспроизведения таблицы трассировки.|[Воспроизведение таблицы трассировки (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)|  
|Содержит описание процесса воспроизведения файла трассировки.|[Воспроизведение файла трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)|  
|Содержит описание процесса воспроизведения одиночного события за раз.|[Воспроизведение одиночного события за раз (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-single-event-at-a-time-sql-server-profiler.md)|  
|Содержит описание процесса воспроизведения до точки останова.|[Воспроизведение нагрузки до точки останова (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)|  
|Содержит описание процесса воспроизведения до курсора.|[Воспроизведение нагрузки до курсора (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-cursor-sql-server-profiler.md)|  
|Описание процесса воспроизведения скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)].|[Воспроизведение скрипта на языке Transact-SQL (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-transact-sql-script-sql-server-profiler.md)|  
|Содержит описание процесса создания шаблона трассировки.|[Создание шаблона трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)|  
|Содержит описание процесса изменения шаблона трассировки.|[Изменение шаблона трассировки (приложение SQL Server Profiler)](./modify-trace-templates.md)|  
|Содержит описание процесса настройки глобальных параметров трассировки.|[Настройка глобальных параметров трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/set-global-trace-options-sql-server-profiler.md)|  
|Содержит описание процесса поиска значения или столбца данных во время трассировки.|[Поиск значения или столбца данных во время трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/find-a-value-or-data-column-while-tracing-sql-server-profiler.md)|  
|Содержит описание процесса создания шаблона на основе выполняемой трассировки.|[Извлечь шаблон из выполняемой трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)|  
|Содержит описание процесса создания шаблона на основе файла или таблицы трассировки.|[Извлечь шаблон из файла трассировки или таблицы трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)|  
|Описание процесса создания скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)] на основе выполняемой трассировки.|[Создание скрипта Transact-SQL для выполнения трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/create-a-transact-sql-script-for-running-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса экспорта шаблона трассировки.|[Экспорт шаблона трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)|  
|Содержит описание процесса импорта шаблона трассировки.|[Импорт шаблона трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)|  
|Содержит описание процесса извлечения скрипта из трассировки.|[Извлечение скрипта из трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/extract-a-script-from-a-trace-sql-server-profiler.md)|  
|Содержит описание процесса согласования трассировки с данными журнала производительности Windows.|[Сопоставить трассировку с данными журнала производительности Windows (приложение SQL Server Profiler)](./correlate-a-trace-with-windows-performance-log-data.md)|  
|Содержит описание процесса упорядочения столбцов, отображаемых в трассировке.|[Упорядочивание столбцов, отображаемых в трассировке (приложение SQL Server Profiler)](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md)|  
|Описание процесса запуска [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Запуск приложения SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)|  
|Содержит описание процесса сохранения трассировок и шаблонов трассировок.|[Сохранение трассировок и шаблонов трассировок](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|  
|Содержит описание процесса изменения шаблонов трассировок.|[Изменение шаблонов трассировки](../../tools/sql-server-profiler/modify-trace-templates.md)|  
|Содержит описание процесса согласования трассировки с данными журнала производительности Windows.|[Сопоставление трассировки с журналом производительности Windows](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|  
|Описание процесса просмотра и анализа трассировок с помощью [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Просмотр и анализ трассировок с помощью приложения SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|  
|Описание процесса анализа взаимоблокировок с помощью [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Анализ взаимоблокировок в приложении SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|  
|Содержит описание процесса анализа запросов с помощью результатов инструкции SHOWPLAN в приложении SQL Server Profiler.|[Анализ запросов с помощью результатов инструкции SHOWPLAN в приложении SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|  
|Описание процесса анализа взаимоблокировок с помощью [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Фильтрация трассировок с помощью приложения SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|  
|Здесь описывается использование возможностей воспроизведения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Воспроизведение трассировок](../../tools/sql-server-profiler/replay-traces.md)|  
|Список зависящих от контекста тем справки для [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Справка F1 приложения SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-f1-help.md)|  
|Позволяет получить список системных хранимых процедур, используемых [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] для контроля производительности и активности.|[Хранимые процедуры приложения SQL Server Profiler (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|  

## <a name="see-also"></a>См. также раздел

- [Категория событий Locks](../../relational-databases/event-classes/locks-event-category.md)
- [Категория событий Sessions](../../relational-databases/event-classes/sessions-event-category.md)
- [Категория событий Stored Procedures](../../relational-databases/event-classes/stored-procedures-event-category.md)
- [Категория событий TSQL](../../relational-databases/event-classes/tsql-event-category.md)
- [Мониторинг производительности и действий сервера](../../relational-databases/performance/server-performance-and-activity-monitoring.md)