---
description: FILE_IDEX (Transact-SQL)
title: FILE_IDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILE_IDEX
- FILE_IDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILE_IDEX function
- IDs [SQL Server], files
- file IDs [SQL Server]
- names [SQL Server], files
- identification numbers [SQL Server], files
- file names [SQL Server], FILE_IDEX
ms.assetid: 7532fea5-ee5e-4edd-b98b-111a7ba56c8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0f7739a422e82e496c772454bb596980497adef3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092304"
---
# <a name="file_idex-transact-sql"></a>FILE_IDEX (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Эта функция возвращает номер идентификатора файла для указанного логического имени файла данных, файла журнала, или полнотекстового файла в текущей базе данных. 
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
FILE_IDEX ( file_name )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *file_name*  
Выражение типа **sysname**, представляющее имя файла, для которого необходимо вернуть значение идентификатора файла (FILE_IDEX). 
  
## <a name="return-types"></a>Типы возвращаемых данных  
**int**  
  
**NULL** в случае ошибки  
  
## <a name="remarks"></a>Комментарии  
*file_name* соответствует логическому имени файла, отображенному в столбце **name** в представлении каталога [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) или [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).  
  
Используйте `FILE_IDEX` в списке SELECT, предложении WHERE или любом другом объекте с поддержкой выражений. Дополнительные сведения см. в разделе [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-retrieving-the-file-id-of-a-specified-file"></a>A. Получение идентификатора для указанного файла  
Этот пример возвращает идентификатор файла `AdventureWorks_Data`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_IDEX('AdventureWorks2012_Data') AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
1  
(1 row(s) affected)  
```  
  
### <a name="b-retrieving-the-file-id-when-the-file-name-is-not-known"></a>Б. Получение идентификатора файла, имя которого неизвестно  
Этот пример возвращает идентификатор файла журнала `AdventureWorks`. Фрагмент кода Transact-SQL (T-SQL) выбирает логическое имя файла из представления каталога `sys.database_files`, где тип файла — `1` (журнал).  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_IDEX((SELECT TOP (1) name FROM sys.database_files WHERE type = 1)) AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
2  
```  
  
### <a name="c-retrieving-the-file-id-of-a-full-text-catalog-file"></a>В. Получение идентификатора файла полнотекстового файла каталога  
Этот пример возвращает идентификатор полнотекстового файла. Фрагмент кода T-SQL выбирает логическое имя файла из представления каталога `sys.database_files`, где тип файла — `4` (полнотекстовый). Этот код возвращает значение NULL, если полнотекстовый каталог не существует.
  
```sql  
SELECT FILE_IDEX((SELECT name FROM sys.master_files WHERE type = 4))  
AS 'File_ID';  
```  
  
## <a name="see-also"></a>См. также  
 [Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
