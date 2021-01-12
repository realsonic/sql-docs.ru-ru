---
description: CREATE FULLTEXT CATALOG (Transact-SQL)
title: CREATE FULLTEXT CATALOG (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CATALOG_TSQL
- CREATE_FULLTEXT_TSQL
- FULLTEXT_TSQL
- FULLTEXT CATALOG
- CREATE FULLTEXT CATALOG
- CREATE_FULLTEXT_CATALOG_TSQL
- CATALOG
- FULLTEXT_CATALOG_TSQL
- CREATE FULLTEXT
- FULLTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- CREATE FULLTEXT CATALOG statement
ms.assetid: d7a8bd93-e2d7-4a40-82ef-39069e65523b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4d3520d2a9d8c1c90507bb52742bd888eb1925cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095845"
---
# <a name="create-fulltext-catalog-transact-sql"></a>CREATE FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает полнотекстовый каталог в базе данных. Один полнотекстовый каталог может содержать несколько полнотекстовых индексов, но любой полнотекстовый индекс может быть частью только одного полнотекстового каталога. Каждая база данных может содержать ноль или более полнотекстовых каталогов.  
  
 Нельзя создать полнотекстовые каталоги в базах данных **master**, **model** и **tempdb**.  
  
> [!IMPORTANT]  
>  Начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], полнотекстовый каталог является виртуальным объектом и не входит в файловую группу. Полнотекстовый каталог — это логическое понятие, обозначающее группу полнотекстовых индексов.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
CREATE FULLTEXT CATALOG catalog_name  
     [ON FILEGROUP filegroup ]  
     [IN PATH 'rootpath']  
     [WITH <catalog_option>]  
     [AS DEFAULT]  
     [AUTHORIZATION owner_name ]  
  
<catalog_option>::=  
     ACCENT_SENSITIVITY = {ON|OFF}  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *catalog_name*  
 Имя нового каталога. Имя каталога должно быть уникальным среди других имен каталогов в текущей базе данных. Также имя файла, который соответствует полнотекстовому каталогу (см. ON FILEGROUP), должно быть уникальным среди всех файлов базы данных. Если имя каталога уже используется для другого каталога в базе данных, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку.  
  
 Длина имени каталога не может превышать 120 символов.  
  
 ON FILEGROUP *filegroup*  
 Начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], это предложение не оказывает влияние на работу системы.  
  
 IN PATH **'** _rootpath_ **'**  
 > [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], это предложение не оказывает влияние на работу системы.  
  
 ACCENT_SENSITIVITY = {ON|OFF}  
 Указывает, будет ли каталог учитывать диакритические знаки для полнотекстового индексирования. Если это свойство меняется, индекс перестраивается. По умолчанию в параметрах сортировки базы данных установлено учитывать диакритические знаки. Чтобы увидеть параметры сортировки базы данных, используйте представление каталога **sys.databases**.  
  
 Для определения текущего значения учета диакритических знаков для полнотекстового каталога используйте функцию FULLTEXTCATALOGPROPERTY со значением свойства **accentsensitivity** применительно к *catalog_name*. Если возвращенное значение равно 1, полнотекстовый каталог учитывает диакритические знаки; если значение равно 0, каталог не учитывает диакритические знаки.  
  
 AS DEFAULT  
 Указывает, что каталог является каталогом по умолчанию. При создании полнотекстовых индексов без явного указания полнотекстового каталога используется каталог по умолчанию. Если существующий полнотекстовый каталог уже помечен как AS DEFAULT, установка этого нового каталога AS DEFAULT сделает этот каталог полнотекстовым каталогом по умолчанию.  
  
 AUTHORIZATION *owner_name*  
 Устанавливает владельцем полнотекстового каталога пользователя или роль базы данных. Если аргумент *owner_name* является ролью, это должно быть имя роли, членом которой является текущий пользователь, либо пользователь, выполняющий инструкцию, должен быть владельцем базы данных или системным администратором.  
  
 Если аргумент *owner_name* является именем пользователя, имя пользователя должно быть одним из следующих:  
  
-   имя пользователя, выполняющего инструкцию;  
  
-   имя пользователя, для которого пользователь, выполняющий инструкцию, имеет разрешение на IMPERSONATE;  
  
-   или пользователь, выполняющий команду, должен быть владельцем базы данных или системным администратором.  
  
 Пользователю *owner_name* должно быть предоставлено разрешение TAKE OWNERSHIP для указанного полнотекстового каталога.  
  
## <a name="remarks"></a>Комментарии  
 Идентификаторы полнотекстовых каталогов начинаются с 00005 и увеличиваются на единицу для каждого вновь создаваемого каталога.  
  
## <a name="permissions"></a>Разрешения  
 Пользователь должен иметь разрешение CREATE FULLTEXT CATALOG для базы данных или быть членом предопределенной роли базы данных **db_owner** или **db_ddladmin**.  
  
## <a name="examples"></a>Примеры  
 Следующий пример создает полнотекстовый каталог, а также полнотекстовый индекс.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume) KEY INDEX PK_JobCandidate_JobCandidateID;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [sys.fulltext_catalogs (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Полнотекстовый поиск](../../relational-databases/search/full-text-search.md)   
 
  
  
