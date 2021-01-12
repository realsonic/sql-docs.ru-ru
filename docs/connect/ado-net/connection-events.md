---
title: События подключений
description: События подключений позволяют получать информационные сообщения от источника данных и определять, изменилось ли его состояние.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8151915dc6c16c6225fec9ab90cb5a88e86b992f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771454"
---
# <a name="connection-events"></a>События подключений

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Поставщик данных Microsoft SqlClient для SQL Server имеет объекты **Connection** с двумя событиями, которые можно использовать для получения информационных сообщений из источника данных или для определения того, изменилось ли состояние объекта **Connection**. В следующей таблице описываются события объекта **Connection**.

|Событие|Описание|  
|-----------|-----------------|  
|**InfoMessage**|Возникает, когда из источника данных возвращается информационное сообщение. Информационные сообщения - это сообщения из источника данных, которые не приводят к формированию исключения.|  
|**StateChange**|Возникает при изменении состояния объекта **Connection**.|  

## <a name="work-with-the-infomessage-event"></a>Работа с событием InfoMessage

При помощи события <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> объекта <xref:Microsoft.Data.SqlClient.SqlConnection> можно получать предупреждения и информационные сообщения из источника данных SQL Server. Ошибки со степенью серьезности от 11 до 16, возвращаемые из источника данных, вызывают формирование исключения. Однако событие <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> также можно использовать, чтобы получать сообщения из источника данных, которые не связаны с ошибками. В случае с Microsoft SQL Server любая ошибка с серьезностью 10 или меньше считается информационным сообщением, их можно отслеживать при помощи события <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>. Дополнительные сведения см. в статье [Степени серьезности ошибок компонента Database Engine](/sql/relational-databases/errors-events/database-engine-error-severities).

Событие <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> принимает объект <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs>, в свойстве **Errors** которого содержится коллекция сообщений из источника данных. У объектов **Error** этой коллекции можно запрашивать номер ошибки и текст сообщения, а также сведения об источнике ошибки. Поставщик данных Microsoft SqlClient для SQL Server также включает сведения о базе данных, хранимой процедуре и номере строки, связанной с сообщением.

### <a name="example"></a>Пример

В следующем примере кода показано, как добавлять обработчик для события <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handle-errors-as-infomessages"></a>Обработка ошибок как событий InfoMessages

Событие <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> обычно вызывается только для информационных и предупреждающих сообщений, которые отправляются с сервера. Однако когда возникает реальная ошибка, выполнение методов **ExecuteNonQuery** или **ExecuteReader**, которые инициировали серверную операцию, приостанавливается с формированием исключения.

Если требуется продолжить обработку остальных инструкций команды несмотря ни на какие ошибки, выдаваемые сервером, следует задать свойству <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A> объекта <xref:Microsoft.Data.SqlClient.SqlConnection> значение `true`. В этом случае соединение вызовет для ошибок событие <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>, а не будет формировать исключение и прерывать обработку. После этого клиентское приложение сможет обработать это событие и отреагировать на условия ошибки.

> [!NOTE]
> Ошибка со степенью серьезности 17 и выше, в результате которой сервер прекращает обработку команды, должна обрабатываться как исключение. В этом случае исключение формируется независимо от того, как обрабатывается ошибка в событии <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

## <a name="work-with-the-statechange-event"></a>Работа с событием StateChange

Событие **StateChange** возникает при изменении состояния объекта **Connection**. Событие **StateChange** воспринимается объектом <xref:System.Data.StateChangeEventArgs>, который позволяет определять состояние объекта **Connection** при помощи свойств **OriginalState** и **CurrentState**. Свойство **OriginalState** является перечислением <xref:System.Data.ConnectionState>, которое указывает состояние объекта **Connection** до его изменения. Свойство **CurrentState** является перечислением <xref:System.Data.ConnectionState>, которое указывает состояние объекта **Connection** после его изменения.

В следующем примере кода событие **StateChange** используется, чтобы написать сообщение в консольном окне при изменении состояния объекта **Connection**.

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>См. также

- [подключение к источнику данных](connecting-to-data-source.md);
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
