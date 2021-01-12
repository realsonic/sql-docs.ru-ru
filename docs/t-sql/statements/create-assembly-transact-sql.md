---
description: CREATE ASSEMBLY (Transact-SQL)
title: CREATE ASSEMBLY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASSEMBLY
- CREATE ASSEMBLY
- CREATE_ASSEMBLY_TSQL
- ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], validating
- validating assemblies
- CREATE ASSEMBLY statement
- assemblies [CLR integration], creating
ms.assetid: d8d1d245-c2c3-4325-be52-4fc1122c2079
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 13b639ace60f98ce9ab55cc0280130131f6851ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098412"
---
# <a name="create-assembly-transact-sql"></a>CREATE ASSEMBLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Создает управляемый модуль приложений, содержащий метаданные класса и управляемый код, например объект в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ссылаясь на этот модуль, в базе данных можно создать функции среды CLR, хранимые процедуры CLR, триггеры CLR, определяемые пользователем статистические вычисления и типы.  
  
> [!WARNING]
>  Среда CLR использует управление доступом для кода (CAS) в .NET Framework, которое больше не поддерживается в качестве границы безопасности. Сборки среды CLR, созданные с помощью `PERMISSION_SET = SAFE`, могут получать доступ к внешним системным ресурсам, вызывать неуправляемый код и получать права системного администратора. Начиная с [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], появился параметр `sp_configure`, называемый `clr strict security`, для повышения безопасности сборок среды CLR. `clr strict security` включен по умолчанию и рассматривает сборки `SAFE` и `EXTERNAL_ACCESS`, как если бы они были помечены `UNSAFE`. Параметр `clr strict security` можно отключить для обеспечения обратной совместимости, но это делать не рекомендуется. Корпорация Майкрософт рекомендует подписывать все сборки с помощью сертификата или асимметричного ключа с соответствующим именем входа, которому предоставлено разрешение `UNSAFE ASSEMBLY` в базе данных master. Дополнительные сведения см. в статье о параметре [clr strict security](../../database-engine/configure-windows/clr-strict-security.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE ASSEMBLY assembly_name  
[ AUTHORIZATION owner_name ]  
FROM { <client_assembly_specifier> | <assembly_bits> [ ,...n ] }  
[ WITH PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE } ]  
[ ; ]  
<client_assembly_specifier> :: =  
        '[\\computer_name\]share_name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```  
  
## <a name="arguments"></a>Аргументы  
 *assembly_name*  
 Имя сборки. Имя должно быть уникально в базе данных и являться допустимым [идентификатором](../../relational-databases/databases/database-identifiers.md).  
  
 AUTHORIZATION *owner_name*  
 Указывает имя пользователя или роли в качестве владельца сборки. Аргумент *owner_name* должен быть именем роли, членом которой является текущий пользователь, или текущий пользователь должен иметь разрешение IMPERSONATE для *owner_name*. Если атрибут не указан, владельцем становится текущий пользователь.  
  
 \<client_assembly_specifier>  
Задает локальный путь или местоположение в сети, где расположена передаваемая сборка, а также имя файла манифеста, соответствующее сборке.  \<client_assembly_specifier> можно задать в виде фиксированной строки или выражения с переменными, в результате подстановки значений которых получается фиксированная строка. Инструкция CREATE ASSEMBLY не поддерживает загрузку многомодульных сборок. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также ищет любые зависимые сборки данной сборки в том же месте и передает их с тем же владельцем в качестве сборки корневого уровня. Если зависимые сборки не найдены и если они уже не загружены в текущую базу данных, CREATE ASSEMBLY завершается неудачно. Если зависимые сборки уже загружены в текущую базу данных, владелец этих сборок должен быть тот же, что и у только что созданной сборки.

> [!IMPORTANT]
> База данных SQL Azure не поддерживает создание сборки на основе файла.
  
 \<client_assembly_specifier> не может быть задано, если для пользователя, совершившего вход, выполняется олицетворение.  
  
 \<assembly_bits>  
 Список двоичных значений, которые составляют сборку и ее зависимые сборки. Первое значение в списке считается сборкой корневого уровня. Значения, соответствующие зависимым сборкам, могут быть заданы в любом порядке. Любые значения, которые не соответствует зависимостям корневой сборки, не учитываются.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 *varbinary_literal*  
 Литерал **varbinary**.  
  
 *varbinary_expression*  
 Выражение типа **varbinary**.  
  
 PERMISSION_SET { **SAFE** | EXTERNAL_ACCESS | UNSAFE }  
