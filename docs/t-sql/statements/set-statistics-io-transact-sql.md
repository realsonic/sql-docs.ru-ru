---
description: SET STATISTICS IO (Transact-SQL)
title: SET STATISTICS IO (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_IO_TSQL
- IO
- IO_TSQL
- SET STATISTICS IO
dev_langs:
- TSQL
helpviewer_keywords:
- disk I/O statistics [SQL Server]
- I/O [SQL Server], disk activity information
- disks [SQL Server], statement statistics
- STATISTICS IO option
- statements [SQL Server], statistical information
- SET STATISTICS IO statement
- statistical information [SQL Server], disk activity
ms.assetid: 7033aac9-a944-4156-9ff4-6ef65717a28b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7b1f3fac5ab29e44dd3150cba5882cd1dfc4e271
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079754"
---
# <a name="set-statistics-io-transact-sql"></a>SET STATISTICS IO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Позволяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отображать сведения об активности диска, связанной с выполнением инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET STATISTICS IO { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Если параметру STATISTICS IO задано значение ON, отображаются статистические сведения, а если OFF, то они не отображаются.   
  
 Если этот параметр имеет значение ON, будут возвращаться статистические сведения обо всех инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)], пока параметру не будет задано значение OFF.  
  
 В следующей таблице перечислены и описаны элементы вывода.  
  
|Элемент вывода|Значение|  
|-----------------|-------------|  
|**Таблица**|Имя таблицы.|  
|**Число просмотров**|Количество операций поиска или сканирования, запущенных после достижения конечного уровня в любом направлении с целью получения всех значений при построении окончательного набора данных для вывода.<br /><br /> Число просмотров равно 0, если используется уникальный или кластеризованный индекс первичного ключа и происходит поиск только одного значения. Например, `WHERE Primary_Key_Column = <value>`.<br /><br /> Число просмотров равно 1, если при поиске одного значения используется неуникальный кластеризованный индекс, который определен в столбце непервичного ключа. Это позволяет проводить проверку на наличие повторяющихся значений ключа, который вы ищете. Например, `WHERE Clustered_Index_Key_Column = <value>`.<br /><br /> Число просмотров равно N, где N — количество различных операций поиска или сканирования, начатых по левую или правую сторону от конечного уровня после обнаружения значения ключа с помощью ключа индекса.|  
|**логические операции чтения**|Число страниц, считанных из кэша данных.|  
|**физические операции чтения**|Число страниц, считанных с диска.|  
|**операции упреждающего чтения**|Число страниц, помещенных в кэш для запроса.|  
|**lob логических чтений**|Число страниц, считанных из кэша данных. Включает **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** или страницы индекса columnstore.|  
|**физические операции чтения lob**|Число страниц, считанных с диска. Включает **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** или страницы индекса columnstore.|  
|**lob упреждающих чтений**|Число страниц, помещенных в кэш для запроса. Включает **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** или страницы индекса columnstore.|

 Параметр настройки SET STATISTICS IO устанавливается во время запуска или выполнения, но не во время синтаксического анализа.

> [!NOTE]  
> При получении столбцов больших объектов (LOB) инструкциями языка Transact-SQL для некоторых операций получения может потребоваться многократный обход дерева LOB. При этом инструкция SET STATISTICS IO может выдавать значения логических считываний выше ожидаемых.

## <a name="permissions"></a>Разрешения  
 Для использования инструкции SET STATISTICS IO пользователи должны обладать соответствующими разрешениями на выполнение инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Разрешение SHOWPLAN не требуется.  
  
## <a name="examples"></a>Примеры  
 В данном примере отображается число логических и физических операций чтения, выполняемых сервером [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по мере обработки инструкций.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS IO ON;  
GO  
SELECT *   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS IO OFF;  
GO  
```  
  
 Результирующий набор:  
  
```  
Table 'ProductCostHistory'. Scan count 1, logical reads 5, physical   
reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0,   
lob read-ahead reads 0.  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL (Transact-SQL)](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME (Transact-SQL)](../../t-sql/statements/set-statistics-time-transact-sql.md)  
  
  
