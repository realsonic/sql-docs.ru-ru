---
description: CREATE VIEW (Transact-SQL)
title: CREATE VIEW (Transact-SQL)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 781dc13180d75e771402822259ad5ef6a913ac28
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170316"
---
# <a name="create-view-transact-sql"></a>CREATE VIEW (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Создает виртуальную таблицу, содержимое которой (столбцы и строки) определяется запросом. Используйте эту инструкцию для создания представления данных, содержащихся в одной или более таблицах базы данных. Например, представление можно использовать в следующих целях.  
  
-   Для направления, упрощения и настройки восприятия информации в базе данных каждым пользователем.  
  
-   В качестве механизма безопасности, позволяющего пользователям обращаться к данным через представления, но не предоставляя им разрешений на непосредственный доступ к базовым таблицам.  
  
-   Для предоставления интерфейса обратной совместимости, моделирующего таблицу, схема которой изменилась.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE [ OR ALTER ] VIEW [ schema_name . ] view_name [ (column [ ,...n ] ) ]
[ WITH <view_attribute> [ ,...n ] ]
AS select_statement
[ WITH CHECK OPTION ]
[ ; ]  
  
<view_attribute> ::=
{  
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]
}
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

OR ALTER  
 **Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1)).   
  
 Условно изменяет представление только в том случае, если оно уже существует. 
 
 *schema_name*  
 Имя схемы, которой принадлежит представление.  
  
 *view_name*  
 Имя представления. Имена представлений должны соответствовать требованиям, предъявляемым к идентификаторам. Указывать имя владельца представления не обязательно.  
  
 *column*  
 Имя, которое будет иметь столбец в представлении. Имя столбца требуется только в тех случаях, когда столбец формируется на основе арифметического выражения, функции или константы, если два или более столбцов могут по иной причине получить одинаковые имена (как правило, в результате соединения) или если столбцу представления назначается имя, отличное от имени столбца, от которого он произведен. Назначать столбцам имена можно также в инструкции SELECT.  
  
 Если аргумент *column* не указан, столбцам представления назначаются такие же имена, которые имеют столбцы в инструкции SELECT.  
  
> [!NOTE]  
>  В столбцах представления разрешения для имени столбца применяются с инструкцией CREATE VIEW или ALTER VIEW вне зависимости от источника базовых данных. Например, если в инструкции CREATE VIEW были заданы разрешения для столбца **SalesOrderID**, инструкция ALTER VIEW может переименовать столбец **SalesOrderID**, например в **OrderRef**, и все же иметь разрешения, связанные с представлением, в котором используется столбец **SalesOrderID**.  
  
 AS  
 Определяет действия, которые должны быть выполнены в представлении.  
  
 *select_statement*  
 Инструкция SELECT, которая определяет представление. В этой инструкции можно указывать более одной таблицы и другие представления. Для выбора объектов, указанных в предложении SELECT создаваемого представления, необходимы соответствующие разрешения.  
  
 Представление не обязательно является простым подмножеством строк и столбцов одной конкретной таблицы. С помощью предложения SELECT можно создавать представление, использующее более одной таблицы, или другие представления любой степени сложности.  
  
 При использовании в определении индексированного представления инструкция SELECT должна содержать указание одной таблицы или соединять инструкцией JOIN несколько таблиц с необязательной статистической обработкой.  
  
 Предложения SELECT, используемые в определении представления, не могут включать следующие элементы:  
  
-   предложение ORDER BY, если только в списке выбора инструкции SELECT нет также предложения TOP;  
  
    > [!IMPORTANT]  
    >  Предложение ORDER BY используется исключительно для определения строк, возвращаемых предложениями TOP или OFFSET в определении представления. Предложение ORDER BY не гарантирует упорядочивания результатов при запросе к представлению, если оно не указано в самом запросе.  
  
-   ключевое слово INTO;  
  
-   предложение OPTION;  
  
