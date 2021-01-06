---
title: Приостановка и возобновление сеанса зеркального отображения базы данных
description: Узнайте, как приостановить и возобновить сеанс зеркального отображения базы данных SQL Server с помощью SQL Server Management Studio или Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- resuming database mirroring
- database mirroring [SQL Server], sessions
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: 05ede3b4-6abe-4442-abb7-9f5aee1d6bc0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: dad800c08e44975716c9bd4f32d0e716400a1ae7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644308"
---
# <a name="pause-or-resume-a-database-mirroring-session-sql-server"></a>Приостановка или возобновление сеанса зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как приостановить и возобновить зеркальное отображение базы данных [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Выполнение ReplaceThisText с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После приостановки или возобновления зеркального отображения базы данных](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
 В любой момент сеанс зеркального отображения базы данных можно приостановить, что дает возможность повышать производительность при возникновении узких мест. Затем в любое время приостановленный сеанс можно возобновить.  
  
> [!CAUTION]  
>  После принудительного обслуживания при повторном подключении исходного основного сервера зеркальное отображение приостанавливается. Возобновление зеркального отображения в данной ситуации может привести к потере данных исходным основным сервером. Дополнительные сведения о том, как действовать при возможной потере данных, см. в разделе [Переключение ролей во время сеанса зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Приостановить или возобновить сеанс зеркального отображения базы данных можно на странице **Свойства базы данных — зеркальное отображение** .  
  
#### <a name="to-pause-or-resume-database-mirroring"></a>Приостановление или возобновление зеркального отображения базы данных  
  
1.  Во время сеанса зеркального отображения базы данных установите соединение с экземпляром главного сервера, в обозревателе объектов щелкните имя сервера и разверните дерево сервера.  
  
2.  Разверните **Базы данных** и выберите нужную базу данных.  
  
3.  Щелкните базу данных правой кнопкой мыши, выберите **Задачи**, а затем **Зеркальное отображение**. Откроется страница **Зеркальное отображение** диалогового окна **Свойства базы данных** .  
  
4.  Чтобы приостановить сеанс, выберите пункт **Приостановить**.  
  
     Запрашивается подтверждение. Если нажать кнопку **Да**, то сеанс будет приостановлен, а кнопка изменится на **Продолжить**.  
  
     Дополнительные сведения о влиянии приостановки сеанса см. в разделе [Приостановка и возобновление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md).  
  
5.  Чтобы возобновить сеанс, нажмите кнопку **Продолжить**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-pause-database-mirroring"></a>Приостановка зеркального отображения базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] для любого участника.  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
     ALTER DATABASE *имя_базы_данных* SET PARTNER SUSPEND  
  
     где *имя_базы_данных* — это зеркально отображаемая база данных, сеанс которой нужно приостановить.  
  
     В следующем примере показана приостановка образца базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER SUSPEND;  
    ```  
  
##### <a name="to-resume-database-mirroring"></a>Возобновление зеркального отображения базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] для любого участника.  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Выполните следующую инструкцию Transact-SQL:  
  
     ALTER DATABASE *имя_базы_данных* SET PARTNER RESUME  
  
     где *database_name* — зеркально отображаемая база данных, сеанс которой нужно возобновить.  
  
     В следующем примере показана приостановка образца базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER RESUME;  
    ```  
  
##  <a name="follow-up-after-pausing-or-resuming-database-mirroring"></a><a name="FollowUp"></a> Дальнейшие действия. После приостановки или возобновления зеркального отображения базы данных  
  
-   **После приостановки зеркального отображения базы данных**  
  
     В базе данных-источнике примите меры предосторожности, чтобы избежать переполнения журнала транзакций. Дополнительные сведения см. в статье [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
-   **После возобновления зеркального отображения базы данных**  
  
     Возобновление зеркального отображения базы данных переводит зеркальную базу данных в состояние SYNCHRONIZING. Если уровень безопасности установлен в FULL, зеркало захватывается основным сервером, и зеркальная база данных переходит в состояние SYNCHRONIZED. В этот момент возможна отработка отказа. Если присутствует следящий сервер, установленный в положение ON, возможна автоматическия отработка отказа. В случае отсутствия следящего сервера возможна отработка отказа вручную.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Удаление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
