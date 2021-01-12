---
title: Строки подключения и файлы конфигурации
description: Сведения о том, как хранить строки подключений для приложений ADO.NET в файле конфигурации в качестве лучшей методики для обеспечения безопасности и обслуживания.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 37df2641-661e-407a-a3fb-7bf9540f01e8
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8f1e0ac5fc52576f0e47ccb8be4b8e90899c6e68
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771466"
---
# <a name="connection-strings-and-configuration-files"></a>Строки подключения и файлы конфигурации

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Внедрение строк соединения в код приложения может привести к появлению уязвимых мест в системе безопасности и проблем с обслуживанием. Незашифрованные строки подключения, скомпилированные в исходный код приложения, можно просматривать с помощью средства [Ildasm.exe (IL Disassembler)](/dotnet/framework/tools/ildasm-exe-il-disassembler). Кроме того, после изменения строки соединения необходимо перекомпилировать приложение. По этим причинам рекомендуется хранить строки соединения в файле конфигурации приложения.

## <a name="work-with-application-configuration-files"></a>Работа с файлами конфигурации приложения

Файлы конфигурации приложения содержат настройки, относящиеся к отдельному приложению. Например, приложение ASP.NET может иметь один или несколько файлов **web.config**, а приложение Windows — дополнительный файл **app.config**. В файлах конфигурации присутствуют общие элементы, хотя имя и расположение любого файла конфигурации в значительной степени зависят от того, где размещается приложение.

### <a name="the-connectionstrings-section"></a>Раздел connectionStrings

Строки подключения могут храниться в виде пар "ключ/значение" в разделе **connectionStrings** элемента **configuration** файла конфигурации приложения. Дочерние элементы включают **add**, **clear** и **remove**.

Следующий фрагмент файла конфигурации демонстрирует схему и синтаксис хранения строки соединения. Атрибут **name** является именем, которое задано для уникальной идентификации строки подключения, чтобы ее можно было получить во время выполнения. **providerName** имеет значение **Microsoft.Data.SqlClient** (то есть поставщик данных Microsoft SqlClient для SQL Server).

```xml  
<?xml version='1.0' encoding='utf-8'?>  
  <configuration>  
    <connectionStrings>  
      <clear />  
      <add name="Name"
       providerName="Microsoft.Data.SqlClient"
       connectionString="Valid Connection String;" />  
    </connectionStrings>  
  </configuration>  
```  

> [!NOTE]
> Можно сохранить часть строки соединения в файле конфигурации и для ее дополнения во время выполнения использовать класс <xref:System.Data.Common.DbConnectionStringBuilder>. Это удобно в сценариях, в которых заранее неизвестны элементы строки соединения или желательно не сохранять конфиденциальные данные в файле конфигурации. Дополнительные сведения см. в статье [Connection String Builders](connection-string-builders.md) (Построители строк подключения).

### <a name="use-external-configuration-files"></a>Использование внешних файлов конфигурации

Внешние файлы конфигурации представляют собой отдельные файлы, каждый из которых содержит фрагмент файла конфигурации, состоящий из одного раздела. В таком случае основной файл конфигурации ссылается на внешний файл конфигурации. Хранение раздела **connectionStrings** в физически отдельном файле становится удобным в ситуациях, когда может потребоваться внесение изменений в строки подключения после развертывания приложения. Например, в ASP.NET по умолчанию после изменения файлов конфигурации осуществляется перезапуск домена приложения, что приводит к потере сведений о состоянии. Но изменение внешнего файла конфигурации не вызывает перезапуск приложения. Возможность применения внешних файлов конфигурации не ограничивается ASP.NET; их можно также использовать в приложениях Windows. Кроме того, для ограничения доступа к внешним файлам конфигурации могут использоваться средства обеспечения безопасности доступа к файлам и разрешения. Работа с внешними файлами конфигурации во время выполнения осуществляется в прозрачном режиме и не требует разработки специального кода.

Для хранения строк подключения во внешнем файле конфигурации создайте отдельный файл, содержащий единственный раздел **connectionStrings**. Не следует включать какие-либо дополнительные элементы, разделы или атрибуты. В данном примере показан синтаксис внешнего файла конфигурации.

```xml  
<connectionStrings>  
  <add name="Name"
   providerName="Microsoft.Data.SqlClient"
   connectionString="Valid Connection String;" />  
</connectionStrings>  
```  

 В основном файле конфигурации приложения для указания полного имени и расположения внешнего файла используется атрибут **configSource**. В следующем примере применяется ссылка на внешний файл конфигурации с именем `connections.config`.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
