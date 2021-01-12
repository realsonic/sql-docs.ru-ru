---
description: sys.xml_indexes (Transact-SQL)
title: sys.xml_indexes (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_indexes_TSQL
- xml_indexes_TSQL
- sys.xml_indexes
- xml_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_indexes catalog view
ms.assetid: 3408de72-b067-4fda-b5d5-8e856dfd9db3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e83cc5aa28c8d657237893a036da84ec1f4c944
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097934"
---
# <a name="sysxml_indexes-transact-sql"></a>sys.xml_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке для каждого XML-индекса.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Наследует столбцы из [sys. indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).|  
|**using_xml_index_id**|**int**|NULL = первичный XML-индекс.<br /><br /> Не NULL = вторичный XML-индекс.<br /><br /> Значение, не равное NULL, является ссылкой самосоединения на первичный XML-индекс.|  
|**secondary_type**|**char(1)**|Описание типа вторичного индекса:<br /><br /> вторичный XML-индекс P = PATH;<br /><br /> вторичный XML-индекс V = VALUE;<br /><br /> вторичный XML-индекс R = PROPERTY;<br /><br /> NULL = первичный XML-индекс.|  
|**secondary_type_desc**|**nvarchar(60)**|Описание типа вторичного индекса:<br /><br /> PATH = вторичный XML-индекс PATH;<br /><br /> VALUE = вторичный XML-индекс VALUE;<br /><br /> PROPERTY = вторичный XML-индекс PROPERTY;<br /><br /> NULL = первичный XML-индекс.|  
|**xml_index_type**|**tinyint**|Тип индекса:<br /><br /> 0 = первичный XML-индекс<br /><br /> 1 = вторичный XML-индекс<br /><br /> 2 = выборочный XML-индекс<br /><br /> 3 = вспомогательный выборочный XML-индекс|  
|**xml_index_type_description**|**nvarchar(60)**|Описание типа индекса.<br /><br /> PRIMARY_XML<br /><br /> Вторичный XML-индекс<br /><br /> Выборочный XML-индекс<br /><br /> Вспомогательный выборочный XML-индекс|  
|**path_id**|**int**|Значение NULL для всех XML-индексов, за исключением вспомогательного выборочного XML-индекса.<br /><br /> В противном случае — идентификатор повышенного пути, по которому строится вспомогательный выборочный XML-индекс. Это значение совпадает со значением path_id из системного представления sys.selective_xml_index_paths.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
