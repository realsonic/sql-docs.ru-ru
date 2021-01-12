---
description: Инструкции SET (Transact-SQL)
title: Инструкции SET (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET
- SET_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ISO SET statements
- queries [SQL Server], executing
- dates [SQL Server], SET statements
- time [SQL Server], SET statements
- SET statement, about SET statement
- SET statement
- statistical information [SQL Server], SET statements
- locking [SQL Server], SET statements
ms.assetid: f7e107f8-0fcf-408b-b30f-da2323eeb714
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azure-sqldw-latest ||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: e5c2bf8f1bfe5815a8eb696b5b837d70fbb9b21b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079344"
---
# <a name="set-statements-transact-sql"></a>Инструкции SET (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

Язык [!INCLUDE[tsql](../../includes/tsql-md.md)] предоставляет несколько инструкций SET, которые изменяют текущий сеанс, управляя специфическими данными. Инструкции SET группируются в категории, показанные в следующей таблице.  
  
Сведения об установке локальных переменных с помощью инструкции SET см. в разделе [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
|Категория|Операторы|  
|--------------|----------------|  
|Инструкции даты и времени|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)<br /><br /> [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|  
|Инструкции блокировки|[SET DEADLOCK_PRIORITY](../../t-sql/statements/set-deadlock-priority-transact-sql.md)<br /><br /> [SET LOCK_TIMEOUT](../../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|Прочие инструкции|[SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)<br /><br /> [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)<br /><br /> [SET FIPS_FLAGGER](../../t-sql/statements/set-fips-flagger-transact-sql.md)<br /><br /> [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)<br /><br /> [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)<br /><br /> [SET OFFSETS](../../t-sql/statements/set-offsets-transact-sql.md)<br /><br /> [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)|  
|Инструкции выполнения запросов|[SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)<br /><br /> [SET ARITHIGNORE](../../t-sql/statements/set-arithignore-transact-sql.md)<br /><br /> [SET FMTONLY](../../t-sql/statements/set-fmtonly-transact-sql.md)<br /> Примечание. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br /><br /> [SET NOCOUNT](../../t-sql/statements/set-nocount-transact-sql.md)<br /><br /> [SET NOEXEC](../../t-sql/statements/set-noexec-transact-sql.md)<br /><br /> [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)<br /><br /> [SET PARSEONLY](../../t-sql/statements/set-parseonly-transact-sql.md)<br /><br /> [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)<br /><br /> [SET RESULT SET CACHING](../../t-sql/statements/set-result-set-caching-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) (предварительная версия)<br /> Примечание.  Эта возможность действует только для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].<br /><br /> [SET ROWCOUNT](../../t-sql/statements/set-rowcount-transact-sql.md)<br /><br /> [SET TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md)|  
|Инструкции настроек ISO|[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_OFF](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)<br /><br /> [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)<br /><br /> [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)<br /><br /> [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)|  
|Статистические инструкции|[SET FORCEPLAN](../../t-sql/statements/set-forceplan-transact-sql.md)<br /><br /> [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md)<br /><br /> [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md)<br /><br /> [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)<br /><br /> [SET STATISTICS IO](../../t-sql/statements/set-statistics-io-transact-sql.md)<br /><br /> [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)<br /><br /> [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)<br /><br /> [SET STATISTICS TIME](../../t-sql/statements/set-statistics-time-transact-sql.md)|  
|Инструкции управления транзакциями|[SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)<br /><br /> [SET REMOTE_PROC_TRANSACTIONS](../../t-sql/statements/set-remote-proc-transactions-transact-sql.md)<br /><br /> [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)<br /><br /> [SET XACT_ABORT](../../t-sql/statements/set-xact-abort-transact-sql.md)| 
  
## <a name="considerations-when-you-use-the-set-statements"></a>Рекомендации по использованию инструкций SET  
  
- Все инструкции SET выполняются во время выполнения или запуска, кроме следующих инструкций, которые выполняются во время анализа:

  - SET FIPS_FLAGGER
  - SET OFFSETS
  - SET PARSEONLY
  - SET QUOTED_IDENTIFIER  
  
- Если инструкция SET выполняется в хранимой процедуре или триггере, значение параметра SET восстанавливается после того, как хранимая процедура или триггер вернет управление. Также, если инструкция SET указана в динамической строке SQL, которая выполняется с помощью процедуры **sp_executesql** или инструкции EXECUTE, значение параметра SET восстанавливается после того, как управление вернется из пакета, указанного в динамической строке SQL.  
  
- Хранимые процедуры выполняются с настройками SET, указанными во время выполнения, кроме инструкций SET ANSI_NULLS и SET QUOTED_IDENTIFIER. Хранимые процедуры, использующие инструкцию SET ANSI_NULLS или SET QUOTED_IDENTIFIER, используют настройку, указанную в хранимой процедуре во время создания. При использовании внутри хранимой процедуры любые установки SET игнорируются.  
  
- Параметр **user options** процедуры **sp_configure** допускает настройку в пределах сервера и работает с множеством баз данных. Эта настройка ведет себя так же, как и явная инструкция SET, за исключением того, что возникает во время входа в систему.  
  
- Настройки базы данных, устанавливаемые с помощью инструкции ALTER DATABASE, действительны только на уровне базы данных и применяются только при явном задании. Параметры базы данных переопределяют параметры экземпляра, которые устанавливаются с помощью процедуры **sp_configure**.  
  
- Если в инструкции SET используется значение ON и OFF, можно указать любое из них для нескольких параметров SET.
  
    > [!NOTE]  
    >  Это не относится к статистическим параметрам SET.
  
     Например, инструкция `SET QUOTED_IDENTIFIER, ANSI_NULLS ON` устанавливает параметры QUOTED_IDENTIFIER и ANSI_NULLS в значение ON.  
  
- Настройки инструкции SET переопределяют идентичные настройки параметров базы данных, которые были установлены с помощью инструкции ALTER DATABASE. Например, значение, указанное в инструкции SET ANSI_NULLS, перекроет настройку базы данных для параметра ANSI_NULL. Кроме того, для некоторых настроек подключений автоматически устанавливается значение ON, если пользователь подключается к базе данных, основываясь на значениях, заданных предыдущим использованием настроек процедуры **sp_configure user options**, или на значениях, которые применимы ко всем подключениям ODBC и OLE/DB.  
  
- Настройка SET LOCK_TIMEOUT не влияет на выполнение инструкций ALTER, CREATE и DROP DATABASE.  
  
- Если инструкция SET глобальной или быстрой настройки устанавливает несколько настроек, то следующая инструкция SET быстрой настройки переопределяет предыдущие настройки для всех параметров, которых она касается. Если параметр SET, которого касается инструкция SET быстрой настройки, устанавливается после запуска такой инструкции, то индивидуальная инструкция SET переопределяет соответствующие быстрые настройки. Пример инструкции SET быстрой настройки: SET ANSI_DEFAULTS.  
  
- При использовании пакетов контекст базы данных определяется пакетом, установленным с помощью инструкции USE. Незапланированные запросы и все другие инструкции, которые выполняются за пределами хранимых процедур и которые содержатся в пакетах, наследуют настройки параметров базы данных и подключения, установленные с помощью инструкции USE.  
  
- Запросы режима MARS совместно используют глобальное состояние, которое содержит большую часть настроек параметров SET последней сессии. Параметры SET могут измениться при выполнении любого запроса. Эти изменения специфичны для контекста запроса, в котором они устанавливаются, и не влияют на другие параллельные запросы режима MARS. Однако после завершения выполнения запроса новые параметры SET копируются в глобальное состояние сеанса. Новые запросы, которые выполняются в том же самом сеансе, после этого изменения будут использовать новые значения параметров SET.  
  
- При выполнении хранимой процедуры из пакета либо из другой хранимой процедуры она выполняется под значениями параметров, которые установлены в базе данных, содержащей хранимую процедуру. Например, если хранимая процедура **db1.dbo.sp1** вызывает хранимую процедуру **db2.dbo.sp2**, то хранимая процедура **sp1** выполняется под текущим значением уровня совместимости базы данных **db1**, а хранимая процедура **sp2** — под текущим значением уровня совместимости базы данных **db2**.  
  
- Когда инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] ссылается на объекты, которые размещены в многих базах данных, к ней применяется текущий контекст базы данных и текущий контекст подключения. В этом случае, если инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)] находится в пакете, текущим контекстом соединения является база данных, определенная инструкцией USE. Если инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)] находится в хранимой процедуре, то контекстом соединения является база данных, которая содержит хранимую процедуру.  
  
