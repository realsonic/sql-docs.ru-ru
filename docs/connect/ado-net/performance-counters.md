---
title: Счетчики производительности в SqlClient
description: Счетчики производительности поставщика данных Microsoft SqlClient для SQL Server можно использовать для отслеживания состояния приложения и используемых им ресурсов подключения с помощью Монитора производительности Windows или программным способом.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804292"
---
# <a name="performance-counters-in-sqlclient"></a>Счетчики производительности в SqlClient

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Счетчики производительности <xref:Microsoft.Data.SqlClient> можно использовать для мониторинга состояния приложения и используемых им ресурсов подключения. Показания счетчиков производительности можно отслеживать с помощью системного монитора Windows или получить к ним доступ программным путем с помощью класса <xref:System.Diagnostics.PerformanceCounter> в пространстве имен <xref:System.Diagnostics>.

## <a name="available-performance-counters"></a>Доступные счетчики производительности

В настоящее время имеется 14 различных счетчиков производительности, доступных для <xref:Microsoft.Data.SqlClient>, как показано в следующей таблице.

|Счетчик производительности|Описание|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|Количество соединений с сервером базы данных в секунду.|  
|`HardDisconnectsPerSecond`|Количество разрывов соединений с сервером базы данных в секунду.|  
|`NumberOfActiveConnectionPoolGroups`|Количество уникальных активных групп пулов соединений. Этот счетчик управляется числом уникальных строк соединения, найденных в домене приложения.|  
|`NumberOfActiveConnectionPools`|Общее число пулов соединений.|  
|`NumberOfActiveConnections`|Количество текущих активных соединений. **Примечание.**  Этот счетчик производительности по умолчанию не включен. Инструкции о том, как его включить, см. в разделе [Активация счетчиков, отключенных по умолчанию](#ActivatingOffByDefault).|  
|`NumberOfFreeConnections`|Количество соединений, доступных в пулах соединений. **Примечание.**  Этот счетчик производительности по умолчанию не включен. Инструкции о том, как его включить, см. в разделе [Активация счетчиков, отключенных по умолчанию](#ActivatingOffByDefault).|  
|`NumberOfInactiveConnectionPoolGroups`|Количество уникальных групп пулов соединений, отмеченных для усечения. Этот счетчик управляется числом уникальных строк соединения, найденных в домене приложения.|  
|`NumberOfInactiveConnectionPools`|Количество неактивных пулов соединений, не участвовавших в последних операциях и ожидающих удаления.|  
|`NumberOfNonPooledConnections`|Количество активных соединений, не помещенных в пулы.|  
|`NumberOfPooledConnections`|Количество активных соединений, которые управляются инфраструктурой пулов соединений.|  
|`NumberOfReclaimedConnections`|Количество соединений, затребованных сборкой мусора, в которой приложение не вызывает методы `Close` и `Dispose`. **Примечание.** Если подключения не закрывать и не удалять явно, производительность может снижаться.|  
|`NumberOfStasisConnections`|Количество соединений, ожидающих в настоящий момент завершения действия и поэтому доступных для приложения.|  
|`SoftConnectsPerSecond`|Количество активных соединений, извлекаемых из пула соединений. **Примечание.**  Этот счетчик производительности по умолчанию не включен. Инструкции о том, как его включить, см. в разделе [Активация счетчиков, отключенных по умолчанию](#ActivatingOffByDefault).|  
|`SoftDisconnectsPerSecond`|Количество активных соединений, возвращаемых в пул соединений. **Примечание.**  Этот счетчик производительности по умолчанию не включен. Инструкции о том, как его включить, см. в разделе [Активация счетчиков, отключенных по умолчанию](#ActivatingOffByDefault).|  

### <a name="connection-pool-groups-and-connection-pools"></a>Группы пула подключений и пулы подключений

При использовании проверки подлинности Windows (встроенная безопасность) необходимо следить за счетчиками `NumberOfActiveConnectionPoolGroups` и `NumberOfActiveConnectionPools`. Причина в том, что группы пулов соединений сопоставлены с уникальными строками соединений. Если используется встроенная безопасность, то пулы соединений сопоставляются со строками соединений и дополнительно создают специальные пулы для отдельных идентификаторов Windows. Например, если Кирилл и Мария, находящиеся в одном домене приложений, используют строку соединения `"Data Source=MySqlServer;Integrated Security=true"`, создается группа пула соединений для этой строки соединения и два дополнительных пула - один для Кирилла, другой для Марии. Если Петр и Елена используют строку подключения с одинаковым именем входа SQL Server `"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`, создается только один пул для идентификатора **<myUserID>** .

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>Активация счетчиков, отключенных по умолчанию

Счетчики производительности `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` и `SoftConnectsPerSecond` отключены по умолчанию. Чтобы включить их, добавьте в файл конфигурации приложения следующие данные:

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>Получение значений счетчиков производительности

В следующем приложении командной строки показан способ получения значений счетчиков производительности. Чтобы возвратить данные для всех счетчиков производительности поставщика данных Microsoft SqlClient для SQL Server, подключения должны быть открыты и активны.

> [!NOTE]
> Здесь используется пример [базы данных **AdventureWorks**](../../samples/adventureworks-install-configure.md). Строка подключения, представленная в примере кода, предполагает, что база данных установлена и доступна на локальном компьютере, и что созданы имена входа, соответствующие представленным в строках подключения. Имена входа SQL Server придется создать, если сервер использует параметры безопасности по умолчанию, разрешающие только проверку подлинности Windows. При необходимости измените строки соединения, чтобы они соответствовали среде.

### <a name="example"></a>Пример

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>См. также

- [подключение к источнику данных](connecting-to-data-source.md);
- [Профилирование во время выполнения](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [Общие сведения о мониторинге пороговых значений производительности](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
