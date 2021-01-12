---
description: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
title: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT
- SET_QUERY_GOVERNOR_COST_LIMIT_TSQL
- QUERY_GOVERNOR_COST_LIMIT
- QUERY_GOVERNOR_COST_LIMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT statement
- connections [SQL Server], overriding
- QUERY_GOVERNOR_COST_LIMIT option
- overriding connection values
ms.assetid: 3424bb44-6915-462d-a8d7-fe834af81387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fb2a2c1dda36b6e2378c6197e1e87cc9190a535b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100744"
---
# <a name="set-query_governor_cost_limit-transact-sql"></a>SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Переопределяет текущее значение параметра **query governor cost limit** для текущего соединения.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET QUERY_GOVERNOR_COST_LIMIT value  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *value*  
 Значение типа numeric или integer, указывающее максимально возможное время выполнения запроса. Значения округляются в меньшую сторону до ближайшего целого числа. Отрицательные значения округляются до 0. Если задать значение больше нуля, регулятор запросов запрещает выполнение всех запросов, оценочная стоимость которых превышает это значение. Если указать значение 0 (значение по умолчанию), регулятор запросов будет отключен, что разрешает выполнение всех запросов без ограничения времени.  
  
 Цена запроса — это предполагаемое время в секундах, которое требуется для завершения запроса в конкретной конфигурации оборудования.  
  
## <a name="remarks"></a>Примечания  
 Использование инструкции SET QUERY_GOVERNOR_COST_LIMIT относится только к текущему соединению и продолжается в течение текущего соединения. С помощью [параметра конфигурации сервера configure the query governor cost](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) хранимой процедуры **sp_configure** можно изменить предельное значение стоимости, используемое регулятором запросов в рамках всего сервера. Дополнительные сведения о настройке этого параметра см. в разделах [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) и [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 Значение параметра SET QUERY_GOVERNOR_COST_LIMIT устанавливается во время выполнения или запуска, но не во время синтаксического анализа.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="see-also"></a>См. также  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
