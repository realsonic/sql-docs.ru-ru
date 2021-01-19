---
description: CREATE ENDPOINT (Transact-SQL)
title: CREATE ENDPOINT (Transact-SQL)
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENDPOINT
- CREATE ENDPOINT
- ENDPOINT_TSQL
- CREATE_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HTTP SOAP support [SQL Server]
- CREATE ENDPOINT statement
- Availability Groups [SQL Server], configuring
- endpoints [SQL Server], creating
- SOAP [SQL Server built-in support], endpoints
- SOAP [SQL Server built-in support], sqlbatch
- DATABASE_MIRRORING option
- HTTP protocol option [SQL Server]
- SOAP [SQL Server built-in support], ad hoc
- TCP protocol option [SQL Server]
- SERVICE_BROKER option
- Availability Groups [SQL Server], endpoint
ms.assetid: 6405e7ec-0b5b-4afd-9792-1bfa5a2491f6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c787027e1cebf844339a8cdc70bd23a6d8c974bd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171656"
---
# <a name="create-endpoint-transact-sql"></a>CREATE ENDPOINT (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает конечные точки и определяет их параметры, включая методы, доступные клиентским приложениям. Дополнительные сведения о разрешениях см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 Синтаксис CREATE ENDPOINT логически может быть разбит на две части.  
  
-   Первая часть начинается с AS и заканчивается перед предложением FOR.  
  
     В этой части задаются сведения, связанные с транспортным протоколом (TCP), устанавливается номер прослушиваемого порта для конечной точки, а также указывается метод проверки подлинности конечной точки и/или список IP-адресов (при наличии), доступ к конечной точке от которых нужно запретить.  
  
-   Вторая часть начинается с предложения FOR.  
  
     В этой части определяется, какие полезные данные будут поддерживаться конечной точкой. Полезные данные могут иметь один из нескольких поддерживаемых типов: [!INCLUDE[tsql](../../includes/tsql-md.md)], компонент Service Broker, зеркальное отображение базы данных. В эту часть также включаются сведения, зависящие от выбранного типа.  
  
> **ПРИМЕЧАНИЕ.** Собственные веб-службы XML (конечные точки SOAP/HTTP) удалены, начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
AS { TCP } (  
   <protocol_specific_arguments>  
        )  
FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_arguments>  
        )  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( xx.xx.xx.xx IPv4 address ) | ( '__:__1' IPv6 address ) ]  
  
)  
  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ [ , ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
   ]  
   [ [ , ] MESSAGE_FORWARDING = { ENABLED | DISABLED } ]  
   [ [ , ] MESSAGE_FORWARD_SIZE = forward_size ]  
)  
  

