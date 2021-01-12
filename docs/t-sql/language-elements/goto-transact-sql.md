---
description: GOTO (Transact-SQL)
title: GOTO (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GOTO
- GOTO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- skipping statements
- Transact-SQL statements, skipping
- labels [SQL Server]
- statements [SQL Server], skipping
- GOTO statement
ms.assetid: 589b6f8e-dc80-416f-9e74-48bed5337f58
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d47b0b4b847e6cb3a6fee75a6b1ea561f1c57f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100353"
---
# <a name="goto-transact-sql"></a>GOTO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Переводит поток выполнения на метку. Инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] или инструкции, следующие за инструкцией GOTO, пропускаются, и выполнение продолжается с метки. Инструкция GOTO и метки могут быть включены в процедуру, пакет или блок инструкций. Инструкции GOTO могут быть вложенными.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Define the label:   
label:   
Alter the execution:  
GOTO label   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *label*  
 Точка, с которой начинается обработка инструкций после перехода на текущую метку с помощью инструкции GOTO. Метки должны соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Метка может применяться без инструкции GOTO как метод комментирования.  
  
## <a name="remarks"></a>Комментарии  
 Инструкция GOTO может существовать внутри условных инструкций, управляющих потоком, блоков инструкций или процедур, однако не может ссылаться на метку, расположенную вне этого пакета. Инструкция GOTO может ссылаться на метку, расположенную как до нее, так и после.  
  
## <a name="permissions"></a>Разрешения  
 Разрешения GOTO по умолчанию предоставляются всем допустимым пользователям.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как использовать разрешения `GOTO` в качестве механизма ветви.  
  
```  
DECLARE @Counter int;  
SET @Counter = 1;  
WHILE @Counter < 10  
BEGIN   
    SELECT @Counter  
    SET @Counter = @Counter + 1  
    IF @Counter = 4 GOTO Branch_One --Jumps to the first branch.  
    IF @Counter = 5 GOTO Branch_Two  --This will never execute.  
END  
Branch_One:  
    SELECT 'Jumping To Branch One.'  
    GOTO Branch_Three; --This will prevent Branch_Two from executing.  
Branch_Two:  
    SELECT 'Jumping To Branch Two.'  
Branch_Three:  
    SELECT 'Jumping To Branch Three.';  
```  
  
## <a name="see-also"></a>См. также  
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [BREAK (Transact-SQL)](../../t-sql/language-elements/break-transact-sql.md)   
 [CONTINUE (Transact-SQL)](../../t-sql/language-elements/continue-transact-sql.md)   
 [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md)   
 [WAITFOR (Transact-SQL)](../../t-sql/language-elements/waitfor-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
  
