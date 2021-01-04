---
title: Применение локального аудита и сбор данных о потреблении
description: Сведения о локальном аудите, который используется в SQL Server для сбора и отправки диагностических данных в корпорацию Майкрософт.
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f927e003673cb4397250fe532d57452ddb4e6445
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474565"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>Локальный аудит для сбора данных об использовании и данных диагностики (CEIP) SQL Server

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>Введение

Microsoft SQL Server включает в себя функции, которые могут собирать и отправлять в Майкрософт сведения о компьютере или устройстве пользователя через Интернет. Эти сведения называются *стандартными сведениями о компьютере*. Компонент локального аудита для [сбора данных об использовании и диагностических данных SQL Server](usage-and-diagnostic-data-configuration-for-sql-server.md) записывает собранные службой данные в указанную папку. Эти данные (журналы) будут отправлены в корпорацию Майкрософт. Локальный аудит позволяет клиентам просматривать все данные, которые корпорация Майкрософт собирает с помощью этой функции для обеспечения соответствия, выполнения нормативных требований или соблюдения конфиденциальности.  

Для SQL Server 2016 с накопительным пакетом обновления 2 и накопительным пакетом обновления 3 локальный аудит настраивается на уровне экземпляра для ядра СУБД SQL Server и служб Analysis Services (SSAS). Для SQL Server 2016 с накопительным пакетом обновления 4, SQL Server 2016 с пакетом обновления 1 (SP1) и более поздних выпусков локальный аудит также включен для служб SQL Server Integration Services (SSIS). Другие устанавливаемые компоненты SQL Server и средства SQL Server, которые скачиваются или устанавливаются позднее, не имеют функций локального аудита для сбора данных об использовании и диагностических данных.

## <a name="remarks"></a>Remarks

 - Удаление или отключение службы SQL CEIP не поддерживается. 
 - Удаление ресурсов SQL CEIP из группы кластера не поддерживается. 

