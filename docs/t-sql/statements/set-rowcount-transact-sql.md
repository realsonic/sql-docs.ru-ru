---
description: SET ROWCOUNT (Transact-SQL)
title: SET ROWCOUNT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_ROWCOUNT_TSQL
- ROWCOUNT_TSQL
- SET ROWCOUNT
- ROWCOUNT
dev_langs:
- TSQL
helpviewer_keywords:
- row return limitations [SQL Server]
- SET ROWCOUNT statement
- number of rows affected by statement
- ROWCOUNT option
- counting rows
- stopping queries
- limiting rows returned
- queries [SQL Server], stopping
ms.assetid: c6966fb7-6421-47ef-98f3-82351f2f6bdc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fca568e68a289943cc49324b45991120f0578eb2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091930"
---
# <a name="set-rowcount-transact-sql"></a>SET ROWCOUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Приводит к завершению обработки запроса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] после возвращения указанного количества строк.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SET ROWCOUNT { number | @number_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *number* | @*number_var*  
 Количество строк, выраженное целым числом, которое необходимо обработать, перед завершением запроса.  
  
## <a name="remarks"></a>Remarks  
  
> [!IMPORTANT]  
>  Использование инструкции SET ROWCOUNT не будет оказывать влияния на инструкции DELETE, INSERT и UPDATE в последующей версии SQL Server. При программировании избегайте использования инструкции SET ROWCOUNT с инструкциями DELETE, INSERT и UPDATE и постарайтесь внести изменения в приложения, которые используют ее в настоящее время. Для аналогичного поведения используйте синтаксис TOP. Дополнительные сведения см. в разделе [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md).  
  
 Для выключения этого параметра и возвращения всех строк, укажите SET ROWCOUNT 0.  
  
 Установка параметра ROWCOUNT приводит к тому, что большинство инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)] прекращают обработку, если на них влияет указанное число строк. Это включает триггеры. Параметр ROWCOUNT не влияет на динамические курсоры, но ограничивает набор строк набора ключей и нечувствительных курсоров. Пользоваться этим параметром следует осторожно.  
  
 Инструкция SET ROWCOUNT переопределяет ключевое слово TOP инструкции SELECT, если параметр ROWCOUNT имеет меньшее значение.  
  
 Значение параметра ROWCOUNT устанавливается во время выполнения, а не во время синтаксического анализа.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
 Инструкция SET ROWCOUNT завершает обработку после указанного числа строк. В следующем примере обратите внимание на то, что более 500 строк удовлетворяют условию: значение столбца `Quantity` меньше `300`. Однако после применения SET ROWCOUNT возвращаются не все строки.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT count(*) AS Count  
FROM Production.ProductInventory  
WHERE Quantity < 300;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Count 
 ----------- 
 537 
 
 (1 row(s) affected)
 ```  
  
 Теперь сделайте `ROWCOUNT` равным `4` и верните все строки, чтобы показать, что возвращается только 4 строки.  
  
```sql
SET ROWCOUNT 4;  
SELECT *  
FROM Production.ProductInventory  
WHERE Quantity < 300;  
GO  
  
-- (4 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Инструкция SET ROWCOUNT завершает обработку после указанного числа строк. Обратите внимание, что в следующем примере больше 20 строк удовлетворяют условию `AccountType = 'Assets'`. Однако после применения SET ROWCOUNT возвращаются не все строки.  
  
```sql
-- Uses AdventureWorks  
  
SET ROWCOUNT 5;  
SELECT * FROM [dbo].[DimAccount]  
WHERE AccountType = 'Assets';  
```  
  
 Чтобы вернуть все строки, установите для параметра ROWCOUNT значение 0.  
  
```sql
-- Uses AdventureWorks  
  
SET ROWCOUNT 0;  
SELECT * FROM [dbo].[DimAccount]  
WHERE AccountType = 'Assets';  
```  
  
## <a name="see-also"></a>См. также  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

