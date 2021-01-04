---
title: Счетчики производительности для службы ReportServer, объекты производительности | Документы Майкрософт
description: Узнайте больше о счетчиках производительности для объектов производительности ReportServer:Service и ReportServerSharePoint:Service, входящих в развертывание SQL Server 2012.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8224c126d9f23cb5f13c517400ec79e04d127ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466615"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>Счетчики производительности для службы ReportServer, объекты производительности
  В этом разделе описываются счетчики производительности для объектов производительности **ReportServer:Service** и **ReportServerSharePoint:Service** , входящих в развертывание [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .  
  
> [!NOTE]  
>  Эти объекты производительности служат для наблюдения за событиями на локальном сервере отчетов. При запуске сервера отчетов в масштабном развертывании счетчики относятся к текущему серверу, а не к масштабному развертыванию в целом.  
  
 Объекты производительности доступны в системном мониторе Windows (**Perfmon.exe**). Дополнительные сведения см. в документации по Windows. [Профилирование среды выполнения](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 В этом разделе:  
  
-   [Счетчики производительности ReportServer:Service (сервер отчетов в собственном режиме)](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service (сервер отчетов в режиме SharePoint)](#bkmk_ReportServerSharePoint)  
  
-   [Использование командлетов PowerShell для возврата списков](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> Счетчики производительности ReportServer:Service (сервер отчетов в собственном режиме)  
 Объект производительности **ReportServer:Service** включает коллекцию счетчиков для отслеживания связанных с HTTP и памятью событий для экземпляра сервера отчетов. Этот объект производительности отображается однократно для каждого экземпляра служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] на компьютере; счетчики объекта производительности можно добавлять или удалять для каждого экземпляра. Счетчики для экземпляра по умолчанию отображаются в формате **ReportServer:Service**. Счетчики для именованных экземпляров отображаются в формате **ReportServer$\<**_instance_name_*_>:Service_* .  
  
 Объект производительности **ReportServer:Service** впервые появился в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Он предоставляет часть счетчиков, входивших в службы IIS и в [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] в предыдущих версиях служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Эти новые счетчики являются уникальными для служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Они отслеживают связанные с HTTP события для сервера отчетов, такие как запросы, соединения и попытки входа. Кроме того, этот объект производительности включает счетчики для отслеживания событий управления памятью.  
  
 В следующей таблице перечислены счетчики, включенные в объект производительности **ReportServer:Service** .  
  
 ![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell") Следующий скрипт Windows PowerShell вернет список счетчиков производительности для CounterSetName  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|Счетчик|Описание|  
|-------------|-----------------|  
|**Активные соединения**|Количество активных в текущий момент соединений на сервере.|  
|**Всего получено байтов**|Число байт, полученных сервером. Этот счетчик ведет подсчет общего приблизительного числа байтов, полученных как диспетчером отчетов, так и сервером отчетов.|  
|**Получено байт/с**|Число байтов, полученных сервером за одну секунду. Этот счетчик обновляется только при завершении передачи. Это означает, что значение счетчика остается равным 0, а затем значение растет после завершения передачи.|  
|**Всего отправлено байтов**|Число байтов, отправленных сервером. Этот счетчик ведет подсчет общего приблизительного числа байтов, оправленных как диспетчером отчетов, так и сервером отчетов.|  
|**Отправлено байт/с**|Число байтов, отправленных сервером за секунду. Этот счетчик обновляется только при завершении передачи. Это означает, что значение счетчика остается равным 0, а затем значение растет после завершения передачи.|  
|**Общее число ошибок**|Общее число ошибок, возникших во время выполнения HTTP-запросов. Эти ошибки включают 400-е и 500-е коды состояния HTTP.|  
|**Ошибок/с**|Общее число ошибок, возникших во время выполнения HTTP-запросов за одну секунду. Эти ошибки включают 400-е и 500-е коды состояния HTTP.|  
|**Всего попыток входа**|Число попыток входа, выполненных из типов проверки подлинности RSWindows. Типы проверок подлинности RSWindows включают RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos и RSWindowsBasic. Нулевое значение (0) отвечает за нестандартную проверку подлинности.|  
|**Попыток входа в секунду**|Число попыток входа.|  
|**Всего успешных входов**|Число успешных входов для типов проверки подлинности RSWindows. Типы проверок подлинности RSWindows включают RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos и RSWindowsBasic. Нулевое значение (0) отвечает за нестандартную проверку подлинности.|  
|**Успешных входов в секунду**|Число успешных входов.|  
|**Состояние нагрузки на память**|Одно из следующих чисел от 1 до 5, указывающее текущее состояние памяти на сервере.<br /><br /> 1: нет нагрузки<br /><br /> 2: низкая нагрузка<br /><br /> 3: средняя нагрузка<br /><br /> 4: высокая нагрузка<br /><br /> 5: чрезмерная нагрузка|  
|**Объем сжатия памяти**|Число байтов, запрошенных сервером для сжатия используемой памяти.|  
|**Уведомлений о сжатии памяти в секунду**|Число уведомлений, сформированных сервером в последнюю секунду для сжатия используемой памяти. Это число указывает, как часто сервер испытывает недостаток памяти.|  
|**Количество отключенных запросов**|Число отсоединений запросов, возникших из-за сбоя в канале связи.|  
|**Количество выполняющихся запросов**|Количество запросов, обрабатываемых в настоящий момент.|  
|**Количество неразрешенных запросов**|Число запросов, завершенных с кодом состояния HTTP 401.|  
|**Количество отказанных запросов**|Общее число запросов, не обработанных из-за недостаточных ресурсов сервера. Этот счетчик отражает число запросов, возвращенных с кодом состояния HTTP 503, указывающим на то, что сервер слишком загружен.|  
|**Всего запросов**|Общее число запросов, полученных службой сервера отчетов с момента запуска. Этот счетчик ведет подсчет запросов, отправленных диспетчеру отчетов, а также запросов, отправленных диспетчером отчетов серверу отчетов.|  
|**Запросов/с**|Количество обрабатываемых за секунду запросов. Это значение отражает текущую пропускную способность приложения.|  
|**Количество задач в очереди**|Число задач, ожидающих доступности потока для обработки. Каждый запрос, выполненный к серверу отчетов, соответствует одной или нескольким задачам. Этот счетчик представляет только число задач, готовых к обработке; он не включает число выполняющихся в настоящее время задач.|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service (сервер отчетов в режиме SharePoint)  
 Объект производительности **ReportServerSharePoint:Service** был добавлен в версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 ![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell") Следующий скрипт Windows PowerShell вернет список счетчиков производительности для CounterSetName  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|Счетчик|Описание|  
|-------------|-----------------|  
|**Состояние нагрузки на память**||  
|**Объем сжатия памяти**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Использование командлетов PowerShell для возврата списков  
 ![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell") Следующий скрипт Windows PowerShell вернет список счетчиков производительности для CounterSetName "ReportServerSharePoint:Service":  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>См. также:  
 [Наблюдение за производительностью сервера отчетов](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Счетчики производительности для объектов производительности веб-службы MSRS 2011 и службы Windows MSRS 2011 (собственный режим)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Счетчики производительности для объектов производительности веб-службы MSRS 2011 и службы Windows MSRS 2011 в режиме интеграции с SharePoint (режим интеграции с SharePoint)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
