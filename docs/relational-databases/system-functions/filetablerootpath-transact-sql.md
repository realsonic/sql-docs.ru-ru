---
description: FileTableRootPath (Transact-SQL)
title: FileTableRootPath (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- FileTableRootPath_TSQL
- FileTableRootPath
dev_langs:
- TSQL
helpviewer_keywords:
- FileTableRootPath function
ms.assetid: 0cba908a-c85c-4b09-b16a-df1cb333c629
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f08273adcf261ecfdafaa70793d50b31360f7e40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093878"
---
# <a name="filetablerootpath-transact-sql"></a>FileTableRootPath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает корневой путь UNC для конкретной таблицы FileTable или для текущей базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
FileTableRootPath ( [ '[schema_name.]FileTable_name' ], @option )  
```  
  
## <a name="arguments"></a>Аргументы  
 *FileTable_name*  
 Имя таблицы FileTable. *FileTable_name* имеет тип **nvarchar**. Этот параметр является необязательным. Значением по умолчанию является текущая база данных. Указывать *schema_name* также необязательно. Для *FileTable_name* можно передать значение null, чтобы использовать значение параметра по умолчанию.  
  
 *\@функцию*  
 Целочисленное выражение, определяющее способ форматирования серверных компонентов пути. *\@ параметр* может иметь одно из следующих значений:  
  
|Значение|Описание|  
|-----------|-----------------|  
|**0**|Возвращает имя сервера, преобразованное в формат NetBIOS, например<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Это значение по умолчанию.|  
|**1**|Возвращает имя сервера без преобразования, например:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Возвращает полный путь сервера, например:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Возвращаемый тип  
 **nvarchar(4000)**  
  
 Когда база данных принадлежит к Always On группе доступности, функция **FileTableRootPath** возвращает имя виртуальной сети (VNN) вместо имени компьютера.  
  
## <a name="general-remarks"></a>Общие замечания  
 Функция **FileTableRootPath** возвращает значение null, если выполняется одно из следующих условий.  
  
-   Недопустимое значение *FileTable_name* .  
  
-   Вызывающая сторона не имеет достаточных разрешений для ссылки на указанную таблицу или текущую базу данных.  
  
-   Параметр FILESTREAM *database_directory* не задан для текущей базы данных.  
  
 Дополнительные сведения см. в статье [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="best-practices"></a>Рекомендации  
 Чтобы код и приложения были независимы от текущего компьютера и базы данных, следует избегать создания кода с использованием абсолютных путей. Вместо этого получите полный путь к файлу во время выполнения, используя функции **FileTableRootPath** и **GetFileNamespacePath** вместе, как показано в следующем примере. По умолчанию функция **GetFileNamespacePath** возвращает относительный путь к файлу, находящемуся внутри корневого пути к базе данных.  
  
```sql  
USE MyDocumentDatabase;  
  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
@fullPath = varchar(1000);  
  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath()  
FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Функция **FileTableRootPath** требует:  
  
-   разрешение SELECT на FileTable, чтобы получить корневой путь конкретной таблицы FileTable;  
  
-   **db_datareader** или более высокого разрешения на получение корневого пути для текущей базы данных.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано, как вызвать функцию **FileTableRootPath** .  
  
```  
USE MyDocumentDatabase;  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase"  
SELECT FileTableRootPath();  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'dbo.MyFileTable');  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'MyFileTable');  
```  
  
## <a name="see-also"></a>См. также:  
 [Работа с каталогами и путями в таблицах FileTable](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
