---
description: sys.syslanguages (Transact-SQL)
title: Языки sys.sys(Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syslanguages
- sys.syslanguages_TSQL
- syslanguages
- syslanguages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syslanguages system table
- sys.syslanguages compatibility view
ms.assetid: f216d1cd-997c-42f0-a737-abbdfcd88383
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2174d08692915c5053486f403462d2c1ac16966d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094207"
---
# <a name="syssyslanguages-transact-sql"></a>sys.syslanguages (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит одну строку для каждого языка, представленного в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|langid|**smallint**|Уникальный идентификатор языка.|  
|формат даты|**nchar(3)**|Формат представления даты, например DMY.|  
|datefirst|**tinyint**|Первый день недели: 1 для Понедельник, 2 для вторника и так далее до 7 для воскресенья.|  
|обновление|**int**|Зарезервировано для системного использования.|  
|name|**sysname**|Официальное название языка, например Franзais.|  
|alias|**sysname**|Альтернативное название языка, например French.|  
|months|**nvarchar (372)**|Список полных названий месяцев через запятую в порядке с января до декабря. Каждое название может содержать не более 20 символов.|  
|shortmonths|**nvarchar (132)**|Список сокращенных названий месяцев через запятую в порядке с января до декабря. Название может содержать не более 9 символов.|  
|days|**nvarchar (217)**|Список названий дней недели через запятую в порядке с понедельника до воскресенья. Каждое название может содержать не более 30 символов.|  
|lcid|**int**|Код локали языка в [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|  
|msglangid|**smallint**|Идентификатор группы сообщений компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
  
 Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] содержит следующие установленные языки.  
  
|Английское название языка|Код языка в Windows|Идентификатор группы сообщений компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]|  
|---------------------|------------------|-----------------------------------------|  
|Английский|1033|1033|  
|Немецкий|1031|1031|  
|Французский|1036|1036|  
|Японский|1041|1041|  
|Датский|1030|1030|  
|Испанский|3082|3082|  
|Итальянский|1040|1040|  
|Нидерландский|1043|1043|  
|Норвежский|2068|2068|  
|Португальский|2070|2070|  
|Финский|1035|1035|  
|Шведский|1053|1053|  
|Чешский|1029|1029|  
|Венгерский|1038|1038|  
|Польский|1045|1045|  
|Румынский|1048|1048|  
|Хорватский|1050|1050|  
|Словацкий|1051|1051|  
|Slovene|1060|1060|  
|Греческий|1032|1032|  
|Болгарский|1026|1026|  
|русском языке|1049|1049|  
|Турецкий|1055|1055|  
|British English|2057|1033|  
|Эстонский|1061|1061|  
|Латышский|1062|1062|  
|Литовский|1063|1063|  
|Португальский (Бразилия)|1046|1046|  
|Китайский (традиционный)|1028|1028|  
|Корейский|1042|1042|  
|Китайский (упрощенный)|2052|2052|  
|Арабский|1025|1025|  
|Тайский|1054|1054|  
  
## <a name="see-also"></a>См. также:  
 [Представления совместимости &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
