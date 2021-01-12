---
description: CREATE ROUTE (Transact-SQL)
title: CREATE ROUTE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_ROUTE_TSQL
- ROUTE
- CREATE ROUTE
- ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lifetimes [Service Broker]
- routes [Service Broker], creating
- forwarding brokers [SQL Server]
- service names [Service Broker]
- database mirroring [SQL Server], routing
- expired routes [SQL Server]
- activating routes
- CREATE ROUTE statement
ms.assetid: 7e695364-1a98-4cfd-8ebd-137ac5a425b3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: fc34a3eb5987a7348733755ee69aafda87e16ac3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091967"
---
# <a name="create-route-transact-sql"></a>CREATE ROUTE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Добавляет новый маршрут к таблице маршрутов для текущей базы данных. Для исходящих сообщений компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] определяет маршруты, проверяя таблицу маршрутов в локальной базе данных. Для сообщений диалогов, начатых в другом экземпляре, включая пересылаемые сообщения, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] проверяет маршруты в базе данных **msdb**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE ROUTE route_name  
[ AUTHORIZATION owner_name ]  
WITH    
   [ SERVICE_NAME = 'service_name', ]  
   [ BROKER_INSTANCE = 'broker_instance_identifier' , ]  
   [ LIFETIME = route_lifetime , ]  
   ADDRESS =  'next_hop_address'  
   [ , MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Аргументы  
 *route_name*  
 Имя создаваемого маршрута. Новый маршрут создается в текущей базе данных и принадлежит участнику, указанному в предложении AUTHORIZATION. Не могут быть указаны имена сервера, базы данных и схемы. Аргумент *route_name* должен быть допустимым аргументом **sysname**.  
  
 AUTHORIZATION *owner_name*  
 Устанавливает заданного пользователя или роль базы данных в качестве владельца маршрута. Аргумент *owner_name* может быть именем любого допустимого пользователя или роли, если текущий пользователь является членом предопределенной роли базы данных **db_owner** или предопределенной роли сервера **sysadmin**. В противном случае аргумент *owner_name* должен быть именем текущего пользователя, именем пользователя, для которого у текущего пользователя есть разрешение IMPERSONATE, или именем роли, которой принадлежит текущий пользователь. Если это предложение опущено, то маршрут принадлежит текущему пользователю.  
  
 WITH  
 Представляет предложения, которые определяют создаваемый маршрут.  
  
 SERVICE_NAME = **'** _service\_name_ **'**  
 Указывает имя удаленной службы, находящейся по этому маршруту. Значение *service_name* должно точно совпадать с именем, используемым удаленной службой. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит побайтовое сравнение при поиске соответствия значению *service_name*. Другими словами, при сравнении учитывается регистр и не применяются текущие параметры сортировки. Если аргумент SERVICE_NAME опущен, этот маршрут соответствует любому имени службы, но имеет более низкий приоритет, чем маршрут с аргументом SERVICE_NAME. Маршрут с именем службы **'SQL/ServiceBroker/BrokerConfiguration'**  — это маршрут к службе уведомления конфигурации брокера. В маршруте к этой службе может не указываться экземпляр компонента Service Broker.  
  
 BROKER_INSTANCE = **'** _broker\_instance\_identifier_ **'**  
 Указывает базу данных, в которой расположена целевая служба. Параметр *broker_instance_identifier* должен являться идентификатором экземпляра брокера для удаленной базы данных. Этот идентификатор можно получить, выполнив следующий запрос в выбранной базе данных:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID()  
```  
  
 Если предложение BROKER_INSTANCE опущено, то маршрут соответствует любому экземпляру брокера. Маршрут, соответствующий любому экземпляру брокера, имеет более высокий приоритет соответствия, чем маршрут с явным экземпляром брокера, когда диалог не указывает экземпляр брокера. Для диалогов, указывающих экземпляр брокера, маршрут с экземпляром брокера имеет более высокий приоритет, чем маршрут, соответствующий любому экземпляру брокера.  
  
 LIFETIME **=** _route\_lifetime_  
 Время в секундах, в течение которого [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] хранит маршрут в таблице маршрутизации. По истечении этого времени действие маршрута истекает, и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при выборе маршрута для новых диалогов далее его не рассматривает. Если предложение опущено, то аргумент *route_lifetime* имеет значение NULL, и срок маршрута никогда не истекает.  
  
 ADDRESS **='** _next\_hop\_address_ **'**  
Для Управляемого экземпляра SQL аргумент `ADDRESS` должен задавать локальный адрес. 

Указывает сетевой адрес для данного маршрута. Аргумент *next_hop_address* задает адрес TCP/IP в следующем формате:  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** _port\_number_  
  
 Указанный аргумент *port_number* должен соответствовать номеру порта конечной точки компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на указанном компьютере. Его можно получить, выполнив к выбранной базе данных следующий запрос:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Если служба находится на зеркальной базе данных, необходимо также указать MIRROR_ADDRESS для другого экземпляра зеркальной базы данных. В противном случае он не будет учитываться в маршруте.  
  
 Если для маршрута в аргументе **next_hop_address** указывается значение *'LOCAL'* , сообщение доставляется службе текущего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если в аргументе **next_hop_address** указывается значение *'TRANSPORT'* , сетевой адрес определяется на основе сетевого адреса, указанного в имени службы. Маршрут со значением **'TRANSPORT'** может не указывать имя службы или экземпляр брокера.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Указывает сетевой адрес зеркальной базы данных, если одна зеркальная база данных находится по адресу *next_hop_address*. Аргумент *next_hop_mirror_address* задает адрес TCP/IP в следующем формате:  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** *port_number*  
  
 Указанный аргумент *port_number* должен соответствовать номеру порта конечной точки компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на указанном компьютере. Его можно получить, выполнив к выбранной базе данных следующий запрос:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Если указано предложение MIRROR_ADDRESS, маршрут должен указать предложения SERVICE_NAME и BROKER_INSTANCE. Для маршрутов с аргументом **next_hop_address**, имеющим значение **'LOCAL'** или *'TRANSPORT'* , нельзя указывать зеркальный адрес.  
  
## <a name="remarks"></a>Remarks  
 Таблица маршрутизации, в которой хранятся маршруты, представляет собой таблицу метаданных, данные из которой могут быть получены с помощью представления каталога **sys.routes**. Это представление каталога может быть обновлено только с помощью инструкций CREATE ROUTE, ALTER ROUTE и DROP ROUTE.  
  
 По умолчанию таблица маршрутов в каждой базе данных содержит один маршрут. Этот маршрут называется **AutoCreatedLocal**. Маршрут указывает значение **'LOCAL'** для аргумента *next_hop_address* и соответствует любому имени службы и идентификатору экземпляра брокера.  
  
 Если в аргументе **next_hop_address** указывается значение *'TRANSPORT'* , сетевой адрес определяется на основе сетевого адреса, указанного в имени службы. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может успешно обрабатывать имена служб, которые начинаются с сетевого адреса в формате, допустимом для *next_hop_address*.  
  
 Таблица маршрутов может содержать любое количество маршрутов, указывающих одну и ту же службу, сетевой адрес и идентификатор экземпляра брокера. В этих случаях компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит выбор маршрута при помощи процедуры поиска наиболее точного соответствия сведений, указанных в диалоге, данным, содержащимся в таблице маршрутизации.  
  
 Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не удаляет из таблицы маршрутизации маршруты с истекшим сроком действия. Маршрут с истекшим сроком можно активировать с помощью инструкции ALTER ROUTE.  
  
 Маршрут не может быть временным объектом. Допускаются имена маршрутов, начинающиеся с символа **#** , но они являются постоянными объектами.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на создание маршрута по умолчанию имеют члены предопределенных ролей базы данных **db_ddladmin** и **db_owner** и члены предопределенной роли сервера **sysadmin**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-tcpip-route-by-using-a-dns-name"></a>A. Создание TCP/IP-маршрута с помощью DNS-имени  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/Expenses`. Маршрут указывает, что сообщения для этой службы передаются по протоколу TCP на порт `1234` узла, который определяется DNS-именем `www.Adventure-Works.com`. Целевой сервер доставляет сообщения по прибытию их на экземпляр брокера, определенный уникальным идентификатором `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql 
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://www.Adventure-Works.com:1234' ;  
```  
  
### <a name="b-creating-a-tcpip-route-by-using-a-netbios-name"></a>Б. Создание TCP/IP-маршрута с помощью NetBIOS-имени  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/Expenses`. Маршрут указывает, что сообщения для этой службы передаются по протоколу TCP на порт `1234` узла, который определяется NetBIOS-именем `SERVER02`. По прибытии сообщения целевой сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доставляет его экземпляру базы данных, определенному уникальным идентификатором `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH   
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://SERVER02:1234' ;  
```  
  
### <a name="c-creating-a-tcpip-route-by-using-an-ip-address"></a>В. Создание TCP/IP-маршрута с помощью IP-адреса  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/Expenses`. Маршрут указывает, что сообщения для этой службы передаются по протоколу TCP на порт `1234` узла с IP-адресом `192.168.10.2`. По прибытии сообщения целевой сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доставляет его экземпляру брокера, определенному уникальным идентификатором `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://192.168.10.2:1234' ;  
```  
  
### <a name="d-creating-a-route-to-a-forwarding-broker"></a>Г. Создание маршрута к брокеру пересылки  
 В следующем примере создается маршрут к перенаправляющему брокеру на сервере `dispatch.Adventure-Works.com`. Поскольку не заданы ни имя службы, ни идентификатор экземпляра брокера, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует этот маршрут для служб, маршруты которых не определены.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    ADDRESS = 'TCP://dispatch.Adventure-Works.com' ;   
```  
  
### <a name="e-creating-a-route-to-a-local-service"></a>Д. Создание маршрута к локальной службе  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/LogRequests` в том же экземпляре, что и маршрут.  
  
```sql  
CREATE ROUTE LogRequests  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/LogRequests',  
    ADDRESS = 'LOCAL' ;  
```  
  
### <a name="f-creating-a-route-with-a-specified-lifetime"></a>Е. Создание маршрута с заданным временем существования  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/Expenses`. Срок жизни маршрута равен `259200` секундам, что составляет 72 часа.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    LIFETIME = 259200,  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234' ;  
```  
  
### <a name="g-creating-a-route-to-a-mirrored-database"></a>Ж. Создание маршрута к зеркальной базе данных  
 В следующем примере создается маршрут к службе `//Adventure-Works.com/Expenses`. Эта служба размещена на зеркальной базе данных. Одна из зеркальных баз данных расположена по адресу `services.Adventure-Works.com:1234`, вторая — по адресу `services-mirror.Adventure-Works.com:1234`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = '69fcc80c-2239-4700-8437-1001ecddf933',  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234',   
    MIRROR_ADDRESS = 'TCP://services-mirror.Adventure-Works.com:1234' ;  
```  
  
### <a name="h-creating-a-route-that-uses-the-service-name-for-routing"></a>З. Создание маршрута, использующего для маршрутизации имя службы  
 В следующем примере создается маршрут, который использует имя службы для определения сетевого адреса, на который будут отправляться сообщения. Обратите внимание, что маршрут, указывающий `'TRANSPORT'` в качестве сетевого адреса, имеет более низкий приоритет совпадения, чем другие маршруты.  
  
```sql  
CREATE ROUTE TransportRoute  
    WITH ADDRESS = 'TRANSPORT' ;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER ROUTE (Transact-SQL)](../../t-sql/statements/alter-route-transact-sql.md)   
 [DROP ROUTE (Transact-SQL)](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
