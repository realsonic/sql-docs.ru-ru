---
description: Управление хранением данных журнала в темпоральных таблицах с системным управлением версиями
title: Управление данными журнала в темпоральных таблицах с системным управлением версиями
ms.custom: seo-lt-2019
ms.date: 05/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7925ebef-cdb1-4cfe-b660-a8604b9d2153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c0dac2b5d344969f96016a86622ee4e07ab8c23d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168188"
---
# <a name="manage-retention-of-historical-data-in-system-versioned-temporal-tables"></a>Управление хранением данных журнала в темпоральных таблицах с системным управлением версиями


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Вместе с темпоральными таблицами с системным управлением версиями таблица журнала может увеличить размер базы данных больше, чем это делают обычные таблицы, особенно в следующих условиях.

- Данные журнала сохраняются в течение длительного периода времени.
- Обновляется или удаляется шаблон изменения данных большого объема.

Большой и постоянно растущий объем таблицы журнала может стать проблемой не только из-за затрат на хранение. Кроме этого, он может повлиять на производительность при выполнении темпоральных запросов. Таким образом, разработка политики хранения данных для управления данными в таблице журнала является важным аспектом планирования и управления жизненным циклом всех темпоральных таблиц.

## <a name="data-retention-management-for-history-table"></a>Управление хранением данных для таблицы журнала

Управление хранением данных темпоральных таблиц начинается с определения необходимых сроков хранения для каждой таблицы. Политика хранения в большинстве случаев должна рассматриваться как часть бизнес-логики в приложении, использующем темпоральные таблицы. Например, к приложениям, связанным с аудитом данных и переходами по времени, предъявляются жесткие требования, касающиеся того, как долго данные журнала должны быть доступны для запросов по сети.

Когда вы определите срок хранения данных, следующим шагом будет разработка плана управления данными журнала, а именно определение способа и места хранения данных журнала, а также способа удаления данных после истечения их срока хранения. Существуют четыре подхода к управлению данными журнала в темпоральной таблице журнала:

