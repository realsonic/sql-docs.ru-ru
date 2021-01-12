---
description: ALTER RESOURCE GOVERNOR (Transact-SQL)
title: ALTER RESOURCE GOVERNOR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER RESOURCE GOVERNOR
- ALTER_RESOURCE_GOVERNOR_TSQL
- ALTER RESOURCE GOVERNOR RECONFIGURE
- ALTER_RESOURCE_GOVERNOR_RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE GOVERNOR
- RECONFIGURE, ALTER RESOURCE GOVERNOR
ms.assetid: 442c54bf-a0a6-4108-ad20-db910ffa6e3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 087b03fe9fab6e505295121cdd34b07dbf9db2c2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092061"
---
# <a name="alter-resource-governor-transact-sql"></a>ALTER RESOURCE GOVERNOR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Эта инструкция применяется для выполнения следующих действий Resource Governor в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Применение изменений конфигурации, указанных при вызове инструкций CREATE|ALTER|DROP WORKLOAD GROUP, или CREATE|ALTER|DROP RESOURCE POOL, или CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
-   Включение или отключение регулятора ресурсов.  
  
-   Настройка классификации для входящих запросов.  
  
-   Сброс статистики группы рабочей нагрузки и пула ресурсов.  
  
-   Устанавливает максимальное число операций ввода-вывода для тома диска.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER RESOURCE GOVERNOR   
      { DISABLE | RECONFIGURE }  
    | WITH ( CLASSIFIER_FUNCTION = { schema_name.function_name | NULL } )  
    | RESET STATISTICS  
    | WITH ( MAX_OUTSTANDING_IO_PER_VOLUME = value )   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 DISABLE  
 Отключение регулятора ресурсов. В результате отключения регулятора ресурсов происходит следующее.  
  
-   Функция-классификатор не выполняется.  
  
-   Новые соединения автоматически попадают в группу по умолчанию.  
  
-   Инициированные системой запросы попадают во внутреннюю группу рабочей нагрузки.  
  
-   Все существующие параметры групп рабочей нагрузки и пулов ресурсов сбрасываются в значения по умолчанию. В этом случае при достижении ограничений не возникает никаких событий.  
  
-   Обычное наблюдение за системой не затрагивается.  
  
-   Изменения конфигурации можно внести, но изменения не будут выполнены, если включен регулятор ресурсов.  
  
-   При перезапуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] регулятор ресурсов не будет загружать конфигурацию; вместо этого будут настроены только внутренние группы и пулы, а также группы и пулы, заданные по умолчанию.  
  
 RECONFIGURE  
 Если регулятор ресурсов не включен, то RECONFIGURE включит регулятор ресурсов. В результате включения регулятора ресурсов произойдет следующее.  
  
-   Будет выполнена функция-классификатор для новых соединений, что позволит связать их рабочую нагрузку с определенными группами рабочей нагрузки.  
  
-   Ограничения ресурсов, заданные в конфигурации регулятора ресурсов, будут соблюдены и применены.  
  
-   Любые изменения конфигурации, внесенные в то время, пока регулятор ресурсов был отключен, затрагивают запросы, которые существовали до включения регулятора ресурсов.  
  
 Параметр RECONFIGURE, использованный в ситуации, когда Resource Governor выполняется, применяет любые изменения конфигурации, запрашиваемые при выполнении инструкций CREATE|ALTER|DROP WORKLOAD GROUP, или CREATE|ALTER|DROP RESOURCE POOL, или CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
