---
title: sqlagent90, приложение
description: Приложение sqlagent90 запускает агент SQL Server из командной строки. Используйте его при диагностике агента SQL Server или при наличии указаний поставщика поддержки.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- starting SQL Server Agent
- sqlagent90 application
- SQL Server Agent, starting
- command prompt utilities [SQL Server], sqlagent90
ms.assetid: e8b80e8d-d0c9-4500-a868-0ce08233da08
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 63f185f686d21c5c9a490bee83d2c9afaf449b8f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641894"
---
# <a name="sqlagent90-application"></a>sqlagent90, приложение
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Приложение **sqlagent90** запускает агент [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] из командной строки. Обычно агент [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] должен запускаться из среды [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] или с помощью методов SQL-DMO в приложении. Запускайте приложение **sqlagent90** из командной строки только, когда выполняется диагностика агента [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или если такое указание получено от основного поставщика поддержки.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sqlagent90  
-c [-v] [-i instance_name]  
```  
  
## <a name="arguments"></a>Аргументы  
 **-c**  
 Указывает, что агент [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запускается из командной строки и не зависит от диспетчера управления службами Microsoft Windows. При использовании параметра **-c** агентом [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] нельзя управлять с помощью оснастки "Службы" в элементе "Администрирование" панели управления и из диспетчера конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Этот аргумент является обязательным.  
  
 **-v**  
 Указывает, что агент [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] работает в подробном режиме и производит запись диагностических данных в окно командной строки. Диагностические данные — это те же данные, которые записываются в журнал ошибок агента [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 **-i** *имя_экземпляра*  
 Указывает, что агент [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] подключается к именованному экземпляру [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , определенному аргументом *имя_экземпляра*.  
  
## <a name="remarks"></a>Remarks  
 После отображения сообщения с указанием авторских прав приложение **sqlagent90** отображает выводимые данные в окне командной строки, только если указан параметр **-v** . Чтобы остановить работу приложения **sqlagent90**, нажмите клавиши CTRL+C в командной строке. Не закрывайте окно командной строки перед остановкой работы приложения **sqlagent90**.  
  
## <a name="see-also"></a>См. также:  
 [Задачи автоматизированного администрирования (агент SQL Server)](../ssms/agent/automated-administration-tasks-sql-server-agent.md)  
  
