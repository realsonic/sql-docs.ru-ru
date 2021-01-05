---
title: Необходимые условия для преобразования доставки журналов в группы доступности
description: Описание необходимых условий для преобразования доставки журналов в группу доступности Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 2738ce65-205e-4682-92d8-dc7e37c58b2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28c847a517212d5171ca54814ed8c3963f8d2bb6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642545"
---
# <a name="prerequisites-to-convert-log-shipping-to-always-on-availability-groups"></a>Необходимые условия для преобразования доставки журналов в группы доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описаны предварительные условия для преобразования базы данных-источника доставки журналов вместе с одной или несколькими базами данных-получателями в базу данных-источник и базы данных-получатели AlwaysOn.  
  
> [!NOTE]  
>  В качестве первичной базы данных для доставки журналов вы можете настроить любую базу данных-источник или базу данных-получатель (по возможности предназначенную для чтения).  
  
  
##  <a name="availability-group-prerequisites"></a><a name="AGPrereqsRealAddress"></a> Предварительные условия для группы доступности  
 Чтобы разрешить выполнение заданий резервного копирования в первичной реплике группы доступности, используйте следующие параметры резервного копирования групп доступности AlwaysOn:  
  
|Свойство|Параметр|  
|--------------|-------------|  
|Автоматическое резервное копирование группы доступности|Только в первичной реплике|  
|Приоритет резервного копирования первичной реплики.|> 0|  
  
 **Дополнительные сведения см. в следующих разделах:**  
  
 [Просмотр свойств группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
 [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="log-shipping-prerequisites"></a><a name="LogShipPrereqs"></a> Предварительные условия для доставки журналов  
  
-   База данных-источник доставки журналов должна находиться на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором размещается первоначальная или текущая первичная реплика группы доступности.  
  
-   Для преобразования базы данных-получателя доставки журналов в базу данных-получатель AlwaysOn необходимо:  
  
    -   использовать то же имя, что и у базы данных-источника.  
  
    -   Размещение на экземпляре сервера, на котором размещается вторичная реплика для группы доступности.  
  
 После выполнения задания резервного копирования для базы данных-источника отключите задание резервного копирования и после выполнения задания по восстановлению для выбранной базы данных-получателя отключите задание восстановления.  
  
 После создания всех баз данных-получателей для группы доступности при необходимости проведения резервного копирования во вторичных репликах нужно изменить параметры автоматического резервного копирования для группы доступности.  
  
 **Дополнительные сведения см. в следующих разделах:**  
  
 [Преобразование конфигурации доставки журналов в группу доступности](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group) (блог по SQL Server)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Доставка журналов**  
  
-   [Обновление доставки журналов до SQL Server 2016 (Transact-SQL)](../../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Удаление доставки журналов (SQL Server)](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
 **Группы доступности AlwaysOn**  
  
-   [Использование мастера групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Использование диалогового окна "Создание группы доступности" (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Создание группы доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Создание группы доступности (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   **Блоги**  
  
     [Преобразование конфигурации доставки журналов в группу доступности](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group)  
  
     [Добавление базы данных-источника доставки журналов и баз данных-получателей к существующей группе доступности](/archive/blogs/sqlalwayson/add-a-log-shipping-primary-database-and-secondary-databases-to-an-existing-availability-group)  
  
     [Блоги команды разработчиков SQL Server AlwaysOn: официальный блог по SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)  
  
-   **Технические документы**  
  
     [Руководство по миграции. Переход на группы доступности AlwaysOn с предыдущих развертываний, сочетающих зеркальное отображение базы данных и доставку журналов](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))  
  
     [Технические документы Майкрософт Microsoft по SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
