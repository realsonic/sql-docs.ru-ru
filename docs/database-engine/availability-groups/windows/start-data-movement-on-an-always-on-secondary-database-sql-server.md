---
title: Запуск перемещения данных в базу данных-получатель
description: Узнайте, как запустить перемещение данных для базы данных-получателя, участвующей в группе доступности Always On на SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], databases
ms.assetid: 498eb3fb-6a43-434d-ad95-68a754232c45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 262e797573690f8a648b552e782b0161f41ca7cc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643010"
---
# <a name="start-data-movement-on-an-always-on-secondary-database-sql-server"></a>Запуск перемещения данных для базы данных-получателя AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе приведены сведения о запуске синхронизации данных после добавления базы данных в группу доступности AlwaysOn. Для каждой новой первичной реплики необходимо подготовить базы данных-получатели на экземплярах сервера, размещающих вторичные реплики. Затем каждую из этих баз данных-получателей необходимо вручную присоединить к группе доступности.  
  
> [!NOTE]  
>  Если пути к файлам на всех экземплярах сервера, размещающих реплики доступности группы доступности, одинаковые, то [мастер создания групп доступности](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), [мастер добавления реплики в группу доступности](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)или [мастер добавления базы данных в группу доступности](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md) может автоматически запустить синхронизацию данных.  
  
 Для запуска синхронизации данных вручную потребуется поочередное подключение ко всем экземплярам сервера, на которых размещаются вторичные реплики для группы доступности, и выполнение следующих действий.  
  
1.  Восстановление текущих резервных копий всех баз данных-источников и их журналов транзакций (с помощью инструкции RESTORE WITH NORECOVERY). Можно использовать один из следующих альтернативных подходов.  
  
    -   Вручную восстановить последнюю резервную копию базы данных-источника с помощью инструкции RESTORE WITH NORECOVERY, а затем восстановить все последовательные резервные копии журнала с помощью инструкции RESTORE WITH NORECOVERY. Выполните эту последовательность восстановления на каждом экземпляре сервера, на котором расположена вторичная реплика этой группы доступности.  
  
         **Дополнительные сведения см. в следующих разделах:**  
  
         [Подготовка базы данных-получателя для присоединения к группе доступности вручную (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
    -   При добавлении одной или нескольких баз данных-источников к группе доступности можно будет перевести одну или несколько соответствующих баз данных-получателей из доставки журналов в группы доступности AlwaysOn. Для выполнения переноса базы данных-получателя доставки журналов необходимо, чтобы эта база называлась так же, как и база данных-источник, и размещалась на экземпляре сервера, на котором находится вторичная реплика для группы доступности. Кроме того, группа доступности должна быть настроена так, чтобы первичная реплика была первым кандидатом для выполнения резервного копирования (то есть иметь приоритет резервного копирования >0). После завершения задания резервного копирования для базы данных-источника необходимо отключить задание резервного копирования, а после выполнения задания по восстановлению для выбранной базы данных-получателя — отключить задание восстановления.  
  
        > [!NOTE]  
        >  После создания всех баз данных-получателей для группы доступности, при необходимости проведения резервного копирования на вторичных репликах, нужно изменить параметры автоматического резервного копирования для группы доступности.  
  
         **Дополнительные сведения см. в следующих разделах:**  
  
         [Необходимые условия для выполнения перехода от использования доставки журналов к использованию групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
         [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
2.  Как можно скорее присоедините все новые подготовленные базы данных-получатели к группе доступности.  
  
     **Дополнительные сведения см. в следующих разделах:**  
  
     [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Связанные задачи  
  
-   [Использование диалогового окна "Создание группы доступности" (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Использование мастера добавления реплики в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Использование мастера добавления базы данных в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
