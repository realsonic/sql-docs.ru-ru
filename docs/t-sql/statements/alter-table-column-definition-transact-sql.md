---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3a0724503ed6843972730afc5179c0d6ba9f18a1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100274"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Определяет свойства столбца, которые добавляются в таблицу с помощью инструкции [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *column_name*  
 Имя столбца, который требуется изменить, добавить или удалить. *column_name* может иметь длину от 1 до 128 символов. Для новых столбцов, созданных с типом данных timestamp, аргумент *column_name* можно пропустить. Если для столбца типа **timestamp** не указан аргумент *column_name*, используется имя **timestamp**.  
  
 [ _type_schema_name_ **.** ] *type_name*  
 Тип данных добавленного столбца и схема, которой он принадлежит.  
  
 *type_name* может быть:  
  
-   Системным типом данных [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Псевдонимом типа данных, основанным на системном типе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Псевдонимы типов данных должны быть созданы с помощью инструкции CREATE TYPE прежде, чем их можно будет использовать в определении таблицы;  
  
-   Определяемый пользователем тип [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] и схема, к которой он принадлежит. Определяемые пользователем типы [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] должны быть созданы с помощью инструкции CREATE TYPE прежде, чем их можно будет использовать в определении таблицы.  
  
 Если аргумент *type_schema_name* не указан, компонент [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] ссылается на аргумент *type_name* в следующем порядке:  
  
-   системный тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   в установленной по умолчанию для текущего пользователя схеме в текущей базе данных;  
  
-   Схема **dbo** в текущей базе данных.  
  
*precision*  
 Точность указанного типа данных. Дополнительные сведения о допустимых значениях точности см. в разделе [Точность, масштаб и длина (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
*масштаб*  
 Масштаб указанного типа данных. Дополнительные сведения о допустимых значениях масштаба см. в разделе [Точность, масштаб и длина (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
**max**  
 Применяется только к типам данных **varchar**, **nvarchar** и **varbinary**. Они используются для хранения 2^31 байт символов или двоичных данных, либо 2^30 байт данных в формате Юникод.  
  
**CONTENT**  
 Определяет, что каждый экземпляр типа данных **xml** в *column_name* может включать в себя несколько элементов верхнего уровня. Аргумент CONTENT применим только к данным типа **xml** и может быть указан только в том случае, если одновременно указан аргумент *xml_schema_collection*. Если он не задан, то CONTENT имеет обычный смысл, заданный по умолчанию.  
  
DOCUMENT  
 Определяет, что каждый экземпляр типа данных **xml** в *column_name* может включать в себя только один элемент верхнего уровня. Аргумент DOCUMENT применим только к данным типа **xml** и может быть указан только в том случае, если одновременно указан аргумент *xml_schema_collection*.  
  
 *xml_schema_collection*  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Применим только к типу данных **xml** для коллекции схем XML, связанной с этим типом. Перед помещением столбца **xml** в схему она должна быть создана в базе данных при помощи инструкции [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
FILESTREAM  
 При необходимости указывает атрибут хранилища FILESTREAM для столбца, в котором параметр *type_name* имеет тип данных **varbinary(max)** .  
  
 Если для столбца указан атрибут FILESTREAM, то в таблице также должен быть столбец типа данных **uniqueidentifier** с атрибутом ROWGUIDCOL. Этот столбец не должен допускать значений NULL и должен иметь относящееся к одному столбцу ограничение UNIQUE или PRIMARY KEY. Значение идентификатора GUID для столбца должно быть предоставлено приложением во время вставки данных или ограничением DEFAULT, в котором используется функция NEWID ().  
  
 Столбец ROWGUIDCOL нельзя удалить, а связанные ограничения нельзя изменить, если в таблице определен столбец FILESTREAM. Столбец ROWGUIDCOL можно удалить только после удаления последнего столбца FILESTREAM.  
  
 Если для столбца задан атрибут хранилища FILESTREAM, то все значения для этого столбца хранятся в контейнере данных FILESTREAM в файловой системе.  
  
 Пример использования определения столбца см. в разделе [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md).  
  
COLLATE *collation_name*  
 Задает параметры сортировки для столбца. Если не указано, столбцу назначаются параметры сортировки, принятые в базе данных по умолчанию. Могут использоваться параметры сортировки Windows или параметры сортировки SQL. Список и дополнительные сведения см. в разделах [Имя параметра сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметра сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 Предложение COLLATE может быть использовано для изменения параметров сортировки только для столбцов с типом данных **char**, **varchar**, **nchar** или **nvarchar**.  
  
 Дополнительные сведения о предложении COLLATE см. в разделе [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).  
  
 NULL | NOT NULL  
 Определяет, допустимы ли для столбца значения NULL. Параметр NULL не является ограничением в строгом смысле слова, но может быть указан так же, как и NOT NULL.  
  
[ CONSTRAINT *constraint_name* ]  
 Указывает начало определения значения DEFAULT. Для сохранения совместимости с более ранними версиями сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] значению DEFAULT может быть присвоено имя ограничения. Аргумент *constraint_name* должен подчиняться правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md), за исключением того, что он не может начинаться с символа решетки (#). Если аргумент *constraint_name* не задан, то определению DEFAULT назначается автоматически сформированное имя.  
  
DEFAULT  
 Ключевое слово, задающее значение по умолчанию для столбца. Определения DEFAULT могут использоваться для указания значений по умолчанию для новых столбцов в существующих строках данных. Определения DEFAULT не могут быть применены к столбцам типа **timestamp** или к столбцам, имеющим свойство IDENTITY. Если значение по умолчанию указывается для столбца определяемого пользователем типа, этот тип должен поддерживать неявное преобразование выражения *constant_expression* в определяемый пользователем тип.  
  
*constant_expression*  
 Символьное значение, NULL или системная функция, используемая в качестве значения столбца по умолчанию. Если используется в сочетании со столбцом определяемого пользователем типа [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], то реализация типа должна поддерживать неявное преобразование выражения *constant_expression* в определяемый пользователем тип.  
  
WITH VALUES   
 При добавлении столбца AND ограничение DEFAULT, если столбец допускает значения NULL с использованием WITH VALUES, задает для существующих строк значение нового столбца в качестве значения, указанного в DEFAULT *constant_expression*. Если добавляемый столбец не допускает значения NULL, для существующих строк значение столбца всегда будет присваиваться в качестве значения, предоставляемого в DEFAULT *constant expression*. Начиная с SQL Server 2012, может использоваться операция с метаданными [adding-not-null-columns-as-an-online-operation](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation).
При использовании в ситуации, когда связанный столбец не добавляется, никакого эффекта не будет.
 
 Указывает, что значение, указанное в выражении DEFAULT *constant_expression*, сохраняется в новом столбце, добавляемом к существующим строкам. Если добавленный столбец допускает значения NULL и указан оператор WITH VALUES, новый столбец, добавленный к существующим строкам, заполняется значением по умолчанию. Если предложение WITH VALUES не указано для столбцов, допускающих значение NULL, новый столбец для существующих строк заполняется значением NULL. Если новый столбец не допускает значения NULL, значение по умолчанию сохраняется во всех строках независимо от того, указан оператор WITH VALUES или нет.  
  
IDENTITY  
 Указывает, что новый столбец является столбцом идентификаторов. Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] присваивает столбцу уникальное возрастающее значение. Когда добавляются столбцы идентификаторов к существующим таблицам, к существующим строкам таблицы добавляются номера идентификаторов с этим начальным значением и приращением. Порядок, в котором выполняется обновление строк, не гарантирован. Номера идентификаторов также формируются для всех новых строк, которые добавляются.  
  
 Столбцы идентификаторов наиболее часто используются в сочетании с ограничениями PRIMARY KEY для выполнения функции уникального идентификатора строки таблицы. Свойство IDENTITY может назначаться для столбцов типа **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** или **numeric(p,0)** . Для каждой таблицы можно создать только один столбец идентификаторов. Ключевое слово DEFAULT и значения по умолчанию не могут применяться к столбцам идентификаторов. Начальное значение и шаг приращения для столбца идентификаторов должны быть либо заданы вместе, либо не заданы вообще. Если ничего не указано, применяется значение по умолчанию (1,1).  
  
> [!NOTE]  
>  Нельзя изменить существующий столбец таблицы, добавив в него свойство IDENTITY.  
  
 Не поддерживается добавление столбца идентификаторов к опубликованной таблице, так как это может привести к расхождению данных при репликации столбца на подписчик. Значения в столбце идентификаторов на издателе зависят от порядка, в котором строки изменяемой таблицы хранятся физически. Строки могут быть сохранены на подписчике иначе; следовательно, значение в столбце идентификаторов может отличаться для одних и тех же строк.  
  
 Чтобы запретить свойство IDENTITY столбца и при вставке строк задавать его значения явно, можно воспользоваться инструкцией [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md).  
  
*seed*  
 Значение, используемое для первой строки, загружаемой в таблицу.  
  
*increment*  
 Значение приращения, добавляемое к значению столбца предыдущей строки для получения значения столбца новой строки.  
  
NOT FOR REPLICATION  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Может быть задано для свойства IDENTITY. Если это предложение задано для свойства IDENTITY, то значения в столбцах идентификаторов не увеличиваются при выполнении агентами репликации операций по вставке строк.  
  
ROWGUIDCOL  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Показывает, что столбец является столбцом глобального уникального идентификатора строки. ROWGUIDCOL может быть назначен только для столбца **uniqueidentifier**, и только один столбец **uniqueidentifier** в таблице может быть объявлен как столбец ROWGUIDCOL. ROWGUIDCOL не может быть назначен столбцам с типами, определенными пользователем.  
  
 Свойство ROWGUIDCOL не обеспечивает уникальности значений, хранимых в столбце. Кроме того, оно не производит автоматического формирования значений для новых строк, вставляемых в таблицу. Для формирования уникальных значений для каждого столбца можно использовать функцию NEWID в инструкциях INSERT или определить функцию NEWID как значение по умолчанию для столбца. Дополнительные сведения см. в разделах [NEWID (Transact-SQL)](../../t-sql/functions/newid-transact-sql.md) и [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md).  
  
SPARSE  
 Указывает, что столбец является разреженным столбцом. Хранилище разреженных столбцов оптимизируется для значений NULL. Для разреженных столбцов нельзя указать параметр NOT NULL. Дополнительные ограничения и сведения о разреженных столбцах см. в разделе [Разреженные столбцы](../../relational-databases/tables/use-sparse-columns.md).  
  
\<column_constraint>  
 Определения аргументов ограничения столбцов см. в разделе [column_constraint (Transact-SQL)](../../t-sql/statements/alter-table-column-constraint-transact-sql.md).  
  
 ENCRYPTED WITH  
 Указывает столбцы шифрования с помощью функции [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
 COLUMN_ENCRYPTION_KEY = *key_name*  
 Указывает пустой ключ шифрования столбца. Дополнительные сведения см. в разделе [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/create-column-encryption-key-transact-sql.md).  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 **Детерминированное шифрование** использует метод, который всегда создает одно и то же зашифрованное значение для любого текстового значения. Это позволяет выполнять поиск с помощью сравнения на равенство, группирование и объединение таблиц по зашифрованным значениям. При этом несанкционированные пользователи могут определять некоторую информацию о зашифрованных значениях путем анализа повторов в зашифрованном столбце. Соединить две таблицы по столбцам с детерминированным шифрованием можно только в том случае, если оба столбца шифруются с помощью одного ключа шифрования столбца. При использовании детерминированного шифрования необходимо указать порядок сортировки binary2 в параметрах сортировки для символьных столбцов.  
  
 **Случайное шифрование** использует метод, который шифрует данные менее предсказуемым образом. Случайное шифрование более безопасное, но предотвращает любые вычисления и индексацию в зашифрованных столбцах, если экземпляр SQL Server не поддерживает функцию [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).
  
 Если вы используете функцию Always Encrypted (без безопасных анклавов), к столбцам, поиск которых осуществляется на основе параметров или параметров группирования, например ИНН, следует применять детерминированное шифрование. Используйте случайное шифрование для таких данных, как номер кредитной карты, которые не группируются с другими записями, не используются для соединения таблиц и не могут быть параметром поиска, поскольку для поиска нужной строки с зашифрованным столбцом используются другие столбцы (например, номер транзакции).  

 При использовании функции Always Encrypted с безопасными анклавами советуем использовать случайное шифрование.  
  
 Столбцы должны иметь подходящий тип данных.  
  
ALGORITHM  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
Должно быть **'AEAD_AES_256_CBC_HMAC_SHA_256'** .  
  
 Дополнительные сведения, в том числе об ограничениях функции, см. в разделе [Постоянное шифрование (компонент Database Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
   
ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
 **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Указывает маску для динамического маскирования данных. *mask_function* — это имя функции маскирования с соответствующими параметрами. Доступны следующие функции:  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 Параметры функции см. в разделе [Динамическое маскирование данных](../../relational-databases/security/dynamic-data-masking.md).  
  
## <a name="remarks"></a>Remarks  
 Если добавлен столбец, имеющий тип данных **uniqueidentifier**, он может быть определен с помощью умолчания, которое использует функцию NEWID() для предоставления новому столбцу значений уникального идентификатора для каждой существующей строки в таблице.  
  
 Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не ограничивает порядок задания свойств DEFAULT, IDENTITY, ROWGUIDCOL или ограничений столбца в определении столбца.  
  
 Инструкция ALTER TABLE будет завершена с ошибками, если при добавлении столбца размер строки данных превысит 8060 байт.  
  
## <a name="examples"></a>Примеры  
 Примеры см. в статье [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
