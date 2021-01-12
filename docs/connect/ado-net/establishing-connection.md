---
title: Установка подключения
description: Рекомендации по подключению к SQL Server с поставщиком SqlClient.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b3d92f49fb4eae6184a673ffc25ff6bbdf521626
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771596"
---
# <a name="establishing-connection"></a>Установка подключения

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Для создания подключения к Microsoft SQL Server используется объект <xref:Microsoft.Data.SqlClient.SqlConnection> поставщика данных Microsoft SqlClient для SQL Server. Сведения о безопасном хранении и извлечении строк подключения см. в статье [Защита сведений о подключении](protecting-connection-information.md).

## <a name="closing-connections"></a>Закрытие подключений

Рекомендуется всегда закрывать соединение после использования, чтобы обеспечить его возврат в пул. Блок `Using` в Visual Basic или C# автоматически удаляет соединение при выходе в коде из блока даже при наличии необработанного исключения. Дополнительные сведения об операторе using см. [здесь](/dotnet/csharp/language-reference/keywords/using-statement) и [здесь](/dotnet/visual-basic/language-reference/statements/using-statement).

Можно также использовать методы `Close` или `Dispose` объекта Connection. Соединения, которые явно не закрыты, нельзя добавить или вернуть в пул. Например, соединение, которое вышло за пределы области, но явно закрыто не было, будет возвращено в пул соединений только в том случае, если был достигнут максимальный размер этого пула, а соединение еще действует.

> [!NOTE]
> В методе `Finalize` вашего класса нельзя вызывать методы `Close` или `Dispose` объектов **Connection**, **DataReader** или любого другого управляемого объекта. В методе завершения следует только освобождать неуправляемые ресурсы, которыми ваш класс непосредственно владеет. Если класс не владеет какими-либо неуправляемыми ресурсами, не включайте в его определение метод `Finalize`. Дополнительные сведения см. в статье [Сборка мусора](/dotnet/standard/garbage-collection/index).

> [!NOTE]
> События входа в систему и выхода из системы не вызываются на сервере при выборке подключения из пула подключений и при возврате его в пул подключений, поскольку при возврате в пул подключений подключение фактически не закрывается. Дополнительные сведения см. в разделе [Пулы подключений SQL Server (ADO.NET)](sql-server-connection-pooling.md).

## <a name="connecting-to-sql-server"></a>Подключение к SQL Server

Сведения о допустимых именах и значениях формата строки см. в свойстве <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> объекта <xref:Microsoft.Data.SqlClient.SqlConnection>. Можно также использовать класс <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> для создания синтаксически правильных строк соединения во время выполнения. Дополнительные сведения см. в статье [Connection String Builders](connection-string-builders.md) (Построители строк подключения).

В следующем примере кода демонстрируется способ создания и открытия соединения с базой данных SQL Server.

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>Встроенные функции безопасности и ASP.NET

Встроенные функции безопасности (доверенные соединения) SQL Server помогают обеспечить защиту при подключении к SQL Server, позволяя не передавать идентификатор пользователя и пароль в строке подключения, поэтому именно этот метод проверки подлинности подключения рекомендуется к использованию. Встроенная безопасность основана на использовании текущего идентификатора безопасности, или маркера выполняемого процесса. Для классических приложений этим удостоверением обычно является идентификатор выполнившего вход пользователя.

Идентификатор безопасности приложений ASP.NET может быть настроен на получение одного из нескольких различных параметров. Дополнительные сведения об идентификаторе безопасности, который используется в приложении ASP.NET при соединении с SQL Server, см. в статьях [Олицетворение ASP.NET](/previous-versions/aspnet/xh507fc5(v=vs.100)), [Проверка подлинности ASP.NET ](/previous-versions/aspnet/eeyk640h(v=vs.100)) и практическом руководстве [ Доступ к SQL Server с помощью встроенных функций безопасности Windows](/previous-versions/aspnet/bsz5788z(v=vs.100)).

## <a name="see-also"></a>См. также

- [подключение к источнику данных](connecting-to-data-source.md);
- [Строки подключения](connection-strings.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
