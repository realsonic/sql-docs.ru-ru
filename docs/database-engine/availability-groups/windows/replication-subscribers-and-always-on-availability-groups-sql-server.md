---
title: Подписчики репликации и группы доступности (SQL Server)
description: Сведения о том, что произойдет, если в группе доступности Always On, содержащей базу данных, которая является подписчиком репликации, выполняется отработка отказа в SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/08/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e566d450a19d3a488a8bc29b4459ec9daf5499a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644183"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>Подписчики репликации и группы доступности AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Если в группе доступности AlwaysOn, содержащей базу данных, которая является подписчиком репликации, выполняется отработка отказа, то подписка на репликацию может завершиться ошибкой. Для владельцев принудительной подписки на репликацию транзакций агент распространителя сможет продолжить репликацию автоматически после отработки отказа, если подписка была создана с использованием имени прослушивателя группы доступности. Для владельцев подписки по запросу на репликацию транзакций агент распространителя сможет продолжить репликацию автоматически после отработки отказа, если подписка была создана с использованием имени прослушивателя группы доступности, а исходный сервер подписчика настроен и запущен. Это обусловлено тем, что задания агента распространения создаются только для исходного подписчика (первичная реплика группы доступности). Для подписчиков на публикацию слиянием администратор репликации должен вручную перенастроить подписчик путем повторного создания подписки.  
  
## <a name="what-is-supported"></a>Что поддерживается  
 Репликация [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает автоматический переход на другой ресурс для издателя и подписчиков транзакций. Подписчики на публикацию слиянием могут входить в группу доступности, но после отработки отказа потребуется вручную настроить нового подписчика. Группы доступности нельзя объединять со сценариями WebSync и SQL Server Compact.  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>Создание подписок транзакций в среде AlwaysOn  
 Для настройки и отработки отказа группы доступности подписчика в отношении репликации транзакций выполните следующие действия.  
  
1.  Прежде чем создавать подписку, добавьте базу данных подписчика в соответствующую группу доступности AlwaysOn.  
  
2.  Добавьте прослушиватель группы доступности подписчика в качестве связанного сервера во все узлы группы доступности. Это действие гарантирует, что все возможные участники отработки отказа будут знать о существовании прослушивателя и смогут к нему подключиться.  
  
3.  Используя сценарий в **Создание Репликации Транзакции Принудительной Подписки** разделе ниже, создайте подписку, используя имя прослушивателя группы доступности подписчика. После отработки отказа имя прослушивателя всегда будет допустимым, а фактическое имя сервера подписчика будет зависеть от того, какой узел стал основным.  
  
    > [!NOTE]  
    >  Подписка должна быть создана с использованием сценария [!INCLUDE[tsql](../../../includes/tsql-md.md)] и не может быть создана с использованием [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Создание подписки по запросу.  
  
    1.  Используя пример скрипта, приведенный ниже в разделе **Создание подписки по запросу для репликации транзакций**, создайте подписку, указав имя прослушивателя группы доступности подписчика. 
   
    2.  После отработки отказа создайте задание агента распространителя в новой первичной реплике с помощью хранимой процедуры **sp_addpullsubscription_agent**. 
  
 Создавая подписку по запросу с базой данных подписки в группе доступности, после каждой отработки отказа рекомендуется отключать задание агента распространителя в старой первичной реплике и включать его в новой первичной реплике.  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>Создание Репликации Транзакции Принудительной Подписки  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  

## <a name="creating-a-transactional-replication-pull-subscription"></a>Создание подписки по запросу для репликации транзакций  
  
```  
-- commands to execute at the subscriber, in the subscriber database:  
use [<subscriber database name>]  
EXEC sp_addpullsubscription @publisher= N'<publisher name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>',
        @subscription_type = N'pull';
Go

EXEC sp_addpullsubscription_agent 
        @publisher =  N'<publisher name>', 
        @subscriber = N'<availability group listener name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>' ;
        @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>Возобновление работы агентов слияния после переноса группы доступности подписчика  
 Для репликации слиянием администратор репликации должен вручную перенастроить подписчик следующим образом.  
  
1.  Выполните процедуру **sp_subscription_cleanup** , чтобы удалить старую подписку для подписчика. Запустите действие на новой первичной реплике (которая раньше была вторичной репликой).  
  
2.  Создайте подписку заново, начиная с нового моментального снимка.  
  
> [!NOTE]  
>  Текущий процесс неудобен для подписчиков репликации слиянием, однако основным сценарием для репликации слиянием являются отключенные пользователи (ПК, ноутбуки, переносные устройства), которые не используют группы доступности AlwaysOn в подписчике.  
  
  
