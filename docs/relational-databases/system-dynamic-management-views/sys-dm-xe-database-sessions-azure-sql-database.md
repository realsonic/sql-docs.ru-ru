---
description: sys.dm_xe_database_sessions (база данных SQL Azure)
title: sys.dm_xe_database_sessions (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 33ea5179-16bb-4abd-96cc-9bc696e80987
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: d800d67e59bb72c6120ec786adaf5863a80bdf5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093952"
---
# <a name="sysdm_xe_database_sessions-azure-sql-database"></a>sys.dm_xe_database_sessions (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Возвращает сведения о событиях сеанса. События представляют собой дискретные точки выполнения. Предикаты можно применять к событиям, чтобы остановить их запуск, если событие не содержит необходимых данных.  
  
||  
|-|  
|**Применимо к** версии [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 12 и всем более поздним версиям.|  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|Адрес сеанса событий в памяти. Не допускает значение NULL.|  
|event_name|**nvarchar(60)**|Имя события, к которому привязано действие. Не допускает значение NULL.|  
|event_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, в котором содержится событие. Не допускает значение NULL.|  
|event_predicate|**nvarchar (2048)**|XML-представление дерева предиката, применяемого к событию. Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Количество элементов связей  
По меньшей мере 2015-07-13 "sys.dm_xe_objects" является одним из этих XEvents DMV, которые не содержат "_database" в имени. Не опечатка или ошибка в правом столбце следующей таблицы. Это имя совпадает в Microsoft SQL Server и базе данных SQL Azure.  
  
|От|Кому|Relationship|  
|--------|------|----------------|  
|sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_session_address|sys.dm_xe_database_sessions. Address|«многие к одному»|  
|sys. DM _xe_database_session_events. event_package_guid, sys. DM _xe_database_session_events. event_name|sys.dm_xe_objects.name, sys.dm_xe_objects.package_guid|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
[Расширенные события в Базе данных SQL Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
[Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
