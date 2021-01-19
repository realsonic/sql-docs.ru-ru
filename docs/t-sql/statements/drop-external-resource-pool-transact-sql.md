---
description: DROP EXTERNAL RESOURCE POOL (Transact-SQL)
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: dphansen
ms.author: davidph
manager: cgronlund
ms.openlocfilehash: 08f25755ca4742b0e14379c40e22e99741eedbda
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172396"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Удаляет внешний пул ресурсов Resource Governor, используемый для определения ресурсов для внешних процессов. 

::: moniker range="=sql-server-2016||>=sql-server-linux-ver15"
Для [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] внешний пул управляет `rterm.exe`, `BxlServer.exe` и другими сформированными процессами.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Для [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] внешний пул управляет `rterm.exe`, `python.exe`, `BxlServer.exe` и другими сформированными процессами.
::: moniker-end

Внешние пулы ресурсов создаются с помощью [CREATE EXTERNAL RESOURCE POOL (Transact-SQL) ](../../t-sql/statements/create-external-resource-pool-transact-sql.md) и изменяются с помощью [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md).  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*pool_name*  
Имя удаляемого внешнего пула ресурсов.  
  
## <a name="remarks"></a>Remarks

Нельзя удалить пул внешний ресурсов, если он содержит группы рабочей нагрузки.  

Нельзя удалить внутренний пул или пул по умолчанию Resource Governor.  

При выполнении инструкций DDL рекомендуется иметь представление о состояниях регулятора ресурсов. Дополнительные сведения см. в разделе [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) (Регулятор ресурсов).  

## <a name="permissions"></a>Разрешения

Требуется разрешение `CONTROL SERVER`.  

## <a name="examples"></a>Примеры

В следующем примере удаляется внешний пул ресурсов с именем `ex_pool`.  

```sql
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>См. также:

+ [Параметр конфигурации сервера «external scripts enabled»](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)
+ [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)
