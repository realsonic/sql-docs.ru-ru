---
title: Просмотр и чтение журнала диагностики для экземпляра отказоустойчивого кластера
description: Узнайте, как просматривать и читать журнал выполнения диагностики, который создается экземпляром отказоустойчивого кластера SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 68074bd5-be9d-4487-a320-5b51ef8e2b2d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 61b9788f0f8ee76979869c9775d81990435bdb5b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642701"
---
# <a name="view-and-read-failover-cluster-instance-diagnostics-log"></a>Просмотр и чтение журнала диагностики экземпляра отказоустойчивого кластера
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Все критические ошибки и события предупреждений для библиотеки ресурсов SQL Server записываются в журнал событий Windows. Диагностические сведения, связанные с SQL Server и записываемые в журнал, перехватываются хранимой процедурой [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) и записываются в файлы журнала диагностики отказоустойчивого кластера SQL Server (также называемые журналами *SQLDIAG*).  
  
-   **Перед началом работы**  [Рекомендации](#Recommendations), [Безопасность](#Security)  
  
-   **Просмотр журнала диагностики с помощью следующего:**  [среды SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
-   **Настройка параметров журнала диагностики с помощью следующего:** [Transact-SQL](#TsqlConfigure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
 По умолчанию журналы SQLDIAG хранятся в локальной папке LOG в каталоге экземпляра SQL Server, например C:\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\LOG, на том узле, где выполняется экземпляр отказоустойчивого кластера (FCI). Размер каждого файла журнала SQLDIAG ограничен 100 МБ. На компьютере сохраняются десять таких файлов журнала, после чего они освобождаются для новых журналов.  
  
 В журналах используется формат файлов расширенных событий. Для чтения файлов, созданных расширенными событиями, можно использовать системную функцию **sys.fn_xe_file_target_read_file** . Возвращается одно событие в каждой строке в формате XML. Выполните запрос к системному представлению для синтаксического анализа XML-данных в виде результирующего набора. Дополнительные сведения см. в разделе [sys.fn_xe_file_target_read_file (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для запуска **fn_xe_file_target_read_file** требуется разрешение VIEW SERVER STATE.  
  
 Откройте среду SQL Server Management Studio в качестве администратора  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Просмотр файлов журнала диагностики:**  
  
1.  В меню **Файл** выберите **Открыть**, **Файл** и выберите файл журнала диагностики для просмотра.  
  
2.  События отображаются в виде строк в правой панели, причем по умолчанию показаны только два столбца, **name** и **timestamp** .  
  
     Это также приводит к активации меню **ExtendedEvents** .  
  
3.  Для отображения большего числа столбцов перейдите в меню **ExtendedEvents** и укажите **Выбрать столбцы**.  
  
     Откроется диалоговое окно с доступными столбцами, позволяя выбрать столбцы для отображения.  
  
4.  Можно фильтровать и сортировать данные событий, используя меню **ExtendedEvents** и выбирая параметр **Фильтр** .  
  
##  <a name="view-diagnostic-log-files-with-transact-sql"></a><a name="TsqlProcedure"></a> Просмотр файлов журнала диагностики с помощью Transact-SQL  
 **Просмотр файлов журнала диагностики:**  
  
 Для просмотра всех записей в файле журнала SQLDIAG используйте следующий запрос.  
  
```  
SELECT  
xml_data.value('(event/@name)[1]','varchar(max)') AS 'Name'  
,xml_data.value('(event/@package)[1]','varchar(max)') AS 'Package'  
,xml_data.value('(event/@timestamp)[1]','datetime') AS 'Time'  
,xml_data.value('(event/data[@name=''state'']/value)[1]','int') AS 'State'  
,xml_data.value('(event/data[@name=''state_desc'']/text)[1]','varchar(max)') AS 'State Description'  
,xml_data.value('(event/data[@name=''failure_condition_level'']/value)[1]','int') AS 'Failure Conditions'  
,xml_data.value('(event/data[@name=''node_name'']/value)[1]','varchar(max)') AS 'Node_Name'  
,xml_data.value('(event/data[@name=''instancename'']/value)[1]','varchar(max)') AS 'Instance Name'  
,xml_data.value('(event/data[@name=''creation time'']/value)[1]','datetime') AS 'Creation Time'  
,xml_data.value('(event/data[@name=''component'']/value)[1]','varchar(max)') AS 'Component'  
,xml_data.value('(event/data[@name=''data'']/value)[1]','varchar(max)') AS 'Data'  
,xml_data.value('(event/data[@name=''info'']/value)[1]','varchar(max)') AS 'Info'  
FROM  
 ( SELECT object_name AS 'event'  
  ,CONVERT(xml,event_data) AS 'xml_data'  
  FROM sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\SQLNODE1_MSSQLSERVER_SQLDIAG_0_129936003752530000.xel',NULL,NULL,NULL)   
)   
AS XEventData  
ORDER BY Time;  
  
```  
  
> [!NOTE]  
>  Можно отфильтровать результаты для определенных компонентов или состояний с помощью предложения WHERE.  
  
##  <a name="configure-diagnostic-log-properties-with-transact-sql"></a><a name="TsqlConfigure"></a> Настройка свойств журнала диагностики с помощью Transact-SQL  
 **Настройка свойств журнала диагностики:**  
  
> [!NOTE]  
>  Пример этой процедуры см. в подразделе [Примеры (Transact-SQL)](#TsqlExample)далее в этом разделе.  
  
 С помощью инструкции языка описания данных DDL **ALTER SERVER CONFIGURATION** можно запускать или останавливать запись в журнал диагностических данных, полученных с помощью хранимой процедуры [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md), а также задавать такие параметры конфигурации журналов SQLDIAG, как количество переключений файлов журнала, размер файлов журнала и расположение файлов. Дополнительные сведения о синтаксисе см. в разделе [Setting diagnostic log options](../../../t-sql/statements/alter-server-configuration-transact-sql.md#Diagnostic).  
  
###  <a name="examples-transact-sql"></a><a name="ConfigTsqlExample"></a> Примеры (Transact-SQL)  
  
####  <a name="setting-diagnostic-log-options"></a><a name="TsqlExample"></a> Setting diagnostic log options  
 В примерах этого раздела показана установка значений параметра журнала диагностики.  
  
##### <a name="a-starting-diagnostic-logging"></a>A. Запуск регистрации диагностических данных в журнале  
 В следующем примере запускается запись в журнал диагностических данных.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
##### <a name="b-stopping-diagnostic-logging"></a>Б. Останов регистрации диагностических данных в журнале  
 В следующем примере запись в журнал диагностических данных прекращается.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
##### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>В. Задание расположения журналов диагностических данных  
 В следующем примере для журналов диагностических данных задается расположение по указанному пути к файлам.  
  
```  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
##### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>Г. Задание максимального размера каждого из журналов диагностики  
 В следующем примере задан максимальный размер каждого из журналов диагностики, равный 10 мегабайтам.  
  
```  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
## <a name="see-also"></a>См. также:  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
