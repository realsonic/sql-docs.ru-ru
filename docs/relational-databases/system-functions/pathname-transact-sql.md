---
description: PathName (Transact-SQL)
title: PathName (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- PathName_TSQL
- PathName
dev_langs:
- TSQL
helpviewer_keywords:
- PathName FILESTREAM [SQL Server]
ms.assetid: 6b95ad90-6c82-4a23-9294-a2adb74934a3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 85058a3d551a385e4d2de8aed2ea9f56dc3b19c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093915"
---
# <a name="pathname-transact-sql"></a>PathName (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает путь в виде большого двоичного объекта (BLOB) FILESTREAM. API OpenSqlFilestream использует этот путь для возврата маркера, который приложение может использовать для работы с данными большого двоичного объекта с помощью API-интерфейсов Win32. Функция PathName доступна только для чтения.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
column_name.PathName ( @option [ , use_replica_computer_name ] )  
```  
  
## <a name="arguments"></a>Аргументы  
 *column_name*  
 Имя столбца FILESTREAM типа **varbinary (max)** . *column_name* должно быть именем столбца. Он не может быть выражением или результатом инструкции CAST или CONVERT.  
  
 Запрос пути к столбцу любого другого типа данных или для колумнсат **varbinary (max)** не имеет атрибута хранилища FILESTREAM, что вызовет ошибку во время компиляции запроса.  
  
 *\@функцию*  
 Целочисленное [выражение](../../t-sql/language-elements/expressions-transact-sql.md) , определяющее способ форматирования серверного компонента пути. *\@ параметр* может принимать одно из следующих значений. Значение по умолчанию — 0.  
  
|Значение|Описание|  
|-----------|-----------------|  
|0|Возвращает имя сервера, преобразованное в формат BIOS, например: `\\SERVERNAME\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
|1|Возвращает имя сервера без преобразования, например: `\\ServerName\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F1`|  
|2|Возвращает полный путь к серверу, например: `\\ServerName.MyDomain.com\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
  
 *use_replica_computer_name*  
 Битовое значение, определяющее, как имя сервера должно возвращаться в группе доступности Always On.  
  
 Если база данных не принадлежит группе доступности Always On, значение этого аргумента игнорируется. В пути всегда используется имя компьютера.  
  
 Если база данных принадлежит к Always On группе доступности, то в выходных данных функции **PathName** значение *use_replica_computer_name* имеет следующий результат:  
  
|Значение|Описание|  
|-----------|-----------------|  
|Не указано.|Функция возвращает в пути имя виртуальной сети.|  
|0|Функция возвращает в пути имя виртуальной сети.|  
|1|Функция возвращает в пути имя компьютера.|  
  
## <a name="return-type"></a>Возвращаемый тип  
 **nvarchar(max)**  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращаемое значение является полным логическим путем или путем NETBIOS объекта BLOB. PathName не возвращает IP-адрес. Возвращается значение NULL, если объект FILESTREAM BLOB не создан.  
  
## <a name="remarks"></a>Комментарии  
 Столбец ROWGUID должен быть виден любому запросу, который вызывает функцию PathName.  
  
 Объект FILESTREAM BLOB можно создать только с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-reading-the-path-for-a-filestream-blob"></a>A. Считывание пути для объекта FILESTREAM BLOB  
 В следующем примере значение функции `PathName` присваивается переменной типа `nvarchar(max)`.  
  
```sql  
DECLARE @PathName nvarchar(max);  
SET @PathName = (  
    SELECT TOP 1 photo.PathName()  
    FROM dbo.Customer  
    WHERE LastName = 'CustomerName'  
    );  
```  
  
### <a name="b-displaying-the-paths-for-filestream-blobs-in-a-table"></a>Б. Отображение путей объектов FILESTREAM BLOB в таблице  
 В следующем примере создаются и отображаются пути трех объектов FILESTREAM BLOB.  
  
```sql  
-- Create a FILESTREAM-enabled database.  
-- The c:\data directory must exist.  
CREATE DATABASE PathNameDB  
ON  
PRIMARY ( NAME = ArchX1,  
    FILENAME = 'c:\data\archdatP1.mdf'),  
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = ArchX3,  
    FILENAME = 'c:\data\filestreamP1')  
LOG ON  ( NAME = ArchlogX1,  
    FILENAME = 'c:\data\archlogP1.ldf');  
GO  
  
USE PathNameDB;  
GO  
  
-- Create a table, add some records, and  
-- create the associated FILESTREAM  
-- BLOB files.  
  
CREATE TABLE TABLE1  
    (  
        ID int,  
        RowGuidColumn UNIQUEIDENTIFIER  
                      NOT NULL UNIQUE ROWGUIDCOL,  
        FILESTREAMColumn varbinary(MAX) FILESTREAM  
    );  
GO  
  
INSERT INTO TABLE1 VALUES  
 (1, NEWID(), 0x00)  
,(2, NEWID(), 0x00)  
,(3, NEWID(), 0x00);  
GO  
  
SELECT FILESTREAMColumn.PathName() AS 'PathName' FROM TABLE1;  
  
--Results  
--PathName  
------------------------------------------------------------------------------------------------------------  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\DD67C792-916E-4A76-8C8A-4A85DC5DB908  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\2907122B-2560-4CB9-86DC-FBE7ABA1843B  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\922BE0E0-CAB9-4403-90BF-945BD258E4BC  
--  
--(3 row(s) affected)  
GO  
  
--Drop the database to clean up.  
USE master;  
GO  
DROP DATABASE PathNameDB;  
```  
  
## <a name="see-also"></a>См. также:  
 [Большой двоичный объект &#40;BLOB-объект& #41;Данные&#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [GET_FILESTREAM_TRANSACTION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)   
 [Доступ к данным FILESTREAM с OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
  
