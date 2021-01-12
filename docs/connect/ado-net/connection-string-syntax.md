---
title: Синтаксис строки подключения
description: Сведения о синтаксисе строк подключения в поставщике данных Microsoft SqlClient для SQL Server. Синтаксис для каждого поставщика описывается в соответствующем свойстве ConnectionString.
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 42b1f8704be721c0b52b3c42946c9abbf7efd7db
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771637"
---
# <a name="connection-string-syntax"></a>Синтаксис строки подключения

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

В <xref:Microsoft.Data.SqlClient> есть объект `Connection`, который наследуется от <xref:System.Data.Common.DbConnection>, а также свойство <xref:System.Data.Common.DbConnection.ConnectionString%2A> для конкретного поставщика. Синтаксис строки подключения для конкретного поставщика SqlClient описывается в соответствующем свойстве `ConnectionString`. Дополнительные сведения о синтаксисе строки подключения см. в разделе <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="connection-string-builders"></a>Построители строк подключения

 Поставщик данных Microsoft SqlClient для SQL Server предоставляет описанный ниже построитель строк подключения.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

Построители строк соединения позволяют создавать во время выполнения синтаксически правильные строки соединения, и поэтому в коде не требуется вручную объединять значения строк соединения. Дополнительные сведения см. в статье [Connection String Builders](connection-string-builders.md) (Построители строк подключения).

## <a name="windows-authentication"></a>Проверка подлинности Windows

Для соединения с источниками данных рекомендуется использовать проверку подлинности Windows (которую также называют *встроенной безопасностью*), если эти источники ее поддерживают. В следующей таблице демонстрируется синтаксис проверки подлинности Windows, который используется в поставщике данных Microsoft SqlClient для SQL Server.

|Поставщик|Синтаксис|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>Строки подключений SqlClient

Синтаксис для строки подключения <xref:Microsoft.Data.SqlClient.SqlConnection> документирован в свойстве <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>. Свойство <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> используется для возврата или задания строки подключения для базы данных SQL Server. Кроме того, ключевые слова строки подключения сопоставляются со свойствами в <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

> [!IMPORTANT]
> Для ключевого слова `Persist Security Info` используется значение по умолчанию `false`. Значение `true` или `yes` позволяет получить из строки соединения конфиденциальные данные (в том числе идентификатор пользователя и пароль) после открытия соединения. Задайте для `Persist Security Info` значение `false`, чтобы убедиться, что ненадежный источник не сможет получить доступ к конфиденциальным данным строки подключения.

### <a name="windows-authentication-with-sqlclient"></a>Проверка подлинности Windows при работе с SqlClient.

Все следующие формы синтаксиса используют для подключения к базе данных **AdventureWorks**, размещенной на локальном сервере, проверку подлинности Windows.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>Проверка подлинности SQL Server с использованием SqlClient.

Для соединения с SQL Server предпочтительно использовать проверку подлинности Windows. Однако если требуется проверка подлинности SQL Server, то имя пользователя и пароль указываются с помощью приведенного ниже синтаксиса. В этом примере символы звездочки представляют допустимое имя пользователя и пароль.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Если при подключении к Базе данных SQL Azure или Azure Synapse Analytics вы указываете имя входа в формате `user@servername`, значение `servername` в имени входа должно соответствовать значению, указанному для `Server=`.

> [!NOTE]
> Проверка подлинности Windows имеет приоритет над именами входа SQL Server. Если указать значение Integrated Security=true, а также ввести имя пользователя и пароль, то имя пользователя и пароль не будут учитываться и будет применяться проверка подлинности Windows.

### <a name="connect-to-a-named-instance-of-sql-server"></a>Подключение к именованному экземпляру SQL Server

Чтобы подключиться к именованному экземпляру SQL Server, используйте синтаксис *имя_сервера\имя_экземпляра*.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

Кроме того, в свойстве <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> объекта `SqlConnectionStringBuilder` можно задать имя экземпляра при построении строки подключения. Свойство <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> объекта <xref:Microsoft.Data.SqlClient.SqlConnection> доступно только для чтения.