-   ссылку на временную таблицу или табличную переменную.  
  
 Так как аргумент *select_statement* использует инструкцию SELECT, допустимо включать в состав предложения FROM указания \<join_hint> и \<table_hint>. Дополнительные сведения см. в разделе [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md) и [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md). 
  
 В аргументе *select_statement* можно использовать функции и множественные инструкции SELECT, разделенные оператором UNION или UNION ALL.  
  
 CHECK OPTION  
 Обеспечивает соответствие всех выполняемых для представления инструкций, изменяющих данные, критериям, заданным в выражении *select_statement*. Если строка изменяется посредством представления, предложение WITH CHECK OPTION гарантирует, что после фиксации изменений доступ к данным из представления сохранится.  
  
> [!NOTE]
>  CHECK OPTION применяется только к обновлениям, выполненным через представление. Он неприменим к обновлениям, выполненным непосредственно в базовых таблицах представления.  
  
 ENCRYPTION  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Выполняет шифрование элементов представления [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md), содержащих текст инструкции CREATE VIEW. Использование предложения WITH ENCRYPTION предотвращает публикацию представления в рамках репликации SQL Server.  
  
 SCHEMABINDING  
 Привязывает представление к схеме базовой таблицы или таблиц. Если аргумент SCHEMABINDING указан, нельзя изменить базовую таблицу или таблицы таким способом, который может повлиять на определение представления. Сначала нужно изменить или удалить само представление для сброса зависимостей от таблицы, которую требуется изменить. При использовании аргумента SCHEMABINDING инструкция *select_statement* должна включать двухкомпонентные (_schema_ **.** _object_) имена таблиц, представлений или пользовательских функций, упоминаемых в предложении. Все указанные в инструкции объекты должны находиться в одной базе данных.  
  
 Представления или таблицы, входящие в представление, созданное при помощи предложения SCHEMABINDING, не могут быть сброшены, пока это представление не будет удалено или изменено таким образом, чтобы оно более не было привязано к схеме. В противном случае компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдаст ошибку. Кроме того, выполнение инструкций ALTER TABLE для таблиц, которые входят в представления, привязанные к схемам, завершается ошибкой, если эти инструкции влияют на определение представления.  
  
 VIEW_METADATA  
 Указывает, что экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвратит в API-интерфейсы DB-Library, ODBC и OLE DB сведения метаданных о представлении вместо базовой таблицы или таблиц, когда метаданные режима обзора затребованы для запроса, который ссылается на представление. Метаданные режима обзора — это дополнительные метаданные, которые экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает вышеназванным клиентским API-интерфейсам. Эти метаданные позволяют клиентским API-интерфейсам реализовывать обновляемые клиентские курсоры. Метаданные режима обзора содержат сведения о базовой таблице, которой принадлежат столбцы в результирующем наборе.  
  
 Для представлений, созданных с применением предложения VIEW_METADATA, метаданные режима обзора возвращают имя представления, а не имена базовых таблиц при описании столбцов из представления в результирующем наборе.  
  
 В представлении, созданном с предложением WITH VIEW_METADATA, все столбцы, за исключением столбца **timestamp**, поддерживают обновление, если представление включает триггеры INSTEAD OF INSERT или INSTEAD OF UPDATE. Дополнительные сведения об обновляемых представлениях см. в разделе «Примечания».  
  
## <a name="remarks"></a>Remarks  
 Представление может быть создано только в текущей базе данных. Инструкция CREATE VIEW должна быть первой в пакетном запросе. Представление может включать не более 1 024 столбцов.  
  
 При выполнении запросов через представление компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверяет, существуют ли все указанные в инструкции объекты базы данных, верны ли они в контексте инструкции и соответствуют ли инструкции модификации данных правилам обеспечения целостности данных. Если проверка завершается ошибкой, возвращается сообщение об ошибке. При успешной проверке операция преобразуется в операцию над базовой таблицей или таблицами.  
  
 Если представление зависит от удаленной таблицы или представления, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] в ответ на попытку использования представления возвращает сообщение об ошибке. Если создана новая таблица или представление, а структура таблицы не изменилась по сравнению с предыдущей базовой таблицей для замены удаленной, то представление можно использовать снова. Если структура новой таблицы или представления отличается от предыдущей, представление нужно удалить и создать заново.  
  
 Если представление создано без применения предложения SCHEMABINDING, то при изменении базовых объектов, влияющих на определение представления, выполните хранимую процедуру [sp_refreshview](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md). В противном случае результат запроса представления может быть непредвиденным.  
  
 При создании представления сведения о нем сохраняются в следующих представлениях каталога: [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md), [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) и [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md). Текст инструкции CREATE VIEW сохраняется в представлении каталога [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Запрос, в котором используется индекс представления, определенного с выражением типа **numeric** или **float**, может привести к результатам, отличным от результатов подобного запроса, в котором не используется индекс представления. Это отличие может быть обусловлено ошибками округления при выполнении запросов INSERT, DELETE или UPDATE для базовых таблиц.  
  
 При создании представления компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] сохраняет значения SET QUOTED_IDENTIFIER и SET ANSI_NULLS. Эти исходные значения используются для синтаксического анализа данных представления при обращениях к нему. Таким образом, при доступе к представлению какие-либо заданные во время клиентского сеанса значения SET QUOTED_IDENTIFIER и SET ANSI_NULLS не влияют на определение представления.  
  
