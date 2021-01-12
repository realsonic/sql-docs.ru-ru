---
description: sys.fn_helpcollations (Transact-SQL)
title: sys.fn_helpcollations (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_helpcollations
- fn_helpcollations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_helpcollations function
- collations [SQL Server], supported
- fn_helpcollations function
ms.assetid: b5082e81-1fee-4e2c-b567-5412eaee41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016|| = azure-sqldw-latest ||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8185f4977f784a0b2590d6a5fdf4a210dd2cd328
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101437"
---
# <a name="sysfn_helpcollations-transact-sql"></a>sys.fn_helpcollations (Transact-SQL)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает список всех поддерживаемых параметров сортировки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```
fn_helpcollations ()  
```  
  
## <a name="tables-returned"></a>Возвращаемые таблицы

 **fn_helpcollations** возвращает следующие сведения.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|Имя|**sysname**|Имя стандартных параметров сортировки|  
|Описание|**nvarchar (1000)**|Описание параметров сортировки|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает параметры сортировки Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также поддерживает ограниченное число (<80) параметров сортировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые были разработаны до [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддержки параметров сортировки Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметры сортировки по-прежнему поддерживаются для обеспечения обратной совместимости, но не должны использоваться для новых задач разработки. Дополнительные сведения о параметрах сортировки Windows см. в статье [Имя параметров сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md). Дополнительные сведения о параметрах сортировки см. в разделе [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Примеры

 Следующий код возвращает имена всех параметров сортировки с двоичной сортировкой, начинающихся с буквы `L`.

> [!Note]
> Запросы Azure синапсе Analytics к fn_helpcollations () должны выполняться в базе данных master.  
  
```sql  
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name like 'L%' AND Description LIKE '% binary sort';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name                   Description  
 -------------------    ------------------------------------  
 Lao_100_BIN            Lao-100, binary sort  
 Latin1_General_BIN     Latin1-General, binary sort  
 Latin1_General_100_BIN Latin1-General-100, binary sort  
 Latvian_BIN            Latvian, binary sort  
 Latvian_100_BIN        Latvian-100, binary sort  
 Lithuanian_BIN         Lithuanian, binary sort  
 Lithuanian_100_BIN     Lithuanian-100, binary sort  
  
 (7 row(s) affected)  
 ```
  
## <a name="see-also"></a>См. также:

[COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)   
[COLLATIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/collation-functions-collationproperty-transact-sql.md)  
[Поддержка параметров сортировки базы данных для Azure синапсе Analytics](https://azure.microsoft.com/blog/database-collation-support-for-azure-sql-data-warehouse-2)  
