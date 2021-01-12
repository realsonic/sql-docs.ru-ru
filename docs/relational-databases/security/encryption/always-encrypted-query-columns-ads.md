---
description: Выполнение запросов к столбцам, использующим Always Encrypted, с помощью Azure Data Studio
title: Выполнение запросов к столбцам, использующим Always Encrypted, с помощью Azure Data Studio | Документация Майкрософт
ms.custom: ''
ms.date: 5/19/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0a61514ff76bbae9e403683529d017f6fcb1c079
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101886"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>Выполнение запросов к столбцам, использующим Always Encrypted, с помощью Azure Data Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

В этой статье описывается выполнение запросов к столбцам, зашифрованным при помощи [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md), с использованием [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md). При использовании Azure Data Studio возможно:
- извлечение значений зашифрованных данных, хранящихся в зашифрованных столбцах; 
- извлечение значений открытого текста, хранящихся в зашифрованных столбцах;  
- отправка значений открытого текста, предназначенных для зашифрованных столбцов (например, в инструкциях `INSERT` или `UPDATE` и в инструкциях `SELECT` в виде параметра поиска в предложениях `WHERE`). 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Извлечение зашифрованных данных, хранящихся в зашифрованных столбцах    
В этом разделе описывается извлечение данных, хранящихся в зашифрованных столбцах, в виде зашифрованного текста.

