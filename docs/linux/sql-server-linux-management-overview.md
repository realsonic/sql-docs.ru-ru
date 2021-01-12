---
title: Управление SQL Server на Linux
description: В этой статье содержатся ссылки на стандартные задачи и средства управления для SQL Server на Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 6bd8eb0b-593d-467e-87ea-ab1c4dbcd1ea
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 04ece9daf696d73b9de96f48a08e32136fe6ab66
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094642"
---
# <a name="choose-the-right-tool-to-manage-sql-server-on-linux"></a>Выбор подходящего средства для управления SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Управлять SQL Server на Linux можно несколькими способами. В следующем разделе приводится краткий обзор различных средств и методов управления со ссылками на дополнительные ресурсы.

## <a name="mssql-conf"></a>mssql-conf 

Средство **mssql-conf** настраивает SQL Server на Linux. Дополнительные сведения см. в статье [Настройка SQL Server на Linux с помощью средства mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="transact-sql"></a>Transact-SQL

Почти все, что можно делать в клиентском средстве, можно выполнять и с помощью инструкций Transact-SQL. SQL Server предоставляет [динамические административные представления](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md), которые позволяют запрашивать состояние и конфигурацию SQL Server. Существуют также [команды Transact-SQL](../t-sql/language-reference.md) для задач управления базами данных. Эти команды можно выполнять в любом клиентском средстве, которое поддерживает подключение к SQL Server и выполнение запросов Transact-SQL, например [sqlcmd](sql-server-linux-setup-tools.md) или [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md).

## <a name="azure-data-studio"></a>Azure Data Studio

Azure Data Studio — это новое кроссплатформенное средство для управления SQL Server. Дополнительные сведения см. в статье [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

## <a name="sql-server-management-studio-on-windows"></a>SQL Server Management Studio в Windows

SQL Server Management Studio (SSMS) — это приложение Windows, которое предоставляет графический пользовательский интерфейс для управления SQL Server. Хотя в настоящее время оно выполняется только в Windows, с его помощью можно подключаться удаленно к экземплярам SQL Server на Linux. Дополнительные сведения об использовании SSMS для управления SQL Server см. в статье [Управление SQL Server на Linux с помощью SSMS](sql-server-linux-manage-ssms.md).

## <a name="mssql-cli-preview"></a>mssql-cli (предварительная версия)

Корпорация Майкрософт выпустила новое кроссплатформенное средство выполнения скриптов для SQL Server, которое называется [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/). В настоящее время оно находится в режиме предварительной версии.

## <a name="powershell"></a>PowerShell

PowerShell предоставляет полнофункциональную среду командной строки для управления SQL Server на Linux. Дополнительные сведения см. в статье [Использование PowerShell для управления SQL Server на Linux](sql-server-linux-manage-powershell.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об SQL Server на Linux см. в статье [SQL Server на Linux](sql-server-linux-overview.md).