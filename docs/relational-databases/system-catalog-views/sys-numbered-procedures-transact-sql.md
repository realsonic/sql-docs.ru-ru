---
description: sys.numbered_procedures (Transact-SQL)
title: sys.numbered_procedures (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.numbered_procedures_TSQL
- numbered_procedures
- sys.numbered_procedures
- numbered_procedures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.numbered_procedures catalog view
ms.assetid: 5b6d6498-bac6-4266-94b9-d16ef5089cf0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34d67487512792261397d80e9353edecf4775b82
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095514"
---
# <a name="sysnumbered_procedures-transact-sql"></a>sys.numbered_procedures (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждой хранимой процедуры SQL Server, созданной как нумерованная процедура. В нем не отображается строка для базовой (с номером 1) хранимой процедуры. Записи для базовых хранимых процедур можно найти в таких представлениях, как **sys. Objects** и **sys. процедуры**.  
  
> [!IMPORTANT]  
>  Пронумерованные процедуры являются устаревшими. Использование нумерованных процедур не рекомендуется. При компиляции запроса, использующего это представление каталога, инициируется событие DEPRECATION_ANNOUNCEMENT.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта хранимой процедуры.|  
|**procedure_number**|**smallint**|Номер этой процедуры в данном объекте (2 или больше).|  
|**definition**|**nvarchar(max)**|Текст SQL Server, определяющий эту процедуру.<br /><br /> NULL = зашифрован.|  
  
> [!NOTE]  
>  Аргументы, связанные с языком XML и средой CLR, для нумерованных процедур не поддерживаются.  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
