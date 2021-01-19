---
title: 'Группы доступности Always On: взаимодействие'
description: Описываются различные функции, которые могут и не могут работать вместе с группами доступности Always On.
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: 60c298991f80586ed9b830b856b84b3640ac509e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170796"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Группы доступности Always On: взаимодействие (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

В этом разделе описывается совместимость [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] с другими функциями [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> Функции, совместимые с группами доступности Always On

В следующей таблице перечислены функции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , совместимые с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Ссылка в столбце **Дополнительные сведения** указывает, что имеются замечания по совместимости данной функции.

|Компонент|Дополнительные сведения|
|:------|:---------------|
|система отслеживания измененных данных|[Репликация, отслеживание изменений, изменение данных и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|отслеживание изменений|[Репликация, отслеживание изменений, изменение данных и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Автономные базы данных|[Автономные базы данных с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|Шифрование базы данных|[Зашифрованные базы данных с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|Моментальные снимки базы данных|[Моментальные снимки баз данных для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM и FileTable|[FILESTREAM и FileTable с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|Полнотекстовый поиск|Примечание. Полнотекстовые индексы синхронизируются с базами данных-получателями Always On.|
|доставка журналов;|[Необходимые условия для выполнения перехода от использования доставки журналов к использованию групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|Удаленное хранилище больших двоичных объектов|[Удаленное хранилище больших двоичных объектов (RBS) и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|Репликация|[Настройка репликации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [Обслуживание базы данных публикации AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [Репликация, отслеживание изменений, изменение данных и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [Подписчики репликации и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Службы Analysis Services|[Службы Analysis Services с группами доступности AlwaysOn](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Службы Reporting Services|Используйте вторичные реплики, доступные только для чтения, в качестве источника данных для отчетов для снижения нагрузки на первичную реплику, доступную для чтения и записи.<br /><br /> [Службы Reporting Services с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Компонент Service Broker|[Компонент Service Broker с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|Агент SQL Server|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> Функции, совместимые с группами доступности AlwaysOn с определенными ограничениями

Следующие функции взаимодействуют с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] с определенными ограничениями. Дополнительные сведения см. в статьях по ссылкам.

- Межбазовые транзакции и распределенные транзакции ([!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] и Windows Server 2016). Дополнительные сведения см. в статье [Транзакции между базами данных и распределенные транзакции для групп доступности AlwaysOn и зеркального отображения базы данных (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).
- [Сборщик данных системы статистики запроса](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query) не может надежно работать в среде с недоступными для чтения вторичными базами данных. Чтобы использовать сборщик данных системы статистики запросов, разрешите [доступ на чтение](configure-read-only-access-on-an-availability-replica-sql-server.md) всех вторичных реплик группы доступности. 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> Функции, несовместимые с группами доступности AlwaysOn

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] не работает в сочетании со следующими функциями.

- Зеркальное отображение базы данных. Дополнительные сведения см. в статье [Транзакции между базами данных и распределенные транзакции для групп доступности AlwaysOn и зеркального отображения базы данных (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

## <a name="related-content"></a><a name="RelatedContent"></a> См. также

- **Блоги**

  [Руководство по миграции. Миграция на отказоустойчивую кластеризацию и группы доступности SQL Server 2012 с предшествующих кластеров и зеркальных отображений](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [Блоги команды разработчиков SQL Server Always On: официальный блог по SQL Server Always On](/archive/blogs/sqlalwayson/)
  [Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)

- **Технические документы**

  [Руководство по миграции. Переход на группы доступности Always On с предыдущих развертываний, сочетающих зеркальное отображение базы данных и доставку журналов](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Руководство по решениям режима Always On в Microsoft SQL Server для обеспечения высокой доступности и аварийного восстановления](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [Технические документы Майкрософт по SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>См. также:

[Обзор групп доступности Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[Предварительные требования, ограничения и рекомендации для групп доступности Always On (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)