- [База данных Stretch](#using-stretch-database-approach)
- [секционирование таблиц;](#using-table-partitioning-approach)
- [пользовательский сценарий очистки.](#using-custom-cleanup-script-approach)
- [политика хранения](#using-temporal-history-retention-policy-approach).

 С каждым из этих подходов логика миграция или очистки журнала данных основывается на столбце, который соответствует концу периода в текущей таблице. Значение конца периода для каждой строки определяет момент, когда версия строки становится "закрытой", т. е. когда она попадает в таблицу журнала. Например, условие `SysEndTime < DATEADD (DAYS, -30, SYSUTCDATETIME ())` указывает на то, что записи журнала старше чем один месяц должны быть удалены или перемещены из таблицы журнала.

> [!NOTE]
> В примерах этого раздела используется [пример темпоральной таблицы](creating-a-system-versioned-temporal-table.md).

## <a name="using-stretch-database-approach"></a>Использование базы данных Stretch

> [!NOTE]
> Использование Stretch Database применимо только к [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] и не может применяться к [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

[база данных Stretch;](../../sql-server/stretch-database/stretch-database.md) в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] прозрачно переносит данных журнала в Azure. Для обеспечения дополнительной защиты можно зашифровать передаваемые данные с помощью функции SQL Server [Постоянное шифрование](../security/encryption/always-encrypted-database-engine.md) . Кроме того, для защиты данных можно использовать [защиту на уровне строк](../../relational-databases/security/row-level-security.md) и другие дополнительные функции безопасности SQL Server с помощью средств Temporal и базы данных Stretch.

Используя базу данных Stretch, можно растянуть часть таблиц или все темпоральные таблицы журнала в Azure, и сервер SQL Server автоматически переместит данные журнала в Azure. Использование технологии Stretch для таблицы журнала не влияет на взаимодействие с темпоральной таблицей в части изменения данных и темпоральных запросов.

- **Растяжение всей таблицы журнала**. Настройте базу данных Stretch для всей таблицы журнала, если вашим главным сценарием является аудит данных с частым изменением данных и редкими запросами данных журнала. Другими словами, используйте этот подход, если производительность темпоральных запросов не критична. В этом случае экономическая эффективность, предоставляемая Azure, может оказаться привлекательной. При растяжении всей таблицы журнала можно использовать либо мастер растяжения, либо службу Transact-SQL. Оба примера приведены ниже.

- **Растяжение части таблицы журнала**. Настройте базу данных Stretch только для части таблицы журнала, чтобы повысить производительность, если ваш главный сценарий включает в себя в основном запросы последних данных журнала, но при этом необходимо сохранить возможность запрашивать более ранние данные журнала при удаленном хранении этих данных по невысокой цене. Используя службу Transact-SQL, это можно сделать, указав функцию предиката, чтобы не переносить все строки из таблицы журнала, а указать только часть из них. При работе с темпоральными таблицами обычно имеет смысл перемещать данные по условию времени (т. е. на основе возраста версий строк в таблице журнала).

  Используя детерминированные функции предиката, можно сохранить часть журнала в одной базе данных вместе с текущими данными, а остальные переместить в Azure. Примеры и ограничения см. в разделе [Выбор строк для миграции с использованием функции фильтров (база данных Stretch)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Поскольку недетерминированные функции являются недопустимыми, то, если требуется перемещать данные журнала по принципу скользящего окна, необходимо регулярно изменять определение встроенной функции предиката так, чтобы строки, сохраняемые локально, оставались неизменно одного возраста. Скользящее окно позволяет непрерывно перемещать данные журнала старше одного месяца в Azure. Пример такого подхода приведен ниже.

> [!NOTE]
> Stretch Database перемещает данные в Azure. Таким образом, необходимо иметь учетную запись Azure и подписку для выставления счетов. Чтобы получить бесплатную пробную учетную запись Azure, щелкните [Бесплатная пробная версия на один месяц](https://azure.microsoft.com/pricing/free-trial/).

Настроить темпоральную таблицу журнала для растяжения можно с помощью мастера растяжения или службы Transact-SQL, а включить растяжение для темпоральной таблицы журнала можно, если системное управление версиями включено (значение **ON**). Растяжение текущей таблицы не допускается, так как оно не имеет смысла.

### <a name="using-the-stretch-wizard-to-stretch-the-entire-history-table"></a>Использование мастера растяжения для растяжения всей таблицы журнала

Самым простым способом для начинающих является использование мастера растяжения для включения растяжения для всей базы данных. Затем нужно выбрать темпоральную таблицу журнала в мастере растяжения (в этом примере предполагается, что таблица Department определена в качестве темпоральной таблицы с системным управлением версиями в изначально пустой базе данных). В [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]нельзя щелкнуть правой кнопкой мыши саму темпоральную таблицу журнала и выбрать команду "Растяжение".

1. Щелкните базу данных правой кнопкой мыши и наведите указатель на пункт **Задачи**, затем на пункт **Растяжение** и выберите пункт **Включить** , чтобы запустить мастер.
2. В окне **Выбор таблиц** установите флажок рядом с темпоральной таблицей журнала и нажмите кнопку "Далее".

    ![Выбор таблицы журнала на странице "Выбор таблиц"](../../relational-databases/tables/media/stretch-wizard-2-for-temporal.png "Выбор таблицы журнала на странице "Выбор таблиц"")
3. В окне **Настройка Azure** укажите учетные данные для входа. Выполните вход в Microsoft Azure или зарегистрируйте учетную запись. Выберите подписку, которую будете использовать, и регион Azure. Затем создайте новый сервер или выберите существующий. Щелкните **Далее**.

    ![Создание нового сервера Azure — мастер Stretch Database](../../relational-databases/tables/media/stretch-wizard-4.png "Создание нового сервера Azure — мастер Stretch Database")
4. В окне **Учетные данные безопасности** укажите пароль для главного ключа базы данных для обеспечения безопасности учетных данных исходной базы данных SQL Server и нажмите кнопку "Далее".

    ![Страница "Учетные данные безопасности" мастера Stretch Database](../../relational-databases/tables/media/stretch-wizard-6.png "Страница "Учетные данные безопасности" мастера Stretch Database")
5. В окне **Выбор IP-адреса** укажите диапазон IP-адресов для вашего сервера SQL Server, необходимый для взаимодействия сервера Azure с сервером SQL Server (если выбран существующий сервер, для которого уже существует правило брандмауэра, просто нажмите кнопку "Далее", чтобы использовать это правило). Нажмите кнопку **Далее**, а затем — кнопку **Готово**, чтобы включить Stretch Database и выполнить растяжение темпоральной таблицы журнала.

    ![Страница "Выбор IP-адреса" мастера Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Страница "Выбор IP-адреса" мастера Stretch Database")
6. По завершении работы мастера убедитесь в том, что растяжение для вашей базы данных успешно включено. Проверьте значки в обозревателе объектов. Они должны указывать, что для базы данных выполнено растяжение.

> [!NOTE]
> Если не удается разрешить растяжение для базы данных, просмотрите журнал ошибок. Распространенной ошибкой является неверная настройка правила брандмауэра.

См. также

- [Включение базы данных Stretch для базы данных](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)
- [Запуск мастера включения растяжения для базы данных](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)
- [Настройка базы данных Stretch для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)

### <a name="using-transact-sql-to-stretch-the-entire-history-table"></a>Использование Transact-SQL для растяжения всей таблицы журнала

Вы также можете использовать Transact-SQL, чтобы включить растяжение на локальном сервере и [включить растяжение для базы данных](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md). После этого можно [использовать Transact-SQL, чтобы разрешить Stretch Database в таблице](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). Используя базу данных, для которой уже была разрешена операция растяжения, выполните следующий сценарий Transact-SQL, позволяющий выполнить растяжение существующей таблицы журнала с системным управлением версиями:

```sql
ALTER TABLE <history table name>
SET (REMOTE_DATA_ARCHIVE = ON (MIGRATION_STATE = OUTBOUND));
```

### <a name="using-transact-sql-to-stretch-a-portion-of-the-history-table"></a>Использование Transact-SQL для растяжения части таблицы журнала

Чтобы выполнить растяжение только части таблицы журнала, начните с создания [встроенной функции предиката](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Для примера предположим, что встроенная функция предиката была настроена в первый раз 1 декабря 2015 г. и требуется выполнить растяжение в Azure всех записей с датой журнала старше 1 ноября 2015 г. Чтобы сделать это, начнем с создания следующей функции:

```sql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2015-11-01T00:00:00', 101) ;
```

Далее используем следующий сценарий для добавления предиката фильтра в таблицу журнала и зададим для состояния перемещения значение OUTBOUND, чтобы включить перемещение данных для таблицы журнала на основе предиката.

```sql
ALTER TABLE <history table name>
SET (
      REMOTE_DATA_ARCHIVE = ON
        (
          FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151101 (SysEndTime)
            , MIGRATION_STATE = OUTBOUND
        )
    )
;
```

Для обеспечения скользящего окна необходимо задать функцию предиката точно на каждый день (т. е. изменять условие фильтрации строк каждый день, прибавляя один день). Приведенный далее скрипт должен выполняться 2 декабря 2015 г.:

```sql
BEGIN TRAN
/*(1) Create new predicate function definition */
  CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151102(@systemEndTime datetime2)
   RETURNS TABLE
    WITH SCHEMABINDING
      AS
        RETURN SELECT 1 AS is_eligible
          WHERE @systemEndTime < CONVERT(datetime2,'2015-11-02T00:00:00', 101)
  GO
 
/*(2) Set the new function as filter predicate */
  ALTER TABLE <history table name>
    SET
      (
        REMOTE_DATA_ARCHIVE = ON
          (
            FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151102(SysEndTime),
              MIGRATION_STATE = OUTBOUND
          )
      )
COMMIT ;
```

Чтобы гарантировать правильность функции в любой день, используйте агент SQL Server или другой механизм планирования.

## <a name="using-table-partitioning-approach"></a>Использование секционирования таблиц

[Секционирование таблиц](../partitions/create-partitioned-tables-and-indexes.md) может улучшить управляемость и масштабируемость больших таблиц. Используя секционирование таблиц журнала, можно реализовать пользовательскую очистку данных или архивирование в автономном режиме по условию времени. Кроме того, секционирование таблиц обеспечивает выигрыш в производительности при выполнении запросов по темпоральным таблицам на подмножестве данных журнала путем устранения секций.

Секционирование таблиц позволяет реализовать принцип скользящего окна для перемещения более старой части данных журнала из таблицы журнала и поддержания размера сохраняемой части постоянным по критерию возраста. В таблице журнала сохраняются только те данные, срок которых равен заданному сроку хранения. Операция по перемещению данных из таблицы журнала поддерживается, если для параметра SYSTEM_VERSIONING задано значение ON. Это означает, что можно очистить часть данных журнала без введения периода обслуживания или блокировки регулярных рабочих нагрузок.

> [!NOTE]
> Чтобы можно было выполнить переключение секций, кластеризованный индекс в таблице журнала должен быть согласован со схемой секционирования. (Она должна содержать SysEndTime.) Таблица журнала по умолчанию, создаваемая системой, содержит кластеризованный индекс, который включает в себя столбцы SysEndTime и SysStartTime, что оптимально для секционирования, вставки новых данных журнала и выполнения типичных темпоральных запросов. Дополнительные сведения см. в разделе [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Использование скользящего окна предполагает выполнение двух наборов задач.

- Задачи настройки секционирования
- Повторяющиеся задачи обслуживания секций

Для иллюстрации предположим, что нам нужно хранить данные журнала в течение 6 месяцев и что мы хотим сохранять каждый месяц данных в отдельной секции. Кроме того, предположим, что мы активировали системное управление версиями в сентябре 2015 г.

При настройке секционирования создается начальная конфигурация секционирования для таблицы журнала. В этом примере мы создадим секции в количестве, равном размеру скользящего окна в месяцах, и еще одну дополнительную пустую предварительно подготовленную секцию (о ней будет сказано ниже). Эта конфигурация гарантирует, что система будет правильно сохранять новые данные при первом запуске повторяющейся задачи обслуживания секции, а также что никогда не произойдет разбиение секций с данными, а значит, не потребуется перемещение данных, на которое может потребоваться много времени. Эту задачу следует выполнить с помощью службы Transact-SQL, используя пример сценария, приведенный ниже.

На следующем рисунке показана начальная конфигурация секционирования для сохранения данных за 6 месяцев.

![Схема начальной конфигурации секционирования для сохранения данных за шесть месяцев.](../../relational-databases/tables/media/partitioning.png "Секционирование")

> [!NOTE]
> При настройке секционирования изучите вопросы, связанные с производительностью, чтобы понять, как влияет на производительность использование параметров RANGE LEFT и RANGE RIGHT.

Первая и последняя секции "открыты" на верхней и нижней границах соответственно, чтобы обеспечить секцию назначения для каждой новой строки независимо от значения в столбце секционирования. Со временем новые строки в таблице журнала будут перемещаться в более высокие секции. Когда будет заполнена шестая секция, будет достигнут заданный срок хранения. Это тот момент, когда в первый раз должна быть запущена повторяющаяся задача обслуживания секций. (В этом примере должен быть запланирован периодический запуск задачи один раз в месяц.)

На следующем рисунке представлены повторяющиеся задачи обслуживания секций (см. инструкции ниже).

![Схема: повторяющиеся задачи обслуживания секций.](../../relational-databases/tables/media/partitioning2.png "Секционирование2")

Подробные инструкции для повторяющихся задач обслуживания секций.

1. ОТКЛЮЧЕНИЕ. Создайте промежуточную таблицу, а затем переключите секцию между таблицей журнала и промежуточной таблицей с помощью инструкции [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md) с параметром SWITCH PARTITION (см. "Пример В. Переключение секций между таблицами").

    ```sql
    ALTER TABLE <history table> SWITCH PARTITION 1 TO <staging table>
    ```

    После переключения секций можно, при необходимости, архивировать данные из промежуточной таблицы, а затем удалить или сделать усечение промежуточной таблицы, чтобы она была готова к следующему выполнению этой повторяющейся задачи обслуживания секций.
2. СЛИЯНИЕ ДИАПАЗОНА. Выполните слияние пустой секции 1 с секцией 2, используя инструкцию [ALTER PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md) с параметром MERGE RANGE (см. пример Б). При удалении нижней границы с использованием этой функции происходит, по сути, слияние пустой секции 1 с бывшей секцией 2 и образование новой секции 1. Порядковые номера других секций также изменяются.
3. РАЗБИЕНИЕ ДИАПАЗОНА. Создайте пустую секцию 7, используя инструкцию [ALTER PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md) с параметром SPLIT RANGE (см. пример A). При добавлении верхней границы с использованием этой функции происходит по сути создание отдельной секции для предстоящего месяца.

### <a name="use-transact-sql-to-create-partitions-on-history-table"></a>Использование службы Transact-SQL для создания секций в таблице журнала

Используйте сценарий Transact-SQL, приведенный в окне кода ниже, для создания функции секционирования, схемы секционирования и повторного создания кластеризованного индекса для согласования секций со схемой секционирования. В этом примере будет создано скользящее окно размером шесть месяцев с ежемесячными секциями, начиная с сентября 2015 г.

```sql
BEGIN TRANSACTION
/*Create partition function*/
    CREATE PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime] (datetime2(7))
        AS RANGE LEFT FOR VALUES
          (
            N'2015-09-30T23:59:59.999'
          , N'2015-10-31T23:59:59.999'
          , N'2015-11-30T23:59:59.999'
          , N'2015-12-31T23:59:59.999'
          , N'2016-01-31T23:59:59.999'
          , N'2016-02-29T23:59:59.999'
          )
/*Create partition scheme*/
    CREATE PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime]
        AS PARTITION [fn_Partition_DepartmentHistory_By_SysEndTime]
            TO ([PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY])
/*Re-create index to be partition-aligned with the partitioning schema*/
    CREATE CLUSTERED INDEX [ix_DepartmentHistory] ON [dbo].[DepartmentHistory]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )  
    WITH
        (
            PAD_INDEX = OFF
          , STATISTICS_NORECOMPUTE = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = ON
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
          , DATA_COMPRESSION = PAGE
        )
    ON [sch_Partition_DepartmentHistory_By_SysEndTime] ([SysEndTime])

COMMIT TRANSACTION;
```

### <a name="using-transact-sql-to-maintain-partitions-in-sliding-window-scenario"></a>Использование службы Transact-SQL для сохранения секций в сценарии со скользящим окном

Используйте сценарий Transact-SQL, приведенный в окне кода ниже, для поддержания секций для случая скользящего окна. В этом примере мы отключим секцию для сентября 2015 г., используя инструкцию MERGE RANGE, а затем добавим новую секцию для марта 2016 г., используя инструкцию SPLIT RANGE.

```sql
BEGIN TRANSACTION
/*(1) Create staging table */
    CREATE TABLE [dbo].[staging_DepartmentHistory_September_2015]
        (
            [DeptID] [int] NOT NULL
          , [DeptName] [varchar](50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
          , [ManagerID] [int] NULL
          , [ParentDeptID] [int] NULL
          , [SysStartTime] [datetime2](7) NOT NULL
          , [SysEndTime] [datetime2](7) NOT NULL
        ) ON [PRIMARY]
    WITH
        (
            DATA_COMPRESSION = PAGE
        )
/*(2) Create index on the same filegroups as the partition that will be switched out*/
    CREATE CLUSTERED INDEX [ix_staging_DepartmentHistory_September_2015]
        ON [dbo].[staging_DepartmentHistory_September_2015]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )
    WITH
        (
            PAD_INDEX = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = OFF
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
        )
    ON [PRIMARY]
/*(3) Create constraints matching the partition that will be switched out*/
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015] WITH CHECK
        ADD CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
            CHECK ([SysEndTime]<=N'2015-09-30T23:59:59.999')
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015]
        CHECK CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
/*(4) Switch partition to staging table*/
    ALTER TABLE [dbo].[DepartmentHistory]
        SWITCH PARTITION 1 TO [dbo].[staging_DepartmentHistory_September_2015]
        WITH (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 MINUTES, ABORT_AFTER_WAIT = NONE))
/*(5) [Commented out] Optionally archive the data and drop staging table
      INSERT INTO [ArchiveDB].[dbo].[DepartmentHistory]
      SELECT * FROM [dbo].[staging_DepartmentHistory_September_2015];
      DROP TABLE [dbo].[staging_DepartmentHIstory_September_2015];
*/
/*(6) merge range to move lower boundary one month ahead*/
    ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]()
        MERGE RANGE(N'2015-09-30T23:59:59.999')
/*(7) Create new empty partition for "April and after" by creating new boundary point and specifying NEXT USED file group*/
    ALTER PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime] NEXT USED [PRIMARY]
        ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]() SPLIT RANGE(N'2016-03-31T23:59:59.999')
COMMIT TRANSACTION
```

Можно немного изменить сценарий, приведенный выше, и использовать его в регулярной ежемесячной процедуре обслуживания:

1. На шаге (1) создадим новую промежуточную таблицу для месяца, который нужно удалить (в нашем примере следующим будет октябрь).
2. В шаге (3) создадим и проверим ограничение, соответствующее месяцу данных, который нужно удалить: `[SysEndTime]<=N'2015-10-31T23:59:59.999'` для секции октября.
3. В шаге (4) выполним операцию SWITCH для секции 1 и переключим ее на вновь созданную промежуточную таблицу.
4. В шаге (6) изменим функцию секционирования путем слияния нижней границы: `MERGE RANGE(N'2015-10-31T23:59:59.999'` после перемещения данных за октябрь.
5. В шаге (7) разобьем секцию, создав новую верхнюю границу: `SPLIT RANGE (N'2016-04-30T23:59:59.999'` после перемещения данных за октябрь.

Тем не менее оптимальным решением будет регулярно запускать типовой сценарий Transact-SQL, который может выполнять необходимые действия каждый месяц без изменения сценария. Можно обобщить описанный сценарий, введя обработку переданных параметров (нижней границы, которую требуется объединить, и новой границы, создаваемой путем разбиения секции). Чтобы не создавать промежуточную таблицу для каждого месяца, можно создать одну таблицу заранее и повторно использовать ее, изменяя проверочное ограничение в соответствии с той секцией, которая будет выключена. Просмотрите следующие страницы, на которых вы можете найти подсказки о том, [как можно полностью автоматизировать скользящее окно](/previous-versions/sql/sql-server-2005/administrator/aa964122(v=sql.90)) с помощью скрипта Transact-SQL.

### <a name="performance-considerations-with-table-partitioning"></a>Вопросы, связанные с производительностью при секционировании таблиц

Очень важно выполнять операции MERGE и SPLIT RANGE так, чтобы избегать перемещения данных, так как перемещение данных может привести к значительному снижению производительности. Дополнительные сведения см. в разделе [Изменение функции секционирования](../../relational-databases/partitions/modify-a-partition-function.md). Это достигается скорее с помощью параметра RANGE LEFT, чем параметра RANGE RIGHT, когда вы используете инструкцию [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md).

Давайте сначала визуально продемонстрируем смысл параметров RANGE LEFT и RANGE RIGHT:

![Схема параметров RANGE LEFT и RANGE RIGHT.](../../relational-databases/tables/media/partitioning3.png "Секционирование3")

При определении функции секционирования как RANGE LEFT указанные значения являются верхними границами секций. При использовании параметра RANGE RIGHT указанные значения являются нижними границами секций. При использовании операции MERGE RANGE для удаления границы из определения функции секционирования базовая реализация удаляет также секцию, которая содержит эту границу. Если эта секция не пуста, данные будут перемещены в секцию, которая является результатом операции MERGE RANGE.

В сценарии со скользящим окном мы всегда удаляем самую нижнюю границу секции.

- Случай RANGE LEFT. В случае параметра RANGE LEFT самая нижняя граница секции принадлежит секции 1, которая становится пустой (после отключения секции), поэтому операция MERGE RANGE не приводит к перемещению данных.
- Случай RANGE RIGHT. В случае параметра RANGE RIGHT самая нижняя граница секции принадлежит секции 2, которая не является пустой, так как предполагается, что при отключении была очищена секция 1. В этом случае операция MERGE RANGE приведет к перемещению данных. (Данные из секции 2 будут перемещены в секцию 1.) Чтобы избежать этого, при применении параметра RANGE RIGHT в сценарии со скользящим окном необходимо, чтобы секция 1 была всегда пуста. Это означает, что, если используется параметр RANGE RIGHT, необходимо создавать и поддерживать одну дополнительную секцию, чего не нужно делать в случае параметра RANGE LEFT.

**Заключение**. Использование параметра RANGE LEFT при скользящем секционировании значительно упрощает управление секциями и позволяет избежать перемещения данных. Однако определение границ секционирования с использованием параметра RANGE RIGHT несколько проще, так как при этом не требуется решать вопросы, связанные с течением времени.

## <a name="using-custom-cleanup-script-approach"></a>Использование пользовательского сценария очистки

В тех случаях, когда подход Stretch Database или секционирование таблиц неприемлемо, можно применить третий способ удаления данных из таблицы журнала с помощью пользовательского сценария очистки. Удаление данных из таблицы журнала возможно, только если **SYSTEM_VERSIONING = OFF**. Чтобы избежать несогласованности данных, необходимо выполнять очистку в течение периода обслуживания (когда рабочие нагрузки, изменяющие данные, неактивны) или в рамках одной транзакции (на время которой блокируются другие рабочие нагрузки). Для выполнения этой операции требуется разрешение **CONTROL** для текущей таблицы и таблицы журнала.

При выполнении сценария очистки внутри транзакции удаляйте данные небольшими фрагментами с задержкой, чтобы не создавать помеху для работы обычных приложений и запросов пользователей. Не существует единой рекомендации по оптимальному размеру фрагмента удаляемых данных для всех сценариев, но в любом случае удаление более чем 10 000 строк в одной транзакции может оказать существенное влияние на работу.

Для всех темпоральных таблиц используется одна и та же логика очистки. Поэтому можно сравнительно легко автоматизировать весь процесс с помощью универсальной хранимой процедуры, запускаемой периодически для каждой темпоральной таблицы, для которой требуется ограничить данные журнала.

На следующей схеме показано, как должна быть организована логика очистки для одиночной таблицы, чтобы снизить влияние на выполнение рабочих нагрузок.

![Схема, показывающая, как должна быть организована логика очистки для одиночной таблицы, чтобы снизить влияние на выполнение рабочих нагрузок.](../../relational-databases/tables/media/customcleanupscriptdiagram.png "CustomCleanUpScriptDiagram")

Ниже приведены общие рекомендации по реализации процесса. Составьте план ежедневного выполнения очистки с перебором всех темпоральных таблиц, для которых требуется очистка данных. Для планирования этого процесса используйте Агент SQL Server или другое средство:

- Удаляйте данные журнала из каждой темпоральной таблицы, начиная с самой старой строки и до последней, в несколько итераций небольшими фрагментами. Не удаляйте все строки в одной транзакции. Процесс показан на рисунке, приведенном выше.
- Реализуйте каждую итерацию как вызов универсальной хранимой процедуры, удаляющей часть данных из таблицы журнала. (См. пример кода для выполнения этой процедуры ниже.)
- Подсчитайте, сколько строк необходимо удалять из одной темпоральной таблицы при каждом запуске процесса. Зная это число и требуемое количество итераций, динамически определите точки разбиения для каждого вызова процедуры.
- Задайте периоды задержки между итерациями по одной таблице, чтобы уменьшить помеху для работы приложений, обращающихся к этой темпоральной таблице.

Пример хранимой процедуры, удаляющей данные из одной темпоральной таблицы, приведен в следующем фрагменте кода. (Внимательно просмотрите этот код и внесите необходимые поправки прежде, чем применять его в вашей среде.)

```sql
DROP PROCEDURE IF EXISTS sp_CleanupHistoryData;
GO

CREATE PROCEDURE sp_CleanupHistoryData
        @temporalTableSchema sysname
      , @temporalTableName sysname
      , @cleanupOlderThanDate datetime2
AS
    DECLARE @disableVersioningScript nvarchar(max) = '';
    DECLARE @deleteHistoryDataScript nvarchar(max) = '';
    DECLARE @enableVersioningScript nvarchar(max) = '';

DECLARE @historyTableName sysname
DECLARE @historyTableSchema sysname
DECLARE @periodColumnName sysname

/*Generate script to discover history table name and end of period column for given temporal table name*/
EXECUTE sp_executesql
    N'SELECT @hst_tbl_nm = t2.name, @hst_sch_nm = s.name, @period_col_nm = c.name
        FROM sys.tables t1
            JOIN sys.tables t2 on t1.history_table_id = t2.object_id
        JOIN sys.schemas s on t2.schema_id = s.schema_id
            JOIN sys.periods p on p.object_id = t1.object_id
           JOIN sys.columns c on p.end_column_id = c.column_id and c.object_id = t1.object_id
                  WHERE
                 t1.name = @tblName and s.name = @schName'
                , N'@tblName sysname
                , @schName sysname
                , @hst_tbl_nm sysname OUTPUT
                , @hst_sch_nm sysname OUTPUT
                , @period_col_nm sysname OUTPUT'
                , @tblName = @temporalTableName
                , @schName = @temporalTableSchema
                , @hst_tbl_nm = @historyTableName OUTPUT
                , @hst_sch_nm = @historyTableSchema OUTPUT
                , @period_col_nm = @periodColumnName OUTPUT
  
IF @historyTableName IS NULL OR @historyTableSchema IS NULL OR @periodColumnName IS NULL
    THROW 50010, 'History table cannot be found. Either specified table is not system-versioned temporal or you have provided incorrect argument values.', 1

/*Generate 3 statements that will run inside a transaction:
  (1) SET SYSTEM_VERSIONING = OFF,
  (2) DELETE FROM history_table,
  (3) SET SYSTEM_VERSIONING = ON
  On SQL Server 2016, it is critical that (1) and (2) run in separate EXEC statements, or SQL Server will generate the following error:
  Msg 13560, Level 16, State 1, Line XXX
  Cannot delete rows from a temporal history table '<database_name>.<history_table_schema_name>.<history_table_name>'.
*/
SET @disableVersioningScript = @disableVersioningScript + 'ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + '] SET (SYSTEM_VERSIONING = OFF)'
SET @deleteHistoryDataScript = @deleteHistoryDataScript + ' DELETE FROM [' + @historyTableSchema + '].[' + @historyTableName + ']
    WHERE ['+ @periodColumnName + '] < ' + '''' + convert(varchar(128), @cleanupOlderThanDate, 126) + ''''
SET @enableVersioningScript = @enableVersioningScript + ' ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + ']
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = [' + @historyTableSchema + '].[' + @historyTableName + '], DATA_CONSISTENCY_CHECK = OFF )); '

BEGIN TRAN
    EXEC (@disableVersioningScript);
    EXEC (@deleteHistoryDataScript);
    EXEC (@enableVersioningScript);
COMMIT;
```

## <a name="using-temporal-history-retention-policy-approach"></a>Применение подхода с использованием политики хранения темпоральных журналов

> [!NOTE]
> Подход с использованием политики хранения темпоральных журналов может применяться к [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] с SQL Server 2017 начиная с CTP 1.3.

Хранение темпоральных журналов можно настроить на уровне отдельных таблиц, что позволит пользователям создавать гибкие политики устаревания. Применять темпоральные журналы легко: для настройки во время создания таблицы или изменения схемы требуется только один параметр.

Как только политика хранения определена, база данных Azure SQL начинает регулярно проверять, подходят ли исторические строки для автоматической очистки данных. Идентификация совпадающих строк и их удаление из таблицы журналов осуществляется прозрачно, в рамках фоновой задачи, которая планируются и выполняются системой. Условие устаревания строк в таблицах журналов проверяется по столбцу, представляющему окончание периода SYSTEM_TIME. Например, если срок хранения составляет шесть месяцев, для очистки подходят строки, отвечающие следующему условию:

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```
В предыдущем примере предполагалось, что столбец ValidTo соответствует окончанию периода SYSTEM_TIME.

### <a name="how-to-configure-retention-policy"></a>Настройка политики хранения

Прежде чем настраивать политику хранения для темпоральной таблицы, проверьте, включено ли временное хранение журналов на уровне базы данных:

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Флаг базы данных **is_temporal_history_retention_enabled** по умолчанию установлен, однако пользователи могут его изменить с помощью инструкции ALTER DATABASE. Кроме того, флаг автоматически снимается после операции восстановления на определенный момент времени. Чтобы включить очистку хранения журналов для базы данных, выполните следующую инструкцию:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION ON
```

Политика хранения настраивается во время создания таблицы путем указания значения для параметра HISTORY_RETENTION_PERIOD:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH
 (
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
    )
 );
```

Период хранения можно указать в различных единицах времени: днях (DAYS), неделях (WEEKS), месяцах (MONTHS) и годах (YEARS). Если параметр HISTORY_RETENTION_PERIOD опущен, это означает, что срок хранения НЕ ОГРАНИЧЕН (INFINITE). Кроме того, ключевое слово INFINITE можно использовать явным образом.
В некоторых сценариях настройка хранения может потребоваться после создания таблицы либо для того, чтобы изменить значение, заданное ранее. В этом случае используйте инструкцию ALTER TABLE:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

Чтобы узнать текущее состояние политики хранения, выполните указанный ниже запрос, объединяющий флаг включения временного хранения на уровне базы данных со сроками хранения для отдельных таблиц:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```

### <a name="how-sql-database-deletes-aged-rows"></a>Как База данных SQL удаляет устаревшие строки?

Процесс очистки зависит от макета индекса таблицы журналов. Следует отметить, что *ограниченный срок хранения можно настроить только для таблиц журналов с кластеризованным индексом (columnstore или сбалансированным деревом)* . Для очистки устаревших данных во всех темпоральных таблицах с ограниченным периодом хранения создается фоновая задача. Логика очистки для кластеризованного индекса rowstore (сбалансированное дерево) удаляет устаревшие строки небольшими фрагментами (до 10 тыс. строк), что позволяет минимизировать нагрузку на журнал базы данных и подсистему ввода-вывода. Несмотря на то что в логике очистки используется индекс сбалансированного дерева, порядок удаления строк, возраст которых превышает срок хранения, может не соблюдаться. В связи с этим *не включайте в приложения никакие зависимости от порядка очистки*.

Задача очистки для кластеризованного индекса columnstore удаляет всю группу строк за раз (обычно каждая такая группа содержит по миллиону строк) — это очень эффективно, особенно в случаях, когда исторические данные создаются быстрыми темпами.

![Кластеризованный период удержания columnstore](../../relational-databases/tables/media/cciretention.png "Кластеризованный период удержания columnstore")

Превосходное сжатие данных и эффективная очистка хранилища делают кластеризованный индекс columnstore наиболее подходящим для случаев, когда рабочая нагрузка вызывает быстрое образование большого количества исторических данных. Подобная ситуация типична для интенсивных рабочих нагрузок по обработке транзакций, в которых темпоральные таблицы используются для контроля и аудита изменений, анализа тенденций и приема данных Интернета вещей.

Дополнительные сведения см. в статье [Управление историческими данными в темпоральных таблицах с политикой хранения](/azure/sql-database/sql-database-temporal-tables-retention-policy).

## <a name="next-steps"></a>Дальнейшие действия

- [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md)
- [Приступая к работе c темпоральными таблицами с системным управлением версиями](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Проверка согласованности системной темпоральной таблицы](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Секционирование с помощью темпоральных таблиц](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Рекомендации и ограничения для темпоральной таблицы](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Безопасность темпоральной таблицы](../../relational-databases/tables/temporal-table-security.md)
- [Темпоральные таблицы с системным управлением версиями и таблицы, оптимизированные для памяти](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Представления и функции метаданных для временной таблицы](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)