> [!IMPORTANT]
>  На параметр `PERMISSION_SET` влияет параметр `clr strict security`, описанный в начальном предупреждении. Когда `clr strict security` включено, все сборки считаются `UNSAFE`.
 
 Указывает набор разрешений доступа к коду, которые предоставляются сборке при доступе к ней [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если этот аргумент не задан, по умолчанию применяется SAFE.  
  
 Мы рекомендуем использовать SAFE. SAFE является наиболее ограниченным набором разрешений. Код, исполняемый с разрешениями SAFE, не может получить доступ к внешним системным ресурсам, таким как файлы, сеть, переменные окружения или реестр.  
  
 EXTERNAL_ACCESS позволяет сборкам получать доступ к внешним системным ресурсам, таким как файлы, сети, переменные окружения и реестр.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 UNSAFE предоставляет сборкам неограниченный доступ к ресурсам как внутри, так и вне экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Код, исполняемый из сборки с набором прав UNSAFE, может вызывать неуправляемый код.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
> [!IMPORTANT]  
>  SAFE является рекомендованной установкой разрешений для сборок, которые выполняют задачи вычисления и управления данными без доступа к ресурсам вне экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
>   
>  Мы рекомендуем использовать EXTERNAL_ACCESS для сборок, которым необходим доступ к ресурсам вне экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сборки с набором прав EXTERNAL_ACCESS включают надежность и гибкость сборок с набором прав SAFE, но с точки зрения безопасности похожи на сборки с набором прав UNSAFE, потому как сборки с набором прав EXTERNAL_ACCESS запускаются по умолчанию из-под учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и получают доступ к внешним ресурсам из-под этой учетной записи, если только код явно не олицетворяет разрешения вызывающего. Поэтому разрешения на создание сборок EXTERNAL_ACCESS должны быть предоставлены только именам входа, которым разрешено выполнение кода из-под учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения об олицетворении см. в статье [Безопасность интеграции со средой CLR](../../relational-databases/clr-integration/security/clr-integration-security.md).  
>   
>  Задание UNSAFE предоставляет коду в сборке полную свободу выполнять операции в операционном пространстве [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], что потенциально может нанести вред надежности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сборки UNSAFE потенциально могут подвергнуть опасности систему безопасности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или среды CLR. Разрешения UNSAFE должны предоставляться только высоконадежным сборкам. Только члены предопределенной роли сервера **sysadmin** могут создавать и изменять сборки с набором разрешений UNSAFE.  
  
 Дополнительные сведения о наборах разрешений сборки см. в разделе [Проектирование сборок](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="remarks"></a>Комментарии  
 CREATE ASSEMBLY передает сборку, предварительно скомпилированную в виде файла DLL из управляемого кода для использования внутри экземпляра SQL Server.  
 
Если параметр `PERMISSION_SET` включен, операторы `CREATE ASSEMBLY` и `ALTER ASSEMBLY` игнорируются во время выполнения, а параметры `PERMISSION_SET` сохраняются в метаданных. Игнорирование параметра минимизирует сбои в выполнении имеющихся в коде операторов.
 
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не дает возможность регистрировать различные версии сборок с одинаковыми именами, культурой и открытыми ключами.  
  
При попытке доступа к сборке, указанной в \<client_assembly_specifier>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] олицетворяет разрешения контекста безопасности текущего имени входа Windows. Если \<client_assembly_specifier> задает расположение в сети (UNC-путь), олицетворение текущего имени входа не распространяется на это расположение в сети из-за ограничений передачи прав. В этом случае доступ осуществляется при помощи контекста безопасности учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Учетные данные (ядро СУБД)](../../relational-databases/security/authentication-access/credentials-database-engine.md).
  
 Кроме корневой сборки, указанной в аргументе *assembly_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пытается загрузить сборки, на которые ссылается передаваемая корневая сборка. Если сборка, на которую ссылаются, уже передана в базу данных в ходе предыдущих выполнений инструкций CREATE ASSEMBLY, эта сборка не передается, но становится доступной для корневой сборки. Если зависимая сборка не была предварительно передана, но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может обнаружить файл манифеста в исходном каталоге, CREATE ASSEMBLY возвращает ошибку.  
  
 Если какая-либо зависимая сборка, на которую ссылается корневая сборка, еще не находится в базе данных и косвенно загружена вместе с корневой сборкой, она будет иметь тот же набор разрешений, что и сборка корневого уровня. Если зависимая сборка должна быть создана с набором разрешений, отличным от набора разрешений сборки корневого уровня, она должна быть передана напрямую перед корневой сборкой с необходимым набором разрешений.  
  
## <a name="assembly-validation"></a>Проверка сборки  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет проверки двоичных файлов сборки, переданных посредством инструкции CREATE ASSEMBLY для обеспечения следующего.  
  
-   Двоичный файл сборки является верным с правильными метаданными и сегментами кода, и сегменты кода содержат правильные инструкции промежуточного языка корпорации Майкрософт (MSIL).  
  
