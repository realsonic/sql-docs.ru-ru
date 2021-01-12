---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: c7a3153a34088b6b75c9b51f0cf8829049edfc6e
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797786"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|15401|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|SEC_INVALIDLOGINORGROUP|
|Текст сообщения|Не найден пользователь или группа Windows NT "%s". Проверьте имя еще раз.|
||

## <a name="explanation"></a>Пояснение

Эта ошибка возникает, когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удается создать имя для входа на основе субъекта Windows (например, пользователя домена или группы домена Windows). Пользователю выводится сообщение об ошибке наподобие следующего:

> Ошибка 15401. Не найден пользователь или группа Windows NT "%s". Проверьте имя еще раз.

## <a name="cause"></a>Причина

Эта ошибка может возникать по одной из следующих причин:

- имя для входа не существует в Active Directory;
- контроллер домена недоступен;
- при добавлении локальной учетной записи в качестве имени домена не используется BUILTIN;
- существуют проблемы с разрешениями имен.

## <a name="user-action"></a>Рекомендуемые действия

В следующих разделах приведены действия, которые можно предпринять для каждой из описанных выше причин.

### <a name="verify-the-login-you-are-trying-to-add"></a>Проверьте имя для входа, которое вы пытаетесь добавить

1. Убедитесь, что имя для входа в Windows по-прежнему существует в домене. Ваш администратор сети мог удалить имя для входа в Windows по определенным причинам, и вам, возможно, не удастся предоставить ему доступ к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
1. Убедитесь, что введены правильные имена домена и имени для входа, а также, что вы используете следующий формат: `Domain\User`
1. Если имя для входа существует, и оно правильное, но вы по-прежнему получаете сообщение об ошибке, перейдите к следующим разделам этой статьи.

### <a name="verify-if-the-domain-controller-is-available"></a>Проверьте доступность контроллера домена

Если контроллер домена, в котором находится имя для входа (тот же или другой домен), недоступен по какой-либо причине, может появиться сообщение об ошибке 15401.

Если имя для входа находится в домене, отличном от домена [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], убедитесь, что между доменами существуют правильные доверительные отношения.

Убедитесь, что контроллер домена имени для входа доступен, выполнив команду проверки связи с компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Проверьте IP-адрес и имя контроллера домена.

### <a name="verify-the-domain-name-for-local-accounts"></a>Проверьте доменное имя для локальных учетных записей

Для локальных (не доменных) учетных записей требуется специальная обработка. Если вы пытаетесь добавить локальную учетную запись с локального компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], убедитесь, что в качестве имени домена используется BUILTIN.

### <a name="check-for-name-resolution-issues"></a>Проверьте, нет ли проблем с разрешениями имен

При возникновении проблем с разрешением имени компьютера, который используется при добавлении имени для входа или группы, может появиться сообщение об ошибке 15401.

Убедитесь, что ваш механизм разрешения имен (например, WINS, DNS, HOSTS или LMHOSTS) настроен правильно.

## <a name="see-also"></a>См. также раздел

- [Тестирование канала между локальным компьютером и его доменом](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](/sql/relational-databases/system-stored-procedures/sp-change-users-login-transact-sql)
