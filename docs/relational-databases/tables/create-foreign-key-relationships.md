---
description: Создание связей по внешнему ключу
title: Создание связей по внешнему ключу | Документация Майкрософт
ms.custom: ''
ms.date: 06/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 933532a516f18aab01e1a32584590d65940c75b6
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170206"
---
# <a name="create-foreign-key-relationships"></a>Создание связей по внешнему ключу


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

В этой статье описывается, как создать связи внешнего ключа в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Связь создается между двумя таблицами, чтобы связать строки одной таблицы со строками другой.

## <a name="permissions"></a>Разрешения

Создание новой таблицы с внешним ключом требует разрешения [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) в базе данных и разрешения [ALTER](../../t-sql/statements/alter-schema-transact-sql.md) на схему, в которой создается таблица.

Создание внешнего ключа в существующей таблице требует разрешения [ALTER](../../t-sql/statements/alter-table-transact-sql.md) на таблицу.

## <a name="limits-and-restrictions"></a><a name="BeforeYouBegin"></a> Пределы и ограничения

- Ограничение внешнего ключа не обязательно должно быть связано только с ограничением первичного ключа в другой таблице. Внешние ключи также могут быть определены, чтобы ссылаться на столбцы ограничения UNIQUE в другой таблице.
- Если столбцу, имеющему ограничение внешнего ключа, задается значение, отличное от NULL, такое же значение должно существовать и в указываемом столбце. В противном случае будет возвращено сообщение о нарушении внешнего ключа. Для обеспечения проверки всех значений сложного ограничения внешнего ключа задайте параметр NOT NULL для всех столбцов, участвующих в индексе.
- Ограничения FOREIGN KEY могут ссылаться только на таблицы в пределах той же базы данных на том же сервере. Межбазовую ссылочную целостность необходимо реализовать посредством триггеров. Дополнительные сведения см. в статье об инструкции [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- Ограничения FOREIGN KEY могут ссылаться на другие столбцы той же таблицы и считаются ссылками на себя.
- Ограничение FOREIGN KEY, определенное на уровне столбцов, может содержать только один ссылочный столбец. Этот столбец должен принадлежать к тому же типу данных, что и столбец, для которого определяется ограничение.
- Ограничение FOREIGN KEY, определенное на уровне таблицы, должно содержать такое же число ссылочных столбцов, какое содержится в списке столбцов в ограничении. Тип данных каждого ссылочного столбца должен также совпадать с типом соответствующего столбца в списке столбцов.
- [!INCLUDE[ssDE](../../includes/ssde-md.md)] не имеет предопределенного ограничения на число ограничений FOREIGN KEY, которые могут содержаться в таблице, ссылающейся на другие таблицы. [!INCLUDE[ssDE](../../includes/ssde-md.md)] также не ограничивает число ограничений FOREIGN KEY, принадлежащих другим таблицам, которые ссылаются на определенную таблицу. Но фактическое количество используемых ограничений FOREIGN KEY ограничивается конфигурацией оборудования, базы данных и приложения. Максимальное количество таблиц и столбцов, на которые может ссылаться таблица в качестве внешних ключей (исходящих ссылок), равно 253. [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и последующие версии увеличивает ограничение на количество других таблиц и столбцов, которые могут ссылаться на столбцы в одной таблице (входящие ссылки), с 253 до 10 000. (Требуется уровень совместимости не менее 130.) Увеличение имеет следующие ограничения:

  - Превышение 253 ссылок на внешние ключи поддерживается только для операций DELETE и UPDATE DML. Операции MERGE не поддерживаются.
  - Таблица со ссылкой внешнего ключа на саму себя по-прежнему ограничена 253 ссылками на внешние ключи.
  - Превышение числа в 253 ссылки на внешние ключи в настоящее время недоступно для индексов columnstore, оптимизированных для памяти таблиц или Stretch Database.

- Ограничения FOREIGN KEY не применяются к временным таблицам.
- Если внешний ключ определен на столбце определяемого пользователем типа данных CLR, реализация этого типа должна поддерживать двоичную сортировку. Дополнительные сведения об определяемых пользователем типах данных CLR см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Столбец типа **varchar(max)** может участвовать в ограничении FOREIGN KEY только при условии, что первичный ключ, на который он ссылается, также имеет тип данных **varchar(max)** .

## <a name="create-a-foreign-key-relationship-in-table-designer"></a>Создание связи по внешнему ключу в конструкторе таблиц

### <a name="using-sql-server-management-studio"></a>Использование среды SQL Server Management Studio

1. В обозревателе объектов щелкните правой кнопкой мыши таблицу, которая будет содержать внешний ключ для связи, и выберите пункт **Конструктор**.

   Таблица откроется в окне [**Конструктор таблиц**](../../ssms/visual-db-tools/design-tables-visual-database-tools.md).
2. В меню **конструктора таблиц** выберите пункт **Связи**.
3. В диалоговом окне **Связи внешнего ключа** щелкните **Добавить**.

   Связь появится в списке **Выбранные связи** с именем, установленным системой, в формате format FK_\<*tablename*>_\<*tablename*>, где *tablename* (имя таблицы) является именем внешнего ключа.
4. Щелкните нужную связь в списке **Выбранные связи** .
5. Щелкните **Спецификация таблиц и столбцов** в сетке справа и нажмите кнопку с многоточием ( **...** ) справа от свойства.
6. В диалоговом окне **Таблицы и столбы** в раскрывающемся списке **Первичный ключ** выберите таблицу, которая будет находиться на стороне первичного ключа связи.
7. В сетке внизу выберите столбцы, составляющие первичный ключ таблицы. В соседней ячейке сетки справа от каждого столбца выберите соответствующий столбец внешнего ключа таблицы внешнего ключа.

   **Конструктор таблиц** автоматически предлагает имя для связи. Чтобы его изменить, отредактируйте содержимое текстового поля **Имя связи** .
8. Нажмите кнопку **OК** , чтобы создать связь.
9. Закройте окно конструктора таблиц и **сохраните** внесенные изменения, чтобы изменения связи внешнего ключа вступили в силу.

## <a name="create-a-foreign-key-in-a-new-table"></a>Создание внешнего ключа в новой таблице

### <a name="using-transact-sql"></a>Использование Transact-SQL

В следующем примере создается таблица и определяется ограничение внешнего ключа для столбца `TempID`, ссылающегося на столбец `SalesReasonID` в таблице `Sales.SalesReason` базы данных AdventureWorks. Предложения ON DELETE CASCADE и ON UPDATE CASCADE используются для обеспечения распространения изменений, вносимых в таблицу `Sales.SalesReason` на таблицу `Sales.TempSalesReason` .    

```sql
CREATE TABLE Sales.TempSalesReason 
   (
      TempID int NOT NULL, Name nvarchar(50)
      , CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID)
      , CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
        REFERENCES Sales.SalesReason (SalesReasonID)
        ON DELETE CASCADE
        ON UPDATE CASCADE
   )
;
```

## <a name="create-a-foreign-key-in-an-existing-table"></a>Создание внешнего ключа в существующей таблице

### <a name="using-transact-sql"></a>Использование Transact-SQL
В следующем примере создается внешний ключ для столбца `TempID`, ссылающегося на столбец `SalesReasonID` в таблице `Sales.SalesReason` базы данных AdventureWorks.

```sql
ALTER TABLE Sales.TempSalesReason
   ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
      REFERENCES Sales.SalesReason (SalesReasonID)
      ON DELETE CASCADE
      ON UPDATE CASCADE
;
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в разделе:

- [Ограничения первичных и внешних ключей](primary-and-foreign-key-constraints.md)
- [GRANT (разрешения на базу данных)](../../t-sql/statements/grant-database-permissions-transact-sql.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md).
