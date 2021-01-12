---
description: sys.firewall_rules (база данных SQL Azure)
title: sys.firewall_rules (база данных SQL Azure) | Документация Майкрософт
ms.date: 03/26/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.firewall_rules
- firewall_rules
- sys.firewall_rules_TSQL
- firewall_rules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- firewall_rules
- sys.firewall_rules
ms.assetid: 140d2cd8-9aa1-4cc5-870d-e1dbc873b3fe
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 351ca530c49051b61b6f44b8dc2fbff83623cf8f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097999"
---
# <a name="sysfirewall_rules-azure-sql-database"></a>sys.firewall_rules (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Возвращает сведения о параметрах брандмауэра на уровне сервера, связанных с [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
 Представление `sys.firewall_rules` содержит следующие столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|идентификатор|**INT**|Идентификатор параметра брандмауэра на уровне сервера.|  
|name|**NVARCHAR (128)**|Имя, выбранное для описания и определения параметров брандмауэра на уровне сервера.|  
|start_ip_address|**VARCHAR (45)**|Наименьший IP-адрес в диапазоне параметра брандмауэра на уровне сервера. IP-адреса, которые больше этого адреса или равны ему, могут попытаться подключиться к серверу [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Наименьший возможный IP-адрес: `0.0.0.0`.|  
|end_ip_address|**VARCHAR (45)**|Наибольший IP-адрес в диапазоне параметра брандмауэра на уровне сервера. IP-адреса, которые меньше этого адреса или равны ему, могут попытаться подключиться к серверу [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Наибольший возможный IP-адрес: `255.255.255.255`.<br /><br /> Примечание. попытки подключения Azure разрешены, если это поле и поле **start_ip_address** равны `0.0.0.0` .|  
|create_date|**DATETIME**|Дата и время создания параметра брандмауэра на уровне сервера в формате UTC.<br /><br /> Примечание. время в формате UTC представляет собой аббревиатуру времени в формате UTC.|  
|modify_date|**DATETIME**|Дата и время последнего изменения параметра брандмауэра на уровне сервера в формате UTC.|  
  
## <a name="remarks"></a>Комментарии

 Чтобы получить сведения о параметрах брандмауэра уровня базы данных, связанных с База данных SQL Microsoft Azure, используйте [sys.database_firewall_rules &#40;базе данных SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md).  
  
## <a name="permissions"></a>Разрешения

 Доступ только для чтения к этому представлению доступен всем пользователям с разрешением на подключение к базе данных **master** .  
  
## <a name="see-also"></a>См. также:

[sp_set_firewall_rule (база данных Azure SQL)](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
[sp_delete_firewall_rule &#40;базе данных SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)   
[sp_set_database_firewall_rule (база данных Azure SQL)](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
[sp_delete_database_firewall_rule &#40;базе данных SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)  
[sys.database_firewall_rules &#40;базе данных SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
[Настройка брандмауэра Windows для доступа к ядро СУБД](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)     
[Настройка брандмауэра для доступа к FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)  
[настроить брандмауэр для доступа к серверу отчетов](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md) 