## <a name="updatable-views"></a>Обновляемые представления  
 Можно изменять данные базовой таблицы через представление до тех пор, пока выполняются следующие условия:  
  
-   Любые изменения, в том числе инструкции UPDATE, INSERT и DELETE, должны ссылаться на столбцы только одной базовой таблицы.  
  
-   Изменяемые в представлении столбцы должны непосредственно ссылаться на данные столбцов базовой таблицы. Столбцы нельзя сформировать каким-либо другим образом, в том числе:  
  
    -   с помощью агрегатной функции: AVG, COUNT, SUM, MIN, MAX, GROUPING, STDEV, STDEVP, VAR и VARP;  
  
    -   на основе вычисления. Столбец нельзя вычислить по выражению, включающему другие столбцы. Столбцы, сформированные при помощи операторов UNION, UNION ALL, CROSSJOIN, EXCEPT и INTERSECT, считаются вычисляемыми и также не являются обновляемыми.  
  
-   Предложения GROUP BY, HAVING и DISTINCT не влияют на изменяемые столбцы.  
  
-   Предложение TOP не используется нигде в инструкции *select_statement* представления вместе с предложением WITH CHECK OPTION.  
  
 Вышеназванные ограничения относятся ко всем подзапросам представления в предложении FROM, равно как и к самому представлению. Как правило, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен иметь возможность однозначно трассировать изменения от определения представления до одной базовой таблицы. Дополнительные сведения см. в разделе [Изменение данных через представление](../../relational-databases/views/modify-data-through-a-view.md).  
  
 Если вышеуказанные ограничения не позволяют изменить данные через представление напрямую, рассмотрите следующие варианты.  
  
-   **Триггеры INSTEAD OF**  
  
     Чтобы сделать представление обновляемым, для него можно создать триггеры INSTEAD OF. Триггер INSTEAD OF выполняется вместо инструкции модификации данных, для которой он определен. Этот триггер позволяет пользователю указать набор действий, которые должны быть выполнены для обработки инструкции модификации данных. Таким образом, если для представления создан триггер INSTEAD OF, связанный с конкретной инструкцией модификации данных (INSERT, UPDATE или DELETE), соответствующее представление можно обновлять при помощи этой инструкции. Дополнительные сведения о триггерах INSTEAD OF см. в разделе [Триггеры DML](../../relational-databases/triggers/dml-triggers.md).  
  
-   **Секционированные представления**  
  
     Секционированное представление является в то же время и обновляемым, но при этом действуют некоторые ограничения. При необходимости компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] проводит различие между локальными и распределенными секционированными представлениями. Первый тип включает представления, которые вместе со всеми соответствующими таблицами относятся к одному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а второй — представления, в которых хотя бы одна из таблиц относится к другому или удаленному серверу.  
  
## <a name="partitioned-views"></a>Секционированные представления  
 Секционированное представление — это представление, определенное посредством объединения всех (UNION ALL) таблиц-элементов, структурированных одинаковым образом, но хранимых отдельно в форме разных таблиц либо в одном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], либо в группе автономных экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые называются федеративными серверами баз данных.  
  
