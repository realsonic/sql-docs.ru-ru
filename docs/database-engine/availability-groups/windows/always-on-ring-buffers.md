---
title: Кольцевые буферы для получения сведений о работоспособности в группах доступности
description: Получение определенных диагностических сведений о группах доступности Always On с помощью кольцевых буферов SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 47bb7a1a-c0a5-473c-a7db-d9f4bf3ee650
author: rothja
ms.author: jroth
ms.openlocfilehash: 1feb461ad05167b4669da2ee53c17342bba98195
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641454"
---
# <a name="use-ring-buffers-to-obtain-health-information-about-always-on-availability-groups"></a>Использование кольцевых буферов для получения сведений о работоспособности в группах доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Некоторую диагностическую информацию о группах доступности AlwaysOn можно получить из кольцевых буферов SQL Server или динамического административного представления sys.dm_os_ring_buffers. Кольцевые буферы создаются при установке SQL Server и регистрируют предупреждения в системе SQL Server для внутренней диагностики. Они не поддерживаются, однако из них можно извлечь ценную информацию при устранении неполадок. Эти кольцевые буферы предоставляют еще один источник диагностических сведений при зависании или сбое SQL Server.  
  
 Приведенный ниже запрос Transact-SQL (T-SQL) возвращает все записи событий из кольцевых буферов групп доступности.  
  
```sql  
SELECT * FROM sys.dm_os_ring_buffers WHERE ring_buffer_type LIKE '%HADR%'  
```  
  
 Чтобы улучшить управляемость данных, отфильтруйте их по дате и типу кольцевого буфера. Приведенный ниже запрос возвращает записи из указанного кольцевого буфера, возникшего сегодня.  
  
```sql  
DECLARE @runtime datetime  
SET @runtime = GETDATE()  
SELECT CONVERT (varchar(30), @runtime, 121) as data_collection_runtime,   
DATEADD (ms, -1 * (inf.ms_ticks - ring.[timestamp]), GETDATE()) AS ring_buffer_record_time,   
ring.[timestamp] AS record_timestamp, inf.ms_ticks AS cur_timestamp, ring.*   
FROM sys.dm_os_ring_buffers ring  
CROSS JOIN sys.dm_os_sys_info inf where ring_buffer_type='<RING_BUFFER_TYPE>'  
```  
  
 Столбец "Record" (Запись) в каждой записи содержит диагностические данные в формате XML. Эти XML-данные различаются в зависимости от типа кольцевого буфера. Дополнительные сведения по каждому типу кольцевого буфера см. в разделе [Типы кольцевых буферов для групп доступности](#BKMK_RingBufferTypes). Чтобы улучшить читаемость XML-данных, нужно настроить запрос T-SQL для извлечения требуемых XML-элементов. Например, приведенный ниже запрос возвращает все события из кольцевого буфера RING_BUFFER_HADRDBMGR_API и форматирует XML-данные по отдельным столбцам таблицы.  
  
```sql  
WITH hadr(ts, type, record) AS  
(  
  SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
  FROM sys.dm_os_ring_buffers WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API'  
)  
SELECT   
  ts,  
  type,  
  record.value('(./Record/@id)[1]','bigint') AS [Record ID],  
  record.value('(./Record/@time)[1]','bigint') AS [Time],  
  record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [DBID],  
  record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
  record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
  record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
  record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
##  <a name="availability-groups-ring-buffer-types"></a><a name="BKMK_RingBufferTypes"></a> Типы кольцевых буферов для групп доступности  
 В sys.dm_os_ring_buffers существует четыре кольцевых буфера для групп доступности. Приведенная ниже таблица описывает типы кольцевых буферов и пример содержимого в столбце "Record" (Запись) для каждого из них.  
  
 **RING_BUFFER_HADRDBMGR_API**  
  
 Выполненные или выполняемые переходы состояния записей. При просмотре переходов состояния обратите внимание на значения objectType.  
  
```xml  
<Record id="11" type="RING_BUFFER_HADRDBMGR_STATE" time="860243">  
  <HadrDbMgrState>  
    <objectType>HadrUsers</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>1</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_STATE**  
  
 Записывает вызовы внутреннего метода или функции, выполненные операциями AlwaysOn. Может показывать такие сведения, как приостановка, возобновление или изменения роли, включая точки входа и выхода.  
  
```xml  
<Record id="45" type="RING_BUFFER_HADRDBMGR_STATE" time="1723487912">  
  <HadrDbMgrState>  
    <dbId>5</dbId>  
    <objectType>HadrDbMgr</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>2</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_COMMIT**  
  
```xml  
<Record id="0" type="RING_BUFFER_HADRDBMGR_COMMIT" time="1723475368">  
  <HadrDbMgrCommitPolicy>  
    <dbId>5</dbId>  
    <replicaId>883a18f5-97d5-450f-8f8f-9983a4fa5299</replicaId>  
    <dbHardenPolicy>KillAll</dbHardenPolicy>  
    <dbSyncConfig>0x0</dbSyncConfig>  
    <syncPartnerCount>0</syncPartnerCount>  
    <minSyncPartnerConfig>0</minSyncPartnerConfig>  
    <partnerHardenPolicy>KillAll</partnerHardenPolicy>  
    <partnerSyncConfig>0x0</partnerSyncConfig>  
    <logBlock>0x0000000000000000</logBlock>  
    <leaseExpired>Y</leaseExpired>  
    <partnerChange>N</partnerChange>  
    <role>2</role>  
  </HadrDbMgrCommitPolicy>  
</Record>  
```  
  
 **RING_BUFFER_HADR_TRANSPORT_STATE**  
  
```xml  
<Record id="3" type="RING_BUFFER_HADR_TRANSPORT_STATE" time="1723485399">  
  <HadrTransportState>  
    <agId>08264B79-D10B-412F-B38D-CA07B08E9BD8</agId>  
    <localArId>883A18F5-97D5-450F-8F8F-9983A4FA5299</localArId>  
    <targetArId>628D6349-72DD-4D18-A6E1-1272645660BA</targetArId>  
    <currentState>HadrSession_Configuring</currentState>  
    <targetState>HadrSession_Connected</targetState>  
    <legalTransition>Y</legalTransition>  
  </HadrTransportState>  
</Record>  
```  
  
## <a name="parse-xml-data-from-a-ring-buffer"></a>Анализ XML-данных из кольцевого буфера  
 Вы можете проанализировать поле "Record" (Запись) из проверяемого кольцевого буфера, используя в запросе [метод value() (тип данных XML)](~/t-sql/xml/value-method-xml-data-type.md). Чтобы использовать этот метод, сначала нужно [привести](~/t-sql/functions/cast-and-convert-transact-sql.md) столбец записи в кольцевом буфере к XML. Например, приведенный ниже запрос показывает, как проанализировать RING_BUFFER_HADRDBMGR_API для приведения его в удобочитаемый формат с помощью этого метода.  
  
```sql 
WITH hadr(ts, type, record) AS  
   (SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API')  
SELECT ts,  
type,  
record.value('(./Record/@id)[1]','bigint') AS [Record id],  
record.value('(./Record/@time)[1]','bigint') AS [Time],  
record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [dbid],  
record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
  
