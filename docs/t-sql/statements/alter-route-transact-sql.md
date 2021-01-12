---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 504f1eed0f50a711a671301b10001b44d6097330
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092047"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет данные о существующем маршруте в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *route_name*  
 Имя изменяемого маршрута. Не могут быть указаны имена сервера, базы данных и схемы.  
  
 WITH  
 Представляет предложения, определяющие изменяемый маршрут.  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 Указывает имя удаленной службы, находящейся по этому маршруту. Значение *service_name* должно точно совпадать с именем, используемым удаленной службой. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит побайтовое сравнение при поиске соответствия значению *service_name*. Другими словами, при сравнении учитывается регистр и не применяются текущие параметры сортировки. Маршрут с именем службы **'SQL/ServiceBroker/BrokerConfiguration'**  — это маршрут к службе уведомления конфигурации брокера. В маршруте к этой службе может не указываться экземпляр компонента Service Broker.  
  
 Если опущено предложение SERVICE_NAME, имя службы для маршрута не меняется.  
  
 BROKER_INSTANCE **='** _broker\_instance_ **'**  
 Указывает базу данных, в которой расположена целевая служба. Параметр *broker_instance* должен являться идентификатором экземпляра брокера для удаленной базы данных. Этот идентификатор можно получить, выполнив следующий запрос в выбранной базе данных:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 Если опущено предложение BROKER_INSTANCE, экземпляр брокера для маршрута не меняется.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 LIFETIME **=** _route\_lifetime_  
 Время в секундах, в течение которого [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] хранит маршрут в таблице маршрутизации. По истечении этого времени действие маршрута истекает, и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при выборе маршрута для новых диалогов далее его не рассматривает. Если данное предложение опущено, срок жизни маршрута не меняется.  
  
 ADDRESS **='** _next\_hop\_address_'  

 Для Управляемого экземпляра SQL Azure аргумент `ADDRESS` должен задавать локальный адрес.

 Указывает сетевой адрес для данного маршрута. Аргумент *next_hop_address* задает адрес TCP/IP в следующем формате:  
  
 **TCP://** { *dns_name* | *netbios_name* |*ip_address* } **:** *port_number*  
  
 Указанный аргумент *port_number* должен соответствовать номеру порта конечной точки компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на указанном компьютере. Его можно получить, выполнив к выбранной базе данных следующий запрос:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Если для маршрута в аргументе **next_hop_address** указывается значение *'LOCAL'* , сообщение доставляется службе текущего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если в аргументе **next_hop_address** указывается значение *'TRANSPORT'* , сетевой адрес определяется на основе сетевого адреса, указанного в имени службы. Маршрут со значением **'TRANSPORT'** может обозначать имя службы или экземпляр брокера.  
  
 Если аргумент *next_hop_address* является основным сервером для зеркала базы данных, для зеркального сервера также необходимо указать MIRROR_ADDRESS. Иначе данный маршрут не перейдет к зеркальному серверу при отработке отказа автоматически.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Указывает сетевой адрес для зеркального сервера зеркальной пары, чей основной сервер находится по адресу *next_hop_address*. Аргумент *next_hop_mirror_address* задает адрес TCP/IP в следующем формате:  
  
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
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
## <a name="remarks"></a>Remarks  
 Таблица маршрутизации, хранящая маршруты, представляет собой таблицу метаданных, данные из которой могут быть получены с помощью представления каталога **sys.routes**. Таблица маршрутизации может быть обновлена только с помощью инструкций CREATE ROUTE, ALTER ROUTE и DROP ROUTE.  
  
 Предложения, не указанные в команде ALTER ROUTE, остаются неизменными. Таким образом, инструкция ALTER не может быть использована для указания того, что маршрут не блокируется по времени, что он соответствует какому-либо имени службы или экземпляру брокера. Чтобы изменить эти параметры маршрута, необходимо удалить существующий маршрут и создать новый с обновленными сведениями.  
  
 Если в аргументе **next_hop_address** указывается значение *'TRANSPORT'* , сетевой адрес определяется на основе сетевого адреса, указанного в имени службы. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может успешно обрабатывать имена служб, которые начинаются с сетевого адреса в формате, допустимом для *next_hop_address*. Службы с именами, содержащими действительные сетевые адреса, создадут маршрут к сетевому адресу в имени службы.  
  
 Таблица маршрутизации может содержать любое количество маршрутов, в которых указаны имя службы, сетевой адрес или идентификатор экземпляра брокера. В этих случаях компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит выбор маршрута при помощи процедуры поиска наиболее точного соответствия сведений, указанных в диалоге, данным, содержащимся в таблице маршрутизации.  
  
 Чтобы изменить свойство AUTHORIZATION для службы, используйте инструкцию ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на изменение маршрута по умолчанию предоставляется владельцу маршрута, членам предопределенной роли базы данных **db_ddladmin** или **db_owner** и членам предопределенной роли сервера **sysadmin**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-changing-the-service-for-a-route"></a>A. Изменение службы для маршрута  
 В следующем примере маршрут `ExpenseRoute` изменяется таким образом, чтобы указывать на удаленную службу `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>Б. Изменение целевой базы данных для маршрута  
 В следующем примере целевая база данных для маршрута `ExpenseRoute` изменяется на базу данных, определяемую уникальным идентификатором `D8D4D268-00A3-4C62-8F91-634B89B1E317.`  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>В. Изменение адреса маршрута  
 В следующем примере сетевой адрес для маршрута `ExpenseRoute` к порту TCP `1234` на узле изменяется на IP-адрес `10.2.19.72`.  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>Г. Изменение базы данных и адреса маршрута  
 В следующем примере сетевой адрес маршрута `ExpenseRoute` меняется на TCP-порт `1234` на узле с DNS-именем `www.Adventure-Works.com`. Кроме того, целевая база данных заменяется на базу данных, определяемую уникальным идентификатором `D8D4D268-00A3-4C62-8F91-634B89B1E317`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE ROUTE (Transact-SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE (Transact-SQL)](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
