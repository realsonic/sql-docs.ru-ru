---
title: Удаление следящего сервера зеркального отображения базы данных
description: Описывает, как удалить следящий сервер из сеанса зеркального отображения базы данных с помощью SQL Server Management Studio (SSMS) или Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], turning off
- witness [SQL Server], removing
- database mirroring [SQL Server], witness
ms.assetid: f3ce7afc-8936-4d35-80ce-d0f8fbc318d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8653d60228d2b1a72b5a25001d2f1212824b2608
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641381"
---
# <a name="remove-the-witness-from-a-database-mirroring-session-sql-server"></a>Удаление следящего сервера из сеанса зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как удалить следящий сервер из сеанса зеркального отображения базы данных [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Во время сеанса зеркального отображения владелец базы данных в любой момент может отключить следящий сервер для сеанса зеркального отображения базы данных.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Удаление следящего сервера с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После удаления следящего сервера](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-remove-the-witness"></a>Удаление следящего сервера  
  
1.  Установите соединение с экземпляром основного сервера и на панели **Обозреватель объектов** щелкните имя сервера, чтобы развернуть этот узел.  
  
2.  Раскройте **Базы данных** и выберите базу данных, для которой нужно удалить следящий сервер.  
  
3.  Щелкните базу данных правой кнопкой мыши, выберите **Задачи**, а затем **Зеркальное отображение**. Откроется страница **Зеркальное отображение** диалогового окна **Свойства базы данных** .  
  
4.  Чтобы удалить следящий сервер, удалите его сетевой адрес из поля **Следящий** .  
  
    > [!NOTE]  
    >  При переключении из режима высокого уровня безопасности с автоматической отработкой отказа в высокопроизводительный режим поле **Следящий** автоматически очищается.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-remove-the-witness"></a>Удаление следящего сервера  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] на экземпляре любого из серверов-участников.  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Выполните следующую инструкцию:  
  
     [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) *имя_базы_данных* SET WITNESS OFF  
  
     где *имя_базы_данных* — имя зеркально отображаемой базы данных.  
  
     В следующем примере показано удаление следящего сервера для базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET WITNESS OFF ;  
    ```  
  
##  <a name="follow-up-after-removing-the-witness"></a><a name="FollowUp"></a> Дальнейшие действия. После удаления следящего сервера  
 При отключении следящего сервера [режим работы](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)изменяется в соответствии с параметром безопасности транзакций.  
  
-   Если уровень безопасности транзакций установлен в FULL (значение по умолчанию), то сеанс использует синхронный режим с высоким уровнем защиты без автоматической отработки отказа.  
  
-   Когда безопасность транзакций отключена, сеанс работает асинхронно (в режиме высокой производительности), не требуя кворума. При отключенной безопасности транзакций настоятельно рекомендуется также отключить следящий сервер.  
  
> [!TIP]  
>  Параметры безопасности транзакций для каждого участника на экземпляре сервера доступны через представление каталога [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md), в столбцах **mirroring_safety_level** и **mirroring_safety_level_desc**.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Добавление или замена следящего сервера зеркального отображения базы данных (среда SQL Server Management Studio)](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Изменение безопасности транзакций в сеансах зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)   
 [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)   
 [Следящий сервер зеркального отображения базы данных](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