- Если вы создаете индексы в вычисляемых столбцах и индексированных представлениях и управляете ими, необходимо установить для следующих параметров SET значение ON: ARITHABORT, CONCAT_NULL_YIELDS_NULL, QUOTED_IDENTIFIER, ANSI_NULLS, ANSI_PADDING и ANSI_WARNINGS. Установите для параметра NUMERIC_ROUNDABORT значение OFF.  
  
  Если обязательное значение любого из этих параметров не задано, инструкции INSERT, UPDATE, DELETE, DBCC CHECKDB и DBCC CHECKTABLE для индексированных представлений или таблиц с индексами на основе вычисляемых столбцов не смогут быть выполнены. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сформирует ошибку с указанием всех неправильно заданных параметров. Также [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет выполнять инструкции SELECT в этих таблицах или индексных представлениях, как будто индексы в вычисляемых столбцах или представлениях не существуют. 

- Если для SET RESULT_SET_CACHING указать значение ON, будет включена функция кэширования результатов для текущего сеанса клиента.   Для RESULT_SET_CACHING нельзя указать значение ON для сеанса, если на уровне базы данных указано значение OFF.    Если для SET RESULT_SET_CACHING указать значение OFF, будет отключена функция кэширования результатов для текущего сеанса клиента. Для изменения этого параметра требуется членство в роли public.
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 2-го поколения