> [!NOTE]  
>  Предпочтительным способом локального секционирования данных на один сервер является применение секционированных таблиц. Дополнительные сведения см. в разделе [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 При разработке схемы секционирования должно быть ясно, какие данные относятся к каждой секции. Например, данные таблицы `Customers` распределяются по трем таблицам на трех серверах: `Customers_33` на сервере `Server1`, `Customers_66` на сервере `Server2` и `Customers_99` на сервере `Server3`.  
  
 Секционированное представление на сервере `Server1` определяется следующим образом.  
  
```sql
--Partitioned view as defined on Server1  
CREATE VIEW Customers  
AS  
--Select from local member table.  
SELECT *  
FROM CompanyData.dbo.Customers_33  
UNION ALL  
--Select from member table on Server2.  
SELECT *  
FROM Server2.CompanyData.dbo.Customers_66  
UNION ALL  
--Select from member table on Server3.  
SELECT *  
FROM Server3.CompanyData.dbo.Customers_99;  
```  
  
 Как правило, представление считают секционированным, если оно соответствует следующему формату:  
  
```syntaxsql
SELECT <select_list1>  
FROM T1  
UNION ALL  
SELECT <select_list2>  
FROM T2  
UNION ALL  
...  
SELECT <select_listn>  
FROM Tn;  
```  
  
## <a name="conditions-for-creating-partitioned-views"></a>Требования к созданию секционированных представлений  
  
1.  `list` выборки.  
  
    -   В списке столбцов определения представления выберите все столбцы таблиц-элементов.  
  
    -   Столбцы, занимающие одну и ту же порядковую позицию в каждом `select list`, должны иметь одинаковый тип, включая параметры сортировки. Типы столбцов не просто должны поддерживать неявное преобразование друг в друга: в отличие от оператора UNION в данном случае этого недостаточно.  
  
         Кроме того, хотя бы один столбец (например, `<col>`) должен входить во все списки выбора в одной и той же порядковой позиции. Определите столбец `<col>` таким образом, чтобы для таблиц-элементов `T1, ..., Tn` в столбце `<col>` были определены ограничения CHECK с идентификаторами `C1, ..., Cn` соответственно.  
  
         Ограничение `C1`, определенное для таблицы `T1`, должно иметь следующий формат:  
  
        ```syntaxsql
        C1 ::= < simple_interval > [ OR < simple_interval > OR ...]  
        < simple_interval > :: =   
        < col > { < | > | \<= | >= | = < value >}   
        | < col > BETWEEN < value1 > AND < value2 >  
        | < col > IN ( value_list )  
        | < col > { > | >= } < value1 > AND  
        < col > { < | <= } < value2 >  
        ```
  
    -   Ограничения должны быть такими, чтобы любое указанное значение `<col>` могло удовлетворять не более чем одному из ограничений `C1, ..., Cn`, т. е. они должны формировать совокупность неперекрывающихся интервалов. Столбец `<col>`, для которого определены неперекрывающиеся ограничения, называется столбцом секционирования. Обратите внимание, что столбец секционирования может иметь другие имена в базовых таблицах. Чтобы ограничения соответствовали вышеуказанным требованиям столбца секционирования, они должны находиться во включенном и доверенном состоянии. Если ограничения отключены, включите проверку ограничений с помощью параметра CHECK CONSTRAINT *constraint_name* инструкции ALTER TABLE и проверьте их с использованием параметра WITH CHECK.  
  
         В следующем фрагменте продемонстрированы правильные наборы ограничений:  
  
        ```syntaxsql
        { [col < 10], [col between 11 and 20] , [col > 20] }  
        { [col between 11 and 20], [col between 21 and 30], [col between 31 and 100] }  
        ```  
  
    -   Один столбец не может быть указан в списке выбора несколько раз.  
  
2.  Столбец секционирования  
  
    -   Столбец секционирования является частью первичного ключа (PRIMARY KEY) таблицы.  
  
    -   Он не может быть вычисляемым столбцом, столбцом идентификаторов, столбцом по умолчанию или столбцом типа **timestamp**.  
  
    -   Если для одного столбца таблицы-элемента определено более одного ограничения, ядро СУБД пропускает все ограничения и не учитывает их при определении того, является ли представление секционированным. Чтобы выполнялись требования к секционированному представлению, со столбцом секционирования должно быть связано только одно ограничение секционирования.  
  
    -   На возможность обновления столбца секционирования никакие ограничения не распространяются.  
  
3.  Таблицы-элементы или базовые таблицы `T1, ..., Tn`.  
  
    -   Эти таблицы могут быть или локальными таблицами, или таблицами с других компьютеров, на которых выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Во втором случае для ссылки на таблицу должно быть использовано или четырехкомпонентное имя, или имя в формате функции OPENDATASOURCE или OPENROWSET. Синтаксис функций OPENDATASOURCE и OPENROWSET позволяет указать имя таблицы, но не передаваемого запроса. Дополнительные сведения см. в разделе [OPENDATASOURCE (Transact-SQL)](../../t-sql/functions/opendatasource-transact-sql.md) и [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md).  
  
         Если хотя бы одна таблица-элемент является удаленной, представление называется распределенным секционированным представлением, и тогда вступают в силу дополнительные требования. Они описаны ниже в данном разделе.  
  
    -   Одна таблица не может быть указана два раза в наборе таблиц, объединяемых при помощи инструкции UNION ALL.  
  
    -   Таблицы-элементы не могут иметь индексы, созданные для вычисляемых столбцов в таблице.  
  
    -   Все ограничения первичного ключа (PRIMARY KEY), действующие в таблицах-элементах, должны быть связаны с одинаковым количеством столбцов.  
  
    -   Всем таблицам-элементам в представлении должно быть назначено одинаковое значение заполнения ANSI. Его можно задать либо при помощи аргумента **user options** процедуры **sp_configure**, либо при помощи инструкции SET.  
  
## <a name="conditions-for-modifying-data-in-partitioned-views"></a>Условия изменения данных в секционированных представлениях  
 На инструкции, изменяющие секционированные представления, распространяются следующие ограничения:  
  
-   В инструкции INSERT должны быть указаны значения для всех столбцов представления, даже если в базовых таблицах-элементах действует ограничение DEFAULT для этих столбцов или они допускают значения NULL. Для тех столбцов таблиц-элементов, которые имеют определения DEFAULT, в инструкциях нельзя явно использовать ключевое слово DEFAULT.  
  
-   Значение, вставляемое в столбец секционирования, должно отвечать по крайней мере одному из базовых ограничений. В противном случае операция вставки завершится ошибкой из-за нарушения ограничений.  
  
-   В предложении SET инструкции UPDATE в качестве значения не может быть указано ключевое слово DEFAULT, даже если столбец имеет значение DEFAULT, определенное в соответствующей таблице-элементе.  
  
-   Столбец представления, который является столбцом идентификаторов в одной или нескольких таблицах-элементах, не может быть изменен при помощи инструкции INSERT или UPDATE.  
  
-   Если одна из таблиц-элементов содержит столбец **timestamp**, представление не может быть изменено при помощи инструкции INSERT или UPDATE.  
  
-   Если одна из таблиц-элементов содержит триггер, ограничение ON UPDATE CASCADE/SET NULL/SET DEFAULT или ограничение ON DELETE CASCADE/SET NULL/SET DEFAULT, то представление не может быть изменено.  
  
-   Выполнение операций INSERT, UPDATE и DELETE для секционированного представления не допускается, если осуществляется самосоединение с тем же представлением или с какой-либо из таблиц-элементов, указанных в инструкции.  
  
-   Массовый импорт данных в секционированное представление не поддерживается **bcp** и инструкциями BULK INSERT и INSERT... SELECT * FROM OPENROWSET(BULK...). Однако можно вставить в секционированное представление несколько строк с помощью инструкции [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
    > [!NOTE]  
    >  Для обновления секционированного представления пользователь должен иметь связанные с таблицами-элементами разрешения INSERT, UPDATE и DELETE.  
  
## <a name="additional-conditions-for-distributed-partitioned-views"></a>Дополнительные требования, предъявляемые к распределенным секционированным представлениям  
 При работе с распределенными секционированными представлениями (если одна или несколько таблиц-элементов являются удаленными) действуют следующие дополнительные требования.  
  
-   Для обеспечения атомарности операций на всех узлах, затрагиваемых операцией обновления, запускается распределенная транзакция.  
  
-   Чтобы инструкция INSERT, UPDATE или DELETE была выполнена успешно, установите параметр XACT_ABORT SET в значение ON.  
  
-   Любые столбцы удаленных таблиц типа **smallmoney**, фигурирующих в секционированном представлении, сопоставляются как тип **money**. Таким образом, соответствующие им столбцы локальных таблиц (столбцы, занимающие те же порядковые позиции в списке выбора) должны также иметь тип **money**.  
  
-   При уровне совместимости базы данных 110 и выше любые столбцы типа **smalldatetime** в удаленных таблицах, фигурирующих в секционированном представлении, сопоставляются как тип **smalldatetime**. Соответствующие им столбцы локальных таблиц (столбцы, занимающие те же порядковые позиции в списке выбора) должны иметь тип **smalldatetime**. В этом отличие от более ранних версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], где столбцы в удаленных таблицах типа **smalldatetime**, фигурирующих в секционированном представлении, сопоставляются как тип **datetime**, а соответствующие столбцы в локальных таблицах должны иметь тип **datetime**. Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
-   Никакой связанный сервер в секционированном представлении не может быть замкнут на себя. Это связанный сервер, указывающий на тот же экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 При выполнении операций INSERT, UPDATE и DELETE, в которых задействованы обновляемые секционированные представления и удаленные таблицы, параметр SET ROWCOUNT не учитывается.  
  
 При наличии таблиц-элементов и определения секционированного представления оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] составляет планы эффективного выполнения запросов для доступа к данным из таблиц-элементов. При наличии определений ограничения CHECK обработчик запросов составляет карту распределения значений ключей по таблицам-элементам. Когда пользователь выполняет запрос, обработчик запросов сравнивает карту со значениями, указанными в предложении WHERE, и создает план выполнения, позволяющий свести к минимуму объем передачи данных между серверами-элементами. Следовательно, несмотря на то, что некоторые таблицы-элементы могут храниться на удаленных серверах, экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешает распределенные запросы таким образом, чтобы объем передаваемых распределенных данных оказался минимальным.  
  
