---
title: Журналы диагностики работоспособности библиотеки DLL ресурсов для групп доступности
description: Описывает, как библиотека ресурсов SQL Server наблюдает за работоспособностью группы доступности Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: c1862d8a-5f82-4647-a280-3e588b82a6dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 72a023d4627d348421b392e8f787914edc986505
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643722"
---
# <a name="sql-server-resource-dll-health-diagnostic-logs-for-availability-groups"></a>Журналы диагностики работоспособности библиотеки ресурсов SQL Server для групп доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Для наблюдения за работоспособностью первичной реплики доступности библиотека ресурсов SQL Server, выполняемая в кластере отказоустойчивой кластеризации Windows Server (WSFC), использует хранимую процедуру в экземпляре SQL Server [sp_server_diagnostics](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md).  
  
 Библиотека ресурсов SQL Server поддерживает выделенное открытое соединение с экземпляром SQL Server, через которое экземпляр SQL Server периодически отправляет подробные диагностические сведения о работоспособности в библиотеку ресурсов SQL Server. Диагностика работоспособности вместе с политикой перехода на другой ресурс, настроенной в ресурсе группы доступности в кластере (свойство FailoverConditionLevel), используются кластером для определения того, требуется ли выполнить перезапуск или переход на другой ресурс для группы доступности. Эта хранимая процедура используется для передачи пульса экземпляра SQL Server 2012 и более поздних версий в кластер WSFC, что является более детализированным и надежным способом по сравнению с SQL Server 2008 R2 или более ранних версий, где осуществлялось периодическое подключение к экземпляру с помощью запроса `SELECT @@SERVERNAME`. В результате вы можете управлять условиями, которые активируют переход на другой ресурс, задав значение для свойства FailureConditonLevel группы доступности.  
  
 **Использование журналов диагностики отказоустойчивого кластера SQL Server**
 
 Все диагностические сведения о работоспособности, получаемые библиотекой ресурсов SQL Server от процедуры sp_server_diagnostics, автоматически сохраняются в каталоге журнала по умолчанию для экземпляра SQL Server (%PROGRAMFILES%\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Log). Эти журналы называются SQLDIAG и сохраняются в формате XEL (расширенные события). Эти файлы в каталоге журнала SQL Server имеют следующий формат: \<HOSTNAME>_\<INSTANCENAME>_SQLDIAG_X_XXXXXXXXX.xel. Просмотрев журналы SQLDIAG, можно определить первопричину неисправности ресурсов группы доступности сбоя или события перехода на другой ресурс.  
  
 Чтобы просмотреть журнал SQLDIAG, перетащите XEL-файл в SQL Server Management Studio.  
  
  