### <a name="steps"></a>Шаги
1. Отключите функцию Always Encrypted для подключения к базе данных, относящегося к окну запроса, где будет выполнен запрос `SELECT` для извлечения значений зашифрованного текста. См. раздел [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](#enabling-and-disabling-always-encrypted-for-a-database-connection) ниже.     
1. Выполните запрос `SELECT`. Любые данные, полученные из зашифрованных столбцов, возвращаются в виде двоичных (зашифрованных) значений.   

### <a name="example"></a>Пример
Предположим, что `SSN` — это зашифрованный столбец в таблице `Patients` . В этом случае если функция Always Encrypted, применяемая для подключения к базе данных, отключена, то выполнив приведенный ниже запрос, вы получите двоичные значения зашифрованных данных.   

![Снимок экрана: запрос SELECT * FROM [dbo].[Patients] и его результаты, представленные в виде двоичных зашифрованных значений.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Извлечение значений открытого текста, хранящихся в зашифрованных столбцах    
В этом разделе описывается извлечение данных, хранящихся в зашифрованных столбцах, в виде зашифрованного текста.

### <a name="prerequisites"></a>Предварительные требования
- Azure Data Studio версии 17.1 или более поздней.
- Необходим доступ к главным ключам столбцов и метаданным о ключах, защищающих столбцы, к которым выполняется запрос. Дополнительные сведения см. в разделе [Разрешения для выполнения запросов к зашифрованным столбцам](#permissions-for-querying-encrypted-columns) ниже.
- Главные ключи столбцов должны храниться в хранилище сертификатов Windows или в Azure Key Vault. Azure Data Studio не поддерживает другие хранилища ключей.

### <a name="steps"></a>Шаги
1.  Включите функцию Always Encrypted, применяемую для подключения к базе данных, в окне редактора запросов, где будет выполняться запрос `SELECT` для извлечения и расшифровки данных. Это укажет [поставщику данных .NET Framework для SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (используемому средой Azure Data Studio) расшифровать зашифрованные столбцы в наборе результатов запроса. См. раздел [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](#enabling-and-disabling-always-encrypted-for-a-database-connection) ниже.
1.  Выполните запрос `SELECT`. Все данные, полученные из зашифрованных столбцов, возвращаются в виде значений открытого текста исходных типов данных.
 
### <a name="example"></a>Пример
Предположим, что SSN — это зашифрованный столбец в таблице `Patients`. В этом случае, если функция Always Encrypted, применяемая для подключения к базе данных, включена и при этом у вас есть доступ к главному ключу, настроенному для столбца `SSN`, приведенный ниже запрос вернет значения открытого текста.   

![Снимок экрана: запрос SELECT * FROM [dbo].[Patients] и его результаты, представленные в виде обычных текстовых значений.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Отправка значений открытого текста, предназначенных для зашифрованных столбцов       
В этом разделе описывается выполнение запроса, который отправляет значения, предназначенные для зашифрованного столбца. Например, запрос, который вставляет, обновляет или фильтрует по значению, хранящемуся в зашифрованном столбце.

### <a name="prerequisites"></a>Предварительные требования
- Azure Data Studio версии 18.1 или более поздней.
- Необходим доступ к главным ключам столбцов и метаданным о ключах, защищающих столбцы, к которым выполняется запрос. Дополнительные сведения см. в разделе [Разрешения для выполнения запросов к зашифрованным столбцам](#permissions-for-querying-encrypted-columns) ниже.
- Главные ключи столбцов должны храниться в хранилище сертификатов Windows или в Azure Key Vault. Azure Data Studio не поддерживает другие хранилища ключей.

### <a name="steps"></a>Шаги
1. Включите функцию Always Encrypted, применяемую для подключения к базе данных, в окне редактора запросов, где будет выполняться запрос `SELECT` для извлечения и расшифровки данных. Это даст указание [поставщику данных для SQL Server Microsoft .NET](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (используется в Azure Data Studio) шифровать параметры запроса, предназначенные для зашифрованных столбцов, и расшифровывать результаты, полученные из зашифрованных столбцов. См. раздел [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](#enabling-and-disabling-always-encrypted-for-a-database-connection) ниже. 
1. Включите определение параметров для Always Encrypted в окне запроса. Дополнительные сведения см. в разделе [Параметризация для Always Encrypted](#parameterization-for-always-encrypted) ниже.
1. Объявите переменную Transact-SQL и инициализируйте ее, используя значение, которое нужно отправить (вставить, обновить или использовать для фильтрации) в базу данных. 
1. Выполните запрос, чтобы отправить значение переменной Transact-SQL в базу данных. Azure Data Studio преобразует эту переменную в параметр запроса, значение которого шифруется перед отправкой в базу данных.   

### <a name="example"></a>Пример
Если `SSN` является зашифрованным столбцом `char(11)` в таблице `Patients`, приведенный ниже сценарий попытается найти строку, содержащую `'795-73-9838'` в столбце SSN. Результаты возвращаются, если Always Encrypted включен для подключения к базе данных, в окне запроса включена параметризация для Always Encrypted и имеется доступ к главному ключу столбца, настроенному для столбца `SSN`.   

![Снимок экрана: запрос DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN и его результаты.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Разрешения для выполнения запросов к зашифрованным столбцам

Для выполнения запросов к зашифрованным столбцам требуются разрешения **VIEW ANY COLUMN MASTER KEY DEFINITION** и **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** в базе данных.

Помимо указанных выше разрешений, чтобы расшифровать любые результаты и зашифровать параметры запроса (созданные в процессе параметризации переменных Transact-SQL), также требуется доступ к главному ключу столбца, который используется для защиты целевых столбцов.

- **Хранилище сертификатов — локальный компьютер.** Требуется доступ на **чтение** к сертификату, который используется в качестве главного ключа столбца, либо необходимо быть администратором компьютера.   
- **Azure Key Vault.** Требуются разрешения **get**, **unwrapKey** и **verify** к хранилищу ключей, содержащему главный ключ столбца.

Дополнительные сведения см. в разделе [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Создание и хранение главных ключей столбцов (постоянное шифрование)).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных   
При подключении к базе данных в Azure Data Studio можно включить или отключить функцию Always Encrypted, применяемую для подключения к базе данных. По умолчанию функция Always Encrypted отключена. 

После включения функции Always Encrypted, применяемой для подключения к базе данных, [поставщик данных Microsoft .NET для SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), используемый в Azure Data Studio, получает установку на прозрачное выполнение следующих действий:   
-   расшифровка любых значений, полученных из зашифрованных столбцов и возращенных в результатах запроса;   
-   шифрование значений параметризованных переменных Transact-SQL, предназначенных для зашифрованных столбцов базы данных.   

Если функция Always Encrypted, применяемая для подключения к базе данных, не включена, поставщик данных Microsoft .NET для SQL Server не будет пытаться зашифровать параметры запроса или расшифровать результаты.

Вы можете включить или отключить Always Encrypted при подключении к базе данных. Общие сведения о подключении к базе данных см. в следующих статьях.
- [Краткое руководство. Подключение и отправка запроса к SQL Server с помощью Azure Data Studio](../../../azure-data-studio/quickstart-sql-server.md)
- [Краткое руководство. Использование Azure Data Studio для подключения и обращения к базе данных SQL Azure](../../../azure-data-studio/quickstart-sql-database.md)

Чтобы включить или отключить Always Encrypted, выполните следующие действия.
1. В диалоговом окне **Подключения** нажмите кнопку **Дополнительно...** .
2. Чтобы включить Always Encrypted для подключения, задайте для поля **Always Encrypted** значение **Включено**. Чтобы отключить Always Encrypted, оставьте значение поля **Always Encrypted** пустым или задайте для него значение **Отключено**.
3. Если вы используете [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] и для экземпляра SQL Server настроен безопасный анклав, можно указать протокол анклава и URL-адрес аттестации анклава. Если ваш экземпляр SQL Server не использует безопасный анклав, поля **Протокол аттестации** и **URL-адрес аттестации анклава** следует оставить пустыми. Дополнительные сведения см. в статье [Always Encrypted с безопасными анклавами](always-encrypted-enclaves.md).
4. Нажмите кнопку **OK**, чтобы закрыть **Расширенные свойства**.

![Короткое видео с инструкциями по включению Always Encrypted для подключения.](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

> [!TIP]
> Чтобы переключиться между включением и отключением Always Encrypted для существующего окна запроса, щелкните **Отключить**, а затем щелкните **Подключиться** и выполните описанные выше действия, чтобы повторно подключиться к базе данных с нужными значениями поля **Always Encrypted**. 

> [!NOTE] 
> Кнопка **Изменить подключение** в окне запроса в настоящее время не поддерживает переключение между включением и отключением Always Encrypted.

## <a name="parameterization-for-always-encrypted"></a>Параметризация для Always Encrypted

Параметризация для Always Encrypted — это функция в Azure Data Studio 18.1 и последующих версиях, которая автоматически преобразует переменные Transact-SQL в параметры запросов (экземпляры [класса SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter)). Это позволяет основному [поставщику данных Microsoft .NET для SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) определять данные, предназначенные для зашифрованных столбцов, и шифровать эти данные перед отправкой в базу данных.
  
Без параметризации поставщик данных Microsoft .NET для SQL Server передает все инструкции, созданные в редакторе запросов, в виде непараметризованных запросов. Если запрос содержит литералы или переменные Transact-SQL, предназначенные для зашифрованных столбцов, поставщик данных .NET Framework для SQL Server не сможет определить и зашифровать их перед отправкой запроса в базу данных. В результате этого из-за несоответствия типов (между переменной Transact-SQ литерала открытого текста и зашифрованным столбцом) запрос завершится сбоем. Например, если столбец `SSN` зашифрован, без параметризации следующий запрос завершится сбоем.   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Включение и отключение параметризации для Always Encrypted

По умолчанию параметризация для Always Encrypted отключена.

Чтобы включить/отключить параметризацию для Always Encrypted:

1. Выберите **Файл** > **Настройки** > **Параметры** (**Код** > **Настройки** > **Параметры** на компьютере Mac).
2. Перейдите в раздел **Данные** > **Microsoft SQL Server**.
3. Установите или снимите флажок **Включить определение параметров для Always Encrypted**.
4. Закройте окно **Параметры**.

![Короткое видео с инструкциями по включению и отключению параметризации для Always Encrypted.](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> Параметризацию для Always Encrypted можно включить только в запросе, использующем подключение к базе данных с включенной функцией Always Encrypted (см. раздел [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](#enabling-and-disabling-always-encrypted-for-a-database-connection)). Если эта функция отключена в окне запроса, параметризация переменных Transact-SQL не выполняется.

### <a name="how-parameterization-for-always-encrypted-works"></a>Принципы работы параметризации для Always Encrypted

Если в окне запроса включена и функция Always Encrypted, применяемая для подключения к базе данных, и параметризация для Always Encrypted, Azure Data Studio произведет попытку параметризовать переменные Transact-SQL, соответствующие следующим обязательным требованиям.

- Эти переменные объявлены и инициализированы в одной инструкции (встроенная инициализация). Параметризация переменных, объявленных с использованием разных инструкций `SET`, не выполняется.
- Эти переменные инициализированы с использованием одного литерала. Параметризация переменных, инициализированных с использованием выражений, в том числе любых операторов и функций, не выполняется.

Ниже приведены примеры переменных, которые параметризуются в Azure Data Studio.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Вот некоторые примеры переменных, параметризация которых не выполняется в Azure Data Studio.

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Параметризация происходит успешно в следующих случаях:   
- тип литерала, используемый для инициализации переменной, параметризация которой выполняется, должен совпадать с типом в объявлении переменной;   
- если в качестве объявленного типа переменной используется тип даты или тип времени, переменную нужно инициализировать, используя строку в одном из совместимых с ISO 8601 форматов.   

Ниже приведены примеры объявлений переменных Transact-SQL, которые приводят к ошибкам параметризации переменных.

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio использует технологию Intellisense, чтобы предоставлять сведения о том, для каких переменных можно выполнить параметризацию, а для каких эта операция завершится сбоем (с указанием причины).   

Объявление переменной, для которой можно выполнить параметризацию, подчеркивается линией (как в информационном сообщении) в окне запроса. Если навести указатель мыши на подчеркнутую инструкцию объявления, появится сообщение с результатом параметризации, в том числе значениями основных свойств итогового объекта [SqlParameter Class](/dotnet/api/microsoft.data.sqlclient.sqlparameter) (переменная сопоставляется с [SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype), [Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale) и [SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue)). Кроме того, в представлении **Проблемы** можно просмотреть полный список всех параметризованных переменных. Чтобы открыть представление **Проблемы**, выберите **Представление** > **Проблемы**.    



Если попытка параметризовать переменную в Azure Data Studio завершилась сбоем, объявление переменной будет помечено знаком ошибки. Если навести указатель мыши на помеченную инструкцию выражения, отобразятся результаты ошибки. Полный список ошибок параметризации для всех переменных можно просмотреть в представлении **Проблемы**.

 
> [!NOTE]
> Функция Always Encrypted поддерживает ограниченное подмножество преобразований типов, поэтому во многих случаях необходимо, чтобы тип данных в переменной Transact-SQL совпадал с типом целевого столбца базы данных, для которого она применяется. Например, предположим, что тип столбца `SSN` в таблице `Patients` — это `char(11)`. В этом случае приведенный ниже запрос завершится сбоем, так как тип переменной `@SSN` (`nchar(11)`) не совпадает с типом столбца.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> Если параметризация отключена, весь запрос, в том числе преобразования типов, обрабатывается в SQL Server или Базе данных SQL Azure. При включенной параметризации некоторые преобразования типов выполняются поставщиком данных Microsoft .NET для SQL Server внутри Azure Data Studio. Из-за различия между типами систем Microsoft .NET и SQL Server (например, разная точность некоторых типов, таких как float) результаты, полученные после выполнения запроса с включенной и отключенной функцией параметризации, могут отличаться. 

## <a name="next-steps"></a>Next Steps
- [Разработка приложений с помощью Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>См. также:
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)