---
title: Запуск монитора зеркального отображения баз данных (SSMS)
description: Описание запуска монитора зеркального отображения баз данных в графическом интерфейсе SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- Database Mirroring Monitor [SQL Server], starting
- database mirroring [SQL Server], monitoring
ms.assetid: 53165335-97ca-4f88-8e78-22f1839dee98
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6b18394e9739ca1bc3b9936df80cf853ee62cdfc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641975"
---
# <a name="start-database-mirroring-monitor-sql-server-management-studio"></a>Запуск монитора зеркального отображения баз данных (среда SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Монитор зеркального отображения баз данных является частью монитора [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , запускаемого в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!NOTE]
>  Монитор зеркального отображения баз данных доступен не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
### <a name="to-launch-the-database-mirroring-monitor"></a>Запуск монитора зеркального отображения баз данных  
  
1.  После подключения к экземпляру основного сервера в обозревателе объектов щелкните имя сервера, чтобы развернуть дерево сервера.  
  
2.  Разверните узел **Базы данных** и выберите базу данных для мониторинга.  
  
3.  Щелкните правой кнопкой мыши базу данных, выберите пункт **Задачи**, а затем щелкните **Запустить монитор зеркального отображения баз данных**.  
  
4.  В диалоговом окне **Монитор зеркального отображения баз данных** нажмите кнопку **Зарегистрировать зеркальную базу данных** , чтобы зарегистрировать одну или более зеркальных баз данных.  
  
    > [!NOTE]  
    >  После регистрации базы данных на одном партнере база данных автоматически регистрируется на других партнерах. Если монитор уже имеет учетные данные соединения для экземпляра другого участника, они используются для подключения. В противном случае, монитор попытается подключиться с помощью проверки подлинности Windows. Если необходимо изменить учетные данные для подключения к экземпляру сервера, установите флажок **Показать диалоговое окно «Управление соединениями сервера» после нажатия кнопки «ОК»** .  
  
 Дополнительные сведения о мониторе зеркального отображения баз данных см. в разделе [Обзор монитора зеркального отображения баз данных](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md).  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
  
