---
description: sys.syscomments (Transact-SQL)
title: Комментарии к sys.sys(Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscomments_TSQL
- syscomments
- syscomments_TSQL
- sys.syscomments
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syscomments compatibility view
- syscomments system table
ms.assetid: 767dd410-6bc9-4c4a-ab0f-6d2cf6163426
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34ff2576f81e7756b4e6ee30a98df6617bd0be0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099178"
---
# <a name="syssyscomments-transact-sql"></a>sys.syscomments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит записи для всех представлений, правил, значений по умолчанию, триггеров, ограничений CHECK и DEFAULT, а также для всех хранимых процедур в базе данных. **Текстовый** столбец содержит исходные инструкции определения SQL.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется применять процедуру sys.sql_modules. Дополнительные сведения см. в разделе [sys.sql_modules &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор объекта, к которому применяется текст.|  
|**number**|**smallint**|Номер внутри группирования процедур, если группирование существует.<br /><br /> 0 = записи не являются процедурами.|  
|**идентификатора столбца**|**smallint**|Последовательный номер строки для определения объекта с длиной более 4 000 символов.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**ctext**|**varbinary(8000)**|Приблизительное число байтов в инструкции определения SQL.|  
|**тексттипе**|**smallint**|0 = пользовательский комментарий<br /><br /> 1 = системный комментарий<br /><br /> 4 = зашифрованный комментарий|  
|**language**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Шифрование**|**bit**|Указывает, применялось ли к определению процедуры запутывание.<br /><br /> 0 = запутывание не применялось;<br /><br /> 1 = запутывание применялось.<br /><br /> Важно. для маскировки определений хранимых процедур используйте ключевое слово CREATE PROCEDURE с ключевым словом encryption. **\* \* \* \***|  
|**сжатия**|**bit**|Всегда возвращает 0. Это означает, что процедура сжата.|  
|**text**|**nvarchar(4000)**|Фактический текст инструкции определения SQL.<br /><br /> Семантика расшифрованных выражений соответствует исходному тексту, однако правильность синтаксиса не гарантируется. Например, пробельные символы удаляются из расшифрованного выражения.<br /><br /> Это [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] представление, совместимое с, получает сведения из текущих [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] структур и может возвращать больше символов, чем определение **nvarchar (4000)** . **sp_help** возвращает **nvarchar (4000)** в качестве типа данных текстового столбца. При работе с **syscomments** рекомендуется использовать **nvarchar (max)**. Для новых задач разработки не используйте **syscomments**.|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