<configuration>  
    <connectionStrings configSource="connections.config"/>  
</configuration>  
```  

## <a name="retrieving-connection-strings-at-run-time"></a>Получение строк соединения во время выполнения

В .NET Framework 2.0 в пространстве имен <xref:System.Configuration> появились новые классы, упрощающие извлечение строк подключения из файлов конфигурации во время выполнения. Предусмотрена возможность получить строку соединения программным путем по ее имени или по имени поставщика.

> [!NOTE]
> Файл **machine.config** также содержит раздел **connectionStrings**, включающий строку подключения, используемую Visual Studio. При получении строк подключения из файла **app.config** приложения Windows с помощью имени поставщика в первую очередь загружаются строки подключений из файла **machine.config**, затем записи из файла **app.config**. Добавление ключевого слова **clear** сразу после элемента **connectionStrings** приводит к удалению из памяти всех ссылок, унаследованных от структуры данных, поэтому учитываются только строки подключений, определенные в локальном файле **app.config**.

### <a name="work-with-the-configuration-files"></a>Работа с файлами конфигурации

Начиная с версии .NET Framework 2.0, для работы с файлами конфигурации на локальном компьютере используется класс <xref:System.Configuration.ConfigurationManager>, который заменил устаревший класс <xref:System.Configuration.ConfigurationSettings>. <xref:System.Web.Configuration.WebConfigurationManager> служит для работы с файлами конфигурации ASP.NET. Он создан для работы с файлами конфигурации веб-сервера и предоставляет программный доступ к разделам файла конфигураций, например **system.web**.

> [!NOTE]
> Вызывающему объекту для доступа к файлам конфигурации во время выполнения должны быть предоставлены разрешения. Требуемые разрешения зависят от типа приложения, файла конфигурации и расположения. Дополнительные сведения см. в разделах [Использование классов конфигурации](/previous-versions/aspnet/ms228063(v=vs.100)) и <xref:System.Web.Configuration.WebConfigurationManager> для приложений ASP.NET и <xref:System.Configuration.ConfigurationManager> для приложений Windows.

Для получения строк соединения из файлов конфигурации приложения используется <xref:System.Configuration.ConnectionStringSettingsCollection>. Этот объект содержит коллекцию объектов <xref:System.Configuration.ConnectionStringSettings>, каждый из которых представляет одну запись в разделе **connectionStrings**. Его свойства сопоставляются с атрибутами строк соединения, что позволяет получить строку соединения, указав имя строки или имя поставщика.

|Свойство|Описание|  
|--------------|-----------------|  
|<xref:System.Configuration.ConnectionStringSettings.Name%2A>|Имя строки соединения. Сопоставляется с атрибутом **name**.|  
|<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>|Полное имя поставщика. Сопоставляется с атрибутом **providerName**.|  
|<xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A>|Строка подключения. Сопоставляется с атрибутом **connectionString**.|  

### <a name="example-listing-all-connection-strings"></a>Пример Вывод всех строк подключения

В этом примере выполняется итерация <xref:System.Configuration.ConnectionStringSettingsCollection> и отображаются свойства <xref:System.Configuration.ConnectionStringSettings.Name%2A?displayProperty=nameWithType>, <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A?displayProperty=nameWithType> и <xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A?displayProperty=nameWithType> в окне консоли.

> [!NOTE]
> Файл System.Configuration.dll не включается в проекты всех типов, поэтому для использования классов конфигурации может потребоваться сформировать на него ссылку. Имя и расположение файла конфигурации определенного приложения зависит от типа приложения и процесса размещения.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfig#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfig.cs#1)]

### <a name="example-retrieving-a-connection-string-by-name"></a>Пример Получение строки подключения по имени

Данный пример демонстрирует способ получения строки соединения из файла конфигурации путем указания ее имени. Код создает объект <xref:System.Configuration.ConnectionStringSettings>, сопоставляя указанный входной параметр с именем <xref:System.Configuration.ConfigurationManager.ConnectionStrings%2A>. Если совпадающее имя не найдено, функция возвращает значение `null` (`Nothing` в Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByName#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByName.cs#1)]

### <a name="example-retrieving-a-connection-string-by-provider-name"></a>Пример Получение строки подключения по имени поставщика

В этом примере демонстрируется, как получить строку подключения путем указания имени поставщика в формате *Microsoft.Data.SqlClient*. В коде выполняется итерация по <xref:System.Configuration.ConnectionStringSettingsCollection> и происходит возврат строки соединения для первого найденного <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>. Если имя поставщика не найдено, функция возвращает значение `null` (`Nothing` в Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByProvider.cs#1)]

## <a name="encrypt-configuration-file-sections-using-protected-configuration"></a>Шифрование разделов файлов конфигурации с помощью защищенной конфигурации

В ASP.NET 2.0 появилась новая возможность, *защищенная конфигурация*, с помощью которой можно шифровать в файле конфигурации конфиденциальную информацию. Защищенная конфигурация разрабатывалась в первую очередь для ASP.NET, но ее можно также использовать для шифрования разделов файлов конфигурации в приложениях Windows. Подробное описание возможностей защищенной конфигурации см. в разделе [Шифрование сведений о конфигурации с помощью функции защищенной конфигурации](/previous-versions/aspnet/53tyfkaw(v=vs.100)).

В приведенном ниже фрагменте файла конфигурации показан раздел **connectionStrings** после шифрования. В разделе **configProtectionProvider** задается поставщик защищенной конфигурации, который используется для шифрования и дешифрования строк подключения. Раздел **EncryptedData** содержит зашифрованный текст.

```xml  
<connectionStrings configProtectionProvider="DataProtectionConfigurationProvider">  
  <EncryptedData>  
    <CipherData>  
      <CipherValue>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAH2... </CipherValue>  
    </CipherData>  
  </EncryptedData>  