### <a name="type-system-version-changes"></a>Изменения в версии системы типов

Ключевое слово `Type System Version` в <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> указывает клиентское представление типов SQL Server. Дополнительные сведения о ключевом слове <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> см. в разделе `Type System Version`.

## <a name="connect-and-attach-to-sql-server-express-user-instances"></a>Подключение к пользовательским экземплярам SQL Server Express и их присоединение

Пользовательские экземпляры являются одной из возможностей SQL Server Express. Они дают пользователям под учетной записью с минимальными правами возможность присоединить и запустить базу данных SQL Server без прав администратора. Пользовательский экземпляр выполняется с учетными данными пользователя Windows, а не службы.

Дополнительные сведения см. в статье [Пользовательские экземпляры SQL Server Express](./sql/sql-server-express-user-instances.md).

## <a name="use-trustservercertificate"></a>Использование TrustServerCertificate

Ключевое слово `TrustServerCertificate` применяется только при подключении к экземпляру SQL Server с допустимым сертификатом. Если `TrustServerCertificate` имеет значение `true`, на транспортном уровне будет использоваться протокол TLS/SSL для шифрования канала и не будет проверяться цепочка сертификатов для проверки доверия.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> Если ключевому слову `TrustServerCertificate` присвоено значение `true` и включено шифрование, то будет использоваться уровень шифрования, заданный на сервере, даже если в строке подключения `Encrypt` задано значение `false`. В противном случае соединение не будет установлено.

### <a name="enable-encryption"></a>Включение шифрования

Чтобы включить шифрование, когда на сервере не представлен сертификат, в диспетчере конфигурации SQL Server необходимо настроить параметры **Принудительное шифрование протокола** и **Доверять сертификату сервера**. В этом случае шифрование будет использовать самозаверяющий сертификат сервера, не проверяя наличия подтверждаемого сертификата сервера.

Настройки приложения не могут снизить установленный на SQL Server уровень безопасности, но при необходимости могут повысить его. Приложение может затребовать шифрование, присвоив ключевым словам `TrustServerCertificate` и `Encrypt` значение `true`, гарантируя тем самым, что шифрование будет выполняться, даже если сертификат сервера не подготовлен и для клиента не настроен параметр **Принудительное шифрование протокола**. Но если на клиенте не установлен параметр `TrustServerCertificate`, то сертификат сервера, тем не менее, потребуется.

В следующей таблице перечислены все случаи.

|Параметр «Принудительное шифрование протокола» на клиенте|Параметр «Доверять сертификату сервера» на клиенте|Строка или атрибут «Шифровать/Использовать шифрование для подключения к данным»|Строка подключения или атрибут «Доверять сертификату сервера»|Результат|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|нет|Н/Д|Нет (по умолчанию)|Не учитывается|Шифрование отсутствует.|  
|нет|Недоступно|Да|Нет (по умолчанию)|Шифрование применяется только при наличии подтверждаемого сертификата сервера, в противном случае попытка соединения завершается неудачно.|  
|нет|Недоступно|Да|Да|Шифрование производится всегда, однако при этом может использоваться самозаверяющий сертификат сервера.|  
|Да|нет|Не учитывается|Не учитывается|Шифрование применяется только при наличии подтверждаемого сертификата сервера, в противном случае попытка подключения завершается сбоем.|  
|Да|Да|Нет (по умолчанию)|Не учитывается|Шифрование производится всегда, однако при этом может использоваться самозаверяющий сертификат сервера.|  
|Да|Да|Да|Нет (по умолчанию)|Шифрование применяется только при наличии подтверждаемого сертификата сервера, в противном случае попытка подключения завершается сбоем.|  
|Да|Да|Да|Да|Шифрование производится всегда, однако при этом может использоваться самозаверяющий сертификат сервера.|  

Дополнительные сведения см. в статье [Использование шифрования без проверки](/sql/relational-databases/native-client/features/using-encryption-without-validation).

## <a name="see-also"></a>См. также

- [Строки подключения](connection-strings.md)
- [подключение к источнику данных](connecting-to-data-source.md);
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
