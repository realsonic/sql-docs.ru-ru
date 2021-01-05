---
title: Выполнение приложения SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Узнайте, из каких программ и меню можно запускать SQL Server Profiler и какие фильтры, шаблоны и контексты подключения используются с выходными данными трассировки.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 22e57ffa-63b0-4de3-b92e-df297dda1226
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 09a6d7f612e9a66ad98849ec1c7672b44387d43e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641949"
---
# <a name="run-sql-server-profiler"></a>Выполнение приложения SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] можно запустить несколькими способами для получения данных трассировки в разных сценариях. Можно запустить приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] из меню **Пуск** Windows 10, из меню **Сервис** в помощнике по настройке компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] и из нескольких расположений в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
При первом запуске [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] и выборе в меню **Файл** пункта **Создать трассировку** приложение отображает диалоговое окно **Соединение с сервером**, где можно указать экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подключения.  
## <a name="to-start-sql-server-profiler-from-the-windows-10-start-menu"></a>Запуск приложения SQL Server Profiler из меню "Пуск" Windows 10  
-  Щелкните значок Windows **Пуск** или нажмите клавишу Windows и начните набирать строку "SQL Server Profiler 17". Когда появится плитка **SQL Server Profiler 17**, щелкните ее.   

## <a name="to-start-sql-server-profiler-in-database-engine-tuning-advisor"></a>Запуск SQL Server Profiler в помощнике по настройке ядра СУБД  
-  В меню [!INCLUDE[ssDE](../../includes/ssde-md.md)] Сервис **помощника по настройке компонента** выберите **SQL Server Profiler**.  

## <a name="to-start-sql-server-profiler-in-sql-server-management-studio"></a>Запуск SQL Server Profiler в SQL Server Management Studio  
 Вы можете открыть [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] из нескольких расположений в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. При запуске приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] загружается контекст подключения, шаблон трассировки и выполняется фильтрация контекста точки запуска. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] запускает каждый сеанс SQL Server Profiler в отдельном экземпляре, выполнение которого продолжается после завершения работы [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
### <a name="to-start-sql-server-profiler-from-the-tools-menu"></a>Запуск приложения SQL Server Profiler из меню «Инструменты»  
-  В меню [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Инструменты** щелкните **SQL Server Profiler**.  

### <a name="to-start-sql-server-profiler-from-the-query-editor"></a>Запуск приложения SQL Server Profiler из редактора запросов  
- В редакторе запросов щелкните правой кнопкой мыши, а затем выберите пункт **Трассировка запроса в приложении SQL Server Profiler**.  

  > [!NOTE]  
  >  Контекстом соединения является редактор соединения, шаблон трассировки — TSQL_SP, а применяемый фильтр SPID = окно запроса SPID.  
    
### <a name="to-start-sql-server-profiler-from-activity-monitor"></a>Запуск приложения SQL Server Profiler из монитора активности  
- В мониторе активности щелкните панель **Процессы**, щелкните правой кнопкой процесс, который хотите профилировать, а затем выберите пункт **Трассировка процесса в приложении SQL Server Profiler**.  

    > [!NOTE]  
    >  Когда выбран процесс, контекстом соединения является соединение обозревателя объектов при открытии монитора активности. Шаблон трассировки — это шаблон по умолчанию в зависимости от типа сервера, а идентификатор SPID равен идентификатору SPID для выбранного процесса.  
    
## <a name="net-framework-security"></a>Безопасность .NET Framework  
- В режиме проверки подлинности Windows учетная запись пользователя, от имени которой запускается приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], должна иметь разрешение на подключение к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- Для выполнения трассировки в [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]пользователь должен также иметь разрешение ALTER TRACE.  

## <a name="next-steps"></a>Дальнейшие действия  
 [Обзор SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Использование среды SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)