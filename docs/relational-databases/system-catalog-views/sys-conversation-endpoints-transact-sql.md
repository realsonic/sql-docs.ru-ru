---
description: sys.conversation_endpoints (Transact-SQL)
title: sys.conversation_endpoints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- conversation_endpoints_TSQL
- conversation_endpoints
- sys.conversation_endpoints
- sys.conversation_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.conversation_endpoints catalog view
ms.assetid: 2ed758bc-2a9d-4831-8da2-4b80e218f3ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e62cd4fb5c39a8b24ca25f49e381a23a32186643
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100697"
---
# <a name="sysconversation_endpoints-transact-sql"></a>sys.conversation_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Каждая сторона диалога компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] представляется конечной точкой диалога. Это представление каталога содержит одну запись на каждую конечную точку диалога в базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|conversation_handle|**uniqueidentifier**|Идентификатор данной конечной точки диалога. Не допускает значения NULL.|  
|conversation_id|**uniqueidentifier**|Идентификатор диалога. , совместно используемый обеими сторонами диалога. В паре с содержимым столбца is_initiator уникален в рамках одной этой базы данных. Не допускает значения NULL.|  
|is_initiator|**tinyint**|Является ли данная конечная точка инициатором или целевой точкой диалога.  Не допускает значения NULL.<br /><br /> 1 = инициатор;<br /><br /> 0 = целевая точка.|  
|service_contract_id|**int**|Идентификатор контракта для диалога. Не допускает значения NULL.|  
|conversation_group_id|**uniqueidentifier**|Идентификатор группы сообщений, которой принадлежит данный диалог. Не допускает значения NULL.|  
|service_id|**int**|Идентификатор службы для этой стороны диалога. Не допускает значения NULL.|  
|lifetime|**datetime**|Дата и время окончания диалога. Не допускает значения NULL.|  
|Состояние|**char(2)**|Текущее состояние диалога. Не допускает значения NULL. Одно из двух значений:<br /><br /> Итак, начата отправка исходящих сообщений. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обработал инструкцию BEGIN CONVERSATION для этого диалога, но пока не отправлено ни одного сообщения.<br /><br /> SI   Начат прием данных. Другой экземпляр запущен в новом диалоге с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] еще не до конца получил первое сообщение. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может создать диалог в этом состоянии в том случае, если первая передача фрагментирована или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] получает передачу вне порядка. Однако [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может создать диалог сразу в состоянии CO (ведение диалога), если первая передача, полученная для диалога, содержит первое сообщение целиком.<br /><br /> СОВМЕСТНОе обращение. Диалог открыт, и обе стороны диалога могут отправлять сообщения. Для большинства служб большая часть обмена данными происходит, когда диалог находится в этом состоянии.<br /><br /> DI отключен входящий трафик. Удаленной стороной диалога была выполнена инструкция END CONVERSATION. Состояние диалога остается неизменным до тех пор, пока локальная сторона диалога не выдаст сообщение END CONVERSATION. Приложение может продолжать получать сообщения для диалога. Поскольку удаленная сторона диалога закончила диалог, приложение не может отправлять сообщения в этом диалоге. Когда приложение выполняет инструкцию END CONVERSATION, диалог переходит в состояние CD (закрыто).<br /><br /> DO   Отключена отправка. Локальной стороной диалога была выполнена инструкция END CONVERSATION. Состояние диалога остается неизменным до тех пор, пока удаленная часть диалога не подтвердит сообщение END CONVERSATION. Приложение не может отправлять и получать сообщения для диалога. Когда удаленная сторона диалога подтверждает сообщение END CONVERSATION, диалог переходит в состояние CD (закрыто).<br /><br /> Ошибка ER. В данной конечной точке произошла ошибка. Сообщение об ошибке помещено в очередь приложений. Если очередь приложений пуста, значит это сообщение об ошибке уже обработано приложением.<br /><br /> CD   Закрыто. Конечная точка диалога больше не используется.|  
|state_desc|**nvarchar(60)**|Описание состояния диалогового окна конечной точки. Этот столбец допускает значение NULL. Одно из двух значений:<br /><br /> **STARTED_OUTBOUND**<br /><br /> **STARTED_INBOUND**<br /><br /> **ДИАЛОГ**<br /><br /> **DISCONNECTED_INBOUND**<br /><br /> **DISCONNECTED_OUTBOUND**<br /><br /> **ЗАВЕРШИЛ**<br /><br /> **ошибка**|  
|far_service|**nvarchar(256)**|Название службы на удаленной стороне диалога. Не допускает значения NULL.|  
|far_broker_instance|**nvarchar(128)**|Экземпляр брокера на удаленной стороне диалога. Допускает значение NULL.|  
|principal_id|**int**|Идентификатор участника, чей сертификат используется на локальной стороне диалога. Не допускает значения NULL.|  
|far_principal_id|**int**|Идентификатор пользователя, чей сертификат используется удаленной стороной диалога. Не допускает значения NULL.|  
|outbound_session_key_identifier|**uniqueidentifier**|Идентификатор исходящего ключа шифрования данного диалога. Не допускает значения NULL.|  
|inbound_session_key_identifier|**uniqueidentifier**|Идентификатор входящего ключа шифрования данного диалога. Не допускает значения NULL.|  
|security_timestamp|**datetime**|Время создания локального сеансового ключа сеанса. Не допускает значения NULL.|  
|dialog_timer|**datetime**|Время, когда таймер диалога отправляет сообщение DialogTimer. Не допускает значения NULL.|  
|send_sequence|**bigint**|Номер следующего сообщения в последовательности отправки. Не допускает значения NULL.|  
|last_send_tran_id|**двоичный (6)**|Внутренний идентификатор транзакции для отправки сообщения. Не допускает значения NULL.|  
|end_dialog_sequence|**bigint**|Порядковый номер сообщения End Dialog. Не допускает значения NULL.|  
|receive_sequence|**bigint**|Номер следующего сообщения, ожидаемого в последовательности приема. Не допускает значения NULL.|  
|receive_sequence_frag|**int**|Номер следующего фрагмента сообщения, ожидаемого в последовательности приема. Не допускает значения NULL.|  
|system_sequence|**bigint**|Порядковый номер сообщения последнего системного сообщения для этого диалога. Не допускает значения NULL.|  
|first_out_of_order_sequence|**bigint**|Порядковый номер первого ошибочного сообщения для этого диалога. Не допускает значения NULL.|  
|last_out_of_order_sequence|**bigint**|Порядковый номер последнего ошибочного сообщения для этого диалога. Не допускает значения NULL.|  
|last_out_of_order_frag|**int**|Порядковый номер последнего ошибочного фрагмента сообщения для этого диалога. Не допускает значения NULL.|  
|is_system|**bit**|1, если это системный диалог. Не допускает значения NULL.|  
|priority|**tinyint**|Приоритет диалога, назначенный для этой конечной точки диалога. Не допускает значения NULL.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
