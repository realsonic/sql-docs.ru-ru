---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 42e8bf7a80659467c489d86b8a3ca944ceebe360
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797819"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|18482|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|REMLOGIN_INVALID_SITE|
|Текст сообщения|Невозможно подключиться к серверу "%.ls", так как сервер "%. ls" не определен как удаленный. Убедитесь, что имя сервера указано правильно. %.*ls|
||

## <a name="explanation"></a>Пояснение

Эта ошибка возникает при попытке выполнить удаленный вызов процедур (RPC) с одного сервера на другой (например, путем выполнения хранимой процедуры на удаленном компьютере, используя такую инструкцию, как `EXEC SERV_REMOTE.pubs..byroyalty`). Пользователю выводится сообщение об ошибке, аналогичное приведенному ниже

> Ошибка 18482. Невозможно подключиться к серверу \<SERV_REMOTE>, так как сервер \<SERV_REMOTE> не определен как удаленный. Убедитесь, что имя сервера указано правильно.

## <a name="cause"></a>Причина

Эта ошибка возникает, когда SQL Server не может выполнить удаленный вызов процедур. Это может быть вызвано неправильно настроенным локальным сервером. Чтобы выполнить удаленный вызов процедур, SQL Server сначала определяет локальный сервер, выполняя поиск имени сервера с переменной srvid = 0 в таблице sysservers. Если запись с переменной srvid = 0 не найдена в таблице sysservers или если имя сервера с переменной srvid = 0 относится к имени сервера, которое отличается от имени локального компьютера Windows, появится сообщение об ошибке.

## <a name="user-action"></a>Рекомендуемые действия

Чтобы определить, правильно ли настроен локальный сервер, просмотрите столбец `srvstatus` в master..sysservers. Для локального сервера это значение должно быть равно 0.

Предположим, например, что локальный сервер называется SERV_LOCAL, удаленный сервер — *SERV_REMOTE*, а таблица sysservers содержит следующие сведения:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

В предыдущих выходных данных SERV_LOCAL является локальным сервером, однако для srvid задано значение 1, когда оно должно быть равно 0. Чтобы исправить это, выполните следующие действия:

1. Выполните `sp_dropserver` local_server_name, droplogins (в этом примере следует выполнить `sp_dropserver SERV_LOCAL, droplogins`).
1. Выполните `sp_addserver` local_server_name, LOCAL (в этом примере следует выполнить `sp_addserver SERV_LOCAL, LOCAL`).
1. Остановите и перезапустите SQL Server.

После выполнения этих действий таблица sysservers должна выглядеть следующим образом:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> Для локального сервера значение идентификатора сервера (srvid) должно быть равно 0.

Возможны случаи, когда записи в таблице sysservers выглядят правильно, но при выполнении `select @@servername`возвращается значение NULL. В таких случаях для устранения проблемы по-прежнему нужно выполнить шаги с 1 по 3, перечисленные выше.

## <a name="more-information"></a>Дополнительные сведения

Это сообщение об ошибке может появиться при установке репликации, поскольку процесс установки выполняет удаленные вызовы процедур между серверами, участвующими в репликации.
