---
description: ALTER VIEW (Transact-SQL)
title: ALTER VIEW (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
ms.assetid: 03eba220-13e2-49e3-bd9d-ea9df84dc28c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 76a16ec32341e54c66b7ba72edc50f4d9944b900
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098491"
---
# <a name="alter-view-transact-sql"></a>ALTER VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет ранее созданное представление, в том числе индексированное. Инструкция ALTER VIEW не влияет на зависимые хранимые процедуры или триггеры и не изменяет разрешения.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ]   
[ WITH <view_attribute> [ ,...n ] ]   
AS select_statement   
[ WITH CHECK OPTION ] [ ; ]  
  
<view_attribute> ::=   
{   
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]       
}   
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
``` 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы, которой принадлежит представление.  
  
 *view_name*  
 Представление, которое нужно изменить.  
  
 *column*  
 Имя столбца или разделенные запятыми имена нескольких столбцов, входящих в состав указанного представления.  
  
> [!IMPORTANT]  
>  Заданные для столбца разрешения сохраняются только в том случае, если после выполнения инструкции ALTER VIEW имя столбца остается таким же, каким оно было до этого.  
  
> [!NOTE]  
>  В столбцах представления разрешения для имени столбца применяются с инструкцией CREATE VIEW или ALTER VIEW вне зависимости от источника базовых данных. Например, если в инструкции CREATE VIEW были предоставлены разрешения для столбца **SalesOrderID**, инструкция ALTER VIEW может переименовать этот столбец, например в **OrderRef**, и все же иметь разрешения, связанные с представлением, в котором используется столбец **SalesOrderID**.  
  
 ENCRYPTION  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Если этот аргумент указан, выполняется шифрование элементов представления [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md), содержащего текст инструкции ALTER VIEW. Предложение WITH ENCRYPTION предотвращает публикацию представления при репликации SQL Server.  
  
 SCHEMABINDING  
 Привязывает представление к схеме базовой таблицы или таблиц. Если аргумент SCHEMABINDING указан, изменить базовые таблицы таким образом, чтобы это повлияло на определение представления, нельзя. Для удаления зависимостей из таблицы, которую требуется изменить, вначале нужно изменить или удалить само представление. При использовании аргумента SCHEMABINDING инструкция _select\_statement_ должна включать двухкомпонентные (_schema_ **.** _object_) имена таблиц, представлений или пользовательских функций, упоминаемых в предложении. Все указанные в инструкции объекты должны находиться в одной базе данных.  
  
 Представления или таблицы, входящие в представление, созданное при помощи предложения SCHEMABINDING, не могут быть удалены, пока это представление не будет удалено или изменено так, что больше не будет привязано к схеме. В противном случае компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдаст ошибку. Кроме того, выполнение инструкций ALTER TABLE для таблиц, входящих в представления, связанные со схемами, завершается неудачей, если эти инструкции влияют на определение представления.  
  
 VIEW_METADATA  
 Указывает, что экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвратит в API-интерфейсы DB-Library, ODBC и OLE DB сведения метаданных о представлении вместо базовой таблицы или таблиц, когда метаданные режима обзора затребованы для запроса, который ссылается на представление. Метаданные режима просмотра — это дополнительные метаданные, которые экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] возвращает клиентским API-интерфейсам DB-Library, ODBC и OLE DB. Эти метаданные позволяют клиентским API-интерфейсам реализовывать обновляемые клиентские курсоры. Метаданные режима обзора содержат сведения о базовой таблице, которой принадлежат столбцы в результирующем наборе.  
  
 Для представлений, созданных с применением предложения VIEW_METADATA, метаданные режима обзора возвращают имя представления, а не имена базовых таблиц при описании столбцов из представления в результирующем наборе.  
  
 В представлении, созданном с предложением WITH VIEW_METADATA, все столбцы, за исключением столбца с типом данных **timestamp**, поддерживают обновление, если представление включает триггеры INSERT или UPDATE INSTEAD OF. Дополнительные сведения см. в подразделе "Замечания" раздела [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
 AS  
 Действия, которые должны быть выполнены в представлении.  
  
 *select_statement*  
 Инструкция SELECT, которая определяет представление.  
  
 WITH CHECK OPTION  
 Обеспечивает соответствие всех выполняемых для представления инструкций, изменяющих данные, критериям, заданным в выражении *select_statement*.  
  
## <a name="remarks"></a>Комментарии  
 Дополнительные сведения об ALTER VIEW см. в подразделе "Замечания" раздела [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
> [!NOTE]  
>  Если предыдущее определение представления было создано с использованием предложения WITH ENCRYPTION или CHECK OPTION, эти параметры будут действовать только в том случае, если они включены в инструкцию ALTER VIEW.  
  
 При изменении текущего представления с помощью инструкции ALTER VIEW компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] осуществляет монопольную блокировку схемы представления. Если блокировка предоставлена, и с представлением в настоящий момент не работает ни один пользователь, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] удаляет все копии представления из кэша процедур. Существующие планы, ссылающиеся на представление, остаются в кэше, но при обращении к ним выполняется повторная компиляция.  
  
 Инструкцию ALTER VIEW можно выполнять для индексированных представлений, однако она удаляет все индексы представления.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения инструкции ALTER VIEW необходимо как минимум разрешение ALTER на объект.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается представление `EmployeeHireDate`, содержащее фамилии и имена всех сотрудников, а также даты их приема на работу. Представлению предоставляются необходимые разрешения, но требования изменяются таким образом, чтобы из базы данных извлекались сведения только о сотрудниках, принятых на работу до определенной даты. Для замены представления используется инструкция `ALTER VIEW`.  
  
```sql 
USE AdventureWorks2012 ;  
GO  
CREATE VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
```  
  
 Представление нужно изменить так, чтобы оно включало только сотрудников, принятых на работу до `2002` года. Если не использовать инструкцию ALTER VIEW, а удалить представление и создать его заново, нужно будет повторно выполнить инструкцию GRANT и любые другие инструкции, работающие с разрешениями данного представления.  
  
```sql  
ALTER VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID  
WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [Создание хранимой процедуры](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Внесение изменений в схемы баз данных публикации](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
