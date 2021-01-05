---
title: Удаление зеркального отображения базы данных (SQL Server) | Документы Майкрософт
description: Узнайте, как удалить зеркальное отображение базы данных с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- removing database mirroring [SQL Server]
ms.assetid: bbc4d7f7-3bc7-40d6-a822-af195fe7f8c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5a3cd9ef4faf8e11832288cb2cc6a0ec512f0ded
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641383"
---
# <a name="remove-database-mirroring-sql-server"></a>Удаление зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как удалить зеркальное отображение базы данных [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  Владелец базы данных может в любое время удалить зеркальное отображение базы данных. Для этого он должен вручную остановить сеанс.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Удаление зеркального отображения базы данных с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После удаления зеркального отображения базы данных](#FollowUp)  
  
-   [Связанные задачи](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-remove-database-mirroring"></a>Удаление зеркального отображения базы данных  
  
1.  Во время сеанса зеркального отображения базы данных установите соединение с экземпляром главного сервера, в обозревателе объектов щелкните имя сервера и разверните дерево сервера.  
  
2.  Разверните **Базы данных** и выберите нужную базу данных.  
  
3.  Щелкните базу данных правой кнопкой мыши, выберите **Задачи**, а затем **Зеркальное отображение**. Откроется страница **Зеркальное отображение** диалогового окна **Свойства базы данных** .  
  
4.  На панели **Выбор страницы** щелкните **Зеркальное отображение**.  
  
5.  Для удаления зеркального отображения нажмите **Отключить отображение**. Будет запрошено подтверждение. Если нажать кнопку **Да**, сеанс будет остановлен и зеркальное отображение будет удалено из этой базы данных.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Удалить зеркальное отображение базы данных можно в диалоговом окне **Свойства базы данных**. Откройте страницу **Зеркальное отображение** диалогового окна **Свойства базы данных** .  
  
#### <a name="to-remove-database-mirroring"></a>Удаление зеркального отображения базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] любого из участников зеркального отображения.  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    ALTER DATABASE database_name SET PARTNER OFF  
    ```  
  
     где *database_name* — зеркально отображаемая база данных, сеанс которой необходимо удалить.  
  
     В следующем примере удаляется зеркальное отображение образца базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER OFF;  
    ```  
  
##  <a name="follow-up-removing-database-mirroring"></a><a name="FollowUp"></a> Дальнейшие действия. Удаление зеркального отображения базы данных  
  
> [!NOTE]  
>  Дополнительные сведения о последствиях удаления зеркального отображения базы данных см. в статье [Удаление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   **Если планируется возобновление зеркального отображения базы данных**  
  
     Перед повторным запуском зеркального отображения к зеркальной базе данных необходимо применить все резервные копии журналов, созданные в основной базе данных перед удалением зеркального отображения.  
  
-   **Если возобновление зеркального отображения не планируется**  
  
     При необходимости можно восстановить прежнюю зеркальную базу данных. На экземпляре сервера, который был зеркальным сервером, можно выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
    ```  
    RESTORE DATABASE database_name WITH RECOVERY;  
    ```  
  
    > [!IMPORTANT]  
    >  При восстановлении этой базы данных в режиме «в сети» будут доступны две разные базы данных с одним и тем же именем. Поэтому необходимо предусмотреть, чтобы у клиентов был доступ только к одной из них, обычно к новейшей основной базе данных.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Приостановка или возобновление сеанса зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
-   [Удаление следящего сервера из сеанса зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Настройка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
