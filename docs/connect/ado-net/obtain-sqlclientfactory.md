---
title: Получение SqlClientFactory
description: Сведения о том, как получить SqlClientFactory из класса DbProviderFactories для работы с конкретными источниками данных в .NET.
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771778"
---
# <a name="obtain-a-sqlclientfactory"></a>Получение SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Процесс получения <xref:System.Data.Common.DbProviderFactory> состоит из передачи сведений о поставщике данных классу <xref:System.Data.Common.DbProviderFactories>. На основе этих сведений метод <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> создает строго типизированную фабрику поставщика. Например, чтобы создать <xref:Microsoft.Data.SqlClient.SqlClientFactory>, можно передать `GetFactory` строку с именем поставщика, указанным как "**Microsoft.Data.SqlClient**".

Другая перегрузка метода `GetFactory` принимает <xref:System.Data.DataRow>. После создания фабрики поставщика можно использовать ее методы для создания дополнительных объектов. К методам фабрики `SqlClientFactory` относятся <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A> и <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.

## <a name="register-sqlclientfactory"></a>Регистрация SqlClientFactory

Чтобы получить объект <xref:Microsoft.Data.SqlClient.SqlClientFactory> с помощью класса <xref:System.Data.Common.DbProviderFactories> в .NET Framework, необходимо зарегистрировать его в файле **App.config** или **web.config**. В следующем фрагменте файла конфигурации показан синтаксис и формат для <xref:Microsoft.Data.SqlClient>.  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

Атрибут **invariant** определяет базовый поставщик данных. Этот трехкомпонентный синтаксис имени также применяется при создании новой фабрики и для определения поставщика в файле конфигурации, чтобы имя поставщика вместе со связанной с ним строкой соединения можно было получать во время выполнения.  

> [!NOTE]  
> Так как в .NET Core отсутствует поддержка GAC или глобальной конфигурации, объект <xref:Microsoft.Data.SqlClient.SqlClientFactory> должен быть зарегистрирован путем вызова метода <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A> в проекте.

В следующем образце кода показано использование синтаксиса <xref:Microsoft.Data.SqlClient.SqlClientFactory> в основном приложении .NET.

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>См. также раздел

- [DbProviderFactories](dbproviderfactories.md)
- [Строки подключения](connection-strings.md)
- [Использование классов конфигурации](/previous-versions/aspnet/ms228063(v=vs.100))
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