-   Ссылаться можно на следующие поддерживаемые типы системных сборок в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: Microsoft.Visualbasic.dll, Mscorlib.dll, System.Data.dll, System.dll, System.Xml.dll, Microsoft.Visualc.dll, Custommarshallers.dll, System.Security.dll, System.Web.Services.dll, System.Data.SqlXml.dll, System.Core.dll и System.Xml.Linq.dll. Можно ссылаться и на другие системные сборки, но они должны быть явно зарегистрированы в базе данных.  
  
-   Для сборок, созданных с наборами разрешений SAFE или EXTERNAL ACCESS должны быть соблюдены следующие условия.  
  
    -   Код сборки должен быть типизированным. Типовая безопасность устанавливается путем запуска верификатора общеязыковой среды исполнения для сборки.  
  
    -   Сборка не должна содержать элементы статических данных в своих предложениях, только если они не помечены как доступные только для чтения.  
  
    -   Классы в сборке не могут содержать методов финализации.  
  
    -   Классы или методы сборки должны быть аннотированы только разрешенными атрибутами кода. Дополнительные сведения см. в статье [Пользовательские атрибуты процедур CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).  
  
 Кроме предварительных проверок, которые выполняются при выполнении CREATE ASSEMBLY, существуют дополнительные проверки, которые выполняются во время выполнения кода сборки.  
  
-   Вызов определенных API-функций [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], которые требуют наличия определенного разрешения на доступ к коду, может завершиться ошибкой, если набор разрешений сборки не включает такого разрешения.  
  
-   Для сборок SAFE и EXTERNAL_ACCESS любая попытка вызова API-функций [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], которые аннотированы определенным HostProtectionAttributes, завершится неудачно.  
  
 Дополнительные сведения см. в разделе [Разработка сборок](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="permissions"></a>Разрешения  
 Требуется наличие разрешения на CREATE ASSEMBLY.  
  
 Если указано PERMISSION_SET = EXTERNAL_ACCESS, требуется разрешение **EXTERNAL ACCESS ASSEMBLY** на сервере. Если указано PERMISSION_SET = UNSAFE, требуется разрешение **UNSAFE ASSEMBLY** на сервере.  
  
 Пользователь должен быть владельцем любой сборки, на которую ссылается загружаемая сборка, если сборка уже существует в базе данных. Чтобы передать сборку, используя путь к файлу, текущий пользователь должен иметь проверенное на подлинность имя входа Windows или быть членом предопределенной роли сервера **sysadmin**. Имя входа Windows пользователя, который выполняет CREATE ASSEMBLY, должно иметь разрешение чтения общего ресурса и файлов, загружаемых посредством инструкции.  

### <a name="permissions-with-clr-strict-security"></a>Разрешения со строгой безопасностью среды CLR    
Для создания сборки среды CLR при включении `CLR strict security` требуются следующие разрешения:

- Пользователь должен иметь разрешение `CREATE ASSEMBLY`.  
- Кроме того, должно выполняться одно из следующих условий:  
  - Сборка должна была подписана сертификатом или асимметричным ключом, имеющим соответствующее имя входа с разрешением `UNSAFE ASSEMBLY` на сервере. Рекомендуется использовать подпись сборки.  
  - База данных имеет `TRUSTWORTHY` свойство со значением `ON` и базы данных, принадлежащие имени входа, имеющем разрешение `UNSAFE ASSEMBLY` на сервере. Использовать этот параметр не рекомендуется.  
  
 Дополнительные сведения о наборах разрешений сборки см. в разделе [Проектирование сборок](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="example-a-creating-an-assembly-from-a-dll"></a>Пример А. Создание сборки из библиотеки dll  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Следующий пример предполагает, что компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] установлен в папку по умолчанию на локальном компьютере и приложение-образец HelloWorld.csproj скомпилировано. Дополнительные сведения см. в разделе [Образец "Hello World"](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).  
  
```sql  
CREATE ASSEMBLY HelloWorld   
FROM '<system_drive>:\Program Files\Microsoft SQL Server\100\Samples\HelloWorld\CS\HelloWorld\bin\debug\HelloWorld.dll'  
WITH PERMISSION_SET = SAFE;  
```  

> [!IMPORTANT]
> База данных SQL Azure не поддерживает создание сборки на основе файла.
  
### <a name="example-b-creating-an-assembly-from-assembly-bits"></a>Пример Б. Создание сборки из битов сборки  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Замените примеры битов (которые не являются полными или допустимыми) на ваши биты сборки.  
  
```sql  
CREATE ASSEMBLY HelloWorld  
    FROM 0x4D5A900000000000  
WITH PERMISSION_SET = SAFE;  
```  
  
## <a name="see-also"></a>См. также  
 [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE AGGREGATE (Transact-SQL)](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Сценарии использования и примеры интеграции со средой CLR](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))  
  
