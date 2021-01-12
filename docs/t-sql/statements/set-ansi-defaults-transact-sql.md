---
description: SET ANSI_DEFAULTS (Transact-SQL)
title: SET ANSI_DEFAULTS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET ANSI_DEFAULTS
- ANSI_DEFAULTS
- SET_ANSI_DEFAULTS_TSQL
- ANSI_DEFAULTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ANSI_DEFAULTS option
- SET ANSI_DEFAULTS statement
ms.assetid: bd721d97-6e23-488b-8c8c-c0453d5b3b86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0e9bbbc3a8f5ae996eb55e80896c532c2507dab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095688"
---
# <a name="set-ansi_defaults-transact-sql"></a>SET ANSI_DEFAULTS (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Управляет группой параметров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которая задает определенное поведение стандарта ISO.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Синтаксис для [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] и [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_DEFAULTS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Синтаксис для [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] и [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_DEFAULTS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
ANSI_DEFAULTS является параметром на стороне сервера, который может включить поведение для всех клиентских подключений. Клиент обычно запрашивает параметр при инициализации соединения или сеанса. Пользователи не могут изменять этот серверный параметр.   
Чтобы изменить поведение клиента, пользователи должны использовать определенные методы, например `SQL_COPT_SS_PRESERVE_CURSORS`. Дополнительные сведения см. в разделе [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).
  
При включении (ON) этого параметра включаются следующие параметры ISO.  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET QUOTED_IDENTIFIER
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

В совокупности перечисленные параметры SET стандарта ISO определяют среду обработки запросов на все время сеанса пользователя, выполнения триггера или хранимой процедуры. Однако эти параметры SET не охватывают всех настроек, необходимых для полного соответствия стандарту ISO.  
  
При работе с индексами для вычисляемых столбцов и индексированных представлений в значение ON должны быть установлены следующие четыре значения по умолчанию: `ANSI_NULLS`, `ANSI_PADDING`, `ANSI_WARNINGS` и `QUOTED_IDENTIFIER`.  Кроме них при создании и изменении индексов для вычисляемых столбцов и индексированных представлений должны быть установлены следующие параметры SET:  Другие параметры SET: `ARITHABORT` (ON), `CONCAT_NULL_YIELDS_NULL` (ON) и `NUMERIC_ROUNDABORT` (OFF). Дополнительные сведения о необходимых установках параметров SET для индексированных представлений и индексов вычисляемых столбцов см. в разделе [Анализ использования инструкций SET](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).  
  
При соединении с драйвером ODBC для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или поставщика OLE DB для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр ANSI_DEFAULTS автоматически устанавливается в значение ON. а CURSOR_CLOSE_ON_COMMIT и IMPLICIT_TRANSACTIONS значение OFF. Параметр OFF для `CURSOR_CLOSE_ON_COMMIT` и `IMPLICIT_TRANSACTIONS` может быть настроен в источниках данных ODBC, в атрибутах соединения ODBC или свойствах соединения OLE DB, установленных в приложении перед подключением к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `ANSI_DEFAULTS` при соединении из приложений DB-Library имеет значение по умолчанию OFF.  
  
При выполнении инструкции SET ANSI_DEFAULTS параметр QUOTED_IDENTIFIER устанавливается на стадии синтаксического анализа, а на стадии выполнения устанавливаются следующие параметры:  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::

## <a name="permissions"></a>Разрешения  
Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
В следующем примере для ANSI_DEFAULTS устанавливается значение ON и выполняется инструкция `DBCC USEROPTIONS` для просмотра затронутых параметров.  
  
```sql  
-- SET ANSI_DEFAULTS ON.  
SET ANSI_DEFAULTS ON;  
GO  

-- Display the current settings.  
DBCC USEROPTIONS;  
GO 

-- SET ANSI_DEFAULTS OFF.  
SET ANSI_DEFAULTS OFF;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [DBCC USEROPTIONS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)   
 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
