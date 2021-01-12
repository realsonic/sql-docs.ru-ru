---
description: Справочник по Transact-SQL (компонент Database Engine)
title: Справочник по Transact-SQL (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
ms.assetid: dbba47d7-e08e-4435-b876-35dced1f325d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00d01b9b0646848ceb8b1deff2ab7cbe16518d0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095854"
---
# <a name="transact-sql-reference-database-engine"></a>Справочник по Transact-SQL (компонент Database Engine)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

В этом разделе приводится общее описание поиска и использования разделов справки по Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)] (T-SQL). T-SQL является ключом к использованию продуктов и служб Microsoft SQL. Все средства и приложения, которые взаимодействуют с базами данных SQL, делают это путем отправки команд T-SQL.  

## <a name="t-sql-compliance-to-sql-standard"></a>Соответствие T-SQL стандарту SQL
Подробные технические документы о реализации определенных стандартов в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] см. в [документации по поддержке стандартов Microsoft SQL Server](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Средства, использующие T-SQL
Ниже приведены лишь некоторые средства Майкрософт, использующие команды T-SQL.

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
  
## <a name="locate-the-transact-sql-reference-topics"></a>Поиск разделов справочника по Transact-SQL  
Чтобы найти разделы T-SQL, воспользуйтесь полем поиска в верхней правой части или оглавлением в левой части этой страницы. Можно также ввести ключевое слово T-SQL в окне редактора запросов среды Management Studio и нажать клавишу F1. 
  
## <a name="find-system-views"></a>Поиск системных представлений
Чтобы найти системные таблицы, представления, функции и процедуры, см. ссылки в разделе [Использование реляционных баз данных](../relational-databases/databases/databases.md) в документации по SQL.

- [Представления системного каталога](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Представления совместимости системы](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Системные динамические административные представления](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Системные функции](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Системные представления информационной схемы](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Системные хранимые процедуры](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Системные таблицы](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>Ссылки "Относится к"  
 Справочные разделы T-SQL могут охватывать несколько версий SQL Server, начиная с 2008 года, а также другие службы SQL Azure. В верхней части каждой статьи приведен раздел, указывающий, какие продукты и службы поддерживают описанную в ней функциональность. 

Например, текущий раздел применим ко всем версиям и имеет следующую метку. 
  
 [!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]   

Еще один пример: следующая метка указывает, что раздел применяется только к Azure Synapse Analytics и Parallel Data Warehouse.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

В некоторых случаях функциональность раздела используется продуктом или службой, но поддерживаются не все аргументы. В этом случае дополнительные разделы **относится к** вставляются в описания соответствующих аргументов в теле раздела.  
 
## <a name="get-help-from-microsoft-q--a"></a>Получение помощи на странице вопросов и ответов на сайте Майкрософт  
Справку в Интернете ищите на [сайте Майкрософт на странице вопросов и ответов на форуме по Transact-SQL](/answers/topics/sql-server-transact-sql.html).  
 
## <a name="see-other-language-references"></a>Справочники по другим языкам
Документация по SQL включает и другие справочники по языкам.
  
- [Справочник по языку XQuery](../xquery/xquery-language-reference-sql-server.md)
- [Справочник по языку служб Integration Services](../integration-services/integration-services-language-reference.md)
- [Справочник по языку репликации](../relational-databases/replication/replication-language-reference.md)
- [Справочник по языку служб Analysis Services](../mdx/multidimensional-expressions-mdx-reference.md)  

## <a name="next-steps"></a>Дальнейшие действия
Теперь, когда вы знаете, как найти справочные разделы T-SQL, можно:

- пройти краткий учебник по разработке на T-SQL в разделе [Учебник. Составление инструкций Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md). 
- См. раздел [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  

