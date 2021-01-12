---
description: SET SHOWPLAN_ALL (Transact-SQL)
title: SET SHOWPLAN_ALL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET SHOWPLAN_ALL
- SET_SHOWPLAN_ALL_TSQL
- SHOWPLAN_ALL
- SHOWPLAN_ALL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_ALL statement
- SHOWPLAN_ALL option
- canceling statement execution
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: a500b682-bae4-470f-9e00-47de905b851b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6a8cef73df3a6ced1cac91c997848cf28b966042
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093334"
---
# <a name="set-showplan_all-transact-sql"></a>SET SHOWPLAN_ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Приводит к тому, что Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выполняет инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Вместо этого [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает подробные сведения о выполнении инструкций и предоставляет оценку требований к ресурсам для выполнения этих инструкций.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET SHOWPLAN_ALL { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Задание параметра инструкции SET SHOWPLAN_ALL происходит во время выполнения или запуска инструкций, а не во время синтаксического анализа.  
  
 Если выполнена команда `SET SHOWPLAN_ALL`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сведения о выполнении каждой инструкции, не выполняя ее, и инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] не выполняются. Если параметр принял значение ON, данные обо всех последующих инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] возвращаются до тех пор, пока параметру не будет присвоено значение OFF. Например, если инструкция CREATE TABLE была выполнена, когда `SET SHOWPLAN_ALL` была включена, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет возвращать сообщение об ошибке для всех последующих инструкций SELECT, в которых используется данная таблица, уведомляя пользователей о том, что она не существует. Следовательно, последующие ссылки на эту таблицу не действуют. Когда этому параметру присвоено значение OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет инструкции, не формируя отчеты.  
  
 Инструкция `SET SHOWPLAN_ALL` предназначена для использования приложениями, которые могут обрабатывать ее вывод. Используйте инструкцию SET SHOWPLAN_TEXT для возврата доступного для чтения вывода для приложений командной строки Microsoft Win32, таких как служебная программа **osql**.  
  
 Инструкции SET SHOWPLAN_TEXT и SET SHOWPLAN_ALL не могут использоваться внутри хранимой процедуры; они должны быть единственными инструкциями в пакете.  
  
 Инструкция SET SHOWPLAN_ALL возвращает информацию в виде набора строк, формирующих дерево шагов, которые обработчик запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] совершает для выполнения каждой инструкции. Каждой инструкции, отраженной в выходных данных, соответствует одна строка с текстом инструкции, за которой следуют несколько строк с подробными описаниями шагов выполнения. В таблице ниже приведены столбцы, содержащиеся в выводе.  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|**StmtText**|В строках, не относящихся к типу PLAN_ROW, этот столбец содержит текст инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. В строках типа PLAN_ROW этот столбец содержит описание операции. Этот столбец содержит физический оператор и может также, при необходимости, содержать логический оператор. За этим столбцом может идти описание, зависящее от физического оператора. Дополнительные сведения см. в разделе [Справочник по логическим и физическим операторам Showplan](../../relational-databases/showplan-logical-and-physical-operators-reference.md).|  
