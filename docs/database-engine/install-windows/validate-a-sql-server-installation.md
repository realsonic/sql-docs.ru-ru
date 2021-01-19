---
title: Проверка установки SQL Server | Документы Майкрософт
description: С помощью отчета об обнаружении SQL Server можно проверить, какая версия SQL Server и какие компоненты SQL Server установлены на компьютере.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- validating installations [SQL Server]
ms.assetid: 1689af50-d2b8-4aa6-8f27-cc7127157fc8
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2138be8c23aa6a9849ce19872d3c1b455223540e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170126"
---
# <a name="validate-a-sql-server-installation"></a>Проверка установки SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  С помощью отчета об обнаружении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно проверить, какая версия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и какие компоненты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] установлены на компьютере. На странице **Отчет об обнаруженных установленных компонентах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** показан отчет обо всех продуктах и компонентах [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и [!INCLUDE[ssSQL15](../../includes/sssqlv14-md.md)], установленных на локальном сервере. Отчет об обнаружении компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доступен на странице **Средства** в центре установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ## <a name="run-ssnoversion-features-discovery-report"></a>Запуск отчета об обнаруженных компонентах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 Запустите центр установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], для этого в меню **Пуск** выберите **Все программы**, **[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] \<Version Name>** , **Средства настройки** и **Центр установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Чтобы запустить отчет об обнаружении компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], щелкните **Средства** в левой области навигации **центра установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** и выберите **Установленный отчет об обнаружении компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
 Отчет об обнаруженных компонентах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сохраняется в папке %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<последний_сеанс_программы_установки\>.  
  
 Его создание можно запустить из командной строки. Выполните команду Setup.exe/Action=RunDiscovery из командной строки. Если добавить к ней параметр /q, то пользовательский интерфейс не отобразится, но отчет все равно будет создан в каталоге %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<последний_сеанс_программы_установки\>.  
  
## <a name="see-also"></a>См. также раздел  
 [Просмотр и чтение файлов журналов программы установки SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
