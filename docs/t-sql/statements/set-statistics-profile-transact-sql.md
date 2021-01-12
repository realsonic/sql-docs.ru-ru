---
description: SET STATISTICS PROFILE (Transact-SQL)
title: SET STATISTICS PROFILE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PROFILE
- SET_STATISTICS_PROFILE_TSQL
- PROFILE_TSQL
- SET STATISTICS PROFILE
dev_langs:
- TSQL
helpviewer_keywords:
- profiles [SQL Server], displaying
- statements [SQL Server], profile information
- SET STATISTICS PROFILE statement
- STATISTICS PROFILE option
- statistical information [SQL Server], profiles
ms.assetid: c635e262-35fa-421a-aa6f-a1c30f351647
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 87b6bf68704f65319faebd2c0cdf947329b6118d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079767"
---
# <a name="set-statistics-profile-transact-sql"></a>SET STATISTICS PROFILE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Выводит сведения о профиле для инструкции. Параметр STATISTICS PROFILE предназначен для нерегламентированных запросов, представлений и хранимых процедур.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET STATISTICS PROFILE { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Если параметру STATISTICS PROFILE присвоено значение ON, каждый исполняемый запрос возвращает обычный результирующий набор, за которым следует дополнительный результирующий набор, отображающий профиль выполнения запроса.  
  
 Дополнительный результирующий набор содержит столбцы SHOWPLAN_ALL для запроса, а также следующие дополнительные столбцы.  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|**Строки**|Фактическое количество строк, созданных каждым оператором.|  
|**Executes**|Количество выполнений каждого оператора.|  
  
## <a name="permissions"></a>Разрешения  
 Для запуска инструкции SET STATISTICS PROFILE и просмотра данных пользователи должны иметь следующие разрешения:  
  
-   соответствующие разрешения на выполнение инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   разрешение SHOWPLAN на все базы данных, содержащие объекты, на которые ссылаются инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Для инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], которые не выводят результирующие наборы STATISTICS PROFILE, требуются только соответствующие разрешения на выполнение инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Для инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], которые выводят результирующие наборы STATISTICS PROFILE, должны успешно выполняться проверки как разрешения для выполнения инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], так и разрешения SHOWPLAN. Иначе выполнение инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] прекращается и сведения Showplan не формируются.  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL (Transact-SQL)](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME (Transact-SQL)](../../t-sql/statements/set-statistics-time-transact-sql.md)   
 [SET STATISTICS IO (Transact-SQL)](../../t-sql/statements/set-statistics-io-transact-sql.md)  
  
  
