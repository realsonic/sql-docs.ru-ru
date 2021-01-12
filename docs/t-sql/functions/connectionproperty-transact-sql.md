---
description: CONNECTIONPROPERTY (Transact-SQL)
title: CONNECTIONPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONNECTIONPROPERTY_TSQL
- CONNECTIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- CONNECTIONPROPERTY statement
ms.assetid: 6bd9ccae-af77-4a05-b97f-f8ab41cfde42
author: cawrites
ms.author: chadam
ms.openlocfilehash: 428201b41c42342df355df571763265e64a5da11
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092352"
---
# <a name="connectionproperty-transact-sql"></a>CONNECTIONPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Для запроса, поступающего на сервер, эта функция возвращает сведения о свойствах уникального подключения с поддержкой этого запроса.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CONNECTIONPROPERTY ( property )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*property*  
Свойство подключения. Аргумент *property* может иметь одно из следующих значений:
  
|Значение|Тип данных|Описание|  
|---|---|---|
|net_transport|**nvarchar(40)**|Возвращает описание физического транспортного протокола, используемого этим соединением. Значение NULL не допускается. Возможные возвращаемые значения:<br /><br /> **HTTP**<br /> **Именованный канал**<br /> **Согласованность сеанса**<br /> **Общая память**<br /> **SSL**<br /> **TCP**<br /><br /> и<br /><br /> **VIA**<br /><br /> Примечание. Всегда возвращает значение **Session**, если при подключении используется множественный активный результирующий набор (функция MARS), а также включено использование пулов подключений.|  
|protocol_type|**nvarchar(40)**|Возвращает тип протокола передачи полезных данных. В настоящее время различаются протоколы TDS (TSQL) и SOAP. Допускает значение NULL.|  
|auth_scheme|**nvarchar(40)**|Возвращает схему аутентификации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подключения. Схема проверки подлинности предусматривает использование проверки подлинности Windows (NTLM, KERBEROS, DIGEST, BASIC, NEGOTIATE) либо проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не допускает значение NULL.|  
|local_net_address|**varchar(48)**|Возвращает IP-адрес сервера, с которым установлено определенное подключение. Можно использовать только для подключений, которые в качестве транспорта данных используют протокол TCP. Допускает значение NULL.|  
|local_tcp_port|**int**|Возвращает TCP-порт сервера, с которым установлено подключение, которое использует протокол TCP. Допускает значение NULL.|  
|client_net_address|**varchar(48)**|Запрашивает адрес клиента, который пытается подключиться к этому серверу. Допускает значение NULL.|  
|physical_net_transport|**nvarchar(40)**|Возвращает описание физического транспортного протокола, используемого этим соединением. Возвращает точные данные, если для соединения включен режим MARS.|  
|\<Any other string>||Возвращает значение NULL для недопустимых входных данных.|  
  
## <a name="remarks"></a>Remarks  
**local_net_address** и **local_tcp_port** возвращают значение NULL в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
Возвращаемые значения совпадают с параметрами, показанными для соответствующих столбцов в динамическом административном представлении [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md). Пример:
  
```sql
SELECT   
ConnectionProperty('net_transport') AS 'Net transport',   
ConnectionProperty('protocol_type') AS 'Protocol type';  
```  
  
## <a name="see-also"></a>См. также раздел
[sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
[sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
  
  
