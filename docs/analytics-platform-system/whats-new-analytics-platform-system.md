---
title: Новые возможности
description: Узнайте о новых возможностях Microsoft Analytics Platform System, масштабируемом локальном устройстве, где размещается MPP SQL Server Parallel Data Warehouse.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 004550d95ca69bae45c518b8054886e635e829df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091787"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Новые возможности в системе платформы аналитики — масштабируемое хранилище данных MPP
Узнайте о новых возможностях в последних обновлениях устройств для Microsoft Analytics Platform System (ТД). ТД — это масштабное локальное устройство, которое размещает MPP SQL Server Parallel Data Warehouse. 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
Дата выпуска — Ноябрь 2020

### <a name="scvmm2016"></a>SCVMM2016
ТД CU 7.7 Software обновляет виртуальную машину VMM до Windows Server 2016 и устанавливает SCVMM2016. Версия SCVMM 2012 R2, используемая в данный момент, имеет дату окончания срока действия 2022 июля. Новый диспетчер SCVMM необходимо поддерживать, делая CU 7.7 обязательным обновлением. Клиенты законодателей, как можно скорее выполнить обновление до CU 7.7.

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>Адаптер назначения служб SSIS для SQL Server 2019 в качестве целевого объекта
Новый адаптер загрузки APS SSIS, который поддерживает SQL Server 2019 в качестве целевого объекта развертывания, можно скачать с [сайта скачивания](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
Дата выпуска — 2020 апреля

### <a name="rename-column"></a>Переименование столбца
После обновления до CU 7.6 Клиенты смогут переименовать столбец таблицы, созданной пользователем. Синтаксис, примеры, ограничения и дополнительные сведения см. в разделе [Rename (Transact-SQL)](../t-sql/statements/rename-transact-sql.md) .

### <a name="alter-view"></a>Изменение представления
Теперь клиенты смогут изменять представления. Дополнительные сведения см. в разделе [ALTER View (Transact-SQL)](../t-sql/statements/alter-view-transact-sql.md) .

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
Дата выпуска — сентябрь 2019

### <a name="alter-external-data-source"></a>Изменение внешнего источника данных
Клиенты смогут изменять определение внешнего источника данных с помощью обновления CU 7.5. Клиенты с узлом имен Hadoop с высоким уровнем доступности теперь могут изменять источник данных для изменения аргументов при отработке отказа. Для ТД можно изменить только расположение, RESOURCE_MANAGER_LOCATION и УЧЕТные данные. Дополнительные сведения см. в разделе [Изменение внешнего источника данных](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017&preserve-view=true) .

### <a name="cdh-515-and-516-support-with-polybase"></a>Поддержка CDH 5,15 и 5,16 с Polybase
Polybase на ТД с обновлением CU 7.5 теперь поддерживает версии CDH 5,15 и 5,16 для распространения Hadoop из Cloudera. Используйте вариант 6 для версий CDH 5. x. 

### <a name="try_convert-and-try_cast-support"></a>Поддержка Try_Convert и Try_Cast
CU 7,5 APS теперь поддерживает функции [TRY_CAST](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017&preserve-view=true) и [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017&preserve-view=true) TSQL. Обе эти функции возвращают значение, преобразованное в указанный тип данных, если преобразование выполнено. в противном случае возвращает значение null.

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
Дата выпуска — Май 2019

### <a name="loading-large-rows-with-dwloader"></a>Загрузка больших строк с помощью dwloader
Начиная с ТД CU 7.4, клиенты могут использовать новый dwloader для загрузки строк в таблицы, размер которых превышает 32 КБ (32 768 байт). Новый dwloader поддерживает параметр-l, который принимает целочисленное значение от 32768 до 33554432 (в байтах) для загрузки строк, превышающих 32 КБ. Используйте этот параметр только при загрузке больших строк (более 32 КБ), так как этот параметр выделит больше памяти на клиенте и сервере и может замедлить загрузку. Вы можете скачать новый dwloader с [сайта загрузки](https://www.microsoft.com/download/details.aspx?id=57472).  

### <a name="hdp-30-and-31-support-with-polybase"></a>Поддержка HDP 3,0 и 3,1 с Polybase
Polybase на ТД теперь поддерживает HDP 3,0 и 3,1 с этим обновлением. Используйте вариант 7 для версий HDP 3. x. Дополнительные сведения см. на странице [подключения polybase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) .

### <a name="utf16-file-support-with-polybase"></a>Поддержка файлов UTF16 с Polybase
Polybase теперь поддерживает чтение текстовых файлов с разделителями в кодировке UTF16 (LE). Дополнительные сведения о настройке см. в разделе [Создание формата внешнего файла](../t-sql/statements/create-external-file-format-transact-sql.md) . 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
Дата выпуска — Декабрь 2018

### <a name="common-subexpression-elimination"></a>Исключение общих частей выражений
ТД CU 7.3 улучшает производительность запросов с помощью общего исключения части выражения в оптимизаторе запросов SQL. Улучшение запросов улучшается двумя способами. Первое преимущество — возможность определять и устранять такие выражения, что позволяет сократить время компиляции SQL. Вторым и более важным преимуществом являются операции перемещения данных для этих избыточных частей выражения, поэтому время выполнения запросов становится быстрее. Подробное описание этой функции можно найти [здесь](common-sub-expression-elimination.md).

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>Соединитель AP Informatica для опубликованного 10.2.0 Informatica
Мы выпустили новую версию соединителей Informatica для ТД, которая работает с Informatica версии 10.2.0 и 10.2.0 Hotfix 1. Новые соединители можно скачать с [сайта скачивания](https://www.microsoft.com/download/details.aspx?id=57472).
> [!NOTE]
> Соединитель AP Informatica для Informatica 10.2.0 или 10.2.0 Hotfix 1 не работает в режиме ограниченного TLS 1.2 и требует полной функциональности TLS 1.0 и 1,1.

#### <a name="supported-versions"></a>Поддерживаемые версии

| Версия APS | Informatica PowerCenter | Драйвер |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| ТД 2016 и более поздние версии | 10.2.0, 10.2.0 исправление 1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
Дата выпуска — Октябрь 2018

### <a name="support-for-tls-12"></a>Поддержка TLS 1.2
ТД CU 7.2 поддерживает TLS 1,2. Клиентский компьютер до ТД и обмен данными AP между узлами теперь можно настроить для обмена данными только по протоколу TLS 1.2. Такие средства, как SSDT, SSIS и Dwloader, установленные на клиентских компьютерах, настроенные для обмена данными только через TLS 1,2, теперь могут подключаться к ТД с помощью TLS 1,2. По умолчанию ТД поддерживает все версии TLS (1,0, 1,1 и 1,2) для обеспечения обратной совместимости. Если вы хотите настроить устройство APS для исключительного использования TLS 1,2, это можно сделать, изменив параметры реестра. 

Дополнительные сведения см. [в разделе Настройка TLS 1.2 на ТД](configure-tls12-aps.md).

### <a name="hadoop-encryption-zone-support-for-polybase"></a>Поддержка зоны шифрования Hadoop для Polybase
Теперь Polybase может взаимодействовать с зонами шифрования Hadoop. См. раздел изменения конфигурации APS, которые необходимы в разделе [Настройка безопасности Hadoop](polybase-configure-hadoop-security.md#encryptionzone).

### <a name="insert-select-maxdop-options"></a>Insert-Select параметры MAXDOP
Мы добавили [параметр функции](appliance-feature-switch.md) , который позволяет выбрать параметры MAXDOP больше 1 для операций вставки. Теперь можно задать для параметра MAXDOP значение 0, 1, 2 или 4. Значение по умолчанию — 1.

> [!IMPORTANT]  
> Увеличение MAXDOP иногда может привести к более медленным операциям или ошибкам взаимоблокировки. Если это происходит, измените значение параметра обратно на MAXDOP 1 и повторите операцию.

### <a name="columnstore-index-health-dmv"></a>Динамическое административное представление работоспособности индекса ColumnStore
Сведения о работоспособности индекса columnstore можно просмотреть с помощью **dm_pdw_nodes_db_column_store_row_group_physical_stats** динамического административного представления. Используйте следующее представление, чтобы определить фрагментацию и решить, когда следует перестроить или реорганизовать индекс columnstore.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>Увеличение диапазона дат Polybase для файлов ORC и Parquet
Чтение, импорт и экспорт типов данных даты с помощью Polybase теперь поддерживает даты до 1970-01-01 и после 2038-01-20 для типов файлов ORC и Parquet.

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>Адаптер назначения служб SSIS для SQL Server 2017 в качестве целевого объекта
Новый адаптер загрузки APS SSIS, который поддерживает SQL Server 2017 в качестве целевого объекта развертывания, можно скачать с [сайта скачивания](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
Дата выпуска — июль 2018

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>Команды DBCC не используют слоты выдачи (изменение поведения)
ТД поддерживает подмножество команд T-SQL [DBCC](../t-sql/database-console-commands/dbcc-transact-sql.md) , таких как [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md). Раньше эти команды потребляли [слот выдачи](./workload-management.md?view=aps-pdw-2016-au7&preserve-view=true&#concurrency-slots), уменьшая количество пользовательских нагрузок и запросов, которые можно было выполнить. `DBCC`Теперь команды выполняются в локальной очереди, которая не использует слоты параллелизма пользователей, что повышает общую производительность выполнения запросов.

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>Заменяет некоторые вызовы метаданных объектами каталога
Использование объектов каталога для вызовов метаданных вместо использования SMO демонстрирует улучшение производительности в ТД. Начиная с CU 7.1, некоторые из этих вызовов метаданных теперь используют объекты каталога по умолчанию. Это поведение можно отключить с помощью [переключателя функций](appliance-feature-switch.md) , если клиенты, использующие запросы метаданных, могут столкнуться с проблемами.

### <a name="bug-fixes"></a>Исправленные ошибки
Обновление до SQL Server 2016 с пакетом обновления 2 (SP2) CU2 с ТД CU 7.1. Обновление устраняет некоторые проблемы, описанные ниже.

| Заголовок | Описание |
|:---|:---|
| **Потенциальная взаимоблокировка перемещения кортежей** |Обновление устраняет долговременную возможность взаимной блокировки в распределенной транзакции и фоновом потоке перемещения кортежей. После установки CU 7.1 клиенты, которые использовали TF634 для отключения перемещения кортежей в качестве параметра запуска SQL Server или глобального флага трассировки, могут безопасно удалить его. | 
| **Сбой определенной задержки или запроса интереса** |При этом обновлении некоторые запросы к таблицам CCI с вложенными функциями Lag и Lead будут устранены. | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
Дата выпуска — Май 2018

ТД 2016 является необходимым условием для обновления до AU7. Ниже перечислены новые возможности в APS AU7.

### <a name="auto-create-and-auto-update-statistics"></a>Автоматическое создание и автоматическое обновление статистики
ТД AU7 автоматически создает и обновляет статистику по умолчанию. Для обновления параметров статистики администраторы могут использовать новый пункт меню переключателя компонента в [Configuration Manager](appliance-configuration.md#CMTasks). [Переключатель функции](appliance-feature-switch.md) управляет режимом автоматического создания, автоматического обновления и асинхронным обновлением статистики. Кроме того, параметры статистики можно обновить с помощью инструкции [ALTER DATABASE (Параллельное хранилище данных)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) .

### <a name="t-sql"></a>T-SQL
@varТеперь поддерживается SELECT. Дополнительные сведения см. в разделе [Выбор локальной переменной](../t-sql/language-elements/select-local-variable-transact-sql.md) . 

Теперь поддерживаются хэш-коды и группы заказов указания запросов. Дополнительные сведения см. в разделе [указания (Transact-SQL) — Query](../t-sql/queries/hints-transact-sql-query.md) .

### <a name="feature-switch"></a>Переключение компонентов
ТД AU7 представляет Переключение функций в [Configuration Manager](launch-the-configuration-manager.md). Аутостатсенаблед и Дмспроцессстопмессажетимеаутинсекондс теперь являются настраиваемыми параметрами, которые могут быть изменены администраторами.

### <a name="known-issues"></a>Известные проблемы
При использовании программного обеспечения APS AU7 предоставляется обновление Intel BIOS, устраняющее проблему, описанную в статье *атаки с помощью побочных каналов выполнения*. Атаки, вызывающие уязвимости, называются *устранением рисков Spectre и Meltdown*. Пакет обновления BIOS устанавливается вручную, но не входит в состав установки программного обеспечения APS AU7 Software.

Корпорация Майкрософт рекомендует всем клиентам установить обновленную версию BIOS. Корпорация Майкрософт оценивает влияние затенения виртуальных адресов ядра (квас), косвенного обращения к таблице страниц ядра (КПТИ) и смягчение прогнозов ветвей (ИБП) на различных рабочих нагрузках SQL в различных средах. В некоторых рабочих нагрузках измерения значительно снижены. В зависимости от результатов рекомендация заключается в том, что вы тестируете воздействие обновления BIOS на производительность, прежде чем развертывать их в рабочей среде. См. SQL Server руководстве [здесь](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown).

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
В этом разделе описаны новые возможности для ТД 2016-AU6.

### <a name="sql-server-2016"></a>SQL Server 2016

ТД AU6 работает на последнем выпуске SQL Server 2016 и использует уровень совместимости базы данных по умолчанию 130. SQL Server 2016 обеспечивает поддержку новых функций, таких как:

- Вторичные индексы для кластеризованных индексов columnstore.
- Kerberos для Polybase.

### <a name="t-sql"></a>T-SQL
ТД AU6 поддерживает эти улучшения совместимости T-SQL.  Эти дополнительные языковые элементы упрощают миграцию из SQL Server и других источников данных. 

- [Параметры сортировки SQL на уровне столбцов][] теперь поддерживаются в дополнение к параметрам сортировки Windows.
- [Некластеризованные индексы кластеризованных индексов columnstore][] улучшают производительность запросов, которые выполняют поиск конкретных значений в кластеризованном индексе columnstore. 
- [SELECT...INTO;][] 
- [sp_spaceused ()][] отображает дисковое пространство, используемое или зарезервированное в таблице или базе данных.
- Поддержка [широких таблиц][] аналогична SQL Server 2016. Предыдущее ограничение в 32 КБ для размера строки больше не существует. 

**Типы данных**

- [Varchar (max)][], [nvarchar (max)][] и [varbinary (max)][]. Эти типы данных LOB имеют максимальный размер, равный 2 ГБ. Чтобы загрузить эти объекты, используйте [программу bcp][]. Polybase и dwloader в настоящее время не поддерживают эти типы данных. 
- [ИМЕЕТ sysname][]
- [UNIQUEIDENTIFIER][]
- [Числовые][] и десятичные типы данных.

**Оконные функции**

- [Строки или диапазон][] в предложении OVER инструкции SELECT.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Функции безопасности**

- [CHECKSUM ()][] и [BINARY_CHECKSUM ()][]
- [HAS_PERMS_BY_NAME ()][]

**Дополнительные функции**

- [NEWID ()][]
- [RAND ()][]

### <a name="polybasehadoop-enhancements"></a>Расширения Polybase и Hadoop

- Совместимость с Hortonworks HDP 2,4 и HDP 2,5
- Поддержка Kerberos через учетные данные области базы данных
- Поддержка учетных данных с большими двоичными объектами службы хранилища Azure

### <a name="install-and-upgrade-enhancements"></a>Усовершенствования установки и обновления

**Обновления корпоративной архитектуры** Обновление имеющегося устройства до ТД AU6 устанавливает новейшие обновления встроенного по и драйверов, включая исправления безопасности. 

Новое устройство от HPE или DELL включает все последние обновления, а также:

- Поддержка новейших процессоров поколения (Broadwell)
- Обновление до DDR4 DIMM
- Улучшенная пропускная способность модуля DIMM

**Интеграция**

- Поддержка полного доменного имени (FQDN) позволяет настроить доверие домена для устройства. 
- Чтобы использовать полное доменное имя, необходимо выполнить полное обновление и принять участие в процессе обновления. 

**Сокращение времени простоя** Установка или обновление до ТД AU6 выполняется быстрее и требует меньше времени простоя по сравнению с предыдущими выпусками. Чтобы сократить время простоя, установите или обновите: 

 - Упрощает применение обновлений WSUS с помощью образа, содержащего все обновления до июня 2016
 - Применяет обновления для системы безопасности с обновлением драйвера и встроенного по
 - Помещает последние исправления и служебную программу проверки устройств (ПАВ) на устройство, чтобы они были готовы к установке без необходимости их загрузки.

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[Параметры сортировки SQL на уровне столбцов]: ~/relational-databases/collations/collation-and-unicode-support.md

[Некластеризованные индексы кластеризованных индексов columnstore]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[ИМЕЕТ sysname]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[SELECT...INTO;]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Широкие таблицы]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[Программа bcp]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[СТРОКИ или диапазон]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[CHECKSUM ()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID ()]:/sql/t-sql/functions/newid-transact-sql
[RAND ()]:/sql/t-sql/functions/rand-transact-sql


  

