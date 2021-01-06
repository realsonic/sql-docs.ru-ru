---
title: Страница выбора баз данных (мастер создания группы доступности/добавления базы данных)
description: Описание страницы "Выбор баз данных" для мастеров создания группы доступности и добавления баз данных в графическом интерфейсе SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectdatabases.f1
- sql13.swb.newagwizard.selectdatabases.f1
ms.assetid: 929c5e15-d087-438d-b1f2-aa97c5f8bff8
author: cawrites
ms.author: chadam
ms.openlocfilehash: a6971231c42b91907ad668f5cb93c185f08c9120
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643350"
---
# <a name="select-databases-page-new-availability-group-wizard-and-add-database-wizard"></a>Select Databases Page (New Availability Group Wizard and Add Database Wizard)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описаны параметры на странице **Выбор баз данных** . Эта тема относится к [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] и [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="select-databases-options"></a><a name="PageOptions"></a> Выбор параметров баз данных  
 В сетке **Пользовательские базы данных в этом экземпляре SQL Server** перечислены все локальные пользовательские базы данных. Существуют следующие столбцы.  
  
 **Название**  
 Отображает имя локальной пользовательской базы данных.  

 **Размер**  
 Отображает размер базы данных, если он доступен мастеру.  
  
 **Состояние**  
 Отображает гиперссылку, текст которой показывает, отвечает ли база данных требованиям по добавлению в группу доступности. Если состояние — «**Отвечает требованиям**», то базу данных можно добавить в группу доступности. Если база данных не отвечает всем требованиям, то по гиперссылке **Состояние** приводится краткое объяснение причин, по которым база данных не подходит. Для получения дополнительных сведений щелкните гиперссылку.  
  
 Можно выйти из мастера на странице **Выбор базы данных** на время действий, позволяющих сделать так, чтобы база данных отвечала требованиям. По возвращении на страницу **Выбор баз данных** нажмите кнопку **Обновить** для обновления сетки.  
  
 **Пароль**  
 Если база данных содержит главный ключ базы данных, введите пароль для него.  
  
 **Обновить**  
 Щелкните, чтобы обновить сетку. Это следует сделать после того, как были приняты действия по приведению базы данных в соответствие требованиям.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Использование диалогового окна "Создание группы доступности" (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Использование мастера добавления базы данных в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
