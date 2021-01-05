---
title: Диалоговое окно "Создание группы доступности" в SSMS
description: Узнайте, как использовать диалоговое окно "Создание группы доступности" в среде SQL Server Management Studio для настройки группы доступности Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 1b0a6421-fbd4-4bb4-87ca-657f4782c433
author: cawrites
ms.author: chadam
ms.openlocfilehash: abab816f9248d1981d62bfeb313130a8b71c4256
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641761"
---
# <a name="use-the-new-availability-group-dialog-box-sql-server-management-studio"></a>Используйте диалоговое окно «Создание группы доступности» (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Данный раздел содержит сведения об использовании диалогового окна **Новая группа доступности** в среде [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] для создания группы доступности AlwaysOn в экземплярах [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , которые включены для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. *Группа доступности* определяет набор пользовательских баз данных, которые будут действовать при сбое как единое целое, и набор партнеров по обеспечению отработки отказа, называемых *репликами доступности* и поддерживающих отработку отказа.  
  
> [!NOTE]  
>  Базовые сведения о группах доступности см. в разделе [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
> [!NOTE]  
>  Дополнительные сведения о других способах создания группы доступности см. в подразделе [Связанные задачи](#RelatedTasks)далее в этом разделе.  
  
  
##  <a name="prerequisites"></a><a name="PrerequisitesRestrictions"></a> Предварительные требования  
  
-   Перед созданием группы доступности необходимо, чтобы экземпляры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на которых находятся реплики доступности, были расположены на различных узлах одной отказоустойчивой кластеризации Windows Server (WSFC). Кроме того, убедитесь, что каждый из экземпляров сервера включен для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] и соответствует всем другим обязательным условиям [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Для получения дополнительных сведений настоятельно рекомендуется изучить раздел [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   До создания группы доступности убедитесь, что на каждом экземпляре сервера, на котором будет размещаться реплика доступности, имеется полностью работоспособная конечная точка зеркального отображения базы данных. Дополнительные сведения см. в разделе [Конечная точка зеркального отображения базы данных (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
-   Для использования диалогового окна **Создание группы доступности** необходимы сведения об именах экземпляров сервера, в которых будут размещаться реплики доступности. Также необходимы сведения об именах всех баз данных, которые необходимо добавить к новой группе доступности, а также нужно убедиться, что эти базы данных соответствуют необходимым условиям и ограничениям баз данных доступности, описанным в статье [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). Если ввести недопустимые значения, новая группа доступности не будет работать.  
  
## <a name="limitations"></a><a name="Limitations"></a> Ограничения  
 Диалоговое окно **Создание группы доступности** нельзя использовать для выполнения следующих задач:  
  
-   Создание прослушивателя группы доступности.    
-   Присоединение дополнительных реплик к группе доступности.    
-   Выполнение синхронизации начальных данных.  
  
 Дополнительные сведения об этих задачах конфигурации см. в разделе [Дальнейшие действия. После создания группы доступности](#FollowUp) далее в этом разделе.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется членство в фиксированной роли сервера **sysadmin** и одно из разрешений: CREATE AVAILABILITY GROUP, ALTER ANY AVAILABILITY GROUP или CONTROL SERVER.  
  
##  <a name="using-the-new-availability-group-dialog-box-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование диалогового окна «Создание группы доступности» (SQL Server Management Studio)  
 **Создание группы доступности**  
  
1.  В обозревателе объектов подключитесь к экземпляру сервера, на котором размещена первичная реплика, и щелкните имя сервера.  
  
2.  Разверните узел **Высокий уровень доступности AlwaysOn** .  
  
3.  Щелкните правой кнопкой мыши узел **Группы доступности** , а затем выберите команду **Создание группы доступности** .  
  
4.  Эта команда открывает диалоговое окно **Создание группы доступности** .  
  
5.  На странице **Общие** введите имя новой группы доступности в поле **Имя группы доступности** . Имя должно быть допустимым идентификатором SQL Server и являться уникальным во всех группах доступности в кластере WSFC. Максимальная длина имени группы доступности составляет 128 символов.  
  
6.  В сетке **Базы данных доступности** щелкните **Добавить** и введите имя локальной базы данных, которая должна принадлежать группе доступности. Повторите это действие для каждой добавляемой базы данных. При нажатии **ОК** в диалоговом окне будет выполнена проверка соответствия указанной базы данных предварительным требованиям для включения в группу доступности. Дополнительные сведения об этих предварительных требованиях см. в статье [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
7.  В сетке **Базы данных доступности** щелкните **Добавить** и введите имя экземпляра сервера для размещения вторичной реплики. В диалоговом окне не будут выполняться попытки подключения к этим экземплярам. При указании неправильного имени сервера вторичная реплика будет добавлена, но к ней нельзя будет подключиться.  
  
    > [!TIP]  
    >  Если реплика была добавлена, и не удается подключиться к экземпляру сервера, можно удалить эту реплику и добавить новую. Дополнительные сведения см. в разделах [Удаление вторичной реплики из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) и [Добавление вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
8.  На панели **Выбор страницы** диалогового окна щелкните **Настройки резервного копирования**. Затем на странице **Настройки резервного копирования** укажите местоположение для резервного копирования в роли реплики и задайте приоритеты резервного копирования для всех экземпляров серверов, на которых будет размещаться реплика доступности для этой группы доступности. Дополнительные сведения см. в разделе [Свойства группы доступности. Создание группы доступности (страница настроек резервного копирования)](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
9. Чтобы создать группу доступности, нажмите кнопку **ОК**. После этого в диалоговом окне будет выполнена проверка соответствия указанных баз данных предварительным требованиям.  
  
     Чтобы выйти из диалогового окна без создания группы доступности, нажмите кнопку **Отмена**.  
  
##  <a name="follow-up-after-using-the-new-availability-group-dialog-box-to-create-an-availability-group"></a><a name="FollowUp"></a> Дальнейшие действия. После использования диалогового окна "Создание группы доступности" для создания группы доступности  
  
-   Потребуется поочередное подключение ко всем экземплярам сервера, на которых размещается вторичная реплика для группы доступности, и выполнение следующих действий:  
  
    1.  Присоединение новой вторичной реплики к группе доступности. Дополнительные сведения см. в разделе [Присоединение вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
    2.  Восстановление текущих резервных копий всех баз данных-источников и их журналов транзакций (с помощью инструкции RESTORE WITH NORECOVERY). Дополнительные сведения см. в статье [Ручная подготовка базы данных-получателя для присоединения к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
    3.  Все новые подготовленные базы данных-получатели немедленно присоединяются к группе доступности. Дополнительные сведения см. в статье [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
-   Для новой группы доступности рекомендуется создать прослушиватель группы доступности. Для этого необходимо подключиться к экземпляру сервера, на котором размещена текущая первичная реплика. Дополнительные сведения см. в разделе [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Настройка свойств группы доступности и реплики**  
  
-   [Смена режима доступности для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Смена режима отработки отказа для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Настройка гибкой политики отработки отказа для обеспечения контроля над автоматическим переходом на другой ресурс (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Настройка доступа только для чтения в реплике доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Настройка маршрутизации только для чтения в группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Изменение периода ожидания сеанса для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Завершение настройки группы доступности**  
  
-   [Присоединение вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Подготовка базы данных-получателя для присоединения к группе доступности вручную (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Другие способы создания группы доступности**  
  
-   [Использование мастера групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Создание группы доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Создание группы доступности (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **Включение функции "Группы доступности AlwaysOn"**  
  
-   [Включение и отключение групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Настройка конечной точки зеркального отображения базы данных**  
  
-   [Создание конечной точки зеркального отображения базы данных для групп доступности AlwaysOn (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Устранение неполадок с конфигурацией групп доступности AlwaysOn**  
  
-   [Поиск и устранение неисправностей конфигурации групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Устранение неполадок с операцией добавления файла, давшей сбой (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Руководство по решениям режима AlwaysOn в Microsoft SQL Server для обеспечения высокой доступности и аварийного восстановления](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
