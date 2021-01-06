---
title: Использование FILESTREAM и FileTable с группами доступности
description: Указания по использованию FILESTREAM или FileTable с базами данных, участвующими в группе доступности Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- FileTables [SQL Server], Availability Groups
- FILESTREAM [SQL Server], Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: fdceda9a-a9db-4d1d-8745-345992164a98
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 566c50867511e9e9d02996247c3efecf634b93aa
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644141"
---
# <a name="use-filestream-and-filetable-with-always-on-availability-groups"></a>Использование FILESTREAM и FileTable с группами доступности Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  В этом разделе содержатся сведения об использовании возможностей FILESTREAM и FileTable с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 Поддерживаются все функции FILESTREAM. После отработки отказа доступ к данным FILESTREAM можно получить как на доступных для чтения вторичных репликах, так и на новой первичной реплике.  
  
 Функции FileTable поддерживаются частично. После отработки отказа данные FileTable будут доступны в первичной реплике, при этом они не будут доступны на вторичных репликах, из которых можно выполнять чтение.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Перед добавлением в группу доступности базы данных, в которой используется FILESTREAM (а также возможно и FileTable), следует убедиться, что FILESTREAM поддерживается на всех экземплярах серверов, на которых размещены реплики доступности для группы доступности. Дополнительные сведения см. в статье [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md).  
  
##  <a name="using-virtual-network-names-vnns-for-filestream-and-filetable-access"></a><a name="vnn"></a> Использование имен виртуальной сети для доступа через FILESTREAM или FileTable.  
 Во время включения поддержки FILESTREAM для экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]создается ресурс уровня экземпляра, обеспечивающий доступ к данным FILESTREAM. Доступ к этому ресурсу осуществляется по имени компьютера в следующем формате.  
  
 `\\<computer_name>\<filestream_share_name>`  
  
 Однако в группе доступности AlwaysOn имя компьютера виртуализируется с использованием имени виртуальной сети (VNN). Если компьютер является основной репликой в группе доступности, а базы данных в группе доступности содержат данные FILESTREAM, то будет также создан ресурс в области имен виртуальной сети, обеспечивающий доступ к данным FILESTREAM. Это не повлияет на доступ Transact-SQL к данным FILESTREAM. Однако приложения, использующие API-интерфейсы файловой системы, должны использовать ресурс в области имен виртуальной сети, путь которого задается в следующем формате:  
  
 `\\<VNN>\<filestream_share_name>`  
  
 Ресурс в области имен виртуальной сети создается при наступлении одного из следующих событий.  
  
-   Необходимо добавить базу данных, содержащую данные FILESTREAM, в группу доступности AlwaysOn в первичной реплике. В этом случае ресурс `\\<computer_name>\<filestream_share_name>` уже существует. Создается ресурс `\\<VNN>\<filestream_share_name>` .  
  
-   Поддержка FILESTREAM включается для потокового доступа ввода-вывода для первичной реплики, имеющей группы доступности. Создаются следующие ресурсы.  
  
    1.  `\\<computer_name>\<filestream_share_name>`  
  
    2.  `\\<VNN1>\<filestream_share_name>` для группы доступности 1.  
  
    3.  `\\<VNN2>\<filestream_share_name>` для группы доступности 2.  
  
 Ресурсы в области имен виртуальной сети также распространяются во все вторичные реплики.  
  
 Если база данных, содержащая данные FILESTREAM или FileTable, принадлежит группе доступности AlwaysOn:  
  
-   Функции FILESTREAM и FileTable принимают или возвращают имена виртуальной сети, а не имена компьютеров. Дополнительные сведения об этих функциях см. в разделе [Функции Filestream и FileTable (Transact-SQL)](../../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   При осуществлении любого доступа к данным FILESTREAM или FileTable посредством API-интерфейса файловой системы будут использоваться имена виртуальной сети, а не имена компьютеров.  
  
 Если база данных входит в группу доступности, а ваше приложение пытается получить доступ к ресурсу по имени компьютера в формате `\\<computer_name>\<filestream_share_name>` , то выдается ошибка.  
  
 Если приложение пытается получить доступ к ресурсу с использованием пути в области имен виртуальной сети, а база данных не входит в группу доступности, запрос может завершиться успешно. В этом случае имя виртуальной сети будет разрешено в имя компьютера. Однако так поступать не рекомендуется, поскольку путь в области имен виртуальной сети перестанет функционировать при удалении группы доступности.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Включение и настройка FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)  
  
-   [Включение необходимых компонентов для таблицы FileTable](../../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
 Нет.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
