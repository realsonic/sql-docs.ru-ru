---
title: Завершение обновления ядра СУБД | Документы Майкрософт
description: В этой статье описаны некоторые дополнительные действия, которые может потребоваться выполнить после завершения обновления ядра СУБД SQL Server.
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 3f08087e-e532-416c-8caa-e0ec88c57596
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c309c86f4f6609f1bd5383d311e1ce5781350fd9
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975465"
---
# <a name="complete-the-database-engine-upgrade"></a>Завершение обновления ядра СУБД

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

После завершения обновления SQL Server вам, возможно, потребуется выполнить некоторые дополнительные действия. следующие основные параметры.  
  
После обновления компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]выполните следующие действия.  
  
- **Резервное копирование баз данных**. Создание полной резервной копии каждой базы данных.  

- **Используйте новые возможности**. Некоторые изменения SQL Server 2016, 2017 и 2019 становятся доступными только после изменения уровня совместимости базы данных DATABASE_COMPATIBILITY на 130 или выше.  Дополнительные сведения и рекомендуемый рабочий процесс см. в разделе [Изменение режима совместимости базы данных и использование хранилища запросов](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Если база данных содержит оптимизированные для памяти таблицы, созданные в SQL Server 2014, см. статью [Статистика для таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).
  
- **Службы Integration Services.**  
  
     Миграция пакетов служб Integration Services в формат [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Дополнительные сведения см. в разделе [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
- **Службы Reporting Services:** в случае обновления методом новой установки восстановите ключи шифрования служб Reporting Services. Дополнительные сведения см. в разделе [Резервное копирование и восстановление ключей шифрования служб Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- **Master Data Services:**  обновите схему базы данных MDS и создайте веб-приложение SQL Server 2019. Дополнительные сведения см. в статье [Обновление служб Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md).  
  
- **Data Quality Services:** обновите схему базы данных DQS и проверьте обновление этой схемы. Дополнительные сведения см. в статье [Обновление служб Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
  
- **Полнотекстовый поиск**. Для обеспечения семантической согласованности результатов запроса заполните полнотекстовые каталоги повторно. Дополнительные сведения см. в разделе [Заполнение полнотекстовых индексов](../../relational-databases/search/populate-full-text-indexes.md).  
  
