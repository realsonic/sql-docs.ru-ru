---
title: Прозрачное шифрование данных (TDE) | Документация Майкрософт
description: Сведения о прозрачном шифровании данных (TDE), которое шифрует данные SQL Server, Базы данных SQL Azure и Azure Synapse Analytics. Это называется шифрованием неактивных данных.
ms.custom: ''
ms.date: 05/09/2019
ms.prod: sql
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06288dc0bc6e16ab8928322005254f5fac1ddc61
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171486"
---
# <a name="transparent-data-encryption-tde"></a>Прозрачное шифрование данных (TDE)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

*Прозрачное шифрование данных* (TDE) позволяет шифровать файлы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] и [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)]. Это так называемое шифрование неактивных данных.

Чтобы защитить базу данных, можно принять следующие меры предосторожности.

* Разработка безопасной системы.
* Шифрование конфиденциальных ресурсов.
* Создание брандмауэра вокруг серверов баз данных.

Но если злоумышленник украдет физический носитель, например диски или ленты резервного копирования, то он сможет восстановить или подключить базу данных и просмотреть ее данные.

Одним из решений может стать шифрование конфиденциальных данных в базе данных и использование сертификата для защиты ключей шифрования данных. Таким образом, пользователи без ключей не смогут использовать эти данные. Но этот тип защиты необходимо запланировать заранее.

Функция прозрачного шифрования данных выполняет шифрование и дешифрование ввода-вывода в реальном времени для файлов данных и журналов. Шифрование использует ключ шифрования базы данных (DEK). Загрузочная запись базы данных хранит ключ для доступности во время восстановления. DEK является симметричным ключом. Он защищен сертификатом, который хранится в базе данных master сервера, или асимметричным ключом, который защищает модуль расширенного управления ключами.

Функция прозрачного шифрования данных защищает неактивные данные, то есть файлы данных и журналов. Благодаря ей обеспечивается соответствие требованиям различных законов, постановлений и рекомендаций, действующих в разных отраслях. Это позволяет разработчикам программного обеспечения шифровать данные с помощью алгоритмов шифрования AES и 3DES, не меняя существующие приложения.

