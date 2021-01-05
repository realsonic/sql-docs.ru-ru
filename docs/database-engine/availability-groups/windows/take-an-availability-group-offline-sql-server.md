---
title: Перевод группы доступности в режим "вне сети" (SQL Server) | Документы Майкрософт
description: Узнайте, как перевести группу доступности AlwaysOn из состояния ONLINE в состояние OFFLINE с помощью Transact-SQL в SQL Server.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], take offline
ms.assetid: 50f5aad8-0dff-45ef-8350-f9596d3db898
author: cawrites
ms.author: chadam
ms.openlocfilehash: 361fdb639201fe69c9f31e233dca51a910ed10a7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641560"
---
# <a name="take-an-availability-group-offline-sql-server"></a>Перевод группы доступности в режим «вне сети» (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается перевод группы доступности AlwaysOn из состояния ONLINE в состояние OFFLINE с помощью [!INCLUDE[tsql](../../../includes/tsql-md.md)] в [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] и более поздних версий. У баз данных с синхронной фиксацией потери данных не происходит, поскольку реплика с синхронной фиксацией не синхронизирована, режим OFFLINE вызывает ошибку, а группа доступности остается в режиме ONLINE. Продолжение работы группы доступности в режиме «в сети» защищает несинхронизированные базы данных с синхронной фиксацией от возможной потери данных. После перехода группы доступности в режим «вне сети» ее базы данных становятся недоступными для клиентов, при этом невозможно перевести группу доступности обратно в режим «в сети». Таким образом, переводить группу доступности в режим «вне сети» следует только в целях миграции ресурсов этой группы доступности с одного кластера WSFC на другой.  
  
 Если во время миграции [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]с одного кластера на другой какие-либо приложения подключаются напрямую к первичной реплике группы доступности, то эту группу доступности необходимо перевести в режим «вне сети». Миграция [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] поддерживает обновление операционной системы с минимальным временем простоя групп доступности. Типичный сценарий — использование миграции [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] с одного сервера на другой для обновления до [!INCLUDE[win8](../../../includes/win8-md.md)] или [!INCLUDE[win8srv](../../../includes/win8srv-md.md)]. Дополнительные сведения см. в документе [Миграция между кластерами групп доступности AlwaysOn для обновления ОС](/previous-versions/sql/sql-server-2012/jj873730(v=msdn.10)).  
  
  
> [!CAUTION]  
>  Используйте параметр OFFLINE для миграции ресурсов группы доступности между кластерами или для отработки отказа для группы доступности, доступной для чтения и масштабирования.
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   На экземпляре сервера, где вводится команда OFFLINE, должны быть запущены службы [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] или более поздней версии (выпуск Enterprise Edition или более продвинутый выпуск).    
-   Группа доступности должна быть в данный момент в сети.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
 Прежде чем переводить группу доступности в режим «вне сети», удалите прослушиватели группы доступности. Дополнительные сведения см. в документе [Удаление прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Перевод группы доступности в режим «вне сети»**  
  
1.  Подключитесь к экземпляру сервера, где размещается реплика доступности для группы доступности. Эта реплика может быть первичной или вторичной.  
  
2.  Инструкция [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) используется следующим образом:  
  
     ALTER AVAILABILITY GROUP *имя_группы* OFFLINE  
  
     где *имя_группы* — это имя группы доступности.  
  
### <a name="example"></a>Пример  
 В следующем примере выполняется перевод группы доступности `AccountsAG` в режим «вне сети».  
  
```  
ALTER AVAILABILITY GROUP AccountsAG OFFLINE;  
```  
  
##  <a name="follow-up-after-the-availability-group-goes-offline"></a><a name="FollowUp"></a> Дальнейшие действия. После перехода группы доступности в режим "вне сети"  
  
-   **Ведение журнала операций OFFLINE**.  Идентификатор узла WSFC, где была инициирована операция OFFLINE, сохраняется как в журнале кластера WSFC, так и в журнале SQL ERRORLOG.  
  
-   **Если прослушиватель группы доступности не был удален до перевода группы в автономный режим**.  При переносе группы доступности на другой кластер WSFC удалите имя виртуальной сети и виртуальный IP-адрес прослушивателя. Их можно удалить с помощью консоли управления отказоустойчивым кластером либо с помощью командлета [Remove-ClusterResource](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx) PowerShell или [cluster.exe](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx). Обратите внимание, что программа cluster.exe в Windows 8 является устаревшей.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Удаление прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Смена контекста кластера HADR экземпляра сервера (SQL Server)](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Технические статьи по SQL Server 2012](https://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [Блог команды разработчиков SQL Server Always On: официальный блог по SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>См. также:  
 [Группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
