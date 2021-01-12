---
description: WITH CHANGE_TRACKING_CONTEXT (Transact-SQL)
title: С помощью CHANGE_TRACKING_CONTEXT (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- WITH_CHANGE_TRACKING_CONTEXT_TSQL
- WITH CHANGE_TRACKING_CONTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- WITH CHANGE_TRACKING_CONTEXT
- change tracking [SQL Server], WITH CHANGE_TRACKING_CONTEXT
ms.assetid: 885e33a1-602a-4b94-8380-a63ac935a683
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f35e45272c5618ecb1e4eefc5e5406bb93a5cb1c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092735"
---
# <a name="with-change_tracking_context-transact-sql"></a>WITH CHANGE_TRACKING_CONTEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Позволяет указать контекст изменения, например идентификатор источника, при изменении данных. Например, при использовании отслеживания изменений для приложения может потребоваться разграничение изменений, внесенных самим приложением, и изменений, внесенных внешними приложениями.  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
WITH CHANGE_TRACKING_CONTEXT ( context )  
```  
  
#### <a name="parameters"></a>Параметры  
 *context*  
 Сведения о контексте, предоставляемые вызывающим приложением и сохраняемые вместе с данными отслеживания изменений для соответствующего изменения. *контекст* — **varbinary (128)**.  
  
 Его значением может быть константа или переменная, но не может быть значение NULL.  
  
## <a name="examples"></a>Примеры  
 В следующем примере задается контекст отслеживания изменений для изменения данных.  
  
```  
WITH CHANGE_TRACKING_CONTEXT ( context )  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции отслеживания изменений (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE (Transact-SQL)](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [Отслеживание измененных данных (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
