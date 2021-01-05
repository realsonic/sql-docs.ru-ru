---
title: Страница "Выполнение" (мастеры групп доступности AlwaysOn)
description: Описание параметров на странице "Выполнение" мастера группы доступности Always On в SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.adddatabasewizard.progress.f1
- sql13.swb.addreplicawizard.progress.f1
- sql13.swb.newagwizard.progress.f1
ms.assetid: bd3b0306-8384-4120-a1c9-03825f0ae26a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 040b89181744c0f9a3ab036554987dcb1e01d3de
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642522"
---
# <a name="progress-page-always-on-availability-group-wizards"></a>Страница "Выполнение" (мастеры групп доступности AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Используйте это диалоговое окно для просмотра шагов мастера [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , запущенного в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Индикатор выполнения указывает относительный прогресс шагов мастера.  
  
## <a name="ui-element-list"></a>Список элементов пользовательского интерфейса  
 **Дополнительные сведения**  
 Щелкните стрелку вниз, чтобы отобразить сетку выполнения, в которой по порядку отображены любые завершенные шаги, за которыми отображается текущая операция. Сетка содержит следующие столбцы:  
  
 **имя**;  
 Отображает фразу, которая описывает конкретный шаг.  
  
 **Состояние**  
 Показывает результат завершенных шагов и процент завершения текущего шага.  
  
|Результат|Описание|  
|------------|-----------------|  
|**Error**|Указывает, что при выполнении операции в этом шаге произошла ошибка. Щелкните ссылку, чтобы отобразить диалоговое окно с сообщением, описывающим ошибку.|  
|**Выполняется (** *процент завершения* **)**|Указывает, что операция происходит сейчас и выполняет оценку процентной доли завершения этого шага.|  
|**Успешно**|Указывает, что операция, выполнявшаяся в этом шаге, завершилась успешно.|  
  
 **Меньше сведений**  
 Выберите, чтобы скрыть сетку хода выполнения.  
  
 **Отмена**  
 Нажмите, чтобы пропустить любые остающиеся операции и завершить работу мастера.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Использование диалогового окна "Создание группы доступности" (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Использование мастера добавления реплики в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Использование мастера добавления базы данных в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [Использование мастера отработки отказа группы доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