<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
   [ [ [ , ] ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
  
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *endPointName*  
 Назначенное имя создаваемой конечной точки. Используется при обновлении или удалении конечной точки.  
  
 AUTHORIZATION *login*  
 Указывает правильное имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или Windows, которое назначается владельцем только что созданного объекта конечной точки. Если аргумент AUTHORIZATION не указан, по умолчанию владельцем только что созданного объекта становится пользователь, вызывающий инструкцию.  
  
 Чтобы назначить владельца при помощи аргумента AUTHORIZATION, вызывающий должен иметь разрешение IMPERSONATE для указанного значения *login*.  
  
 Сведения о переназначении владельца см. в разделе [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 STATE **=** { STARTED | **STOPPED** | DISABLED }  
 Состояние конечной точки после ее создания. Если при создании конечной точки состояние не указано, по умолчанию устанавливается значение STOPPED.  
  
 STARTED  
 Конечная точка запущена и активно прослушивает соответствующий порт, ожидая соединений.  
  
 DISABLED  
 Конечная точка отключена. В этом состоянии сервер прослушивает запросы к порту, но возвращает клиенту ошибку.  
  
 **STOPPED**  
 Конечная точка остановлена. В этом состоянии сервер не прослушивает порт конечной точки и не отвечает ни на какие попытки запроса на использование конечной точки.  
  
 Сведения об изменении состояния см. в разделе [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 AS { TCP }  
 Указывает используемый транспортный протокол.  
  
 FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING }  
 Указывает тип передаваемых полезных данных.  
  
 В настоящее время не существует специфических для языка [!INCLUDE[tsql](../../includes/tsql-md.md)] аргументов, передаваемых в параметре `<language_specific_arguments>`.  
  
 **Параметры протокола TCP**  
  
 Следующие аргументы относятся только к протоколу TCP.  
  
 LISTENER_PORT **=** _порт_прослушивателя_  
 Указывает номер порта протокола TCP/IP, прослушиваемого компонентом Service Broker на предмет соединений. По соглашению используется порт 4022, но допустим любой порт от 1024 до 32767.  
  
 LISTENER_IP **=** ALL | **(** _4-part-ip_ **)**  |  **(** "*ip_address_v6*" **)**  
 Указывает IP-адрес, с которым конечная точка будет ожидать соединения. По умолчанию значение установлено в ALL. Это значит, что средство прослушивания примет соединение с любым верным IP-адресом.  
  
 Если зеркальное отображение базы данных настраивается с использованием IP-адреса, а не полного имени домена (`ALTER DATABASE SET PARTNER = partner_IP_address` или `ALTER DATABASE SET WITNESS = witness_IP_address`), необходимо указать `LISTENER_IP =IP_address` вместо `LISTENER_IP=ALL` при создании конечных точек зеркального отображения.  
  
 **Параметры SERVICE_BROKER и DATABASE_MIRRORING**  
  
 Параметры AUTHENTICATION и ENCRYPTION используются, если указан аргумент SERVICE_BROKER или DATABASE_MIRRORING.  
  
> [!NOTE]  
>  Для получения сведений об аргументах, относящихся только к SERVICE_BROKER, см. подраздел «Параметры SERVICE_BROKER» ниже в данном разделе. Для получения сведений о параметрах, относящихся только к DATABASE_MIRRORING, см. статью «Параметры DATABASE_MIRRORING» ниже в данном разделе.  
  
 AUTHENTICATION **=** \<authentication_options> Указывает требования к проверке подлинности TCP/IP для соединений данной конечной точки. По умолчанию значение установлено в WINDOWS.  
  
 Поддерживаемые методы проверки подлинности включают в себя NTLM, Kerberos или оба этих метода.  
  
> [!IMPORTANT]  
>  Все соединения зеркального отображения на экземпляре сервера используют одну конечную точку зеркального отображения базы данных. Любая попытка создания дополнительной конечной точки зеркального отображения баз данных приведет к ошибке.  
  
 **\<authentication_options> ::=**  
  
 **WINDOWS** [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Указывает, что конечная точка будет подключена с использованием протокола проверки подлинности Windows. Это значение по умолчанию.  
  
 Если указать режим авторизации (NTLM или KERBEROS), этот метод всегда будет использоваться в качестве протокола проверки подлинности. Значение по умолчанию NEGOTIATE соответствует тому, что конечная точка будет использовать протокол переговоров Windows для выбора NTLM или Kerberos.  
  
 CERTIFICATE *certificate_name*  
 Указывает на то, что конечная точка будет проводить проверку подлинности подключений при помощи сертификата, указанного в аргументе *certificate_name*, для определения идентификатора для авторизации. Противоположная конечная точка должна иметь сертификат с открытым ключом, совпадающим с закрытым ключом указанного сертификата.  
  
 WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ] CERTIFICATE *certificate_name*  
 Указывает на то, что конечная точка будет производить попытки подключения при помощи проверки подлинности Windows и, в случае неудачи, будет пытаться использовать указанный сертификат.  
  
 CERTIFICATE *certificate_name* WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Указывает на то, что конечная точка будет производить попытки подключения при помощи указанного сертификата и, в случае неуспеха, будет пытаться использовать проверку подлинности Windows.  
  
 ENCRYPTION = { DISABLED \| SUPPORTED \| **REQUIRED** } [ALGORITHM { **AES** \| RC4 \| AES RC4 \| RC4 AES } ]  
 Указывает, будет ли использоваться шифрование в процессе. По умолчанию значение установлено в REQUIRED.  
  
 DISABLED  
 Указывает на то, что данные, передаваемые через соединение, не будут зашифрованы.  
  
 SUPPORTED  
 Указывает на то, что данные будут зашифрованы только в случае, если у противоположной конечной точки этот аргумент принял значение SUPPORTED или REQUIRED.  
  
 REQUIRED  
 Указывает на то, что соединение с этой конечной точкой должно использовать шифрование. Поэтому для подключения к конечной точке значение аргумента ENCRYPTION противоположной конечной точки должно быть установлено в значение SUPPORTED или значение REQUIRED.  
  
 При необходимости можно использовать аргумент ALGORITHM для указания формы шифрования на конечной точке, как показано ниже:  
  
 **AES**  
 Указывает на то, что конечная точка должна использовать алгоритм AES. Это значение по умолчанию в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версиях.  
  
 RC4;  
 Указывает на то, что конечная точка должна использовать алгоритм RC4. Это значение по умолчанию до версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
  
 AES RC4  
 Указывает на то, что две конечные точки будут согласовывать алгоритм шифрования, оставляя приоритет за алгоритмом AES.  
  
 RC4 AES  
 Указывает на то, что две конечные точки будут согласовывать алгоритм шифрования, оставляя приоритет за алгоритмом RC4.  
  
> [!NOTE]  
>  Алгоритм RC4 устарел. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется использовать алгоритм AES.  
  
 Если обе конечные точки указывают оба алгоритма, но в разной последовательности, то используется та, которая указана на принимающей стороне.  
  
 **Параметры SERVICE_BROKER**  
  
 Следующие параметры используются, если указан аргумент SERVICE_BROKER.  
  
 MESSAGE_FORWARDING **=** { ENABLED | **DISABLED** }  
 Определяет, будут ли перенаправлены сообщения, полученные конечной точкой и предназначенные для служб, расположенных в других местах.  
  
 ENABLED  
 Перенаправляет сообщения, если доступен адрес перенаправления.  
  
 DISABLED  
 Удаляет сообщения, предназначенные для служб, расположенных в других местах. Это значение по умолчанию.  
  
 MESSAGE_FORWARD_SIZE **=** _forward_size_  
 Указывает максимальный объем хранилища в мегабайтах для размещения в нем сообщений конечной точки, предназначенных для перенаправления.  
  
 **Параметры DATABASE_MIRRORING**  
  
 Следующие параметры используются, если указан аргумент DATABASE_MIRRORING.  
  
 ROLE **=** { WITNESS | PARTNER | ALL }  
 Указывает роль в зеркальном отображении базы данных или роли, поддерживаемые конечной точкой.  
  
 WITNESS  
 Позволяет конечной точке выполнять роль следящего сервера в процессе зеркального отображения.  
  
> [!NOTE]  
>  Для выпуска [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] WITNESS является единственным доступным значением.  
  
 PARTNER  
 Позволяет конечной точке выполнять роль участника в процессе зеркального отображения.  
  
 ALL  
 Позволяет конечной точке выполнять как роль следящего сервера, так и роль участника в процессе зеркального отображения.  
  
 Дополнительные сведения об этих ролях см. в разделе [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!NOTE]  
>  Также для аргумента DATABASE_MIRRORING не определен порт по умолчанию.  
  
## <a name="remarks"></a>Remarks  
 Инструкции DDL ENDPOINT не могут выполняться внутри пользовательской транзакции. Инструкции ENDPOINT DDL не будут вызывать ошибки даже в случае, если активная транзакция уровня изоляции моментального снимка использует изменяемую конечную точку.  
  
 Запросы к объекту ENDPOINT могут быть выполнены следующими пользователями:  
  
-   членами предопределенной роли сервера **sysadmin**;  
  
-   владельцем конечной точки;  
  
-   пользователями или группами, которым предоставлено разрешение CONNECT на конечную точку.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение CREATE ENDPOINT или членство в предопределенной роли сервера **sysadmin** . Дополнительные сведения см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="example"></a>Пример  
  
### <a name="creating-a-database-mirroring-endpoint"></a>Создание конечной точки зеркального отображения базы данных  
 В следующем примере создается конечная точка зеркального отображения базы данных. Конечная точка использует номер порта `7022`, хотя допустим любой доступный номер порта. Конечная точка настроена на использование проверки подлинности Windows только по методу Kerberos. Аргумент `ENCRYPTION` имеет значение `SUPPORTED`, отличное от значения по умолчанию, для передачи зашифрованных или незашифрованных данных. Конечная точка может выступать как в роли участника, так и в роли следящего сервера.  
  
```sql  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (  
       AUTHENTICATION = WINDOWS KERBEROS,  
       ENCRYPTION = SUPPORTED,  
       ROLE=ALL);  
GO  
```  

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv4-address-and-port"></a>Создайте новую конечную точку, указывающую на определенные IPv4-адрес и порт

```sql
CREATE ENDPOINT ipv4_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = (10.0.75.1)
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public; -- Keep existing public permission on default endpoint for demo purpose
GRANT CONNECT ON ENDPOINT::ipv4_endpoint_special
TO login_name;
```

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv6-address-and-port"></a>Создайте новую конечную точку, указывающую на определенные IPv6-адрес и порт

```sql
CREATE ENDPOINT ipv6_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = ('::1')
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public;
GRANT CONNECT ON ENDPOINT::ipv6_endpoint_special

```
  
## <a name="see-also"></a>См. также раздел  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [DROP ENDPOINT (Transact-SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

