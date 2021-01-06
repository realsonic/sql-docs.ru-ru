---
title: Мастер группы доступности. Добавление IP-адреса
description: 'Описываются параметры в диалоговом окне "Добавление IP-адреса", доступном на странице "Выбор реплик" мастера создания групп доступности в SQL Server Management Studio. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilitygrouplistener.addipaddress.f1
ms.assetid: 98c9ad3b-ff3c-4c1d-b344-59a72fca137c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6b6f405786daf40e1241af48ee3e442cea998a6f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643788"
---
# <a name="add-ip-address-dialog-box-sql-server-management-studio"></a>Диалоговое окно «Добавление IP-адреса» (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе справки F1 описываются параметры диалогового окна **Добавление IP-адреса** . Это диалоговое окно открывается из диалогового окна **Создание прослушивателя группы доступности** и из вкладки **Прослушиватель** на странице **Выбор реплик** в [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] или [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)][!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
## <a name="prerequisites"></a>Предварительные требования  
 Прежде чем начинать добавлять подсети в прослушиватель группы доступности, убедитесь, что вы знаете IP-адрес для каждой подсети, а для IPv4-адреса — маску подсети.  
  
##  <a name="add-ip-address-options"></a><a name="PageOptions"></a> Варианты добавления IP-адреса  
 **Подсеть**  
 В раскрывающемся списке выберите адрес подсети, добавляемой в прослушиватель группы доступности. По умолчанию подсеть принимает адреса как IPv4, так и IPv6. При первом использовании диалогового окна **Добавление IP-адреса** в раскрывающемся списке **Подсеть** отображаются оба адреса подсетей для каждой подсети, в которой размещается реплика группы доступности. Чтобы добавить данную подсеть в прослушиватель, выберите один из адресов этой подсети.  
  
 По завершении работы с диалоговым окном **Добавление IP-адреса** нажмите кнопку **OK** , чтобы добавить выбранный адрес подсети в прослушиватель, причем этот адрес исчезнет из раскрывающегося списка **Подсеть** . Все невыбранные адреса подсетей останутся в раскрывающемся списке. Следует добавлять только один адрес каждой подсети в прослушиватель, иначе создание прослушивателя завершится с ошибкой.  
  
 **Адреса**  
 Воспользуйтесь этим полем для ввода статического IP-адреса для выбранного адреса подсети. Для получения этого IP-адреса обратитесь к системному администратору. Проверьте правильность введенного адреса для выбранного адреса подсети, иначе создание прослушивателя завершится с ошибкой.  
  
 **Адрес IPv4**  
 Если вы выбрали для подсети адрес IPv4, введите в этом поле допустимый статический адрес IPv4.  
  
 **Маска подсети**  
 Для адреса IPv4 это доступное только для чтения поле отображает маску подсети для выбранной подсети.  
  
 **Адрес IPv6**  
 Если вы выбрали для подсети адрес IPv6, введите в этом поле допустимый статический адрес IPv6.  
  
 **OK**  
 Щелкните, чтобы добавить подсеть с выбранным адресом, а также указанным статическим IP-адресом. Строка с этими значениями будет добавлена в сетку подсети в диалоговом окне **Создание прослушивателя группы доступности** или **Выбор реплик** .  
  
> [!IMPORTANT]  
>  В диалоговом окне **Добавление IP-адреса** IP-адрес не проверяется. Кроме того, диалоговое окно не мешает добавить второй адрес подсети для подсети, уже добавленной в прослушиватель группы доступности.  
  
 **Отмена**  
 Щелкните, чтобы отменить выбор и вернуться в диалоговое окно **Создание прослушивателя группы доступности** или на вкладку **Прослушиватель** без добавления статического IP-адреса для подсети.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Использование диалогового окна "Создание группы доступности" (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Использование мастера добавления реплики в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Подключение клиента AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
  