## <a name="considerations-for-replication"></a>Аспекты, связанные с репликацией  
 При создании секционированных представлений для таблиц-элементов, задействованных в репликации, следует учитывать следующие факторы.  
  
-   Если базовые таблицы задействованы в репликации слиянием или репликации транзакций с обновляемыми подписками, в список выбора должен быть включен столбец **uniqueidentifier**. 
  
     При выполнении любых операций INSERT в секционированном представлении необходимо предоставлять значение NEWID() для столбца **uniqueidentifier**. При выполнении любых операций UPDATE для столбца **uniqueidentifier** необходимо предоставлять значение NEWID(), так как ключевое слово DEFAULT использовать при этом нельзя.  
  
-   Репликация обновлений, производимых при помощи представления, выполняется так же, как и при репликации таблиц в разных базах данных: таблицы обслуживаются различными агентами репликации, и определенный порядок выполнения обновлений не гарантируется.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения этой инструкции требуется разрешение CREATE VIEW в отношении базы данных и разрешение ALTER в отношении схемы, в которой создается представление.  
  
## <a name="examples"></a>Примеры  

В следующих примерах используются базы данных AdventureWorks 2012 и AdventureWorksDW.  

### <a name="a-using-a-simple-create-view"></a>A. Использование простого разрешения CREATE VIEW  
 В следующем фрагменте представление создается при помощи простой инструкции `SELECT`. Это полезно, если нужно часто выполнять запросы с сочетанием столбцов. Данные этого представления извлекаются из таблиц `HumanResources.Employee` и `Person.Person` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Эти данные включают имена и фамилии сотрудников [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], а также даты их приема на работу. Такое представление можно было бы создать, например для человека, следящего за профессиональными юбилеями, при этом не предоставляя ему доступ ко всем данным, хранящимся в этих таблицах.  
  