> [!IMPORTANT]
> Функция прозрачного шифрования данных не обеспечивает шифрование каналов связи. Дополнительные сведения о способах шифрования данных, передаваемых по каналам связи, см. в разделе [Включение шифрования соединений в ядре СУБД (диспетчер конфигурации SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).
>
>**См. также:**
>
> - [Прозрачное шифрование данных в Базе данных SQL Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)
> - [Начало работы с прозрачным шифрованием данных (TDE) в Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)
> - [Перемещение базы данных, защищаемой прозрачным шифрованием, в другой экземпляр SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)
> - [Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)
> - [Использование соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [Блог по безопасности SQL Server в TDE с вопросами и ответами](/archive/blogs/sqlsecurity/feature-spotlight-transparent-data-encryption-tde)

## <a name="about-tde"></a>Сведения о прозрачном шифровании данных (TDE)

Шифрование файлов базы данных выполняется на уровне страницы. Страницы в зашифрованной базе данных шифруются до записи на диск и дешифруются при чтении в память. Прозрачное шифрование данных не увеличивает размер зашифрованной базы данных.

### <a name="information-applicable-to-sssds"></a>Сведения, применимые к [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]

При использовании прозрачного шифрования данных с [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] версии 12 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] автоматически создает сертификат на уровне сервера, хранящийся в базе данных master. Чтобы переместить базу данных TDE в [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], расшифровывать ее не нужно. Дополнительные сведения об использовании прозрачного шифрования данных с [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] см. в статье [Прозрачное шифрование данных в базе данных SQL Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

### <a name="information-applicable-to-ssnoversion"></a>Сведения, применимые к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

После защиты базы данных ее можно восстановить с помощью правильного сертификата. Дополнительные сведения о сертификатах см. в разделе [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

После включения прозрачного шифрования данных немедленно создайте резервную копию сертификата и связанного с ним закрытого ключа. Если сертификат становится недоступным или вы восстанавливаете или подключаете базу данных на другом сервере, вам потребуются резервные копии сертификата и закрытого ключа. В противном случае вы не сможете открыть базу данных.

Храните сертификат шифрования, даже если вы отключили прозрачное шифрование данных в базе данных. Несмотря на то что база данных не зашифрована, части журнала транзакций могут оставаться защищенными. Вам также может потребоваться сертификат для некоторых операций, пока не будет выполнено полное резервное копирование базы данных.

Сертификат, который превысил свой срок действия, все еще может быть использован для шифрования и расшифровки данных с помощью прозрачного шифрования данных.

### <a name="encryption-hierarchy"></a>Иерархия средств шифрования

На рисунке ниже показана архитектура прозрачного шифрования данных. При использовании прозрачного шифрования данных в [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] пользователь может настраивать только элементы уровня базы данных (ключ шифрования базы данных и фрагменты ALTER DATABASE).

![Архитектура прозрачного шифрования данных](../../../relational-databases/security/encryption/media/tde-architecture.png)

## <a name="enable-tde"></a>Включить TDE

Чтобы использовать прозрачное шифрование данных, выполните следующие действия.

**Применимо к**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

1. Создайте главный ключ.

1. Создайте или получите сертификат, защищенный главным ключом.

1. Создайте ключ шифрования базы данных и защитите его с помощью сертификата.

1. Задайте шифрование базы данных.

В следующем примере демонстрируется шифрование и дешифрование базы данных `AdventureWorks2012` с помощью сертификата с именем `MyServerCert`, установленного на сервере.

```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';
go
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';
go
USE AdventureWorks2012;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;
GO
ALTER DATABASE AdventureWorks2012
SET ENCRYPTION ON;
GO
```

Операции шифрования и дешифрования запланированы в фоновых потоках [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Состояние этих операций можно просмотреть в представлениях каталога и динамических административных представлениях в таблице далее в этой статье.

> [!CAUTION]
> Файлы резервной копии для баз данных с TDE-шифрованием также шифруются с помощью ключа шифрования базы данных. Поэтому для восстановления таких резервных копий необходимо иметь сертификат, защищающий ключ шифрования базы данных. Следовательно, в дополнение к резервному копированию базы данных обязательно сохраняйте резервные копии сертификатов сервера. Если сертификат станет недоступным, это приведет к потере данных.
>
> Дополнительные сведения см. в статье [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="commands-and-functions"></a>Команды и функции

Чтобы использовать следующие инструкции для приема сертификатов TDE, используйте главный ключ базы данных для шифрования. Если они зашифрованы только паролем, то они будут отклонены инструкциями как шифраторы.

> [!IMPORTANT]
> Если защищать сертификаты паролем после того, как TDE использует их, база данных станет недоступной после перезагрузки.

В следующей таблице представлены ссылки и объяснения команд и функций TDE.

|Команда или функция|Назначение|
|-------------------------|-------------|
|[CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|Создает ключ, который шифрует базу данных.| 
|[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|Изменяет ключ, который шифрует базу данных.|
|[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|Удаляет ключ, который шифрует базу данных.|
|[Параметры ALTER DATABASE SET (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|Объясняет параметр **ALTER DATABASE**, который используется для включения TDE.|

## <a name="catalog-views-and-dynamic-management-views"></a>Представления каталога и динамические административные представления

 В следующей таблице показаны представления каталогов и динамические административные представления TDE.

|Представление каталога или динамическое административное представление|Назначение|
|---------------------------------------------|-------------|
|[sys.databases (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Представление каталога со сведениями о базе данных|
|[sys.certificates (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|Представление каталога с сертификатами из базы данных|
|[sys.dm_database_encryption_keys (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|Динамическое административное представление со сведениями о ключах шифрования базы данных и состоянии шифрования базы данных|

## <a name="permissions"></a>Разрешения

Для каждой функции или команды TDE действуют отдельные требования к разрешениям, описанные в таблицах выше.

Для просмотра метаданных, связанных с TDE, необходимо разрешение VIEW DEFINITION на сертификат.

## <a name="considerations"></a>Рекомендации

Во время проверки базы данных на повторное шифрование, выполняемой для операции шифрования, операции обслуживания базы данных отключаются. Для выполнения операции обслуживания базы данных можно использовать однопользовательский режим. Дополнительные сведения см. в разделе [Установка однопользовательского режима базы данных](../../../relational-databases/databases/set-a-database-to-single-user-mode.md).

Состояние шифрования базы данных можно определить с помощью динамического административного представления sys.dm_database_encryption_keys. Дополнительные сведения см. выше в разделе "Представления каталога и динамические административные представления" этой статьи.

В режиме TDE все файлы и файловые группы зашифрованы. Если какие-либо файловые группы в базе данных помечены признаком READ ONLY, то операция шифрования базы данных завершается сбоем.

Если база данных используется в зеркальном отображении базы данных или в доставке журналов, то зашифрованы будут обе базы данных. Транзакции журналов при передаче между ними будут передаваться в зашифрованном виде.

> [!IMPORTANT]
> Полнотекстовые индексы будут шифроваться после включения шифрования базы данных. Такие индексы, созданные в SQL Server версии, предшествующей SQL Server 2008, импортируются в базу данных с помощью SQL Server 2008 или более поздней версии и шифруются с помощью TDE.

> [!TIP]
> Чтобы отслеживать изменения в состоянии прозрачного шифрования для базы данных, используйте аудит базы данных SQL или подсистему аудита SQL Server. Для SQL Server прозрачное шифрование данных отслеживается в группе действий аудита DATABASE_CHANGE_GROUP, которую можно найти в списке [Действия и группы действий подсистемы аудита SQL Server](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).

## <a name="restrictions"></a>Ограничения

При первом шифровании базы данных, изменении ключа или дешифровании базы данных не допускаются следующие операции:

- удаление файла из файловой группы в базе данных;

- удаление базы данных;

- перевод базы данных в автономный режим;

- отсоединение базы данных;

- перевод базы данных или файловой группы в состояние READ ONLY.

При выполнении инструкций CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY или ALTER DATABASE...SET ENCRYPTION не допускаются следующие операции:

- удаление файла из файловой группы в базе данных;

- удаление базы данных;

- перевод базы данных в автономный режим;

- отсоединение базы данных;

- перевод базы данных или файловой группы в состояние READ ONLY.

- использование команды ALTER DATABASE;

- запуск резервного копирования базы данных или файла базы данных;

- запуск восстановления базы данных или файла базы данных;

- создание моментального снимка.

Следующие операции или условия запрещают выполнение инструкций CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY и ALTER DATABASE...SET ENCRYPTION:

- база данных предназначена только для чтения или в ней есть файловые группы, доступные только для чтения;

- выполняется команда ALTER DATABASE;

- выполняется операция резервного копирования;

- база данных находится в состоянии восстановления или в автономном режиме;

- идет создание моментального снимка;

- выполняется задача обслуживания базы данных.

При создании файлов базы данных быстрая инициализация файлов недоступна при включенном TDE.

Чтобы зашифровать ключ шифрования базы данных с помощью асимметричного ключа, используемый асимметричный ключ должен находиться в поставщике расширенного управления ключами.

## <a name="tde-scan"></a>Проверка TDE

Чтобы включить TDE в базе данных, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] должен выполнить проверку шифрования. Проверка считывает каждую страницу из файлов данных в буферный пул, а затем записывает зашифрованные страницы обратно на диск.

Чтобы обеспечить более полный контроль над проверкой шифрования, [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] вводит сканирование TDE, которое имеет синтаксис приостановки и возобновления. Вы можете приостановить сканирование, когда рабочая нагрузка в системе будет очень высокой или в течение критически важного для бизнеса периода, а затем возобновить сканирование позже.

Чтобы приостановить сканирование шифрования TDE, используйте следующий синтаксис:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION SUSPEND;
```

Подобным образом вы можете возобновить сканирование шифрования TDE с помощью следующего синтаксиса:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION RESUME;
```

Столбец encryption_scan_state добавлен в динамическое административное представление sys.dm_database_encryption_keys. Он показывает текущее состояние сканирования шифрования. Кроме того, появился столбец encryption_scan_modify_date, который содержит дату и время последнего изменения состояния сканирования шифрования.

Если экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] перезапускается, когда сканирование шифрования приостановлено, в журнал ошибок при запуске заносится сообщение. Сообщение указывает, что существующее сканирование приостановлено.

## <a name="tde-and-transaction-logs"></a>TDE и журналы транзакций

Использование TDE для базы данных удаляет оставшуюся часть текущего журнала виртуальных транзакций. Удаление приводит к принудительному созданию следующего журнала транзакций. Это поведение гарантирует, что после включения шифрования базы данных в журналах не останется простого текста.

Состояние шифрования файла журнала можно определить, просмотрев столбец `encryption_state` в представлении `sys.dm_database_encryption_keys`, как показано в примере:

```sql
USE AdventureWorks2012;
GO
/* The value 3 represents an encrypted state
   on the database and transaction logs. */
SELECT *
FROM sys.dm_database_encryption_keys
WHERE encryption_state = 3;
GO
```

Дополнительные сведения об архитектуре файлов журнала [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] см. в разделе [Журнал транзакций (SQL Server)](../../../relational-databases/logs/the-transaction-log-sql-server.md).

До изменения ключа шифрования базы данных все данные, записанные в журнале транзакций, будут зашифрованы с помощью предыдущего ключа шифрования базы данных.

Если необходимо дважды изменить ключ шифрования базы данных, необходимо выполнить резервное копирование журнала, прежде чем можно будет снова изменить ключ шифрования базы данных.

## <a name="tde-and-the-tempdb-system-database"></a>TDE и системная база данных tempdb

Системная база данных **tempdb** будет шифроваться, если с помощью TDE шифруется любая другая база данных в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Это может влиять на производительность незашифрованных баз данных в том же экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Дополнительные сведения о системной базе данных **tempdb** см. в разделе [База данных tempdb](../../../relational-databases/databases/tempdb-database.md).

## <a name="tde-and-replication"></a>TDE и репликация

Репликация данных не выполняется автоматически в зашифрованном виде из базы данных с включенным TDE. Необходимо отдельно включить TDE, если нужно защитить базы данных распространителя и подписчика.

Репликация моментальных снимков может хранить данные в незашифрованных промежуточных файлах, таких как BCP-файлы. Это возможно и для начального распределения данных для транзакций и репликации слиянием. Во время такой репликации можно включить шифрование для защиты канала связи.

Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine (диспетчер конфигурации SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="tde-and-always-on"></a>TDE и Always On    
Вы можете [добавить зашифрованную базу данных в группу доступности Always On](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md).  

Чтобы зашифровать базы данных, которые входят в группу доступности, создайте главный ключ и сертификаты или асимметричный ключ (EKM) во всех вторичных репликах перед созданием [ключа шифрования базы данных](../../../t-sql/statements/create-database-encryption-key-transact-sql.md) на первичной реплике.  

Если сертификат используется для защиты ключа шифрования базы данных (DEK), [сделайте резервную копию сертификата](../../../t-sql/statements/backup-certificate-transact-sql.md), созданного на первичной реплике, а затем [создайте сертификат из файла](../../../t-sql/statements/create-certificate-transact-sql.md) во всех вторичных репликах перед созданием ключа шифрования базы данных на первичной реплике. 

## <a name="tde-and-filestream-data"></a>TDE и данные FILESTREAM

Данные **FILESTREAM** не шифруются, даже если включено прозрачное шифрование данных.

<a name="scan-suspend-resume"></a>

## <a name="remove-tde"></a>Удаление TDE

Удалите шифрование из базы данных с помощью инструкции `ALTER DATABASE`.

```sql
ALTER DATABASE <db_name> SET ENCRYPTION OFF;
```

Состояние базы данных можно просмотреть с помощью динамического административного представления [sys.dm_database_encryption_keys](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

Дождитесь завершения расшифровки, прежде чем удалять ключ шифрования базы данных, с помощью [DROP DATABASE ENCRYPTION KEY](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md).

> [!IMPORTANT]
> Создайте резервную копию главного ключа и сертификата, используемых для TDE, в надежном расположении. Главный ключ и сертификат необходимы для восстановления резервных копий, которые были созданы, когда база данных была зашифрована с помощью TDE. После удаления ключа шифрования базы данных создайте резервную копию журнала, а затем новую полную резервную копию расшифрованной базы данных. 

## <a name="tde-and-buffer-pool-extension"></a>TDE и расширение буферного пула

Если вы шифруете базу данных с помощью TDE, файлы, касающиеся расширения буферного пула (BPE), не шифруются. Для этих файлов используйте средства шифрования на уровне файловой системы, такие как BitLocker или файловая система EFS.

## <a name="tde-and-in-memory-oltp"></a>TDE и выполняющаяся в памяти OLTP

Прозрачное шифрование данных можно включить в базе данных, которая содержит объекты OLTP в памяти. В [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] и [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] записи журнала выполняющейся в памяти OLTP шифруются, если включено TDE. В [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] записи журнала выполняющейся в памяти OLTP шифруются, если включено TDE, однако файлы в файловой группе MEMORY_OPTIMIZED_DATA не шифруются.

## <a name="related-tasks"></a>Связанные задачи

[Перемещение базы данных, защищаемой прозрачным шифрованием, в другой экземпляр SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
[Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
[Расширенное управление ключами с помощью Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  

## <a name="related-content"></a>См. также

[Прозрачное шифрование данных в Базе данных SQL Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
[Начало работы с прозрачным шифрованием данных (TDE) в Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)  
[Шифрование SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)  
[Ключи шифрования базы данных и SQL Server (компонент Database Engine)](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

## <a name="see-also"></a>См. также раздел

[Центр безопасности для ядра СУБД SQL Server и Базы данных Azure SQL](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[FILESTREAM (SQL Server)](../../../relational-databases/blob/filestream-sql-server.md)