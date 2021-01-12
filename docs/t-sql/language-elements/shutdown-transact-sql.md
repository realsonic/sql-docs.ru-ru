---
description: SHUTDOWN (Transact-SQL)
title: SHUTDOWN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHUTDOWN_TSQL
- SHUTDOWN
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server, stopping
- shutting down SQL Server
- SHUTDOWN statement
- stopping SQL Server
- immediately stopping SQL Server
ms.assetid: c8b03ff9-688c-4fe8-86e8-bd6bd401c9a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 680652ceacfe82ca4f2372d082b790e066f2692f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097034"
---
# <a name="shutdown-transact-sql"></a>SHUTDOWN (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Немедленно останавливает сервер SQL Server.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SHUTDOWN [ WITH NOWAIT ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 WITH NOWAIT  
 Необязательный элемент. Закрывает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], не выполнив контрольные точки в каждой базе данных. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] осуществляет выход после попытки завершить все пользовательские процессы. При перезапуске сервера ко всем незавершенным транзакциям применяется откат.  
  
## <a name="remarks"></a>Комментарии  
 Если не используется параметр WITHNOWAIT, SHUTDOWN завершает работу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] указанным ниже образом.  
  
1.  Отключает имена входа (за исключением членов предопределенных ролей сервера **sysadmin** и **serveradmin**).  
  
    > [!NOTE]  
    >  Для вывода списка всех текущих пользователей запустите **sp_who**.  
  
2.  Ожидает завершения выполняющихся инструкций Transact-SQL и хранимых процедур. Для вывода списка всех активных процессов и блокировок запустите процедуры **sp_who** или **sp_lock** соответственно.  
  
3.  Вставляет контрольную точку в каждую базу данных.  
  
 Использование инструкции SHUTDOWN снижает до минимума объем автоматических операций восстановления, когда члены предопределенной роли сервера **sysadmin** перезапускают [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Для остановки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать другие инструменты и методы. Каждый из них выполняет контрольные точки во всех базах данных. Можно сбросить зафиксированные данные из кэша данных и остановить сервер:  
  
-   с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   запустив команду **net stop mssqlserver** из командной строки для экземпляра по умолчанию или запустив команду **net stop mssql$**_instancename_ из командной строки для именованного экземпляра;  
  
-   с помощью служб на панели управления.  
  
 Если файл **sqlservr.exe** был запущен из командной строки, нажатие клавиш CTRL+C завершает работу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако при нажатии CTRL+C не вставляются контрольные точки.  
  
> [!NOTE]  
>  Использование любого из этих методов для остановки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отправляет сообщение `SERVICE_CONTROL_STOP` в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Разрешения  
 Разрешения SHUTDOWN назначаются только членам предопределенных ролей сервера **sysadmin** и **serveradmin**, и эти разрешения не могут передаваться.  
  
## <a name="see-also"></a>См. также:  
 [CHECKPOINT (Transact-SQL)](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sp_lock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Приложение sqlservr](../../tools/sqlservr-application.md)   
 [Запуск, остановка, приостановка, возобновление и перезапуск компонента Database Engine, агента SQL и службы браузера SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
