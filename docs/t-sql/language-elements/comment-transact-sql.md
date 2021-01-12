---
description: -- (комментарий) (Transact-SQL)
title: -- (комментарий) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- --_TSQL
- Comment
- --
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- remarks [SQL Server]
- -- (comment character)
- comments [SQL Server]
ms.assetid: 676ea8c2-52c1-4ef6-9354-320f1a091153
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 152276ea9879065e1ffbe809197e82959172dac9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100424"
---
# <a name="---comment-transact-sql"></a>-- (комментарий) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Обозначает текст комментария пользователя. Комментарии могут вставляться отдельной строкой, добавляться в конец командной строки [!INCLUDE[tsql](../../includes/tsql-md.md)] или инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Сервер не обрабатывает комментарий.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- text_of_comment  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *text_of_comment*  
 Строка, содержащая текст комментария.  
  
## <a name="remarks"></a>Remarks  
Используйте два дефиса ( **--** ) для однострочных или вложенных комментариев. Комментарии, вставляемые с использованием символов **--** , завершаются новой строкой, которая задается символом возврата каретки (U+000A), символом перевода строки (U+000D) или сочетанием этих двух символов. Длина комментариев не ограничена. В следующей таблице перечислены сочетания клавиш, используемые для комментирования и раскомментирования текста.
  
|Действие|Standard|  
|------------|--------------|  
|Преобразование выделенного текста в комментарий|CTRL + K, CTRL + C|  
|Снять комментарий с выделенного текста|CTRL + K, CTRL + U|  
  
 Дополнительные сведения о сочетаниях клавиш см. в статье [Сочетания клавиш среды SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
 Сведения о многострочных комментариях см. в статье [Косая черта-звездочка (блок комментариев) (Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере используются символы комментария --.  
  
```sql  
-- Choose the AdventureWorks2012 database.  
USE AdventureWorks2012;  
GO  
-- Choose all columns and all rows from the Address table.  
SELECT *  
FROM Person.Address  
ORDER BY PostalCode ASC; -- We do not have to specify ASC because   
-- that is the default.  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)  
  
  