</connectionStrings>  
```  

Когда зашифрованная строка подключения извлекается во время выполнения, платформа .NET Framework с помощью указанного поставщика дешифрует значение **CipherValue** и передает его приложению. Нет необходимости создавать дополнительный код для управления процессом дешифрования.

### <a name="protected-configuration-providers"></a>Защита поставщиков конфигурации

Поставщики защищенной конфигурации регистрируются в разделе **configProtectedData** файла **machine.config** на локальном компьютере, как показано в приведенном ниже фрагменте, в котором демонстрируются два поставщика защищенной конфигурации, поставляемые с платформой .NET Framework. Показанные значения были усечены для удобства чтения.

```xml  
<configProtectedData defaultProvider="RsaProtectedConfigurationProvider">  
  <providers>  
    <add name="RsaProtectedConfigurationProvider"
      type="System.Configuration.RsaProtectedConfigurationProvider" />  
    <add name="DataProtectionConfigurationProvider"
      type="System.Configuration.DpapiProtectedConfigurationProvider" />  
  </providers>  
</configProtectedData>  
```  

Можно назначить дополнительные поставщики защищенной конфигурации, добавляя их в файл **machine.config**. Кроме того, можно создать собственный поставщик защищенной конфигурации путем наследования от абстрактного базового класса <xref:System.Configuration.ProtectedConfigurationProvider>. В приведенной ниже таблице дано описание двух файлов конфигурации, входящих в платформу .NET Framework.

|Поставщик|Описание|  
|--------------|-----------------|  
|<xref:System.Configuration.RsaProtectedConfigurationProvider>|Использует алгоритм RSA для шифрования и расшифровки данных. Алгоритм RSA можно использовать для шифрования с открытым ключом и цифровых сигнатур. Он также известен как алгоритм с «открытым ключом» или алгоритм асимметричного шифрования, поскольку в нем используется два разных ключа. Для шифрования разделов в файле Web.config и управления ключами шифрования можно использовать [средство регистрации служб IIS ASP.NET (Aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)). ASP.NET расшифровывает файл конфигурации при обработке файла. Удостоверение приложения ASP.NET должно иметь права на чтение ключа шифрования, который используется для шифрования и расшифровки зашифрованных разделов.|  
|<xref:System.Configuration.DpapiProtectedConfigurationProvider>|Использует API-интерфейс защиты данных (DPAPI) для шифрования разделов конфигурации. Он использует встроенные службы шифрования Windows и может быть настроен для защиты отдельных компьютеров или отдельных учетных записей пользователей. Защиту отдельных компьютеров удобно использовать для нескольких приложений на одном сервере, которым требуется совместное использование данных. Защиту учетных записей можно использовать со службами, выполняемыми с определенным удостоверением пользователя, например с общей средой размещения. Каждое приложение выполняется с отдельным идентификатором, что ограничивает доступ к таким ресурсам, как файлы и базы данных.|

Оба поставщика обеспечивают надежное шифрование данных. Однако, если планируется использовать один файл конфигурации на нескольких серверах, как в веб-ферме, то только <xref:System.Configuration.RsaProtectedConfigurationProvider> позволяет экспортировать ключи шифрования, применяемые для шифрования данных, и импортировать их в другой сервер. Дополнительные сведения см. в разделе [Импорт и экспорт защищенных контейнеров ключей RSA для конфигурации](/previous-versions/aspnet/yxw286t2(v=vs.100)).

### <a name="use-the-configuration-classes"></a>Использование классов конфигурации

Пространство имен <xref:System.Configuration> предоставляет классы для программной обработки параметров конфигурации. Класс <xref:System.Configuration.ConfigurationManager> обеспечивает доступ к компьютеру, приложению и пользовательским файлам конфигурации. При создании приложения ASP.NET можно использовать класс <xref:System.Web.Configuration.WebConfigurationManager>, который предоставляет те же функциональные возможности и одновременно позволяет обращаться к уникальным параметрам приложений ASP.NET, в частности, в файле **\<system.web>** .

> [!NOTE]
> Пространство имен <xref:System.Security.Cryptography> содержит классы, которые предоставляют дополнительные возможности шифрования и расшифровки данных. Эти классы можно использовать в том случае, если требуются криптографические службы, недоступные с использованием защищенной конфигурации. Некоторые из этих классов являются оболочками для Microsoft CryptoAPI, а другие представляют собой реализации полностью на управляемом коде. Дополнительные сведения см. в разделе [Службы криптографии](/previous-versions/visualstudio/visual-studio-2008/93bskf9z(v=vs.90)).

### <a name="appconfig-example"></a>Пример App.config

Этот пример демонстрирует переключение шифрования раздела **connectionStrings** в файле **app.config** для приложения Windows. В этом примере процедура принимает имя приложения в качестве аргумента, например, «MyApplication.exe». Затем файл **app.config** зашифровывается и копируется в папку, которая содержит исполняемый файл с именем MyApplication.exe.config.

> [!NOTE]
> Строку соединения можно расшифровать только на компьютере, где она была зашифрована.

В коде используется метод <xref:System.Configuration.ConfigurationManager.OpenExeConfiguration%2A>, чтобы открыть файл **app.config** для изменения, а метод <xref:System.Configuration.ConfigurationManager.GetSection%2A> возвращает раздел **connectionStrings**. Затем код проверяет свойство <xref:System.Configuration.SectionInformation.IsProtected%2A>, вызывая метод <xref:System.Configuration.SectionInformation.ProtectSection%2A> для шифрования раздела, если он не зашифрован. Метод <xref:System.Configuration.SectionInformation.UnprotectSection%2A> вызывается для расшифровки раздела. Метод <xref:System.Configuration.Configuration.Save%2A> завершает операцию и сохраняет изменения.

> [!NOTE]
> Для запуска кода необходимо задать ссылку на `System.Configuration.dll` в проекте.

[!code-csharp[DataWorks ConnectionStrings.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStrings_Encrypt.cs#1)]

### <a name="webconfig-example"></a>Пример Web.config

В этом примере используется метод <xref:System.Web.Configuration.WebConfigurationManager.OpenWebConfiguration%2A> класса `WebConfigurationManager`. Обратите внимание, что в этом случае можно задать относительный путь к файлу **Web.config** с использованием символа "тильда". В коде должна быть ссылка на класс `System.Web.Configuration`.  
  
[!code-csharp[DataWorks ConnectionStringsWeb.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStringsWeb_Encrypt.cs#1)]

Дополнительные сведения о защите приложений ASP.NET см. в статье [Защита веб-сайтов ASP.NET](/previous-versions/aspnet/91f66yxt(v=vs.100)).

## <a name="see-also"></a>См. также

- [Построители строк подключения](connection-string-builders.md)
- [Защита сведений о подключении](protecting-connection-information.md)
- [Использование классов конфигурации](/previous-versions/visualstudio/visual-studio-2008/ms228063(v=vs.90))
- [Настройка приложений](/dotnet/framework/configure-apps/index)
- [Администрирование веб-сайта ASP.NET](/previous-versions/aspnet/6hy1xzbw(v=vs.100))
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)