> [!IMPORTANT]  
>  Чтобы любые изменения конфигурации вступили в силу, необходимо вызвать инструкцию ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
 CLASSIFIER_FUNCTION = { _schema_name_ **.** _function_name_ | NULL }  
 Регистрирует функцию классификации, указанную *schema_name.function_name*. Эта функция классифицирует каждый новый сеанс и назначает запросы сеанса в группу рабочей нагрузки. При использовании значения NULL новые сеансы автоматически назначаются в группу рабочей нагрузки по умолчанию.  
  
 RESET STATISTICS  
 Сбрасывает статистику всех групп рабочей нагрузки и пулов ресурсов. Дополнительные сведения см. в разделах [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) и [sys.dm_resource_governor_resource_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
 MAX_OUTSTANDING_IO_PER_VOLUME = *value*  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 Устанавливает максимальное число поставленных в очередь операций ввода-вывода для тома диска. Эти операции ввода-вывода могут быть операциями чтения или записи любого размера.  Максимальное значение для MAX_OUTSTANDING_IO_PER_VOLUME равно 100. Это значение не в процентах. Этот параметр предназначен для подстройки управление ресурсами ввода-вывода к характеристикам ввода-вывода дискового тома. Рекомендуется поэкспериментировать с различными значениями и ознакомиться с использованием калибровочного средства наподобие IOMeter, [DiskSpd](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) или SQLIO (не рекомендуется), чтобы определить максимальное значение для имеющейся подсистемы хранения. Данный параметр предоставляет проверку безопасности на уровне системы, позволяющую SQL Server соответствовать минимальному числу операций ввода-вывода в секунда для пулов ресурсов, даже если в других пулах значение MAX_IOPS_PER_VOLUME неограниченно. Дополнительные сведения о параметре MAX_IOPS_PER_VOLUME см. в разделе [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
 ALTER RESOURCE GOVERNOR DISABLE, ALTER RESOURCE GOVERNOR RECONFIGURE и ALTER RESOURCE GOVERNOR RESET STATISTICS не могут использоваться в пользовательской транзакции.  
  
 Параметр RECONFIGURE входит в состав синтаксиса Resource Governor, и его не следует путать с ключевым словом [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), которое представляет собой отдельную инструкцию DDL.  
  
 Рекомендуется ознакомиться с состояниями регулятора ресурсов, прежде чем приступить к выполнению инструкций DLL. Дополнительные сведения см. в разделе [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) (Регулятор ресурсов).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CONTROL SERVER.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-starting-the-resource-governor"></a>A. Запуск регулятора ресурсов  
 При первоначальной установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] регулятор ресурсов отключен. В следующем примере выполняется запуск регулятора ресурсов. После выполнения предыдущей инструкции регулятор ресурсов запускается и может использовать стандартные группы рабочей нагрузки и пулы ресурсов.  
  
```sql  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="b-assigning-new-sessions-to-the-default-group"></a>Б. Назначение новых сеансов группе по умолчанию  
 В следующем примере назначаются все новые сеансы группе рабочей нагрузки по умолчанию путем удаления всех существующих функций-классификаторов из конфигурации регулятора ресурсов. Если не назначены функции-классификаторы, все новые сеансы назначаются группе рабочей нагрузки по умолчанию. Это изменение применимо только к новым сеансам. На существующие сеансы оно не влияет.  
  
```sql  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = NULL);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="c-creating-and-registering-a-classifier-function"></a>В. Создание и регистрация функции-классификатора  
 В следующем примере создается функция-классификатор с именем `dbo.rgclassifier_v1`. Эта функция классифицирует каждый новый сеанс на основе имени пользователя или имени приложения и назначает запросы сеанса и запросы в группу рабочей нагрузки. Сеансы, которые не сопоставляются с именами определенного пользователя или приложения, назначаются группе рабочей нагрузки по умолчанию. После этого регистрируется функция-классификатор и применяются изменения конфигурации.  
  
```sql  
-- Store the classifier function in the master database.  
USE master;  
GO  
SET ANSI_NULLS ON;  
GO  
SET QUOTED_IDENTIFIER ON;  
GO  
CREATE FUNCTION dbo.rgclassifier_v1() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
-- Declare the variable to hold the value returned in sysname.  
    DECLARE @grp_name AS sysname  
-- If the user login is 'sa', map the connection to the groupAdmin  
-- workload group.   
    IF (SUSER_NAME() = 'sa')  
        SET @grp_name = 'groupAdmin'  
-- Use application information to map the connection to the groupAdhoc  
-- workload group.  
    ELSE IF (APP_NAME() LIKE '%MANAGEMENT STUDIO%')  
        OR (APP_NAME() LIKE '%QUERY ANALYZER%')  
            SET @grp_name = 'groupAdhoc'  
-- If the application is for reporting, map the connection to  
-- the groupReports workload group.  
    ELSE IF (APP_NAME() LIKE '%REPORT SERVER%')  
        SET @grp_name = 'groupReports'  
-- If the connection does not map to any of the previous groups,  
-- put the connection into the default workload group.  
    ELSE  
        SET @grp_name = 'default'  
    RETURN @grp_name  
END;  
GO  
-- Register the classifier user-defined function and update the   
-- the in-memory configuration.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION=dbo.rgclassifier_v1);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="d-resetting-statistics"></a>Г. Сброс статистики  
 В следующем примере сбрасывается статистика всех групп рабочей нагрузки и пулов ресурсов.  
  
```sql 
ALTER RESOURCE GOVERNOR RESET STATISTICS;  
```  
  
### <a name="e-setting-the-max_outstanding_io_per_volume-option"></a>Д. Установка параметра MAX_OUTSTANDING_IO_PER_VOLUME  
 В следующем примере для параметра MAX_OUTSTANDING_IO_PER_VOLUME задается значение 20.  
  
```sql  
ALTER RESOURCE GOVERNOR  
WITH (MAX_OUTSTANDING_IO_PER_VOLUME = 20);   
```  
  
## <a name="see-also"></a>См. также  
 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)  
  
  
