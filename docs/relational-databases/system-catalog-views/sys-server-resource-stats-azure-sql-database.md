---
description: sys.server_resource_stats (база данных SQL Azure)
title: sys.server_resource_stats (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 06/28/2018
ms.service: sql-database
ms.topic: language-reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current
ms.openlocfilehash: 8a913c3bf4f01828fcf75df1e3c69dca9149e2de
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241825"
---
# <a name="sysserver_resource_stats-azure-sql-database"></a>sys.server_resource_stats (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Возвращает данные об использовании ЦП, операции ввода-вывода и данных хранилища для Управляемый экземпляр Azure SQL. Эти данные собираются и объединяются с пятиминутными интервалами. Для отчетов каждые 15 секунд имеется одна строка. Возвращаемые данные включают загрузку ЦП, размер хранилища, использование операций ввода-вывода и SKU. Данные предыстории хранятся приблизительно в течение 14 суток.

Представление **sys.server_resource_stats** имеет различные определения в зависимости от версии управляемый экземпляр SQL Azure, с которой связана база данных. Рассмотрим эти различия и все изменения, которые требуются приложению при обновлении до новой версии сервера.
 
  
 В следующей таблице описываются столбцы, доступные на сервере v12.  
  
|Столбцы|Тип данных|Описание|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime2**|Время в формате UTC, обозначающее начало пятнадцать-секундного интервала отчетности|  
|end_time|**datetime**|Время в формате UTC, указывающее Окончание интервала формирования отчетов в пятнадцать секунд|
|resource_type|Nvarchar(128)|Тип ресурса, для которого предоставляются метрики|
|resource_name|NVARCHAR(128)|Имя ресурса.|
|sku|NVARCHAR(128)|Управляемый экземпляр уровня служб экземпляра. Допустимы следующие значения: <br><ul><li>Общее назначение</li></ul><ul><li>Критически важный для бизнеса</li></ul>|
|hardware_generation|NVARCHAR(128)|Идентификатор создания оборудования: например, Gen 4 или Gen 5|
|virtual_core_count|int|Представляет число виртуальных ядер на экземпляр (8, 16 или 24 в общедоступной предварительной версии)|
|avg_cpu_percent|Decimal (5, 2)|Среднее использование вычислительных ресурсов в процентах от ограничения уровня служб Управляемый экземпляр, используемого экземпляром. Он вычисляется как сумма времени ЦП всех пулов ресурсов для всех баз данных в экземпляре и распределяется по доступному времени ЦП для этого уровня в заданном интервале.|
|reserved_storage_mb|BIGINT|Зарезервированное хранилище на экземпляр (объем дискового пространства, приобретенный клиентом для управляемого экземпляра)|
|storage_space_used_mb|десятичное число (18, 2)|Хранилище, используемое всеми файлами базы данных в управляемом экземпляре (включая пользовательские и системные базы данных)|
|io_request|BIGINT|Общее число физических операций ввода-вывода в пределах интервала|
|io_bytes_read|BIGINT|Число физических байтов, считанных за интервал|
|io_bytes_written|BIGINT|Число физических байтов, записанных в пределах интервала|

 
> [!TIP]  
>  Дополнительные сведения об этих ограничениях и уровнях служб см. в статьях [управляемый экземпляр уровни служб](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers).  
    
## <a name="permissions"></a>Разрешения  
 Это представление доступно для всех ролей пользователей с разрешениями на подключение к базе данных **master** .  
  
## <a name="remarks"></a>Комментарии  
 Данные, возвращаемые **sys.server_resource_stats** , выражаются как общее значение, используемое в байтах или мегабайтах (в именах столбцов), отличном от avg_cpu, который выражается в процентах от максимально допустимого количества допустимых ограничений для уровня службы или производительности, который вы используете.  
 
## <a name="examples"></a>Примеры  
В следующем примере возвращается среднее использование ЦП за последние семь дней.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.server_resource_stats   
WHERE start_time BETWEEN @s AND @e  
GO;
```  
    
## <a name="see-also"></a>См. также  
 [Уровни служб Управляемого экземпляра](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers)
