---
description: Функции, используемых в базах данных Microsoft SQL
title: Функции, используемых в базах данных Microsoft SQL | Документы Майкрософт
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- built-in functions [SQL Server]
- function [SQL Server] See functions [SQL Server]
- functions [Transact-SQL]
- functions [SQL Server], about functions
- scalar functions
- functions [SQL Server]
ms.assetid: 17186213-5ab5-40b0-b470-b660af1ec44c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b765564b92cad08db5426b653ad816061a6c5dc4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101155"
---
# <a name="what-are-the-sql-database-functions"></a>Функции, используемых в базах данных SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Сведения о категориях встроенных функций, которые можно использовать с базами данных SQL. Вы можете использовать встроенные функции или создавать собственные пользовательские функции.
  
## <a name="aggregate-functions"></a>Агрегатные функции

Агрегатные функции выполняют вычисление на наборе значений и возвращают одиночное значение. Они допускаются в списке выбора или в предложении HAVING инструкции SELECT. Агрегатную функцию можно использовать в сочетании с предложением GROUP BY для статистических вычислений на основе категорий строк. Используйте предложение OVER для вычисления статистического значения на основе определенного диапазона значений. Предложение OVER не может следовать за агрегатными функциями GROUPING и GROUPING_ID.

Все агрегатные функции являются детерминированными. Это означает, что они всегда возвращают одинаковый результат для одинаковых входных значений. Дополнительные сведения см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

## <a name="analytic-functions"></a>Аналитические функции
Аналитические функции вычисляют статистическое значение на основе группы строк. Однако, в отличие от агрегатных функций, аналитические функции могут возвращать несколько строк для каждой группы. Аналитические функции можно использовать для вычисления скользящих средних, промежуточных итогов, процентных долей или первых N результатов в группе.

## <a name="ranking-functions"></a>Ранжирующие функции
Ранжирующие функции возвращают ранжирующее значение для каждой строки в секции. В зависимости от используемой функции значения некоторых строк могут совпадать. Ранжирующие функции являются недетерминированными.

## <a name="rowset-functions"></a>Функции наборов строк
Функции наборов строк возвращают объект, который можно использовать так же, как табличные ссылки в инструкции SQL.

## <a name="scalar-functions"></a>Скалярные функции
Обрабатывают и возвращают одиночное значение. Скалярные функции можно применять везде, где выражение допустимо.

### <a name="categories-of-scalar-functions"></a>Категории скалярных функций
  
|Категория функции|Описание|  
|-----------------------|-----------------|  
|[Функции конфигурации](configuration-functions-transact-sql.md)|Возвращают сведения о текущей конфигурации.|  
|[Функции преобразования](conversion-functions-transact-sql.md)|Поддержка приведения и преобразования типов данных.|  
|[Функции работы с курсорами](cursor-functions-transact-sql.md)|Возвращают сведения о курсорах.|  
|[Функции и типы данных даты и времени](date-and-time-data-types-and-functions-transact-sql.md)|Выполняют операции над исходными значениями даты и времени, возвращают строковые и числовые значения, а также значения даты и времени.|  
|[Функции JSON](json-functions-transact-sql.md)|Проверяют, запрашивают или изменяют данные JSON.|  
|[Логические функции](logical-functions-choose-transact-sql.md)|Выполнение логических операций.|  
|[Математические функции](mathematical-functions-transact-sql.md)|Выполняют вычисления, основанные на числовых значениях, переданных функции в виде аргументов, и возвращают числовые значения.|  
|[Функции метаданных](metadata-functions-transact-sql.md)|Возвращают сведения о базах данных и объектах баз данных.|  
|[Функции безопасности](security-functions-transact-sql.md)|Возвращают данные о пользователях и ролях.|  
|[Строковые функции](string-functions-transact-sql.md)|Выполняют операции со строковым (**char** или **varchar**) входным значением и возвращают строковое или числовое значение.|  
|[Системные функции](../../relational-databases/system-functions/system-functions-category-transact-sql.md)|Выполняют операции над значениями, объектами и параметрами экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и возвращают сведения о них.|  
|[Системные статистические функции](system-statistical-functions-transact-sql.md)|Возвращают статистические сведения о системе.|  
|[Функции обработки текста и изображений](./text-and-image-functions-textptr-transact-sql.md)|Выполняют операции над текстовыми или графическими исходными значениями или столбцами и возвращают сведения о значении.|  
  
## <a name="function-determinism"></a>Детерминизм функций  
 Различаются детерминированные и недетерминированные встроенные функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Функция является детерминированной, если для определенных входных значений она каждый раз возвращает один и тот же результат. Функция является недетерминированной, если она возвращает различные результаты даже для одних и тех же исходных значений. Дополнительные сведения см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="function-collation"></a>Параметры сортировки функций  
 Функции, в которые вводится символьная строка и которые выдают ее, используют параметры сортировки входной строки для строки вывода.  
  
 Функции, которые обрабатывают несимвольные исходные данные и выдают символьную строку, применяют при выводе параметры сортировки по умолчанию для текущей базы данных.  
  
 Функции, обрабатывающие в качестве исходных данных несколько символьных строк и возвращающие символьную строку, задают параметры сортировки для строки вывода по правилам очередности параметров сортировки. Дополнительные сведения см. в статье [Очередность параметров сортировки (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
## <a name="see-also"></a>См. также:  
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)   
 [Использование хранимых процедур (MDX)](../../mdx/using-stored-procedures-mdx.md)  
  