```sql
CREATE VIEW hiredate_view  
AS   
SELECT p.FirstName, p.LastName, e.BusinessEntityID, e.HireDate  
FROM HumanResources.Employee e   
JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
  
```  
  
### <a name="b-using-with-encryption"></a>Б. Использование WITH ENCRYPTION  
 Следующий пример поясняет применение параметра `WITH ENCRYPTION` и обращение к вычисляемым, переименованным и множественным столбцам.  
  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql
CREATE VIEW Purchasing.PurchaseOrderReject  
WITH ENCRYPTION  
AS  
SELECT PurchaseOrderID, ReceivedQty, RejectedQty,   
    RejectedQty / ReceivedQty AS RejectRatio, DueDate  
FROM Purchasing.PurchaseOrderDetail  
WHERE RejectedQty / ReceivedQty > 0  
AND DueDate > CONVERT(DATETIME,'20010630',101) ;  
GO  
  
```  
  
### <a name="c-using-with-check-option"></a>В. Использование WITH CHECK OPTION  
 В следующем примере создается представление `SeattleOnly`, ссылающееся на пять таблиц и допускающее изменение данных только тех сотрудников, которые живут в Сиэтле.  
  
```sql
CREATE VIEW dbo.SeattleOnly  
AS  
SELECT p.LastName, p.FirstName, e.JobTitle, a.City, sp.StateProvinceCode  
FROM HumanResources.Employee e  
INNER JOIN Person.Person p  
ON p.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN Person.BusinessEntityAddress bea   
    ON bea.BusinessEntityID = e.BusinessEntityID   
    INNER JOIN Person.Address a   
    ON a.AddressID = bea.AddressID  
    INNER JOIN Person.StateProvince sp   
    ON sp.StateProvinceID = a.StateProvinceID  
