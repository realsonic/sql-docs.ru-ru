---
description: sys.assembly_references (Transact-SQL)
title: sys.assembly_modules (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assembly_modules
- sys.assembly_modules_TSQL
- assembly_modules
- assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_modules catalog view
ms.assetid: 5f9e644e-8065-49a2-b53d-db7df98f70d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d46e4eca31f16a9f244046f05abe6dda44df3fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099304"
---
# <a name="sysassembly_modules-transact-sql"></a>sys.assembly_references (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждой функции, процедуры или триггера, которые определены сборкой среды CLR. Это представление каталога сопоставляет хранимые процедуры, триггеры или функции среды CLR с их базовой реализацией. Объекты типов TA, AF, PC, FS и FT имеют связанный с ними модуль сборки. Чтобы найти взаимосвязь между объектом и сборкой, можно соединить это представление каталога с другими представлениями каталога. Например, при создании хранимой процедуры CLR она представляется одной строкой в **таблице sys. Objects**, одной строкой в **sys.** Procedures (которая наследуется из **sys. Objects**) и одной строкой в **sys.assembly_modules**. Сама хранимая процедура представлена метаданными в **sys. Objects** и **sys.** Procedures. Ссылки на базовую реализацию CLR процедуры находятся в **sys.assembly_modules**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификационный номер объекта SQL. Уникален в базе данных.|  
|**assembly_id**|**int**|Идентификатор той сборки, откуда был создан этот модуль.|  
|**assembly_class**|**sysname**|Имя класса в сборке, определяющее этот модуль.|  
|**assembly_method**|**sysname**|Имя метода в **assembly_class** , определяющего этот модуль.<br /><br /> Для агрегатных функций (AF) имеет значение NULL.|  
|**null_on_null_input**|**bit**|Модуль выдает выходные значения NULL при любых входных значениях NULL.|  
|**execute_as_principal_id**|**int**|Идентификатор участника базы данных, в контексте которого производится выполнение, как указано в предложении EXECUTE AS функции, хранимой процедуры или триггера среды CLR. <br /><br /> NULL = EXECUTE AS CALLER. Это значение по умолчанию.<br /><br /> ИДЕНТИФИКАТОР указанного участника базы данных = выполнить как SELF, выполнить как *user_name* или выполнить как *login_name*.<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
