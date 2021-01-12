---
description: sys.service_message_types (Transact-SQL)
title: sys.service_message_types (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_message_types
- service_message_types
- sys.service_message_types_TSQL
- service_message_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_message_types catalog view
ms.assetid: 6a38709a-60fe-46f6-89da-718f74f15600
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ab7a81f1f288ad533fa6a32ccec6e1de8dbad9d7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097884"
---
# <a name="sysservice_message_types-transact-sql"></a>sys.service_message_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Это представление каталога содержит по строке на каждый тип сообщений, зарегистрированный в компоненте Service Broker.
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя типа сообщений, уникальное в пределах базы данных. Не допускает значения NULL.|  
|**message_type_id**|**int**|Идентификатор типа сообщений, уникальный в пределах базы данных. Не допускает значения NULL.|  
|**principal_id**|**int**|Идентификатор владельца данного типа сообщений. Допускает значение NULL.|  
|**validation**|**char(2)**|Проверка, осуществляемая брокером перед отправкой сообщений данного типа. Не допускает значения NULL. Одно из двух значений:<br /><br /> N = нет<br /><br /> X = XML<br /><br /> E = пустая|  
|**validation_desc**|**nvarchar(60)**|Описание проверки, осуществляемой брокером перед отправкой сообщений данного типа. Допускает значение NULL. Одно из двух значений:<br /><br /> NONE<br /><br /> XML<br /><br /> EMPTY|  
|**xml_collection_id**|**int**|Если для проверки используется схема XML, — идентификатор используемой коллекции схем.<br /><br /> В противном случае — значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
