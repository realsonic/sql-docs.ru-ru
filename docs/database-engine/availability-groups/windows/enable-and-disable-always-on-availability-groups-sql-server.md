---
title: Включение или отключение функции групп доступности
description: Инструкции по включению и отключению функции групп доступности Always On с помощью Transact-SQL (T-SQL), PowerShell или SQL Server Management Studio.
ms.custom: seodec18
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], disabling
- Availability Groups [SQL Server], enabling
ms.assetid: 7c326958-5ae9-4761-9c57-905972276a8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: b44d3ffbbff0539b899abf8ffc906f6fe922f65f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643600"
---
# <a name="enable-or-disable-always-on-availability-group-feature"></a>Включение или отключение функции групп доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Включение [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] является предварительным условием для того, чтобы экземпляр сервера мог использовать группы доступности. Перед тем как создавать и настраивать любую группу доступности, следует включить компонент [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] на каждом экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , где будет размещаться реплика доступности для одной или нескольких групп доступности.  
  
> [!IMPORTANT]  
>  При удалении и повторном создании кластера WSFC необходимо отключить и повторно включить функцию [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в каждом экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , который обеспечил размещение реплики доступности в исходном кластере WSFC.  
  
  
##  <a name="prerequisites-for-enabling-always-on-availability-groups"></a><a name="Prerequisites"></a> Необходимые условия включения групп доступности AlwaysOn  
  
-   Экземпляр сервера должен находиться на узле отказоустойчивой кластеризации Windows Server (WSFC).  
  
-   Экземпляр сервера должен работать на выпуске SQL Server, который поддерживает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Дополнительные сведения см. в разделе [Функции, поддерживаемые различными выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Группы доступности AlwaysOn нельзя включить на нескольких экземплярах сервера одновременно. После включения групп доступности AlwaysOn подождите, пока служба [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] перезапустится, и только после этого включайте следующий экземпляр.  
  
 Сведения о дополнительных предварительных требованиях к созданию и настройке групп доступности см. в разделе [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
## <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Когда группы доступности AlwaysOn включены в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], экземпляр сервера имеет полный контроль над кластером WSFC.  

 Требуется членство в группе **Администратор** на локальном компьютере и полный контроль над кластером WSFC. При отключении функции AlwaysOn с помощью PowerShell откройте окно командной строки, используя команду **Запуск от имени администратора** .  
  
 Требуются разрешения Active Directory на создание объектов и управление объектами.  
  
##  <a name="determine-whether-always-on-availability-groups-is-enabled"></a><a name="IsEnabled"></a> Определите, включены ли группы доступности AlwaysOn  
  
-   [Среда SQL Server Management Studio](#SSMS1Procedure)  
  
-   [Transact-SQL](#Tsql1Procedure)  
  
-   [PowerShell](#PowerShell1Procedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMS1Procedure"></a> Использование среды SQL Server Management Studio  
 **Определите, включены ли группы доступности AlwaysOn**  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши экземпляр сервера и выберите команду **Свойства**.  
  
2.  В диалоговом окне **Свойства сервера** перейдите на страницу **Общие** . Свойство **HADR включен** имеет одно из следующих значений:  
  
    -   **True**, если группы доступности AlwaysOn включены  
  
    -   **False**, если группы доступности AlwaysOn отключены.  
  
###  <a name="using-transact-sql"></a><a name="Tsql1Procedure"></a> Использование Transact-SQL  
 **Определите, включены ли группы доступности AlwaysOn**  
  
1.  Используйте следующую инструкцию [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) :  
  
    ```  
    SELECT SERVERPROPERTY ('IsHadrEnabled');  
    ```  
  
     Значение свойства сервера **IsHadrEnabled** указывает, может ли экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] входить в группы доступности AlwaysOn следующим образом:  
  
    -   Если **IsHadrEnabled** = 1, это означает, что группы доступности AlwaysOn включены.  
  
    -   Если **IsHadrEnabled** = 0, это означает, что группы доступности AlwaysOn отключены.  
  
    > [!NOTE]  
    >  Дополнительные сведения о свойстве сервера **IsHadrEnabled** см. в разделе [SERVERPROPERTY (Transact-SQL)](../../../t-sql/functions/serverproperty-transact-sql.md).  
  
###  <a name="using-powershell"></a><a name="PowerShell1Procedure"></a> Использование PowerShell  
 **Определите, включены ли группы доступности AlwaysOn**  
  
1.  Установите путь по умолчанию (**cd**), указывающий на экземпляр сервера, на котором требуется определить, включена ли функция [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
2.  Введите следующую команду PowerShell **Get-Item** :  
  
    ```  
    PS SQLSERVER:\SQL\NODE1\DEFAULT> get-item . | select IsHadrEnabled  
    ```  
  
    > [!NOTE]  
    >  Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="enable-always-on-availability-groups"></a><a name="EnableAOAG"></a> Включение групп доступности AlwaysOn  
 **Включение функции AlwaysOn с помощью:**  
  
-   [Диспетчер конфигурации SQL Server](#SQLCM2Procedure)  
  
-   [PowerShell](#PScmd2Procedure)  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM2Procedure"></a> Использование диспетчера конфигурации SQL Server  
 **Включение функции "Группы доступности AlwaysOn"**  
  
1.  Подключитесь к узлу отказоустойчивого кластера Windows Server (WSFC) с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], для которого требуется включить группы доступности Always On.  
  
2.  В меню **Пуск** последовательно выберите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)], **Средства настройки** и **Диспетчер конфигурации SQL Server**.  
  
3.  В окне **Диспетчер конфигурации SQL Server** выберите **Службы SQL Server**, правой кнопкой мыши щелкните SQL Server ( **\<**_instance name_**>)** , где **\<**_instance name_**>** — имя локального экземпляра сервера, для которого требуется включить группы доступности AlwaysOn, после чего щелкните **Свойства**.  
  
4.  Перейдите на вкладку **Высокий уровень доступности AlwaysOn**.  
  
5.  Убедитесь, что поле **Имя отказоустойчивого кластера Windows** содержит имя локального отказоустойчивого кластера. Если это поле не заполнено, в настоящее время этот экземпляр сервера не поддерживает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Либо локальный компьютер не является узлом кластера, кластер WSFC завершил работу, либо этот выпуск [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] не поддерживает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
6.  Установите флажок **Включить группы доступности AlwaysOn** и нажмите кнопку **ОК**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] сохранит внесенные изменения. После этого необходимо вручную перезапустить службу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Это позволит выбрать время перезапуска, которое лучше всего подходит под требования вашего предприятия. После перезапуска службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] функция AlwaysOn будет включена, а для свойства **IsHadrEnabled** будет задано значение 1.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd2Procedure"></a> Использование SQL Server PowerShell  
 **Включение AlwaysOn**  
  
1.  Измените каталог (**cd**) на каталог экземпляра сервера, для которого необходимо включить группы доступности AlwaysOn.  
  
2.  С помощью командлета **Enable-SqlAlwaysOn** включите группы доступности AlwaysOn.  
  
     Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
    > [!NOTE]  
    >  Дополнительные сведения о настройке перезапуска службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для командлета **Enable-SqlAlwaysOn** см. ниже в разделе [Когда командлет перезапускает службу SQL Server?](#WhenCmdletRestartsSQL)  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
####  <a name="example-enable-sqlalwayson"></a><a name="ExmplEnable-SqlHadrServic"></a> Пример. Enable-SqlAlwaysOn  
 Следующая команда PowerShell включает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] на экземпляре SQL Server (*Computer*\\*Instance*).  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
```  
  
##  <a name="disable-always-on-availability-groups"></a><a name="DisableAOAG"></a> Отключение групп доступности AlwaysOn  
  
-   **Перед отключением AlwaysOn:**  
  
     [Рекомендации](#Recommendations)  
  
-   **Отключение AlwaysOn с помощью:**  
  
    -   [Диспетчер конфигурации SQL Server](#SQLCM3Procedure)  
  
    -   [PowerShell](#PScmd3Procedure)  
  
-   **Дальнейшие действия.**  [После отключения Always On](#FollowUp)  
  
> [!IMPORTANT]  
>  Функцию AlwaysOn следует отключать на серверах по одному. После отключения групп доступности AlwaysOn подождите, пока служба [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] перезапустится, и только после этого переходите к другому экземпляру.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
 Перед отключением AlwaysOn на экземпляре сервера рекомендуется выполнить следующие действия:  
  
1.  Если на экземпляре сервера в настоящее время размещается первичная реплика группы доступности, которую нужно сохранить, то рекомендуется вручную переключить группу доступности на синхронизированную вторичную реплику, если это возможно. Дополнительные сведения см. в разделе [Выполнение запланированного перехода на другой ресурс вручную для группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
2.  Удалите все локальные вторичные реплики. Дополнительные сведения см. в разделе [Удаление вторичной реплики из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md).  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM3Procedure"></a> Использование диспетчера конфигурации SQL Server  
 **Отключение функции AlwaysOn**  
  
1.  Подключитесь к узлу отказоустойчивого кластера Windows Server (WSFC) с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], для которого требуется отключить группы доступности Always On.  
  
2.  В меню **Пуск** последовательно укажите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)], **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
3.  В окне **Диспетчер конфигурации SQL Server** выберите **Службы SQL Server**, правой кнопкой мыши щелкните SQL Server ( **\<**_instance name_**>)** , где **\<**_instance name_**>** — имя локального экземпляра сервера, для которого требуется отключить группы доступности AlwaysOn, после чего щелкните **Свойства**.  
  
4.  На вкладке **Высокий уровень доступности Always On** снимите флажок **Включить группы доступности Always On** и нажмите кнопку **ОК**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] сохранит изменения и перезапустит службу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . После перезапуска службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] функция AlwaysOn будет отключена, а для свойства **IsHadrEnabled** будет установлено значение 0, указывающее на то, что группы доступности AlwaysOn отключены.  
  
5.  Рекомендуется ознакомиться с пунктом [Дальнейшие действия. После отключения Always On](#FollowUp) далее в этом разделе.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd3Procedure"></a> Использование SQL Server PowerShell  
 **Отключение функции AlwaysOn**  
  
1.  Измените каталог (**cd**) на каталог экземпляра сервера, включенного в настоящее время, для которого необходимо отключить группы доступности Always On.  
  
2.  С помощью командлета **Disable-SqlAlwaysOn** отключите группы доступности AlwaysOn.  
  
     Например, следующая команда отключает группы доступности AlwaysOn в экземпляре SQL Server (*Computer*\\*Instance*).  Эта команда требует перезапуска экземпляра, который будет предложено подтвердить.  
  
    ```  
    Disable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
    ```  
  
    > [!IMPORTANT]  
    >  Дополнительные сведения о настройке перезапуска службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для командлета **Disable-SqlAlwaysOn** см. ниже в разделе [Когда командлет перезапускает службу SQL Server?](#WhenCmdletRestartsSQL).  
  
     Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="follow-up-after-disabling-always-on"></a><a name="FollowUp"></a> Дальнейшие действия. После отключения Always On  
 После отключения групп доступности AlwaysOn экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] необходимо перезапустить. Диспетчер конфигурации SQL Server автоматически перезапускает экземпляр сервера. Но если использовался командлет **Disable-SqlAlwaysOn**, то потребуется перезапустить экземпляр сервера вручную. Дополнительные сведения см. в статье [sqlservr Application](../../../tools/sqlservr-application.md).  
  
 На перезапущенном экземпляре сервера происходит следующее:  
  
-   базы данных обеспечения доступности не запускаются при запуске сервер SQL Server, из-за чего они становятся недоступными.  
  
-   Единственная поддерживаемая инструкция [!INCLUDE[tsql](../../../includes/tsql-md.md)] AlwaysOn — это [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md). Инструкции CREATE AVAILABILITY GROUP, ALTER AVAILABILITY GROUP и параметры SET HADR инструкции ALTER DATABASE не поддерживаются.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] При отключении групп доступности AlwaysOn метаданные и данные конфигурации [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в WSFC не затрагиваются.  
  
 Если нужно полностью отключить группы доступности AlwaysOn в каждом экземпляре сервера, где размещается реплика доступности для одной или нескольких групп доступности, то рекомендуется выполнить следующие действия:  
  
1.  Если перед отключением AlwaysOn локальные реплики доступности не удалялись, удалите все группы доступности, для которых на экземпляре сервера размещается реплика доступности. Сведения об удалении группы доступности см. в разделе [Удаление группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md).  
  
2.  Чтобы удалить оставшиеся метаданные, удалите все затронутые группы доступности в экземпляре сервера, который входит в состав исходного кластера WSFC.  
  
3.  Все базы данных-источники остаются доступными для всех подключений, однако синхронизация данных между главной и базами данных-получателями останавливается.  
  
4.  Базы данных-получатели переводятся в состояние RESTORING. Вы можете либо удалить эти базы данных, либо восстановить их при помощи функции RESTORE WITH RECOVERY. Однако восстановленные базы данных больше не будут участвовать в синхронизации данных группы доступности.  
  
##  <a name="when-does-a-cmdlet-restart-the-sql-server-service"></a><a name="WhenCmdletRestartsSQL"></a> Когда командлет перезапускает службу SQL Server?  
 В запущенном экземпляре сервера использование командлетов **Enable-SqlAlwaysOn** или **Disable-SqlAlwaysOn** для смены текущей настройки функции AlwaysOn может стать причиной перезапуска службы SQL Server. Алгоритм перезапуска зависит от следующих условий:  
  
|указан параметр -NoServiceRestart;|указан параметр -Force.|Перезапущена ли служба [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ?|  
|--------------------------------------------|---------------------------------|---------------------------------------------------------|  
|Нет|Нет|По умолчанию. Однако командлет выводит следующее сообщение:<br /><br /> **Чтобы выполнить это действие, необходимо перезапустить службу SQL Server для экземпляра сервера "<имя_экземпляра>". Продолжить?**<br /><br /> **[Y] Yes  [N] No  [S] Suspend  [?] Help (значение по умолчанию — "Y"):**<br /><br /> Если указать **N** или **S**, служба не будет перезапущена.|  
|Нет|Да|Служба перезапускается.|  
|Да|Нет|Служба не перезапускается.|  
|Да|Да|Служба не перезапускается.|  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [SERVERPROPERTY (Transact-SQL)](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