WHERE a.City = 'Seattle'  
WITH CHECK OPTION ;  
GO  
```  
  
### <a name="d-using-built-in-functions-within-a-view"></a>Г. Использование встроенных функций в представлении  
 В следующем фрагменте показано определение представления, включающее встроенную функцию. Применяя функцию, следует указывать имя производного столбца.  
  
```sql
CREATE VIEW Sales.SalesPersonPerform  
AS  
SELECT TOP (100) SalesPersonID, SUM(TotalDue) AS TotalSales  
FROM Sales.SalesOrderHeader  
WHERE OrderDate > CONVERT(DATETIME,'20001231',101)  
GROUP BY SalesPersonID;  
GO  

```  
  
### <a name="e-using-partitioned-data"></a>Д. Использование секционированных данных  
 В следующем примере используются таблицы `SUPPLY1`, `SUPPLY2`, `SUPPLY3` и `SUPPLY4`. Они соответствуют таблицам поставщиков из четырех офисов, расположенных в разных странах и регионах.  
  
```sql
--Create the tables and insert the values.  
CREATE TABLE dbo.SUPPLY1 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 1 and 150),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY2 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 151 and 300),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY3 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 301 and 450),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY4 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 451 and 600),  
supplier CHAR(50)  
);  
GO  
--Create the view that combines all supplier tables.  
CREATE VIEW dbo.all_supplier_view  
WITH SCHEMABINDING  
AS  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY1  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY2  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY3  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY4;  
GO
INSERT dbo.all_supplier_view VALUES ('1', 'CaliforniaCorp'), ('5', 'BraziliaLtd')    
, ('231', 'FarEast'), ('280', 'NZ')  
, ('321', 'EuroGroup'), ('442', 'UKArchip')  
, ('475', 'India'), ('521', 'Afrique');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Примеры: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-creating-a-simple-view"></a>Е. Создание простого представления  
 В следующем примере создается представление путем выбора некоторых столбцов из исходной таблицы.  
  
```sql
CREATE VIEW DimEmployeeBirthDates AS  
SELECT FirstName, LastName, BirthDate   
FROM DimEmployee;  
```  
  
### <a name="g-create-a-view-by-joining-two-tables"></a>Ж. Создание представления путем соединения двух таблиц  
 В следующем примере представление создается при помощи инструкции `SELECT` с `OUTER JOIN`. Результаты запроса на соединение заполняют представление.  
  
```sql
CREATE VIEW view1  
AS 
SELECT fis.CustomerKey, fis.ProductKey, fis.OrderDateKey, 
  fis.SalesTerritoryKey, dst.SalesTerritoryRegion  
FROM FactInternetSales AS fis   
LEFT OUTER JOIN DimSalesTerritory AS dst   
ON (fis.SalesTerritoryKey=dst.SalesTerritoryKey);  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW (Transact-SQL)](../../t-sql/statements/alter-view-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [Создание хранимой процедуры](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_refreshview (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sys.views (Transact-SQL)](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

