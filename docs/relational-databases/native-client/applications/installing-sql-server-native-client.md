---
title: Установка
description: SQL Server Native Client 11,0 устанавливается с SQL Server 2016. Узнайте, где устанавливаются компоненты. Существует также распространяемая программа установки.
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
author: markingmyname
ms.author: maghan
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, uninstalling
- SQLNCLI, installing
- SQLNCLI, uninstalling
- Setup [SQL Server Native Client]
- uninstalling SQL Server Native Client
- data access [SQL Server Native Client], uninstalling SQL Server Native Client
- installing SQL Server Native Client
- SQL Server Native Client, installing
- data access [SQL Server Native Client], installing SQL Server Native Client
- removing SQL Server Native Client
ms.assetid: c6abeab2-0052-49c9-be79-cfbc50bff5c1
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 73c8e7f7656c927f65a6fb1b1989f14110e4e7b4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171306"
---
# <a name="installing-sql-server-native-client"></a>Установка собственного клиента SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 устанавливается при установке [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)]. 
 
 Нет собственного клиента SQL Server 2016. Дополнительные сведения см. в разделе [SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client.md). 
 
Также можно извлечь файл sqlncli.msi с веб-страницы пакета дополнительных компонентов SQL Server 2012. Чтобы загрузить самую последнюю версию SQL Server Native Client, перейдите на страницу [Microsoft® SQL Server® 2012 с пакетом дополнительных компонентов](https://www.microsoft.com/download/details.aspx?id=56041). Если [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на компьютере также установлена предыдущая версия собственного клиента, чем SQL Server 2012, то [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] собственный клиент 11,0 будет устанавливаться параллельно с более ранней версией.  
  
 Файлы собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (sqlncli11.dll, sqlnclir11.rll и s11ch_sqlncli.chm) устанавливаются в следующий каталог.  
  
 `%SYSTEMROOT%\system32\`  
  
> [!NOTE]  
>  Все необходимые настройки реестра для поставщика OLE DB собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и драйвера ODBC собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] выполняются в процессе установки.  
  
 Заголовочный файл и библиотека собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (sqlncli.h и sqlncli11.lib) устанавливаются в следующий каталог.  
  
 `%PROGRAMFILES%\Microsoft SQL Server\110\SDK`  
  
 Помимо установки Native Client [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], в рамках установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имеется распространяемый установщик sqlncli.msi, который можно найти на установочном диске [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в следующем каталоге: `%CD%\Setup\`.  
  
 Собственный клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] можно распространять в виде файла sqlncli.msi. При развертывании приложения может потребоваться установка собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Один из способов установки нескольких пакетов в одном (с точки зрения пользователя) сеансе установки состоит в применении технологии построителей цепочек и загрузчиков. Дополнительные сведения см. в статьях [Разработка пользовательского пакета начального загрузчика для Visual Studio 2005](/previous-versions/aa730839(v=vs.80)) и [Добавление настраиваемых необходимых компонентов](/visualstudio/deployment/creating-bootstrapper-packages).  
  
 Версии файла sqlncli.msi для архитектур x64 и Itanium устанавливают и 64-разрядную версию, и 32-разрядную версию собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если приложение планируется использовать на платформе, отличной от той, на которой оно разрабатывалось, можно скачать из центра загрузки Майкрософт версии sqlncli.msi for x64, Itanium и x86.  
  
 При инициировании sqlncli.msi по умолчанию устанавливаются только компоненты клиентской части. Клиентские компоненты — это файлы, поддерживающие запуск приложения, разработанного с помощью [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] собственного клиента. Чтобы установить также компоненты пакета SDK, укажите в командной строке `ADDLOCAL=All`. Пример:  
  
 `msiexec /i sqlncli.msi ADDLOCAL=ALL APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
## <a name="silent-install"></a>Автоматическая установка  
 При использовании параметров /passive, /qn, /qb или /qr программы msiexec необходимо также указать параметр IACCEPTSQLNCLILICENSETERMS=YES, тем самым явно подтверждая принятие условий соглашения конечного пользователя. Этот параметр указывается только прописными буквами.  
  
## <a name="uninstalling-sql-server-native-client"></a>Удаление SQL Server Native Client  
 Поскольку такие приложения [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , как сервер и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] средства, зависят от [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] собственного клиента, важно не удалять [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] собственный клиент, пока не будут удалены все зависимые приложения. Для пользователей с предупреждением о том, что приложение зависит от [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] собственного клиента, используйте параметр APPGUID Install в MSI, как показано ниже.  
  
 `msiexec /i sqlncli.msi APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
 Значением, передаваемым в APPGUID, служит код продукта. Код продукта необходимо создать при использовании установщика (Майкрософт) для формирования пакета установки приложения.  
  
## <a name="see-also"></a>См. также:  
 [Создание приложений с помощью SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)   
 [Инструкции по установке](/previous-versions/sql/)  
  
