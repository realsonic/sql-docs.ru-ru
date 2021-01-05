---
title: Настройка группы доступности для чтения и масштабирования
description: Узнайте, как настроить группу доступности Always On SQL Server для рабочих нагрузок чтения и масштабирования в Windows.
ms.custom: seodec18
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.date: 05/24/2018
ms.topic: how-to
ms.prod: sql
ms.technology: availability-groups
ms.openlocfilehash: c7c7420be3a75ab8ba9e23246e76232c3048a71f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639880"
---
# <a name="configure-read-scale-for-an-always-on-availability-group"></a>Настройка группы доступности Always On для чтения и масштабирования

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Группу доступности AlwaysOn SQL Server для рабочих нагрузок чтения и масштабирования можно настроить в Windows. Существует два типа архитектур для групп доступности:
* Архитектура для высокого уровня доступности, в которой используется диспетчер кластера для улучшенного обеспечения непрерывности бизнес-процессов и которая может содержать доступные для чтения вторичные реплики. Сведения о создании архитектуры высокого уровня доступности см. в статье [Создание и настройка групп доступности в Windows](creation-and-configuration-of-availability-groups-sql-server.md). 
* Архитектура, которая поддерживает только рабочие нагрузки чтения и масштабирования. 

В этой статье описывается создание группы доступности для рабочих нагрузок чтения и масштабирования без диспетчера кластеров. Эта архитектура обеспечивает только чтение и масштабирование. Она не поддерживает высокий уровень доступности.

>[!NOTE]
>В группу доступности с `CLUSTER_TYPE = NONE` могут входить реплики, размещенные на различных платформах операционных систем. Она не поддерживает высокий уровень доступности. Сведения об ОС Linux см. в статье [Настройка группы доступности SQL Server для чтения и масштабирования в Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md).

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## <a name="create-an-availability-group"></a>Создание группы доступности

Создайте группу доступности. Задайте `CLUSTER_TYPE = NONE`. Кроме того, задайте для каждой реплики `FAILOVER_MODE = NONE`. Клиентские приложения с рабочими нагрузками аналитики и отчетности могут подключаться к базам данных — получателям напрямую. Кроме того, можно создать список маршрутизации только для чтения. Подключения к первичной реплике будут переадресовывать запросы на подключение для чтения к каждой из содержащихся в нем вторичных реплик по принципу циклического перебора.

Приведенный ниже скрипт Transact-SQL создает группу доступности с именем `ag1`. Он настраивает реплики группы доступности с параметром `SEEDING_MODE = AUTOMATIC`. Если этот параметр задан, SQL Server будет автоматически создавать базы данных на каждом сервере-получателе после его добавления в группу доступности. 

Обновите следующий сценарий для своей среды. Замените значения `<node1>` и `<node2>` на имена экземпляров SQL Server, где размещаются реплики. Замените значение `<5022>` на порт, заданный для конечной точки. Выполните следующий сценарий Transact-SQL на первичной реплике SQL Server:

```sql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH (
            ENDPOINT_URL = N'tcp://<node2>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-server-instances-to-the-availability-group"></a>Присоединение вторичных экземпляров SQL Server к группе доступности

Приведенный ниже скрипт Transact-SQL присоединяет сервер к группе доступности с именем `ag1`. Обновите сценарий для своей среды. Для присоединения к группе доступности в каждой вторичной реплике SQL Server выполните следующий скрипт Transact-SQL:

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

У этой группы доступности нет конфигурации высокого уровня доступности. Если вам требуется высокий уровень доступности, следуйте инструкциям в статье [Настройка группы доступности AlwaysOn для SQL Server в Linux](../../../linux/sql-server-linux-availability-group-configure-ha.md) или [Создание и настройка групп доступности в Windows](creation-and-configuration-of-availability-groups-sql-server.md).

## <a name="connect-to-read-only-secondary-replicas"></a>Подключение к вторичным репликам только для чтения

Подключаться к вторичным репликам только для чтения можно двумя способами.
* Приложения могут подключаться непосредственно к экземпляру SQL Server, на котором размещена вторичная реплика, и отправлять запросы к базам данных. Дополнительные сведения см. в статье [Вторичные реплики для чтения](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
* Приложения также могут использовать маршрутизацию только для чтения, для которой требуется прослушиватель. Если вы развертываете сценарий масштабирования для чтения без диспетчера кластеров, можно создать прослушиватель, указывающий на IP-адрес текущей первичной реплики и порт, отличный от того, по которому ожидает передачи данных SQL Server. Необходимо будет повторно создать прослушиватель, чтобы он указывал на новый основной IP-адрес после отработки отказа. Дополнительные сведения см. в статье [Маршрутизация только для чтения](listeners-client-connectivity-application-failover.md#ConnectToSecondary).

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Отработка отказа первичной реплики в группе доступности для чтения и масштабирования

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка распределенной группы доступности](./distributed-availability-groups.md)
* [Дополнительные сведения о группах доступности](overview-of-always-on-availability-groups-sql-server.md)
* [Выполнение принудительного перехода на другой ресурс вручную](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)