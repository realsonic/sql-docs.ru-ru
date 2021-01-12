---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc9b58549bbc20fe66caaf2adc61edd4345acd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096661"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Содержит одну строку для каждого сообщения, обработанного компонентом Database Mail. Используйте это представление для просмотра состояния всех сообщений.  
  
 Чтобы просмотреть только сообщения с состоянием сбоя, используйте [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Чтобы просмотреть только неотправленные сообщения, используйте [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Чтобы просмотреть только отправленные сообщения, используйте [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Идентификатор почтового отправления в очереди почты.|  
|**profile_id**|**int**|Идентификатор профиля, используемого для отсылки этого сообщения.|  
|**пользу**|**varchar(max)**|Электронные адреса получателей сообщения.|  
|**copy_recipients**|**varchar(max)**|Электронные адреса получателей копий сообщения.|  
|**blind_copy_recipients**|**varchar(max)**|Электронные адреса получателей копий сообщения, чьи имена не будут отображаться в заголовке сообщения.|  
|**subject**|**nvarchar (510)**|Строка темы сообщения.|  
|**body**|**varchar(max)**|Тело сообщения.|  
|**body_format**|**varchar (20)**|Формат тела сообщения. Допустимые значения — TEXT и HTML.|  
|**importance**|**varchar (6)**|Параметр **важности** сообщения.|  
|**чувствительности**|**varchar (12)**|Параметр **чувствительности** сообщения.|  
|**file_attachments**|**varchar(max)**|Список имен файлов, разделенных точкой с запятой, который прикреплен к сообщению электронной почты.|  
|**attachment_encoding**|**varchar (20)**|Тип вложения.|  
|**запрос**|**varchar(max)**|Запрос, выполненный почтовой программой.|  
|**execute_query_database**|**sysname**|Контекст базы данных, в котором почтовая программа выполнила запрос.|  
|**attach_query_result_as_file**|**bit**|Если значение равно 0, результаты запроса были включены в тело сообщения после его содержимого. Если значение равно 1, результаты были возвращены в виде вложения.|  
|**query_result_header**|**bit**|Если значение равно 1, результаты запроса содержали заголовки столбцов. Если значение равно 0, результаты запроса не включали заголовков столбцов.|  
|**query_result_width**|**int**|Параметр **query_result_width** сообщения.|  
|**query_result_separator**|**char(1)**|Символ, используемый для разделения столбцов в выходных данных запроса.|  
|**exclude_query_output**|**bit**|Параметр **exclude_query_output** сообщения. Дополнительные сведения см. в разделе [sp_send_dbmail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Параметр **append_query_error** сообщения. 0 означает, что компонент Database Mail не отсылает электронное сообщение, если в запросе содержится ошибка.|  
|**send_request_date**|**datetime**|Дата и время помещения сообщения в очередь почты.|  
|**send_request_user**|**sysname**|Пользователь, отправивший сообщение. Это пользовательский контекст процедуры компонента Database Mail, а не поле «От:» с именем отправителя сообщения.|  
|**sent_account_id**|**int**|Идентификатор учетной записи компонента Database Mail, используемой для отсылки этого сообщения.|  
|**sent_status**|**varchar (8)**|Состояние почты. Возможны следующие значения:<br /><br /> **Отправлено** — почта отправлена.<br /><br /> **неотправленные** — компонент Database Mail по-прежнему пытается отправить сообщение.<br /><br /> При повторной **попытке** Database Mail не удалось отправить сообщение, но предпринимается попытка его отправки.<br /><br /> **сбой** . компоненту Database Mail не удалось отправить сообщение.|  
|**sent_date**|**datetime**|Дата и время отсылки сообщения.|  
|**last_mod_date**|**datetime**|Дата и время последнего изменения строки.|  
|**last_mod_user**|**sysname**|Пользователь, внесший последнее изменение в строку.|  
  
## <a name="remarks"></a>Комментарии  
 Используйте представление **sysmail_allitems** , чтобы просмотреть состояние всех сообщений, обрабатываемых Database Mail. Это представление может быть полезным при выявлении неполадок в работе компонента Database Mail. Оно помогает выявлять сущность проблемы, отображая атрибуты отправленных сообщений в сравнении с атрибутами неотправленных сообщений.  
  
 Системные таблицы, представленные в этом представлении, содержат все сообщения и могут привести к увеличению размера базы данных **msdb** . Чтобы уменьшить размеры таблиц, регулярно удаляйте из этого представления старые сообщения. Дополнительные сведения см. в разделе [создание агент SQL Server задания для архивации Database Mail сообщений и журналов событий](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Разрешения  
 Предоставлено предопределенной роли сервера **sysadmin** и роли базы данных **DatabaseMailUserRole** . При выполнении членом предопределенной роли сервера **sysadmin** в этом представлении отображаются все сообщения. Все остальные пользователи могут видеть лишь сообщения, отправленные ими.  
  
  
