---
description: Создание индексов с включенными столбцами
title: Создание индексов с включенными столбцами | Документация Майкрософт
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index size [SQL Server]
- index keys [SQL Server]
- index columns [SQL Server]
- size [SQL Server], indexes
- key columns [SQL Server]
- included columns
- nonclustered indexes [SQL Server], included columns
- designing indexes [SQL Server], included columns
- nonkey columns
ms.assetid: d198648d-fea5-416d-9f30-f9d4aebbf4ec
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d972f88744b6c8b108b1f52a54d4bf8507975017
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171156"
---
# <a name="create-indexes-with-included-columns"></a>Создание индексов с включенными столбцами
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  В этой теме описывается добавление невключенных или неключевых столбцов, чтобы расширить функциональные возможности некластеризованных индексов в [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] при помощи среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Добавление неключевых столбцов позволяет создавать некластеризованные индексы, покрывающие больше запросов. Это обусловлено следующими преимуществами неключевых столбцов.  
  
-   Они могут содержать типы данных, не разрешенные для ключевых столбцов индекса.  
-   Они не учитываются компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] при расчете числа ключевых столбцов индекса и размера ключа индекса.  
  
 Индекс с неключевыми столбцами может значительно повысить производительность запроса, когда все столбцы запроса включены в индекс как ключевые или неключевые. Производительность повышается благодаря тому, что оптимизатор запросов может найти все значения столбцов в этом индексе; при этом нет обращения к данным таблиц или кластеризованных индексов, что приводит к меньшему количеству дисковых операций ввода-вывода.  
  
> [!NOTE]  
> Если индекс содержит все столбцы, на которых в запросе имеются ссылки, это обычно называется *покрытием запроса*.  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="design-recommendations"></a><a name="DesignRecs"></a> Рекомендации по проектированию  
  
-   Переопределите некластеризованные индексы с большим размером ключа индекса, чтобы только столбцы, используемые для поиска и уточняющего запроса, были ключевыми. Все остальные столбцы, покрывающие запрос, сделайте неключевыми столбцами. Таким образом, в наличии будут все столбцы, покрывающие запрос, но сам ключ индекса будет небольшим и эффективным.  
  
-   Включите неключевые столбцы в некластеризованный индекс, чтобы не превышать существующие ограничения на размер индекса (32 столбца) и размер ключа индекса (1 700 байт). До версии [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] использовались ограничения в 16 столбцов и 900 байт. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не учитывает неключевые столбцы при расчете количества ключевых столбцов индекса и размера ключа индекса.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Неключевые столбцы можно задавать только для некластеризованных индексов.  
  
-   Все типы данных, за исключением **text**, **ntext**, и **image** могут использоваться как неключевые столбцы.  
  
-   Вычисляемые столбцы, являющиеся детерминированными, в том числе точными или неточными, могут быть неключевыми столбцами. Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md).  
  
-   Вычисляемые столбцы, полученные на основе типов данных **image**, **ntext** и **text** , могут быть неключевыми столбцами, если тип данных этого вычисляемого столбца допустим в качестве неключевого индексного столбца.  
  
-   Неключевые столбцы можно удалить из таблицы только после удаления из этой таблицы индекса.  
  
-   Неключевые столбцы нельзя изменять, за исключением следующих операций:  
  
    -   изменение поведения столбца в отношении значения NULL с NOT NULL на NULL;  
  
    -   увеличение длины столбцов типов **varchar**, **nvarchar** и **varbinary** .  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER для таблицы или представления. Пользователь должен быть членом предопределенной роли сервера **sysadmin** или предопределенных ролей базы данных **db_ddladmin** и **db_owner**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>Создание индекса с неключевыми столбцами  
  
1.  В обозревателе объектов щелкните знак «плюс», чтобы развернуть базу данных, содержащую таблицу, в которой необходимо создать индекс с неключевыми столбцами.  
  
2.  Чтобы развернуть папку **Таблицы** , щелкните значок «плюс».  
  
3.  Щелкните знак «плюс», чтобы развернуть таблицу, в которой необходимо создать индекс с неключевыми столбцами.  
  
4.  Щелкните правой кнопкой мыши папку **Индексы**, выберите **Создать индекс** и **Некластеризованный индекс...**  
  
5.  В диалоговом окне **Создание индекса** на странице **Общие** введите имя нового индекса в поле **Имя индекса** .  
  
6.  На вкладке **Ключевые столбцы индекса** нажмите кнопку **Добавить…** .  
  
7.  В диалоговом окне **Выбор столбцов из**_имя\_таблицы_ установите флажки для столбцов таблицы, добавляемых к индексу.  
  
8.  Нажмите кнопку **ОК**.  
  
9. На вкладке **Включенные столбцы** нажмите кнопку **Добавить…** .  
  
10. В диалоговом окне **Выбор столбцов из**_имя\_таблицы_ установите флажки для столбцов таблицы, добавляемых в индекс в качестве неключевых столбцов.  
  
11. Нажмите кнопку **ОК**.  
  
12. В диалоговом окне **Создание индекса** нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>Создание индекса с неключевыми столбцами  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Creates a nonclustered index on the Person.Address table with four included (nonkey) columns.   
    -- index key column is PostalCode and the nonkey columns are  
    -- AddressLine1, AddressLine2, City, and StateProvinceID.  
    CREATE NONCLUSTERED INDEX IX_Address_PostalCode  
    ON Person.Address (PostalCode)  
    INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
    GO  
    ```  

## <a name="related-content"></a>См. также  
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)    
[Руководство по проектированию индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
