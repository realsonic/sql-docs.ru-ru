---
title: Настройка зеркального отображения базы данных (SQL Server) | Документы Майкрософт
description: Узнайте о предварительных условиях, рекомендациях и действиях по настройке зеркального отображения базы данных в SQL Server, включая общие сведения о сеансе зеркального отображения базы данных.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b2166452c4b1c6f87b907c2329a897e9c9041b75
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643223"
---
# <a name="setting-up-database-mirroring-sql-server"></a>Настройка зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе содержится описание предварительных условий, рекомендаций и шагов настройки зеркального отображения базы данных. Базовые сведения о зеркальном отображении базы данных см. в разделе [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!IMPORTANT]  
>  Настройку зеркального отображения базы данных рекомендуется выполнять в часы с наименьшей загрузкой, поскольку этот процесс может оказать влияние на производительность.  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> Подготовка экземпляра сервера для размещения на зеркальном сервере  
 Для каждого сеанса зеркального отображения базы данных:  
  
1.  Основной, зеркальный и следящий (если есть) сервера должны быть отдельными экземплярами сервера, размещенными на отдельных системных узлах. Каждый экземпляр сервера должен иметь конечную точку зеркального отображения базы данных. Если нужно создать конечную точку зеркального отображения базы данных, убедитесь, что она доступна для других экземпляров сервера.  
  
     Метод проверки подлинности, применяемый экземпляром сервера при зеркальном отображении базы данных, является свойством его конечной точки зеркального отображения базы данных. Для зеркального отображения базы данных доступны два типа защиты транспорта: проверка подлинности Windows или проверка подлинности на основе сертификата. Дополнительные сведения см. в статье [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Требования к сетевому доступу зависят от типа проверки подлинности.  
  
    -   **При использовании проверки подлинности Windows**  
  
         Если экземпляры участников запущены под другими учетными записями пользователей домена, для каждой требуется имя входа в базе данных **master** . Если имя входа отсутствует, его необходимо создать. Дополнительные сведения см. в разделе [Allow Network Access to a Database Mirroring Endpoint Using Windows Authentication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
    -   **При использовании сертификатов**  
  
         Для обеспечения возможности выполнения проверки подлинности при помощи сертификата при зеркальном отображении базы данных на данном экземпляре сервера, системный администратор должен настроить каждый экземпляр сервера для использования сертификатов, как для входящих, так и для исходящих соединений. Вначале должны быть настроены исходящие соединения. Дополнительные сведения см. в разделе [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Убедитесь, что на зеркальном сервере существуют учетные записи для входа всех пользователей базы данных. Дополнительные сведения см. в разделе [Настройка учетных записей входа для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
3.  На экземпляре сервера, где будет размещена зеркальная база данных, настройте остальные компоненты среды, необходимые для зеркального отображения базы данных. Дополнительные сведения см. в статье [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> Общие сведения. Установление сеанса зеркального отображения базы данных  
 Ниже приведены основные действия по установлению сеанса зеркального отображения.  
  
1.  Создайте зеркальную базу данных, восстановив следующие резервные копии с помощью RESTORE WITH NONRECOVERY для каждой операции восстановления.  
  
    1.  Убедитесь, что в основной базе данных в момент создания резервной копии использовалась модель полного восстановления, а затем восстановите последнюю полную резервную копию базы данных основной базы данных. Зеркальная база данных должна иметь то же имя, что и основная.  
  
    2.  Если с момента полного восстановления резервной копии создавались разностные резервные копии, восстановите самую последнюю из них.  
  
    3.  Восстановите все резервные копии журналов, созданные за период после создания разностной резервной копии.  
  
     Дополнительные сведения см. в статье [Prepare a Mirror Database for Mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Оставшиеся шаги настройки следует выполнять как можно скорее после получения полной резервной копии основной базы данных. Перед началом использования зеркального отображения на участниках необходимо создать резервную копию журнала исходной базы данных и восстановить ее в будущей зеркальной базе данных.  
  
2.  Настроить зеркальное отображение можно с помощью инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] или мастера настройки зеркального отображения баз данных. Дополнительные сведения см. в одном из следующих разделов:  
  
    -   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  По умолчанию сеанс установлен в состояние полной безопасности транзакций (параметр SAFETY установлен в FULL), что способствует запуску сеанса в синхронном, высокого уровня защиты режиме без автоматической отработки отказа. Можно перенастроить сеанс для выполнения либо в режиме высокого уровня защиты с автоматической отработкой отказа, либо в асинхронном режиме высокого уровня производительности, как описано ниже.  
  
    -   **Режим высокого уровня защиты с автоматической отработкой отказа**  
  
         Чтобы сеанс высокого уровня защиты поддерживал автоматическую отработку отказа, добавьте экземпляр следящего сервера.  
  
         **Добавление следящего сервера**  
  
        -   [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  Владелец базы данных может отключить слежение в любой момент. Отключение слежения эквивалентно его отсутствию, и поэтому автоматическая отработка отказа не выполняется.  
  
    -   **Высокопроизводительный режим**  
  
         С другой стороны, если автоматическая отработка отказа нежелательна или важней производительность, а не высокий уровень доступности, можно выключить безопасность транзакций. Дополнительные сведения см. в разделе [Изменение безопасности транзакций в сеансах зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  В режиме высокой производительности параметр WITNESS должен быть установлен в OFF. Дополнительные сведения см. в статье [Кворум. Как следящий сервер влияет на доступность базы данных &#40;зеркальное отображение базы данных&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
> [!NOTE]  
>  Пример использования [!INCLUDE[tsql](../../includes/tsql-md.md)] для настройки зеркального отображения базы данных с проверкой подлинности Microsoft Windows см. в разделе [Пример. Настройка зеркального отображения базы данных с помощью проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
>   
>  Пример использования [!INCLUDE[tsql](../../includes/tsql-md.md)] для настройки зеркального отображения базы данных с проверкой подлинности на основе сертификата см. в разделе [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> в этом разделе  
 [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 Краткое изложение шагов создания зеркальной базы данных и подготовки зеркальной базы данных к возобновлению приостановленного сеанса. Также содержит ссылки на разделы руководства.  
  
 [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 Описание синтаксиса для указания сетевого адреса сервера. Описание того, как сетевой адрес идентифицирует конечную точку зеркального отображения базы данных в экземпляре сервера, а также того, как выяснить полное имя домена системы.  
  
 [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 Описывает, как с помощью мастера настройки безопасности зеркального отображения баз данных запустить зеркальное отображение базы данных.  
  
 [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 Описывает шаги настройки зеркального отображения базы данных, связанные с [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 [Пример. Настройка зеркального отображения базы данных с помощью проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
 Пример всех этапов создания сеанса зеркального отображения базы данных со следящим сервером, использующим проверку подлинности Windows.  
  
 [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 Пример всех этапов создания сеанса зеркального отображения базы данных со следящим сервером, использующим проверку подлинности на основе сертификатов.  
  
 [Настройка учетных записей входа для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 Описание процедуры создания имени входа для экземпляра удаленного сервера, на котором используется учетная запись, отличающаяся от учетной записи экземпляра локального сервера.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Среда SQL Server Management Studio**  
  
-   [Запуск мастера настройки безопасности зеркального отображения баз данных (среда SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [Разрешение сетевого доступа к конечной точке зеркального отображения базы данных с использованием проверки подлинности Windows (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Настройка зеркальной базы данных на использование свойства TRUSTWORTHY (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>См. также:  
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Зеркальное отображение базы данных: взаимодействие и совместимость &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