|**StmtId**|Номер инструкции в текущем пакете.|  
|**NodeId**|Идентификатор узла в текущем запросе.|  
|**Parent**|Идентификатор узла родительского шага.|  
|**PhysicalOp**|Физическая реализация алгоритма для узла. Только для строк типа PLAN_ROWS.|  
|**LogicalOp**|Оператор реляционной алгебры, которому соответствует данный узел. Только для строк типа PLAN_ROWS.|  
|**Argument**|Предоставляет дополнительные сведения о выполняемой операции. Содержимое этого столбца зависит от физического оператора.|  
|**DefinedValues**|Содержит список значений с разделителями-запятыми, вводимых данным оператором. Эти значения могут представлять собой вычисляемые выражения, присутствующие в текущем запросе (например в списке SELECT или предложении WHERE), или внутренние значения, введенные обработчиком запросов для выполнения данного запроса. Затем на эти определенные значения можно ссылаться из других частей запроса. Только для строк типа PLAN_ROWS.|  
|**EstimateRows**|Предполагаемое количество строк вывода от данного оператора. Только для строк типа PLAN_ROWS.|  
|**EstimateIO**|Предполагаемые затраты на ввод-вывод* для данного оператора. Только для строк типа PLAN_ROWS.|  
|**EstimateCPU**|Предполагаемая загрузка ЦП* для данного оператора. Только для строк типа PLAN_ROWS.|  
|**AvgRowSize**|Предполагаемый средний размер строк (в байтах), передаваемых через данный оператор.|  
|**TotalSubtreeCost**|Предполагаемая (совокупная) стоимость* данной операции и всех дочерних операций.|  
|**OutputList**|Содержит список столбцов с разделителями-запятыми, проецируемых текущей операцией.|  
|**:::no-loc text="Warnings":::**|Содержит список предупреждений с разделителями-запятыми, относящихся к текущей операции. Предупреждения могут содержать строку «NO STATS:()» со списком столбцов. Это означает, что оптимизатор запросов попытался принять решение на основе статистики для данного столбца, но она отсутствовала. Поэтому оптимизатору запросов пришлось принимать решение самостоятельно, что могло привести к выбору неэффективного плана запроса. Дополнительные сведения о создании или обновлении статистик столбцов (которые помогают оптимизатору запросов выбирать более эффективный план запроса) см. в разделе [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md). В этом столбце также может содержаться строка «MISSING JOIN PREDICATE», означающая, что производится соединение (с использованием таблиц) без предиката соединения. Случайное удаление предиката соединения может привести к созданию запроса, чье выполнение займет значительно больше времени, чем предполагалось, и вернет большой результирующий набор. При появлении этого предупреждения убедитесь, что предикат соединения не был удален случайно.|  
|**:::no-loc text="Type":::**|Тип узла. Для родительского узла каждого запроса это тип инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] (например SELECT, INSERT, EXECUTE и т. д.). Для дочерних узлов, соответствующих планам выполнения, это тип PLAN_ROW.|  
|**Parallel**|**0** = оператор выполняется не в параллельном режиме.<br /><br /> **1** = оператор выполняется в параллельном режиме.|  
|**EstimateExecutions**|Предполагаемое количество вызовов данного оператора при выполнении текущего запроса.|  
|||

 *Значения показателей основаны на внутреннем измерении времени, а не на показаниях настенных часов. Они используются для определения относительной стоимости плана в сравнении с другими планами.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы использовать инструкцию SET SHOWPLAN_ALL, должны быть необходимые разрешения на выполнение инструкций, для которых запускается инструкция SET SHOWPLAN_ALL, и разрешение SHOWPLAN на доступ ко всем базам данных, содержащим используемые в запросах объекты.  
  
 Чтобы инструкции SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* и EXEC *user_defined_function* создавали планы SHOWPLAN, пользователь должен:  
  
-   обладать необходимыми разрешениями на выполнение инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   обладать разрешением SHOWPLAN для всех баз данных, содержащих объекты (такие как таблицы, представления и т. д.), на которые ссылаются инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Для всех остальных инструкций (например, DDL, USE *database_name*, SET, DECLARE, динамического SQL и т. д.) требуются лишь соответствующие разрешения на выполнение инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Примеры  
 Две следующие инструкции используют параметры SET SHOWPLAN_ALL, чтобы продемонстрировать, как происходят анализ и оптимизация использования индексов в запросах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 В предложении WHERE первого запроса оператор сравнения «равно» (=) применяется к индексированному столбцу. При этом в столбец **LogicalOp** записывается значение "Поиск в кластеризованном индексе", а в столбец **Argument** — имя индекса.  
  
 Во втором запросе в предложении WHERE используется оператор LIKE. Это приводит к использованию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] просмотра кластеризованного индекса и поиску данных, удовлетворяющих условию в предложении WHERE. В столбец **LogicalOp** записывается значение "Просмотр кластеризованного индекса", имя индекса появляется в столбце **Argument**, также в столбец **LogicalOp** записывается значение "Фильтр", а в соответствующем столбце **Argument** появляется условие предложения WHERE.  
  
 Значения в столбцах **EstimateRows** и **TotalSubtreeCost** меньше для первого запроса с использованием индекса. Это означает, что он обрабатывается гораздо быстрее и требует меньших ресурсов, чем запрос без использования индекса.  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_ALL ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, EmergencyContactID   
FROM HumanResources.Employee  
WHERE EmergencyContactID LIKE '1%';  
GO  
SET SHOWPLAN_ALL OFF;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_TEXT (Transact-SQL)](../../t-sql/statements/set-showplan-text-transact-sql.md)   
 [SET SHOWPLAN_XML (Transact-SQL)](../../t-sql/statements/set-showplan-xml-transact-sql.md)  
  
  
