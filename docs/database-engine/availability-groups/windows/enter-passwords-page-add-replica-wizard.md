---
title: Мастер добавления реплики. Страница ввода паролей для групп доступности
description: Описание свойств на странице ввода паролей в мастере добавления реплики SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.addreplicawizard.enterpasswords.f1
ms.assetid: e69207a0-c5c4-44e4-ae9a-4afbb67251d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 632082f11b386fde86a9e89595cf13302cd1bc25
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643558"
---
# <a name="enter-passwords-page-add-replica-wizard-for-always-on-availability-groups"></a>Страница ввода паролей (мастер добавления реплики) для групп доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе справки описываются параметры, приведенные на странице **Ввод паролей** . Эта тема относится к [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 Если реплики, выбранные на странице **Выбор реплик** , содержат базы данных с главным ключом, откроется страница "Ввод паролей".  
  
## <a name="enter-passwords-options"></a>Параметры ввода паролей  
 В сетке **Пользовательские базы данных в этом экземпляре SQL Server** перечислены все локальные пользовательские базы данных. Существуют следующие столбцы.  
  
 **имя**;  
 Отображает имя локальной пользовательской базы данных.  
  
 **Размер**  
 Отображает размер базы данных, если он доступен мастеру.  
  
 **Состояние**  
 Указывает режим **Требуется пароль** для базы данных с главным ключом. Введя пароли для главных ключей базы данных в столбце **Пароли** , нажмите кнопку **Обновить**. Если пароли введены правильно, в столбце **Состояние** будет указано **Пароль введен**.  
  
 Если база данных не содержит главный ключ, в столбце **Состояние** будет указано **Пароль не требуется**.  
  
 **Пароль**  
 Если в столбце **Состояние** указано **Требуется пароль**, введите пароль для главного ключа базы данных.  
  
 **Обновить**  
 Щелкните, чтобы обновить сетку. Мы рекомендуем сделать это после ввода необходимых паролей.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
-   [Использование мастера добавления реплики в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>См. также:  
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
