---
title: Установка репликации SQL Server | Документы Майкрософт
description: Установите компоненты репликации с помощью мастера установки SQL Server или через командную строку.
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b1995d22488693a31adb7c9a421f5a2e32de463d
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975488"
---
# <a name="install-sql-server-replication"></a>Установка репликации SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Компоненты репликации можно установить с помощью мастера установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или через командную строку. Репликацию можно установить при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]или при изменении существующего экземпляра.  
  
После установки компонентов репликации необходимо настроить сервер перед началом использования репликации. Дополнительные сведения см. в разделе [Настройка распространения](../../relational-databases/replication/configure-distribution.md) электронной документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
>[!IMPORTANT]  
>Если при изменении существующего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]устанавливаются компоненты репликации, то после завершения установки необходимо перезапустить агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это действие гарантирует, что агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] распознает подсистемы агента репликации и сможет обращаться к агентам репликации из шагов задания.  
  
## <a name="installing-replication-by-using-setup"></a>Установка репликации с помощью программы установки  
**Установка репликации при установке нового экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Чтобы установить компоненты репликации, включая объекты RMO, на странице **Выбор компонентов** мастера установки выберите **Репликация SQL Server** .  
  
## <a name="installing-replication-from-the-command-prompt"></a>Установка репликации из командной строки  
 **Установка репликации при установке нового экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- См. раздел [Установка SQL Server из командной строки](./install-sql-server-from-the-command-prompt.md).  
  
## <a name="see-also"></a>См. также раздел  
 [Установка SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [Установка SQL Server из командной строки](./install-sql-server-from-the-command-prompt.md)   
 [Функции, поддерживаемые выпусками SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) и [Функции, поддерживаемые выпусками SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
  
