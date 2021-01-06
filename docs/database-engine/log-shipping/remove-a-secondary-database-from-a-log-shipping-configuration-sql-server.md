---
title: Удаление получателя доставки журналов
description: Узнайте, как удалить базу данных — получателя доставки журналов с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- deleting secondary databases
- secondary databases [SQL Server], in log shipping
- removing secondary databases
- secondary data files [SQL Server], removing
- log shipping [SQL Server], secondary databases
ms.assetid: ebe368a4-ca1c-45d0-9a71-3ddbd5b26a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3cb7354772f10be060dd541d2394f146df4eca17
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643891"
---
# <a name="remove-a-secondary-database-from-a-log-shipping-configuration-sql-server"></a>Удаление базы данных-получателя из конфигурации доставки журналов (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается удаление базы данных-получателя доставки журналов в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Удаление базы данных-получателя доставки журналов с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Связанные задачи](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для вызова хранимых процедур доставки журналов необходимо членство в предопределенной роли сервера **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-remove-a-log-shipping-secondary-database"></a>Удаление базы данных-получателя доставки журналов  
  
1.  Подключитесь к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который в данный момент является сервером-источником доставки журналов и разверните этот экземпляр.  
  
2.  Разверните узел **Базы данных**, щелкните правой кнопкой мыши базу данных-источник доставки журналов и выберите пункт **Свойства**.  
  
3.  В области **Выбор страницы** щелкните **Доставка журналов транзакций**.  
  
4.  В меню **Экземпляры сервера-получателя и базы данных** щелкните базу данных, которую необходимо удалить.  
  
5.  Щелкните **Удалить**.  
  
6.  Нажмите кнопку **ОК** , чтобы обновить конфигурацию.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-remove-a-secondary-database"></a>Удаление базы данных-получатель  
  
1.  Чтобы удалить сведения о базе данных-получателе с сервера-источника, запустите на нем хранимую процедуру [sp_delete_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md) .  
  
2.  Чтобы удалить базу данных-получатель с сервера-получателя, запустите на нем хранимую процедуру [sp_delete_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md) .  
  
    > [!NOTE]  
    >  Если других баз данных-получателей с таким же идентификатором получателя нет, процедура **sp_delete_log_shipping_secondary_primary** запускается из процедуры **sp_delete_log_shipping_secondary_database** и удаляет запись об идентификаторе получателя, а также задания копирования и восстановления.  
  
3.  На сервере-получателе отключите задания копирования и восстановления. Дополнительные сведения см. в статье [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Обновление доставки журналов до SQL Server 2016 (Transact-SQL)](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Настройка доставки журналов (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Добавление базы данных-получателя в конфигурацию доставки журналов (SQL Server)](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Удаление доставки журналов (SQL Server)](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Просмотр отчета о доставке журналов (среда SQL Server Management Studio)](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Наблюдение за доставкой журналов (Transact-SQL)](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Переход на вторичный сервер доставки журналов (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Таблицы доставки журналов и хранимые процедуры](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
