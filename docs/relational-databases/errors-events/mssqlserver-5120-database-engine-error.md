---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 331c35a269d63f9f71c3d0fce59485bd08b88df4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638833"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|5120|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|DSK_FCB_FAILURE|  
|Текст сообщения|Ошибка таблицы: Не удалось открыть физический файл "%.*ls". Ошибка операционной системы %d: "%ls".|  
  
## <a name="explanation"></a>Объяснение  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удалось открыть файл базы данных.  Ошибка операционной системы, о которой говорится в сообщении, указывает на конкретные первопричины сбоя. Обычно эта ошибка возникает вместе с другими ошибками, такими как [17204](mssqlserver-17204-database-engine-error.md) или [17207](mssqlserver-17207-database-engine-error.md).
  
## <a name="user-action"></a>Рекомендуемые действия  
  
  Определите причину, исправьте ошибку операционной системы, затем еще раз попытайтесь выполнить операцию. Есть несколько состояний, сведения о которых могут помочь корпорации Майкрософт уменьшить область изучения для определения ошибки. 
  
### <a name="access-is-denied"></a>Доступ запрещен 
В случае возникновения ошибки операционной системы 5 (`Access is Denied`), рассмотрите следующие методы устранения:
   -  Проверьте разрешения, заданные в файле, просмотрев свойства файла в проводнике Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует группы Windows, чтобы подготовить службу контроля доступа для различных ресурсов файла. Убедитесь, что соответствующая группа (с именем SQLServerMSSQLUser$ComputerName$MSSQLSERVER или SQLServerMSSQLUser$ComputerName$InstanceName) имеет необходимые разрешения для файла базы данных, указанного в сообщении об ошибке. Дополнительные сведения см. в статье [Настройка разрешений файловой системы для доступа к компоненту ядра СУБД](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Убедитесь, что группа Windows содержит учетную запись запуска службы или идентификатор безопасности службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
   -  Проверьте учетную запись пользователя, от имени которой сейчас запущена служба [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для получения этих сведений можно использовать диспетчер задач Windows. Найдите значение "Имя пользователя" для исполняемого файла "sqlservr.exe". Если вы недавно изменили учетную запись службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], поддерживаемым способом выполнения этой операции является использование служебной программы [Диспетчер конфигурации SQL Server](../sql-server-configuration-manager.md). 
   -  В зависимости от типа операции — открытие баз данных во время запуска сервера, присоединение базы данных, восстановление базы данных и т. д. — учетная запись, используемая для олицетворения и получения доступа к файлу базы данных, может варьироваться. Сведения о том, какая операция позволяет задавать разрешения к каким учетным записям, см. в статье [Защита данных и файлов журналов](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)). Используйте такие средства, как [Process Monitor](/sysinternals/downloads/procmon), чтобы узнать, каким образом предоставляется доступ к файлу: в контексте безопасности учетной записи запуска службы экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (или идентификатора безопасности службы) либо олицетворенной учетной записи.

      Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] олицетворяет учетные данные для входа пользователя, выполняющего операцию ALTER DATABASE или CREATE DATABASE, в средстве Process Monitor отобразятся, например, следующие сведения.
      
        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>Подключение файлов, размещенных в запоминающем устройстве, подключаемом к сети  
Если не удается повторно подключить базу данных, которая находится в запоминающем устройстве, подключаемом к сети, это сообщение может быть зарегистрировано в журнале приложений.

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

Эта проблема возникает из-за того, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сбрасывает разрешения файлов при отключении базы данных. При попытке повторно подключить базу данных происходит сбой из-за ограниченных разрешений общего доступа.

Чтобы устранить эту проблему, сделайте следующее.
1. Используйте параметр запуска -T для запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте этот параметр запуска, чтобы включить флаг трассировки 1802 в [диспетчере конфигурации SQL Server](../sql-server-configuration-manager.md) (сведения о 1802 см. в разделе [Флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)). Дополнительные сведения об изменении параметров запуска см. в разделе [Параметры запуска службы ядра СУБД](../../database-engine/configure-windows/database-engine-service-startup-options.md).

2. Отключить базу данных можно с помощью приведенной ниже команды.
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. Восстановить подключение базы данных можно с помощью приведенной ниже команды.
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
