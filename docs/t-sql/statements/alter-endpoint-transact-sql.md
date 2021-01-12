---
description: ALTER ENDPOINT (Transact-SQL)
title: ALTER ENDPOINT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER ENDPOINT
- ALTER_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ENDPOINT statement
- modifying endpoints
- endpoints [SQL Server], modifying
ms.assetid: 70f35566-30cf-47c6-8394-dfe5d71629d3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 361f91c88f597b18324d56ac3d331899bd73881c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099561"
---
# <a name="alter-endpoint-transact-sql"></a>ALTER ENDPOINT (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Эта инструкция позволяет изменять существующие конечные точки следующими способами.  
  
-   Путем добавления нового метода к существующей конечной точке.  
  
-   Путем изменения или удаления существующего метода из конечной точки.  
  
-   Путем изменения свойств конечной точки.  
  
> [!NOTE]  
>  В этом подразделе описаны синтаксис и аргументы, характерные для инструкции ALTER ENDPOINT. Описание аргументов, общих для инструкций CREATE ENDPOINT и ALTER ENDPOINT, см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Собственные XML-веб-службы (конечные точки SOAP/HTTP) удалены, начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
ALTER ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
[ AS { TCP } ( <protocol_specific_items> ) ]  
[ FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_items>  
        ) ]  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( 4-part-ip ) | ( "ip_address_v6" ) ]  
)  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
   ]  
  
  [ , MESSAGE_FORWARDING = {ENABLED | DISABLED} ]  
  [ , MESSAGE_FORWARD_SIZE = forwardSize  
)  
  
<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
  
> [!NOTE]  
>  Следующие аргументы являются характерными для инструкции ALTER ENDPOINT. Описания остальных аргументов см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 **AS** { **TCP** }  
 Транспортный протокол нельзя изменить с помощью инструкции **ALTER ENDPOINT**.  
  
 **AUTHORIZATION** *login*  
 Параметр **AUTHORIZATION** недоступен в инструкции **ALTER ENDPOINT**. Владельца можно назначать только в момент создания конечной точки.  
  
 **FOR** { **TSQL** | **SERVICE_BROKER** | **DATABASE_MIRRORING** }  
 Тип полезных данных нельзя изменить с помощью инструкции **ALTER ENDPOINT**.  
  
## <a name="remarks"></a>Комментарии  
 При использовании ALTER ENDPOINT укажите только те параметры, которые необходимо обновить. При отсутствии явного изменения все свойства существующей конечной точки остаются прежними.  
  
 Инструкции ENDPOINT DDL внутри пользовательской транзакции выполняться не могут.  
  
 Дополнительные сведения о выборе алгоритма шифрования для использования с конечной точкой см. в разделе [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
> [!NOTE]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
>   
>  RC4 — относительно слабый алгоритм, а AES — относительно сильный. Однако AES заметно медленнее RC4. Если приоритет защиты выше скорости, рекомендуется использовать алгоритм AES.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо, чтобы пользователь был членом предопределенной роли сервера **sysadmin**, владельцем конечной точки или имел разрешение ALTER ANY ENDPOINT.  
  
 Чтобы изменить принадлежность существующей конечной точки, необходимо применить инструкцию ALTER AUTHORIZATION. Дополнительные сведения см. в разделе [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Дополнительные сведения см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="see-also"></a>См. также  
 [DROP ENDPOINT (Transact-SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
