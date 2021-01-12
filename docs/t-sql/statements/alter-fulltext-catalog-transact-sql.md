---
description: ALTER FULLTEXT CATALOG (Transact-SQL)
title: ALTER FULLTEXT CATALOG (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_FULLEXT_CATALOG_TSQL
- ALTER FULLEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- modifying full-text catalogs
- full-text catalogs [SQL Server], rebuilding
- accent sensitivity
- ALTER FULLTEXT CATALOG statement
- full-text catalogs [SQL Server], modifying
- full-text catalogs [SQL Server], reorganizing
ms.assetid: 31a47aaf-6c7f-48a4-a86a-d57aec66c9cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a694ce465b53f74e3139a017548e7bfecd52b88c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083036"
---
# <a name="alter-fulltext-catalog-transact-sql"></a>ALTER FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Изменяет свойства полнотекстового каталога.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql 
ALTER FULLTEXT CATALOG catalog_name   
{ REBUILD [ WITH ACCENT_SENSITIVITY = { ON | OFF } ]  
| REORGANIZE  
| AS DEFAULT   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *catalog_name*  
 Задает имя изменяемого каталога. Если каталог с заданным именем не существует, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку и не выполняет операцию ALTER.  
  
 REBUILD  
 Указывает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перестроить весь каталог. Во время перестроения каталога существующий каталог удаляется, а на его месте создается новый каталог. Все таблицы, содержащие ссылки полнотекстового индексирования, сопоставляются с новым каталогом. Перестроение сбрасывает полнотекстовые метаданные в системных таблицах базы данных.  
  
 WITH ACCENT_SENSITIVITY = {ON|OFF}  
 Указывает, будет ли изменяемый каталог учитывать диакритические знаки при полнотекстовом индексировании и выполнении запросов.  
  
 Для определения текущего значения учета диакритических знаков для полнотекстового каталога используйте функцию FULLTEXTCATALOGPROPERTY со значением свойства **accentsensitivity** применительно к *catalog_name*. Если функция возвращает «1», полнотекстовый каталог учитывает диакритические знаки; если функция возвращает «0», каталог не учитывает диакритические знаки.  
  
 По умолчанию учет диакритических знаков у каталога и у базы данных одинаков.  
  
 REORGANIZE  
 Указывает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начать *слияние в единый файл*, в ходе которого небольшие индексы, созданные в процессе индексирования, объединяются в один большой индекс. Слияние фрагментов полнотекстового индекса может повысить производительность и освободить ресурсы дисков и памяти. Если в полнотекстовом каталоге происходят частые изменения, следует периодически использовать эту команду для реорганизации каталога.  
  
 REORGANIZE также оптимизирует внутренние структуры индексов и каталогов.  
  
 Помните, что в зависимости от объема проиндексированных данных основное слияние может занять некоторое время. При слиянии больших объемов данных в единый файл может создаваться транзакция с большим временем выполнения, в результате чего усечение журнала транзакций по достижении контрольной точки будет отложено. В этом случае размер журнала транзакций в модели полного восстановления может значительно увеличиться. Перед реорганизацией большого полнотекстового индекса в базе данных, использующей модель полного восстановления, убедитесь, что в журнале транзакций достаточно свободного места для транзакций с большим временем выполнения. Дополнительные сведения см. в разделе [Управление размером файла журнала транзакций](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
 AS DEFAULT  
 Указывает, что этот каталог является каталогом по умолчанию. При создании полнотекстовых индексов без указания конкретного каталога используется каталог по умолчанию. Если существует полнотекстовый каталог по умолчанию, то указание этого каталога AS DEFAULT переопределит существующее значение по умолчанию.  
  
## <a name="permissions"></a>Разрешения  
 Пользователь должен иметь разрешение ALTER для полнотекстового каталога или быть членом предопределенных ролей базы данных **db_owner**, **db_ddladmin** или роли сервера sysadmin.  
  
> [!NOTE]  
>  Для использования ALTER FULLTEXT CATALOG AS DEFAULT пользователь должен иметь разрешение ALTER для полнотекстового каталога и разрешение CREATE FULLTEXT CATALOG для базы данных.  
  
## <a name="examples"></a>Примеры  
 Следующий пример изменяет свойство `accentsensitivity` полнотекстового каталога по умолчанию `ftCatalog`, который учитывает диакритические знаки.  
  
```sql  
--Change to accent insensitive  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog   
REBUILD WITH ACCENT_SENSITIVITY=OFF;  
GO  
-- Check Accentsensitivity  
SELECT FULLTEXTCATALOGPROPERTY('ftCatalog', 'accentsensitivity');  
GO  
--Returned 0, which means the catalog is not accent sensitive.  
```  
  
## <a name="see-also"></a>См. также  
 [sys.fulltext_catalogs (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Полнотекстовый поиск](../../relational-databases/search/full-text-search.md)  
  
  
