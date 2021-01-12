---
description: SET ANSI_PADDING (Transact-SQL)
title: SET ANSI_PADDING (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ANSI_PADDING_TSQL
- ANSI_PADDING
- SET_ANSI_PADDING_TSQL
- SET ANSI_PADDING
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], short values
- ANSI_PADDING option
- short values [SQL Server]
- SET ANSI_PADDING statement
- trailing blanks
ms.assetid: 92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95aef665b10cebc457f586837b9c59f81f03443f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102312"
---
# <a name="set-ansi_padding-transact-sql"></a>SET ANSI_PADDING (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Управляет тем, как столбец хранит значения короче, чем определенный размер столбца, и тем, как столбец хранит значения с завершающими пробелами в данных типов **char**, **varchar**, **binary** и **varbinary** .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис
 
### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Синтаксис для [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] и [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 
```syntaxsql
SET ANSI_PADDING { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Синтаксис для [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] и [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_PADDING ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Столбцы с типом данных **char**, **varchar**, **binary** и **varbinary** имеют определенный размер.  
  
 Этот параметр влияет только на определение новых столбцов. После создания столбца [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сохраняет значения, исходя из значения параметра на время создания столбца. Более поздние изменения этого параметра не влияют на существующие столбцы.  
  
> [!NOTE]  
> Для параметра ANSI_PADDING следует всегда устанавливать значение ON.  
  
 В таблице показаны результаты влияния, оказываемого параметром SET ANSI_PADDING, когда значения вставляются в столбцы с типами данных **char**, **varchar**, **binary** и **varbinary**.  
  
|Параметр|char(*n*) NOT NULL или binary(*n*) NOT NULL|char(*n*) NULL или binary(*n*) NULL|varchar(*n*) или varbinary(*n*)|  
|-------------|----------------------------------------------------|--------------------------------------------|----------------------------------------|  
|ON|Заполнение исходного значения (с замыкающими пробелами для столбцов **char** и с замыкающими нулями для столбцов **binary**) до длины столбца.|Следует тем же правилам, что и для **char(** _n_ **)** или **binary(** _n_ **)** NOT NULL, когда для параметра SET ANSI_PADDING установлено значение ON.|Замыкающие пробелы в символьных значениях, вставляемых в столбцы **varchar**, не усекаются. Замыкающие нули в двоичных значениях, вставляемых в столбцы **varbinary**, не усекаются. Значения не подгоняются под длину столбца.|  
|OFF|Заполнение исходного значения (с замыкающими пробелами для столбцов **char** и с замыкающими нулями для столбцов **binary**) до длины столбца.|Следует тем же правилам, что и для **varchar** или **varbinary**, когда для параметра SET ANSI_PADDING установлено значение OFF.|Замыкающие пробелы в символьных значениях, вставляемых в столбец **varchar**, усекаются. Замыкающие нули в двоичных значениях, вставляемых в столбец **varbinary**, усекаются.|  
  
> [!NOTE]  
> При заполнении столбцы **char** заполняются пробелами, а столбцы **binary** заполняются нулями. При усечении в столбцах **char** усекаются замыкающие пробелы, а в столбцах **binary** усекаются замыкающие нули.  
  
Параметр ANSI_PADDING должен иметь значение ON при создании или изменении индексов по вычисляемым столбцам или индексированным представлениям. Дополнительные сведения о настройке параметров SET с индексированными представлениями и индексами на вычисляемых столбцах см. в подразделе "Рекомендации по использованию инструкций SET" раздела [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md).  
  
По умолчанию значение параметра SET ANSI_PADDING равно ON. Драйвер ODBC для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и поставщик OLE DB для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически устанавливает параметр ANSI_PADDING в значение ON при соединении. Это может быть настроено в источниках данных ODBC, в атрибутах соединения ODBC или свойствах соединения OLE DB, определенных в приложении перед установкой соединения. Значением по умолчанию для параметра инструкции SET ANSI_PADDING является OFF при соединениях из приложений DB-Library.  
  
 Параметр SET ANSI_PADDING не влияет на типы данных **nchar**, **nvarchar**, **ntext**, **text**, **image**, **varbinary(max)**, **varchar(max)** и **nvarchar(max)**. Они всегда демонстрируют поведение, соответствующее установленному для параметра инструкции SET ANSI_PADDING значению ON. Это означает, что конечные пробелы и нули не вырезаются.  
  
Когда для ANSI_DEFAULTS установлено значение ON, параметр ANSI_PADDING включен.  
  
Параметр ANSI_PADDING устанавливается во время выполнения, а не во время синтаксического анализа.  
  
Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос.  
  
```sql  
DECLARE @ANSI_PADDING VARCHAR(3) = 'OFF';  
IF ( (16 & @@OPTIONS) = 16 ) SET @ANSI_PADDING = 'ON';  
SELECT @ANSI_PADDING AS ANSI_PADDING;  
```  
  
## <a name="permissions"></a>Разрешения  
Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
В следующем примере показано, как значение этого параметра влияет на каждый из типов данных.  

Установите для параметра ANSI_PADDING значение ON и выполните тестирование.

```sql  
PRINT 'Testing with ANSI_PADDING ON'  
SET ANSI_PADDING ON;  
GO  
  
CREATE TABLE t1 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t1 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t1 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '\<',  
   varbinarycol  
FROM t1;  
GO  
```

Теперь установите для параметра ANSI_PADDING значение OFF и выполните тестирование.

```sql
PRINT 'Testing with ANSI_PADDING OFF';  
SET ANSI_PADDING OFF;  
GO  
  
CREATE TABLE t2 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t2 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t2 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '<',  
   varbinarycol  
FROM t2;  
GO  
  
DROP TABLE t1;  
DROP TABLE t2;  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY (Transact-SQL)](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
