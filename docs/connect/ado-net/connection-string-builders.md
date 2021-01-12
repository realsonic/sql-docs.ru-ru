---
title: Построители строк подключения
description: Сведения о классах построителя строк подключения, которые используются в ADO.NET для разных поставщиков и наследуются от DbConnectionStringBuilder.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 38c1ad34fb4819460c08f00da4c1dbd31749b7cb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771440"
---
# <a name="connection-string-builders"></a>Построители строк подключения

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

В ранних версиях ADO.NET проверка строк подключения со сцепленными строковыми значениями во время компиляции не выполнялась, поэтому во время выполнения неправильное ключевое слово приводило к вызову <xref:System.ArgumentException>. Поставщик данных Microsoft SqlClient для SQL Server содержит строго типизированный класс построителя строк подключения <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType>, наследующий от <xref:System.Data.Common.DbConnectionStringBuilder>.

## <a name="connection-string-injection-attacks"></a>Атаки путем внедрения кода в строку подключения

Атака путем внедрения данных в строку соединения может произойти при использовании динамического объединения строк для построения строк соединения, основанных на входных данных пользователя. Если строка не проверяется, а вредоносный текст или символы не экранируются, злоумышленник может получить потенциальный доступ к конфиденциальным данным или другим ресурсам сервера. Например, злоумышленник может осуществить атаку, установив точку с запятой и добавив дополнительное значение. Строка подключения анализируется по алгоритму **побеждает последний**, и недопустимые входные данные заменяются допустимыми значениями.

Классы построителей строк соединения созданы для устранения предположений и защиты от синтаксических ошибок и уязвимостей системы безопасности. Они предоставляют методы и свойства, соответствующие известным парам "ключ — значение", разрешенные поставщиком данных. Каждый класс поддерживает фиксированную коллекцию синонимов и может переводить синоним в соответствующее общеизвестное ключевое имя. Выполняются проверки на допустимые пары «ключ-значение», и недопустимая пара вызывает исключение. Кроме того, внедренные значения обрабатываются безопасным образом.

В следующем примере демонстрируется обработка с помощью объекта <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> дополнительного значения для параметра `Initial Catalog`.

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

Выходные данные показывают, что <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> выполняет обработку правильно, то есть экранирует лишние значения двойными кавычками, а не добавляет их в строку подключения в виде новой пары "ключ — значение".

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="build-connection-strings-from-configuration-files"></a>Создание строк подключения из файлов конфигурации

Если некоторые элементы строки соединения известны заранее, их можно сохранить в файле конфигурации и во время выполнения получить для построения полной строки соединения. Например, в отличие от имени сервера, имя базы данных может быть известно заранее. Также можно принудительно задать ввод пользователем имени и пароля во время выполнения, чтобы исключить возможность внедрения других значений в строку соединения.

Один из перегруженных конструкторов для построителя строки соединения принимает в качестве аргумента значение типа <xref:System.String>, что позволяет использовать частичную строку соединения, которую впоследствии пользователь может дополнить. Частичную строку соединения можно сохранить в файле конфигурации и получить во время выполнения.

> [!NOTE]
> Пространство имен <xref:System.Configuration> обеспечивает программный доступ к файлам конфигурации, предоставляя класс <xref:System.Web.Configuration.WebConfigurationManager> для веб-приложений и класс <xref:System.Configuration.ConfigurationManager> для приложений Windows. См. дополнительные сведения в статье [Строки подключения и файлы конфигурации](connection-strings-and-configuration-files.md).

### <a name="example"></a>Пример

В этом примере демонстрируется получение частичной строки соединения из файла конфигурации и ее завершение путем установки свойств <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> и <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> для объекта <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. Файл конфигурации определяется следующим образом.

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> Для запуска кода необходимо задать в проекте ссылку на библиотеку `System.Configuration.dll`.

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>См. также

- [Строки подключения](connection-strings.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
