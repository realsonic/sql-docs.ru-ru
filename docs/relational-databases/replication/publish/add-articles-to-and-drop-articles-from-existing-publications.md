---
title: Добавление и удаление статей (существующие публикации)
description: Узнайте, как добавлять и удалять статьи из существующих публикаций для SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], dropping
- deleting articles
- removing articles
- dropping articles
- adding articles
- administering replication, articles
- publications [SQL Server replication], adding and dropping articles
- articles [SQL Server replication], adding
ms.assetid: b148e907-e1f2-483b-bdb2-59ea596efceb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 183619e1802b435fbc21c2b44e32848bf7e578de
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171906"
---
# <a name="add-articles-to-and-drop-articles-from-existing-publications"></a>Добавление и удаление статей в существующих публикациях
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Возможно добавлять и удалять статьи после создания публикации. Можно добавить статьи в любое время, но действия, необходимые для удаления статей, зависят от типа репликации и времени удаления статьи.  
  
## <a name="adding-articles"></a>добавление статей  
 Добавление статьи включает следующие операции: добавление статьи в публикацию, создание нового моментального снимка публикации, синхронизация подписки для применения схемы и данных для новой статьи.  
  
> [!NOTE]
>  Если при добавлении статьи в публикацию слиянием, существующая статья зависит от новой статьи, необходимо задать порядок обработки для обеих статей с помощью параметра **\@processing_order** процедур [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) и [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Рассмотрим следующий сценарий: необходимо опубликовать таблицу без публикации функции, на которую ссылается эта таблица. Если функция не будет опубликована, то таблица не сможет быть создана на подписчике. При добавлении функции к публикации: задайте значение **1** для параметра **\@processing_order** процедуры **sp_addmergearticle** и значение **2** для параметра **\@processing_order** процедуры **sp_changemergearticle**, указав имя таблицы в параметре **\@article**. Этот порядок обработки гарантирует создание функции на подписчике до создания таблицы, которая зависит от нее. Можно использовать различные числа для каждой статьи при условии, что число для функции меньше числа для таблицы.  
  
1.  Добавьте одну или несколько статей с помощью следующих методов:  
  
    -   [Добавление и удаление статей в существующих публикациях](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)  
  
    -   [Определение статьи](../../../relational-databases/replication/publish/define-an-article.md)  
  
2.  После добавления статьи в публикацию необходимо создать новый моментальный снимок для публикации (и все секции, если это публикация слиянием с параметризованными фильтрами). Затем агент распространителя или агент слияния копирует схему и данные для новой статьи на подписчик (он не инициализирует заново всю публикацию).  
  
    -   Сведения о создании моментального снимка см. в разделе [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Чтобы создать моментальный снимок публикации с параметризованными фильтрами, ознакомьтесь с [этой статьей](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
3.  После создания моментального снимка синхронизируйте подписку, чтобы скопировать схему и данные для новой статьи.  

    -   Сведения о синхронизации принудительной подписки см. в разделе [Synchronize a Push Subscription](../../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
    -   Сведения о синхронизации подписки по запросу см. в разделе [Synchronize a Pull Subscription](../../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
## <a name="dropping-articles"></a>удаление статей  
 Статьи могут быть удалены из публикации в любое время, но следует принять во внимание следующие аспекты:  
  
-   Удаление статьи из публикации не удаляет объект из базы данных публикации или соответствующий объект из базы данных подписки. Используйте DROP \<Object> для удаления этих объектов при необходимости. При удалении статьи, связанной с другими опубликованными статьями с помощью ограничений внешних ключей, мы рекомендуем удалить таблицу на подписчике вручную или выполнить скрипт по требованию: укажите скрипт, который содержит соответствующие инструкции DROP \<Object>. Дополнительные сведения см. в статье [Выполнение скриптов во время синхронизации (программирование репликации на языке Transact-SQL)](../../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md).  
  
-   Для публикации слиянием с уровнем совместимости 90RTM и выше возможно удаление статей в любое время, но требуется новый моментальный снимок. Дополнительно:  
  
    -   Если статья является родительской статьей в фильтре соединения или в отношении логической записи, сначала нужно удалить отношения, и это требует повторной инициализации.  
  
    -   Если статья содержит последний параметризованный фильтр в публикации, подписки должны быть повторно инициализированы.  
  
-   Для публикаций слиянием с уровнем совместимости ниже 90RTM статьи можно удалить без особых размышлений до первоначальной синхронизации подписок. Если статья удалена после синхронизации одной подписки или более, подписки должны быть удалены, заново созданы и синхронизированы.  
  
-   Для публикации моментальных снимков или публикации транзакций статьи можно удалить без особых размышлений до создания подписок. Если статья удалена после создания одной или более подписок, подписки должны быть удалены, заново созданы и синхронизированы. Дополнительные сведения об удалении подписок см. в статьях [Подписка на публикации](../../../relational-databases/replication/subscribe-to-publications.md) и [sp_dropsubscription (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). **sp_dropsubscription** позволяет удалить одну статью из подписки, а не всю подписку.  
  
1.  Удаление статьи из публикации включает удаление статьи и создание нового моментального снимка для публикации. Удаление статьи сделает текущий моментальный снимок недействительным; поэтому должен быть создан новый моментальный снимок.  
  
    -   Дополнительные сведения об удалении статьи из публикации см. в статьях [Add Articles to and Drop Articles from a Publication](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md) (Добавление и удаление статей в публикации) или [Delete an Article](../../../relational-databases/replication/publish/delete-an-article.md) (Удаление статьи).  
  
2.  После удаления статьи из публикации необходимо создать новый моментальный снимок для публикации (и все секции, если это публикация слиянием с параметризованными фильтрами).  
  
    -   Сведения о создании моментального снимка см. в разделе [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Чтобы создать моментальный снимок публикации с параметризованными фильтрами, ознакомьтесь с [этой статьей](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 Как указано выше, в некоторых случаях удаление статьи требует, чтобы подписки были удалены, заново созданы и синхронизированы. Дополнительные сведения см. в статьях [Подписка на публикации](../../../relational-databases/replication/subscribe-to-publications.md) и [Синхронизация данных](../../../relational-databases/replication/synchronize-data.md).  
 
 > [!NOTE]
 > **[!INCLUDE[ssSQL15](../../../includes/sssql14-md.md)] с пакетом обновления 2** или последующим и **[!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] с пакетом обновления 1** или последующим поддерживают удаление таблицы с помощью команды DDL **DROP TABLE** для статей, участвующих в репликации транзакций. Если DROP TABLE DDL поддерживается публикациями, операция DROP TABLE удалит таблицу из публикации и базы данных. Агент чтения журнала выполнит команду очистки для базы данных распространителя удаленной таблицы, а также очистку метаданных издателя. Если средство чтения журнала не обработало все записи журнала, ссылающиеся на удаленную таблицу, он пропустит новые команды, связанные с удаленной таблицей. Уже обработанные записи будут добавлены в базу данных распространителя. Они могут применяться в базе данных подписчика, если агент распространителя обработает их, прежде чем средство чтения журнала очистит устаревшие (удаленные) статьи. Параметр **по умолчанию** для всех публикаций репликации транзакций не поддерживает команду DLL DROP TABLE. Чтобы узнать больше об этом улучшении, ознакомьтесь со [статьей базы знаний 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1).

  
## <a name="see-also"></a>См. также:  
 [Публикация данных и объектов базы данных](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Повторная инициализация подписок](../../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Внесение изменений в схемы баз данных публикации](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
