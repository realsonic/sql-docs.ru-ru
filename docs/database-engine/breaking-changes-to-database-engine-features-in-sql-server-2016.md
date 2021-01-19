---
title: Ядро СУБД. Критические изменения | Документация Майкрософт
titleSuffix: SQL Server 2016
description: Узнайте об изменениях в ядре СУБД в SQL Server 2016 (13.x) и более ранних версий, которые могут привести к нарушению функциональности предыдущей версии при обновлении.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], what's new
- breaking changes [SQL Server]
ms.assetid: 47edefbd-a09b-4087-937a-453cd5c6e061
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea6799570df43f5d49cadd93b98dc89528acacdd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171056"
---
# <a name="breaking-changes-to-database-engine-features-in-sql-server-2016"></a>Критические изменения в функциях ядра СУБД в SQL Server 2016

[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

  В этом разделе описаны критические изменения в [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] и предыдущих версиях [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Эти изменения могут нарушать работу приложений, скриптов или механизмов, основанных на более ранних версиях [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. При обновлении могут возникнуть следующие проблемы.  
  
##  <a name="breaking-changes-in-sssql15"></a><a name="SQL15"></a> Критические изменения в [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]  
  
-   Столбец *sample_ms* таблицы `sys.dm_io_virtual_file_stats` был расширен с типа данных **int** до **bigint**.  
  
-   Столбец *TimeStamp* таблицы `sys.fn_virtualfilestats` был расширен с типа данных **int** до **bigint**.  

-   При уровне совместимости базы данных 130 неявные преобразования типов данных из **datetime** в **datetime2** демонстрируют повышенную точность благодаря учету долей миллисекунд. В результате преобразования дают иные значения. Всегда используйте явное приведение к типу данных datetime2, когда имеется сценарий смешанного сравнения типов данных datetime и datetime2. Дополнительные сведения см. в этой [статье службы поддержки Майкрософт](https://support.microsoft.com/help/4010261).

-   При уровне совместимости базы данных 130 операции, которые выполняют неявные преобразования между определенными числовыми типами и типами данных даты и времени, демонстрируют повышенную точность и могут привести к иным преобразованным значениям. Это включает использование функций, которые требуют вычислений, таких как, например, `DATEDIFF` и `ROUND`. Дополнительные сведения см. в этой [статье службы поддержки Майкрософт](https://support.microsoft.com/help/4010261).

## <a name="previous-versions"></a><a name="previous-versions"></a> Предыдущие версии  

Сведения о критических изменениях в [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] и в некоторых более ранних версиях см. в статье [Критические изменения в функциях ядра СУБД в SQL Server 2016](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016).

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Архивная документация по очень старым версиям SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>См. также:  
 [Нерекомендуемые функции ядра СУБД в SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Нерекомендуемые функции ядра СУБД в SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Обратная совместимость компонента ядра СУБД SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [Улучшения SQL Server 2016 или SQL Server 2017 в Windows для обработки некоторых типов данных и нестандартных операций](https://support.microsoft.com/help/4010261)