Чтобы отказаться от сбора данных, см. сведения в разделе [Отключение локального аудита](#turning-local-audit-on-or-off)

## <a name="prerequisites"></a>Предварительные требования 

Ниже приведены предварительные требования для включения локального аудита на каждом экземпляре SQL Server: 

1. В экземпляре установлены исправления для SQL Server 2016 RTM с накопительным пакетом обновления 2 или более поздней версии. Для служб Integration Services в экземпляре установлены исправления для SQL 2016 RTM с накопительным пакетом обновления 4, SQL 2016 с пакетом обновления 1 (SP1) или более поздних выпусков.

1. Пользователь должен быть системным администратором или использовать роль, позволяющую добавлять и изменять раздел реестра, создавать папки, управлять их безопасностью, а также останавливать и запускать службу Windows.  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>Шаги предварительной настройки перед включением локального аудита

Перед включением локального аудита системный администратор должен сделать следующее:

1. Получить имя экземпляра SQL Server, а также учетную запись для входа в службу CEIP для SQL Server. 

1. Настройте новую папку для файлов локального аудита.

1. Предоставить разрешения для учетной записи входа в службу CEIP для SQL Server.

1. Создать раздел реестра для настройки целевого каталога локального аудита. 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>Получение учетной записи для входа в службу CEIP для SQL Server 

Чтобы получить учетную запись для входа в службу CEIP для SQL Server, сделайте следующее:
 
1. Запустите консоль **Службы**. Для этого нажмите клавиши **Windows+R**, чтобы открыть диалоговое окно **Выполнить**. Введите в текстовом поле *services.msc* и нажмите кнопку **ОК**, чтобы запустить консоль **Службы**.  

2. Перейдите к соответствующей службе. Например, для ядра СУБД найдите строку **Служба CEIP для SQL Server** **(*имя экземпляра*)** . Для служб Analysis Services найдите строку **CEIP для служб SQL Server Analysis Services** **(*имя экземпляра*)** . Для служб Integration Services найдите службу **CEIP для служб SQL Server Integration Services**.

3. Щелкните правой кнопкой службу и выберите **Свойства**. 

4. Откройте вкладку **Вход в систему**. Учетная запись для входа приведена в поле **Указанная учетная запись**. 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>Настройте новую папку для файлов локального аудита.    

Создайте папку (каталог локального аудита), в которую функция локального аудита будет записывать журналы. Например, полный путь к каталогу локального аудита для экземпляра ядра СУБД по умолчанию будет таким: *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >Задайте путь к папке локального аудита за пределами пути установки SQL Server, чтобы избежать возможных проблем с SQL Server из-за включения функций аудита и применения соответствующих исправлений.

|Решение о разработки|Рекомендация|  
|-----------------|----------|  
|Наличие свободного места |При умеренной рабочей нагрузке, когда используется около 10 баз данных, запланируйте около 2 МБ места на диске на базу данных на каждый экземпляр.|  
|Отдельные каталоги | Создайте каталог для каждого экземпляра. Например, используйте *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* для экземпляра SQL Server с именем `MSSQLSERVER`. Это упрощает управление файлами.
|Отдельные папки |Используйте отдельную папку для каждой службы. Например, для определенного имени экземпляра используйте одну папку для ядра СУБД. Если экземпляр Analysis Services использует то же имя экземпляра, создайте для служб Analysis Services отдельную папку. Наличие экземпляров ядра СУБД и служб Analysis Services, настроенных для одной и той же папки, приведет к тому, что все данные локального аудита из обоих экземпляров будут записываться в один файл журнала.| 
|Предоставление разрешений для учетной записи для входа в службу CEIP для SQL Server|Назначьте разрешения **Просмотр содержимого папки**, **Чтение** и **Запись** для доступа к учетной записи входа в службу CIEP для SQL Server.|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>Предоставление разрешений для учетной записи для входа в службу CEIP для SQL Server
  
1. В **проводнике**, перейдите к расположению новой папки.

1. Правой кнопкой мыши щелкните папку и выберите пункт **Свойства**. 

1. На вкладке **Безопасность** щелкните **Изменить** для управления разрешениями.

1. Выберите **Добавить** и введите учетные данные службы CEIP для SQL Server. Например, `NT Service\SQLTELEMETRY`.

1. Выберите **Проверить имена**, чтобы проверить указанное имя, а затем нажмите кнопку **ОК**.

1. В диалоговом окне **Разрешение** выберите учетную запись входа в службу CEIP для SQL Server и щелкните **Просмотр содержимого папки**, **Чтение** и **Запись**.

1. Нажмите кнопку **ОК**, чтобы немедленно применить изменения разрешений. 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>Создание раздела реестра для настройки целевого каталога локального аудита

1. Запустите программу regedit.

1. Перейдите по соответствующему пути CPE.

   | Версия | **_Ядро СУБД_* _ — раздел реестра |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL_ *13**.* имя экземпляра*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **14**.*имя экземпляра*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **15**.*имя экземпляра*\\CPE |
   | &nbsp; | &nbsp; |

   | Версия | ***Службы Analysis Services** _ — раздел реестра |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS_ *13**.* имя экземпляра*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **14**.*имя экземпляра*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **15**.*имя экземпляра*\\CPE |  
   | &nbsp; | &nbsp; |

   | Версия | ***Службы Integration Services** _ — раздел реестра |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\_ *130** |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**140** |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**150** |
   | &nbsp; | &nbsp; |

1. Щелкните путь CPE правой кнопкой мыши и выберите пункт **Создать**. Выберите **Строковое значение**.

1. Назовите новый раздел реестра " `UserRequestedLocalAuditDirectory`". 
 
## <a name="turning-local-audit-on-or-off"></a>Включение и отключение локального аудита

После завершения предварительной настройки вы можете включить локальный аудит. Для этого используйте учетную запись системного администратора или аналогичную роль с разрешением изменять разделы реестра для включения и отключения локального аудита, сделав следующее: 

1. Запустите программу **regedit**.  

1. Перейдите по соответствующему [пути](#create-a-registry-key-setting-to-configure-local-audit-target-directory) CPE. 

1. Щелкните правой кнопкой мыши **UserRequestedLocalAuditDirectory** и выберите пункт *Изменить*. 

1. Чтобы включить локальный аудит, введите соответствующий путь, например *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* .
 
    Чтобы отключить локальный аудит, задайте пустое значение для **UserRequestedLocalAuditDirectory**.

1. Закройте программу **regedit**. 

Если служба CEIP для SQL Server запущена, она должна сразу же распознать настройку локального аудита. Чтобы запустить службу программы улучшения качества SQL Server, системному администратору или пользователю, имеющему права на запуск или остановку служб Windows, требуется выполнить указанные ниже действия. 

1. Запустите консоль **Службы**. Для этого нажмите клавиши **Windows+R**, чтобы открыть диалоговое окно **Выполнить**. Введите в текстовом поле *services.msc* и нажмите кнопку **ОК**, чтобы запустить консоль **Службы**.  

1. Перейдите к соответствующей службе. 

    - Для ядра СУБД используйте **Служба CEIP для SQL Server (*имя экземпляра*)**.     
    - Для служб Analysis Services используйте **CEIP для служб SQL Server Analysis Services (*имя экземпляра*)**.
    - Для служб Integration Services: 
        - Для SQL 2016 используйте *службу CEIP 13.0 для служб SQL Server Integration Services*.
        - Для SQL 2017 используйте *службу CEIP 14.0 для служб SQL Server Integration Services*.
    - Для SQL 2019 используйте *службу CEIP 15.0 для служб SQL Server Integration Services*.

1. Щелкните правой кнопкой службу и выберите "Перезапуск". 

1. Убедитесь в том, что служба находится в состоянии **Выполняется**. 

Функция локального аудита создает один файл журнала в день. Файлы журнала имеют форму `<YYYY-MM-DD>.json`. Например, *2016-07-12.json*. Если в назначенном каталоге уже создан файл за этот день, функция локального аудита добавит в него данные. В противном случае она создает новый файл за этот день. 

  >[!NOTE]
  > После включения локального аудита на создание файла журнала может потребоваться до пяти минут. 

## <a name="maintenance"></a>Обслуживание 

1. Чтобы ограничить использование дискового пространства файлами, записываемыми функцией локального аудита, настройте политику или регулярное задание очистки каталога локального аудита для удаления старых и ненужных файлов.  

2. Защитите путь к каталогу локального аудита, чтобы он был доступен только соответствующим пользователям. Обратите внимание, что файлы журнала содержат сведения, описанные в статье [Настройка SQL Server 2016 для отправки отзывов в корпорацию Майкрософт](https://support.microsoft.com/kb/3153756). Доступ для чтения этого файла должен быть запрещен большинству сотрудников вашей организации.  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>Словарь данных для структуры выходных данных локального аудита 

- Файлы журнала локального аудита имеют формат JSON и содержат набор объектов (строк), представляющих точки данных, которые отправляются в корпорацию Майкрософт во время **emitTime**.
- Каждая строка соответствует определенной схеме, определенной значением **schemaVersion**.
- Каждая строка является выходным значением сеанса службы SQLCEIP, указанного в **sessionID**.
- Строки выводятся в определенной последовательности, заданной значением **sequence**.
- Каждая строка точки данных содержит выходные данные **queryIdentifier**, которые могут быть запросом T-SQL, сеансом XE или сообщением, связанным с типом трассировки, который обозначается значением **traceName**.
- Группирование и контроль версий для **queryIdentifiers** осуществляются вместе с **querySetVersion**.
- **data** содержит выходные данные соответствующего выполнения запроса, занявшего время **queryTimeInTicks**.
- **queryIdentifiers** для запросов T-SQL имеют определение запроса T-SQL, хранящееся в запросе.

| Логическая иерархия сведений о локальном аудите | Связанные столбцы |
| ------ | -------|
| Header | emitTime, schemaVersion 
| Компьютер | operatingSystem 
| Экземпляр | instanceUniqueID, correlationID, clientVersion 
| Сеанс | sessionID, traceName 
| Запрос | sequence, querySetVersion, queryIdentifier, query, queryTimeInTicks 
| Данные |  . 

### <a name="namevalue-pairs-definition-and-examples"></a>Определение пары "имя — значение" и примеры 

Указанные ниже столбцы представляют порядок выходных данных файлов для локального аудита. Односторонний хэш-алгоритм SHA 256 используется для анонимизации значений в указанных ниже столбцах.  

| name | Описание | Примеры значений
|-------|--------| ----------|
|instanceUniqueID| Анонимизированный идентификатор экземпляра | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| Версия схемы SQLCEIP |  3 
|emitTime |Время вывода точки данных в формате UTC | 2016-09-08T17:20:22.1124269Z 
|sessionID | Идентификатор для обслуживания службы SQLCEIP | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | Заполнитель для дополнительного идентификатора | 0 
|sequence | Порядковый номер точек данных, отправленных в рамках сеанса | 15 
|clientVersion | Версия экземпляра SQL Server | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | Версия операционной системы, где установлен экземпляр SQL Server | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | Версия группы определений запросов | 1.0.0.0 
|traceName | Категории трассировки: (SQLServerXeQueries, SQLServerPeriodicQueries, SQLServerOneSettingsException) | SQLServerPeriodicQueries 
|queryIdentifier | Идентификатор запроса | SQLServerProperties.002 
|.   | Выходные сведения, собранные для queryIdentifier в виде вывода запроса T-SQL, сеанса XE или приложения |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tStandard Edition (64-bit) on Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|query| Если это применимо, определение запроса T-SQL, связанное с queryIdentifier, который создает данные.        Этот компонент не отправляется службой CEIP SQL Server. Он включен в локальный аудит только для справки.| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027PolyBaseRole\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | Время, затрачиваемое на выполнение запроса в следующей категории трассировки: (SQLServerXeQueries, SQLServerPeriodicQueries) |  0 
 
### <a name="trace-categories"></a>Категории трассировки 
Сейчас мы собираем следующие категории трассировки. 

- **SQLServerXeQueries**: содержит точки данных, собранные в рамках сеанса расширенных событий.
- **SQLServerPeriodicQueries**: содержит точки данных, собранные периодическими запросами, выполняемыми в экземпляре SQL Server.
- **SQLServerPerDBPeriodicQueries**: содержит точки данных, собранные периодическими запросами, выполняемыми для баз данных (до 30) в экземпляре SQL Server.
- **SQLServerOneSettingsException**: содержит сообщения об исключениях, связанные с обновлением схемы и/или набора запроса.
- **DigitalProductID**: содержит точки данных для статистической обработки обезличенного (SHA-256) хэшированного идентификатора цифрового продукта для экземпляров SQL Server. 

### <a name="local-audit-file-examples"></a>Примеры файла локального аудита



Ниже приведен отрывок выходных данных файла JSON для локального аудита.

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**Какие данные содержатся в файлах журнала локального аудита?**
Эти файлы журналов записываются в формате JSON. Каждая строка соответствует объекту JSON, который представляет фрагмент данных об использовании или диагностических данных, отправляемых корпорации Майкрософт. Имена полей должны быть понятными и очевидными.

**Что произойдет, если администратор базы данных отключит сбор данных об использовании и диагностических данных?**
Запись файла локального аудита не будет выполняться.

**Что произойдет, если нет подключения к Интернету или компьютер защищен брандмауэром?**
Данные об использовании и диагностические данные SQL Server не будут отправляться в корпорацию Майкрософт. При правильной настройке функция по-прежнему будет пытаться записать журналы локального аудита.

**Как администраторы баз данных отключают локальный аудит?**
Удаляя запись раздела реестра UserRequestedLocalAuditDirectory.

**Кто имеет доступ к файлам журнала локального аудита?**
Любой пользователь в вашей организации с доступом к каталогу локального аудита.

**Как администраторы баз данных управляют файлами журналов, записываемыми в указанный каталог?**
Администраторам баз данных необходимо самостоятельно следить за очисткой файлов в каталоге, чтобы избежать использования слишком большого объема места на диске.

**Существует ли клиент или инструмент, который можно использовать для чтения выходных данных JSON?**
Выходные данные можно прочитать в Блокноте, Visual Studio или любом подходящем средстве чтения JSON.
Кроме того, прочитать файл JSON и проанализировать данные можно в экземпляре SQL Server, как показано ниже. Дополнительные сведения чтении файла JSON в SQL Server см. в разделе [Импорт файлов JSON в SQL Server с помощью функции OPENROWSET (BULK) и OPENJSON (Transact-SQL)](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server).

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>См. также раздел
[Локальный аудит для сбора данных об использовании и данных диагностики в SSMS](../ssms/sql-server-management-studio-telemetry-ssms.md)