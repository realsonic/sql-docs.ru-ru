---
title: Параметр конфигурации сервера xp_cmdshell
description: Ознакомьтесь с параметром xp_cmdshell. Узнайте, как с его помощью можно контролировать выполнение расширенной хранимой процедуры xp_cmdshell в SQL Server. Узнайте, как включить этот параметр.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- xp_cmdshell
ms.assetid: c147c9e1-b81d-49c8-b800-3019f4d86a13
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.date: 06/12/2020
ms.openlocfilehash: d12ea9c15f2a07fa6fdd4bc5dd5538fd1522d267
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878837"
---
# <a name="xp_cmdshell-server-configuration-option"></a>Параметр конфигурации сервера xp_cmdshell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этой статье описано, как включить параметр конфигурации SQL Server **xp_cmdshell**. Этот параметр позволяет системным администраторам контролировать, можно ли выполнять в системе [расширенную хранимую процедуру xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md). По умолчанию параметр **xp_cmdshell** отключен в новых установках.

Перед его включением важно учесть возможные проблемы безопасности.

- В новой разработанном коде не следует использовать хранимую процедуру **xp_cmdshell** и, как правило, ее нужно оставить отключенной.
- Для работы некоторых устаревших приложений требуется включить **xp_cmdshell**. Если их нельзя изменить так, чтобы не использовать эту хранимую процедуру, ее можно включить, как описано ниже.

> [!NOTE]  
> Если необходимо использовать процедуру **xp_cmdshell**, из соображений безопасности рекомендуется включить ее только на время выполнения задачи, которой она требуется.

Чтобы включить **xp_cmdshell**, можно использовать [управление на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) или выполнить системную хранимую процедуру **sp_configure**, как показано в следующем примере кода:  
  
``` sql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```  
  
## <a name="next-steps"></a>Дальнейшие действия

- [Расширенная хранимая процедура xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)
- [Параметры конфигурации сервера (SQL Server)](server-configuration-options-sql-server.md)
- [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
