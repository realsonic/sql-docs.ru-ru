---
title: sp_data_source_table_columns
description: sp_data_source_table_columns (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 30205befcdd9da78f4f9a3f002ddd398f49abc55
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005389"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Возвращает список столбцов во внешней таблице источника данных.
  
> [!NOTE]
> Эта процедура введена в [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>Аргументы

`[ @data_source = ] 'data_source'`   
Имя внешнего источника данных, из которого необходимо получить метаданные. Тип — `sysname` .

`[ @table_location = ] 'table_location'`   
Строка расположения таблицы, определяющая таблицу. `table_location` Тип — `nvarchar(max)` .

## <a name="returns"></a>Возвращаемое значение

Хранимая процедура возвращает следующие сведения.

|Имя столбца |Тип данных |Описание|
|---|---|---|
|NAME|nvarchar(max)|Имя столбца.
|TYPE|nvarchar(200)|Имя типа SQL Server
|LENGTH|INT|Длина столбца
|PRECISION|INT|Точность столбца
|SCALE|INT|Масштаб столбца
|COLLATION|nvarchar(200)|SQL Server параметров сортировки столбца
|IS_NULLABLE|bit|Это столбец, допускающий значение null.
|SOURCE_TYPE_NAME|nvarchar(max)|Имя типа, зависящего от внутреннего сервера. В основном используется для отладки. Для источников ODBC это будет соответствовать столбцу результатов TYPE_NAME SQLColumns ().
|ПРИМЕЧАНИЯ|nvarchar(max)|Общие комментарии или описание столбца. В настоящее время всегда имеет значение NULL.|

## <a name="permissions"></a>Разрешения  

Требуется разрешение ALTER ANY EXTERNAL DATA SOURCE.
  
## <a name="remarks"></a>Комментарии  

На экземпляре SQL Server должен быть установлен компонент  [polybase](../../relational-databases/polybase/polybase-guide.md) .

Эта хранимая процедура поддерживает соединители для:

- SQL Server
- Oracle;
- Teradata
- MongoDB
- Cosmos DB

Хранимая процедура не поддерживает универсальные соединители источника ODBC dta.

Понятие пустое и непустое относится к поведению драйвера ODBC и [ `SQLTables` функции](../native-client-odbc-api/sqltables.md). Не пусто указывает, что объект содержит таблицы, а не строки. Например, пустая схема не содержит таблиц в SQL Server. Пустая база данных содержит без таблиц в Teradata. результаты представляют собой SQL Server представление серверной схемы, интерпретируемое соединителем Polybase для серверной части. Отличие заключается в том, что вместо просто передачи результатов вызова ODBC в серверную часть результаты основываются на результатах кода сопоставления типов Polybase.

Используйте [`sp_data_source_objects`](sp-data-source-objects.md) и `sp_data_source_table_columns` для обнаружения внешних объектов. Эти системные хранимые процедуры возвращают схему таблиц, которые доступны для виртуализации. Azure Data Studio использует эти две хранимые процедуры для поддержки [виртуализации данных](../../azure-data-studio/extensions/data-virtualization-extension.md). Используйте `sp_data_source_table_columns` для обнаружения схем внешней таблицы, представленных в SQL Server типах данных.

Из-за различий между параметрами сортировки в исходных данных Hadoop и поддерживаемыми параметрами сортировки в SQL Server 2019, рекомендуемая длина типа данных для столбцов типа данных varchar в внешних таблицах может быть значительно больше, чем ожидалось. Это сделано намеренно.

## <a name="example"></a>Пример  

В следующем примере возвращаются столбцы таблицы для внешней таблицы в SQL Server с именем `server` , принадлежащей схеме с именем `schema` .
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>См. также раздел

- [Приступая к работе с PolyBase](../polybase/polybase-guide.md)
- [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)