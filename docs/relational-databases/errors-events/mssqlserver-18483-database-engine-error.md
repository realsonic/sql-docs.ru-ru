---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 40a75d8ad0bd6237b594f38bbb5cb83be8cca788
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797830"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|18483|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|REMLOGIN_INVALID_USER|
|Текст сообщения|Невозможно подключиться к серверу "%.ls", так как "%. ls" не определено как удаленное имя входа на сервер. Убедитесь, что имя входа указано правильно. %.*ls.|
||

## <a name="explanation"></a>Пояснение

Эта ошибка возникает при попытке настроить распространитель репликации в системе, которая была восстановлена с помощью образа жесткого диска другого компьютера, на котором изначально был установлен экземпляр SQL Server. Пользователю выводится сообщение об ошибке наподобие следующего:

> Службе SQL Server Management Studio не удалось настроить "\<Server>\<Instance>" в качестве распространителя для "\<Server>\<Instance>". Ошибка 18483: Не удалось подключиться к серверу "\<Server>\<Instance>", поскольку имя "distributor_admin" не определено как удаленное имя входа на сервере. Убедитесь, что имя входа указано правильно. %.*ls.

## <a name="cause"></a>Причина

При развертывании [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из образа жесткого диска другого компьютера, на котором установлен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имя сети компьютера, на котором установлен образ, сохраняется в новой установке. Неправильное имя сети приводит к сбою настройки распространителя репликации. Та же проблема возникает при переименовании компьютера после установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="user-action"></a>Рекомендуемые действия

Чтобы обойти эту проблему, замените имя сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на правильное имя сети компьютера. Для этого выполните следующие действия.

1. Войдите в компьютер, на котором вы развертывали [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из образа диска, а затем выполните следующую инструкцию Transact-SQL в SSMS:

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. Перезапустите компьютер, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Чтобы убедиться, что имя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и имя сети компьютера совпадают, выполните следующую инструкцию Transact-SQL:

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>Дополнительные сведения

Вы можете использовать глобальную переменную `@@SERVERNAME` или функцию `SERVERPROPERTY`(ServerName) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы найти имя сети компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Свойство ServerName функции `SERVERPROPERTY` автоматически сообщает об изменении имени сети компьютера при перезагрузке компьютера и службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Глобальная переменная `@@SERVERNAME` сохраняет исходное имя компьютера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], пока имя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не будет сброшено вручную.

### <a name="steps-to-reproduce-the-problem"></a>Шаги для воспроизведения проблемы

На компьютере, на котором вы развернули [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из образа диска, выполните следующие действия.

1. Запустите среду [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].
2. В **обозревателе объектов** разверните имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Щелкните правой кнопкой мыши папку **Репликация** и выберите пункт **Configure distribution Replication** (Настроить репликацию распространения), а затем — **Configure Publishing, Subscribers, and Distribution** (Настроить публикацию, подписчиков и распространение).
4. В диалоговом окне мастера **Настройка распространения** нажмите кнопку **Далее**.
5. В диалоговом окне **Распространитель** щелкните, чтобы выбрать "\<**Server**>\<**Instance**>", который будет выступать в качестве собственного распространителя; SQL Server создаст базу данных распространителя и переключатель журнала, а затем нажмите кнопку **Далее**.
6. В диалоговом окне **Запуск агента SQL Server** нажмите кнопку **Далее**.
7. В диалоговом окне **Папка моментальных снимков** нажмите кнопку **Далее**.

    > [!NOTE]
    > Если появится сообщение с подтверждением пути к папке моментальных снимков, нажмите кнопку **Да**.
8. В диалоговом окне **База данных распространителя** нажмите кнопку **Далее**.
9. В диалоговом окне **Издатели** щелкните **Далее**.
10. В диалоговом окне **Действия мастера** нажмите кнопку **Далее**.
11. В диалоговом окне **Завершение работы мастера** нажмите кнопку **Завершить**.

## <a name="see-also"></a>См. также раздел

- [Переименование компьютера, на который установлен изолированный экземпляр SQL Server](/sql/database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server)
- [@@SERVERNAME (Transact-SQL)](/sql/t-sql/functions/servername-transact-sql)
- [SERVERPROPERTY (Transact-SQL)](/sql/t-sql/functions/serverproperty-transact-sql)
- [sp_addserver (Transact-SQL)](/sql/relational-databases/system-stored-procedures/sp-addserver-transact-sql)
