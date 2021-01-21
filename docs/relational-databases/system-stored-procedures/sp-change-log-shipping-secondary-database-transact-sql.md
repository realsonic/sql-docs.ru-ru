---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 81a5c562ddade5c60458ad491a401dab916c59d4
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620298"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Меняет настройки базы данных-получателя.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @restore_delay = ] 'restore_delay'` Время в минутах, в течение которого сервер-получатель ждет перед восстановлением заданного файла резервной копии. *restore_delay* имеет **тип int** и не может иметь значение null. Значение по умолчанию — 0.  
  
`[ @restore_all = ] 'restore_all'` Если задано значение 1, сервер-получатель восстанавливает все доступные резервные копии журналов транзакций при выполнении задания восстановления. В противном случае восстанавливается один файл. *restore_all* имеет **бит** и не может иметь значение null.  
  
`[ @restore_mode = ] 'restore_mode'` Режим восстановления для базы данных-получателя.  
  
 0 = восстановить журнал с аргументом NORECOVERY;  
  
 1 = восстановление журнала с аргументом STANDBY.  
  
 значение *RESTORE* **bit** и не может быть равно null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Если задано значение 1, то при выполнении операции восстановления пользователи отключаются от базы данных-получателя. По умолчанию равно 0. *disconnect_users* имеет **бит** и не может иметь значение null.  
  
`[ @block_size = ] 'block_size'` Размер в байтах, используемый в качестве размера блока для устройства резервного копирования. *block_size* имеет **тип int** и значение по умолчанию-1.  
  
`[ @buffer_count = ] 'buffer_count'` Общее число буферов, используемых операцией резервного копирования или восстановления. *buffer_count* имеет **тип int** и значение по умолчанию-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Размер (в байтах) максимального запроса ввода или вывода, выданного [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устройством резервного копирования. *max_transfersize* имеет **тип int** и может иметь значение null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Количество минут между операциями восстановления до создания предупреждения. *restore_threshold* имеет **тип int** и не может иметь значение null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Предупреждение, создаваемое при превышении порогового значения восстановления. *threshold_alert* имеет **тип int** и значение по умолчанию 14421.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Указывает, будет ли создаваться предупреждение при превышении *restore_threshold*. 1 = включены; 0 = отключены. *threshold_alert_enabled* имеет **бит** и не может иметь значение null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Продолжительность времени в минутах, в течение которого будет храниться журнал. *history_retention_period* имеет **тип int**. Если не указано, будет использоваться значение 1440.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 **sp_change_log_shipping_secondary_database** должны запускаться из базы данных **master** на сервере-получателе. Эта хранимая процедура выполняет следующее:  
  
1.  При необходимости изменяет параметры в записях **log_shipping_secondary_database** .  
  
2.  При необходимости изменяет запись локального монитора в **log_shipping_monitor_secondary** на сервере-получателе, используя указанные аргументы.  

## <a name="permissions"></a>Разрешения  
 Эту процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
 В этом примере показано использование **sp_change_log_shipping_secondary_database** для обновления параметров базы данных-получателя **LogShipAdventureWorks**.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
