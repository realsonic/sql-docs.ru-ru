---
title: Базовые группы доступности для отдельной базы данных
description: 'Описываются различия между обычной и базовой группой доступности Always On, а также настройка базовой группы доступности. '
ms.custom: seodec18
ms.date: 02/01/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 285adbc7-ac9b-40f6-b4a9-3f1591d3b632
author: cawrites
ms.author: chadam
ms.openlocfilehash: 50db7fba110c33cc6c50211cd1b431bf1380ba26
ms.sourcegitcommit: 44eebb659f9b226c08aea6c31a909b22ad4fec60
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860584"
---
# <a name="basic-always-on-availability-groups-for-a-single-database"></a>Базовые группы доступности Always On для отдельной базы данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 Базовые группы доступности Always On предоставляют высокодоступное решение для SQL Server 2016 и более поздних версий для Standard Edition. Основная группа доступности обеспечивает функционирование среды отработки отказа для одной базы данных. Базовые группы создаются и управляются так же, как обычные (расширенные) [группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) в выпуске Enterprise Edition. В этом документе описаны отличия и ограничения основных групп доступности.

## <a name="features"></a>Компоненты  
 Базовые группы доступности AlwaysOn заменяют нерекомендуемую функцию зеркального отображения базы данных и предоставляют такой же уровень поддержки. Основные группы доступности позволяют поддерживать одну реплику базы данных-источника. Эта реплика может использовать режим синхронной или асинхронной фиксации. Дополнительные сведения о режимах доступности см. в разделе [Режимы доступности (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Вторичная реплика остается неактивной, пока нет необходимости отработки отказа. При такой отработке первичная и вторичная роли меняются местами, в результате чего вторичная реплика становится первичной активной базой данных. Дополнительные сведения об отработке отказа см. в разделе [Отработка отказа и режимы отработки отказа (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md). Основные группы доступности могут работать в гибридной среде, охватывающей локальные данные и облако Microsoft Azure.  
  
## <a name="limitations"></a>Ограничения  
 Набор возможностей основных групп доступности включает часть возможностей расширенных групп доступности из SQL Server 2016 Enterprise Edition. Для основных групп доступности действуют следующие ограничения.  
  
- Ограничение двух реплик (первичная и вторичная). Основные группы доступности для SQL Server 2017 в Linux поддерживают только реплику с дополнительной конфигурацией.
  
- Отсутствует доступ для чтения вторичной реплики.  
  
- Отсутствует возможность резервного копирования вторичной реплики.  

- Отсутствуют проверки целостности на вторичных репликах. 

- Отсутствует поддержка реплик, размещенных на серверах под управлением SQL Server версии ниже SQL Server 2016 Community Technology Preview 3 (CTP3).  

- Поддержка одной базы данных доступности.  
  
- Основные группы доступности не могут быть обновлены до расширенных групп доступности. В этом случае группу необходимо удалить и повторно добавить в группу, содержащую серверы под управлением SQL Server 2016 Enterprise Edition.  
  
- Основные группы доступности поддерживаются только для серверов с ПО выпуска Standard Edition. 

- Основные группы доступности не могут быть частью распределенной группы доступности. 

- Вы можете подключить несколько основных групп доступности к одному экземпляру SQL Server.

  
## <a name="configuration"></a>Конфигурация  
 Базовые группы доступности AlwaysOn можно создать на любых двух серверах SQL Server 2016 Standard Edition. В процессе создания основной группы доступности вам потребуется указать обе реплики.  
  
 Чтобы создать базовую группу доступности, используйте команду transact-SQL **CREATE AVAILABILITY GROUP** и укажите параметр **WITH BASIC** (значение параметра по умолчанию — **ADVANCED**). Вы также можете создать базовую группу доступности, используя пользовательский интерфейс SQL Server Management Studio в версиях от 17.8. Дополнительные сведения см. в разделе [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md). 

В следующем примере создается базовая группа доступности с помощью Transact-SQL (T-SQL): 

```sql
CREATE AVAILABILITY GROUP [BasicAG]
WITH (AUTOMATED_BACKUP_PREFERENCE = PRIMARY,
BASIC,
DB_FAILOVER = OFF,
DTC_SUPPORT = NONE,
REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 0)
FOR DATABASE [AdventureWorks]
REPLICA ON N'SQLVM1\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM1.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)),
    N'SQLVM2\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM2.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO));

GO
```

  
> [!NOTE]  
>  При использовании команды **CREATE AVAILABILITY GROUP** с параметром **WITH BASIC** применяются соответствующие ограничения основных групп доступности. Например, при попытке создания основной группы доступности с доступом для чтения появится ошибка. Таким же образом действуют и другие ограничения. Подробности см. в разделе "Ограничения" этого документа.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
