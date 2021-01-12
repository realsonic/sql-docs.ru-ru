---
description: ALTER EXTERNAL DATA SOURCE (Transact-SQL)
title: ALTER EXTERNAL DATA SOURCE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER EXTERNAL DATA SOURCE
- ALTER_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- polybase, alter external data source statement
- ALTER EXTERNAL DATA SOURCE statement
ms.assetid: a34b9e90-199d-46d0-817a-a7e69387bf5f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c168be7ae24c23c24920e3d5a1d844cb84a2bb2c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082939"
---
# <a name="alter-external-data-source-transact-sql"></a>ALTER EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Изменяет внешний источник данных, используемый для создания внешней таблицы. Внешний источник данных может быть хранилищем Hadoop, или хранилищем BLOB-объектов Azure (WASBS) для SQL Server и хранилищем BLOB-объектов Azure (WASBS), или хранилищем Azure Data Lake (ABFSS/ADL) для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. 

## <a name="syntax"></a>Синтаксис  

```syntaxsql
-- Modify an external data source
-- Applies to: SQL Server (2016 or later) and APS
ALTER EXTERNAL DATA SOURCE data_source_name SET
    {   
        LOCATION = '<prefix>://<path>[:<port>]' [,] |
        RESOURCE_MANAGER_LOCATION = <'IP address;Port'> [,] |
        CREDENTIAL = credential_name
    }  
    [;]  

-- Modify an external data source pointing to Azure Blob storage
-- Applies to: SQL Server (starting with 2017)
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        LOCATION = 'https://storage_account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ] 

-- Modify an external data source pointing to Azure Blob storage or Azure Data Lake storage
-- Applies to: Azure Synapse Analytics
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        [LOCATION = '<location prefix>://<location path>']
        [, CREDENTIAL = credential_name ] 
```

## <a name="arguments"></a>Аргументы  
 data_source_name указывает определяемое пользователем имя источника данных. Имя должно быть уникальным.

 LOCATION = '<prefix>://<path>[:<port>]'. Предоставляет протокол, путь и порт для подключения к внешнему источнику данных. Допустимые параметры расположения см. в разделе [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](create-external-data-source-transact-sql.md#location--prefixpathport).

 RESOURCE_MANAGER_LOCATION = '\<IP address;Port>' (не применяется к [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]) — указывает расположение диспетчера ресурсов Hadoop. Если аргумент указан, оптимизатор запросов может выбрать предварительную обработку данных для запросов PolyBase с помощью вычислительных мощностей Hadoop. Это решение, принимаемое на основе стоимости. Оно называется передачей предиката и может значительно сократить объем данных, передаваемых между Hadoop и SQL, повышая производительность запросов.

 CREDENTIAL = Credential_Name Указывает именованные учетные данные. См. раздел [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

TYPE = [HADOOP | BLOB_STORAGE]   
**Область применения**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
Только для массовых операций — `LOCATION` должен быть допустимым URL-адресом хранилища больших двоичных объектов Azure. Не помещайте **/** , имя файла или параметры подписи общего доступа в конце URL-адреса `LOCATION`.
Используемые учетные данные необходимо создавать, используя `SHARED ACCESS SIGNATURE` в качестве удостоверения. Дополнительные сведения о подписанных URL-адресах см. в статье [Использование подписанных URL-адресов](/azure/storage/storage-dotnet-shared-access-signature-part-1).

  

## <a name="remarks"></a>Remarks
 Одновременно можно изменить только один источник. Параллельные запросы на изменение того же источника приводят к помещению одной инструкции в режим ожидания. Однако одновременно можно изменять разные источники. Эта инструкция может выполняться параллельно с другими инструкциями.

## <a name="permissions"></a>Разрешения  
 Требуется разрешение ALTER ANY EXTERNAL DATA SOURCE.
 > [!IMPORTANT]  
 > Разрешение ALTER ANY EXTERNAL DATA SOURCE предоставляет любому субъекту возможность создания и изменения объекта внешнего источника данных и, таким образом, также предоставляет возможность доступа ко всем учетным данным уровня базы данных в базе данных. Это разрешение следует рассматривать как высоко привилегированное, поэтому его следует предоставлять только доверенным субъектам в системе.


## <a name="examples"></a>Примеры  
 В следующем примере изменяется расположение и расположение диспетчера ресурсов существующего источника данных.

```sql  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
     LOCATION = 'hdfs://10.10.10.10:8020',
     RESOURCE_MANAGER_LOCATION = '10.10.10.10:8032'
    ;
```

 В следующем примере изменяются учетные данные для подключения к источнику данных.

```sql 
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
   CREDENTIAL = new_hadoop_user
    ;
```
 В следующем примере учетные данные изменяются на новое значение LOCATION. Этот пример представляет собой внешний источник данных, созданный для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. 

```sql  
ALTER EXTERNAL DATA SOURCE AzureStorage_west SET
   LOCATION = 'wasbs://loadingdemodataset@updatedproductioncontainer.blob.core.windows.net',
   CREDENTIAL = AzureStorageCredential
```