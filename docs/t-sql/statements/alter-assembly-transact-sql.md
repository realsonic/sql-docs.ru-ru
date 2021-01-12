---
description: ALTER ASSEMBLY (Transact-SQL)
title: ALTER ASSEMBLY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_ASSEMBLY_TSQL
- ALTER ASSEMBLY
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], modifying
- refreshing assemblies
- assemblies [CLR integration], versioning
- assemblies [CLR integration], adding files
- modifying assemblies
- adding files
- ALTER ASSEMBLY statement
ms.assetid: 87bca678-4e79-40e1-bb8b-bd5ed8f34853
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dda1de9a2eb4628765333651c7d074b0c38551db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100914"
---
# <a name="alter-assembly-transact-sql"></a>ALTER ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет сборку, изменяя при этом свойства каталога [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сборки. Инструкция ALTER ASSEMBLY обновляет ее до последней копии модулей [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], содержащих ее реализацию, и добавляет или удаляет связанные с ней файлы. Сборки создаются при помощи инструкции [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  

> [!WARNING]
>  Среда CLR использует управление доступом для кода (CAS) в .NET Framework, которое больше не поддерживается в качестве границы безопасности. Сборки среды CLR, созданные с помощью `PERMISSION_SET = SAFE`, могут получать доступ к внешним системным ресурсам, вызывать неуправляемый код и получать права системного администратора. Начиная с [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], появился параметр `sp_configure`, называемый `clr strict security`, для повышения безопасности сборок среды CLR. `clr strict security` включен по умолчанию и рассматривает сборки `SAFE` и `EXTERNAL_ACCESS`, как если бы они были помечены `UNSAFE`. Параметр `clr strict security` можно отключить для обеспечения обратной совместимости, но это делать не рекомендуется. Корпорация Майкрософт рекомендует подписывать все сборки с помощью сертификата или асимметричного ключа с соответствующим именем входа, которому предоставлено разрешение `UNSAFE ASSEMBLY` в базе данных master. Дополнительные сведения см. в статье о параметре [clr strict security](../../database-engine/configure-windows/clr-strict-security.md).  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER ASSEMBLY assembly_name  
    [ FROM <client_assembly_specifier> | <assembly_bits> ]  
    [ WITH <assembly_option> [ ,...n ] ]  
    [ DROP FILE { file_name [ ,...n ] | ALL } ]  
    [ ADD FILE FROM   
    {   
        client_file_specifier [ AS file_name ]   
      | file_bits AS file_name   
    } [,...n ]   
    ] [ ; ]  
<client_assembly_specifier> :: =  
    '\\computer_name\share-name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
    { varbinary_literal | varbinary_expression }  
  
<assembly_option> :: =  
    PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
  | VISIBILITY = { ON | OFF }  
  | UNCHECKED DATA  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *assembly_name*  
 Имя сборки, которую нужно изменить. Аргумент *assembly_name* уже должен существовать в базе данных.  
  
 FROM \<client_assembly_specifier> | \<assembly_bits>  
 Обновляет сборку до последней копии модулей платформы [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], содержащих ее реализацию. Этот параметр может использоваться при условии, что файлы, связанные с указанной сборкой, отсутствуют.  
  
 \<client_assembly_specifier> указывает сетевое или локальное расположение, в котором находится обновляемая сборка. Сетевое расположение содержит имя компьютера, имя общей папки и путь внутри этой папки. Аргумент *manifest_file_name* указывает имя файла, содержащего манифест сборки.  

> [!IMPORTANT]
> База данных SQL Azure не поддерживает добавление ссылок на файлы.
  
 \<assembly_bits> является двоичным значением для сборки.  
  
 Для зависимых сборок, также нуждающихся в обновлении, должны выполняться отдельные инструкции ALTER ASSEMBLY.  
  
 PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
> [!IMPORTANT]
>  На параметр `PERMISSION_SET` влияет параметр `clr strict security`, описанный в начальном предупреждении. Когда `clr strict security` включено, все сборки считаются `UNSAFE`.  
>  Указывает для сборки свойство набора разрешений на доступ к коду [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Дополнительные сведения об этом свойстве см. в статье [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md).  
> 
> [!NOTE]
>  Параметры EXTERNAL_ACCESS и UNSAFE недоступны в автономной базе данных.  
  
 VISIBILITY = { ON | OFF }  
 Указывает, видима ли сборка для создания на ее основе функций среды CLR, хранимых процедур, триггеров, определяемых пользователем типов и определяемых пользователем агрегатных функций. При установке OFF сборка может быть вызвана только при помощи других сборок. Если имеются объекты базы данных среды CLR, уже созданные с помощью сборки, ее видимость не может быть изменена. Сборки, на которые ссылается аргумент *assembly_name*, передаются по умолчанию как невидимые.  
  
 UNCHECKED DATA  
 По умолчанию инструкция ALTER ASSEMBLY не выполняется в том случае, если она должна проверять согласованность отдельных строк таблицы. Этот параметр позволяет впоследствии провести позже при помощи инструкции DBCC CHECKTABLE. Если данный параметр указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет инструкцию ALTER ASSEMBLY даже в случае, если в базе данных имеются таблицы, содержащие:  
  
-   материализованные вычисляемые столбцы, которые непосредственно или косвенно ссылаются на методы в сборке при помощи функций или методов языка [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   ограничения CHECK, которые непосредственно или косвенно ссылаются на методы в сборке;  
  
-   зависящие от сборки столбцы определяемого пользователем типа данных CLR и типа, реализующего сериализацию формата **UserDefined** (не **собственного**);  
  
-   столбцы определяемого пользователем типа данных CLR, ссылающиеся на представления, созданные при помощи ключевого слова WITH SCHEMABINDING.  
  
 Любые имеющиеся ограничения CHECK отключаются и помечаются как ненадежные. Таблицы, содержащие столбцы, зависящие от сборки, помечаются как содержащие непроверенные данные, и сохраняют эту метку до тех пор, пока не будут проверены явно.  
  
 Указывать этот параметр могут только члены предопределенных ролей базы данных **db_owner** и **db_ddlowner**.  
  
 Для указания этого параметра требуется разрешение **ALTER ANY SCHEMA**.  
  
 Дополнительные сведения см. в разделе [Реализация сборок](../../relational-databases/clr-integration/assemblies-implementing.md).  
  
 [ DROP FILE { *file_name*[ **,** _...n_] | ALL } ]  
 Удаляет из базы данных файл, связанный со сборкой, или все файлы, связанные со сборкой. Если следующей является инструкция ADD FILE, инструкция DROP FILE выполняется в первую очередь. Это позволяет заменить файл другим файлом с тем же именем.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных или Базе данных SQL Azure.  
  
 [ ADD FILE FROM { *client_file_specifier* [ AS *file_name*] | *file_bits* AS *file_name*}  
 Передает на сервер файлы для связывания со сборкой (например, исходный код, файлы отладки или другие сведения) и делает их видимыми в представлении каталога **sys.assembly_files**. *client_file_specifier* указывает расположение, откуда передаются файлы. Вместо этого можно указать список двоичных значений, составляющих файл, с помощью *file_bits*. *file_name* указывает имя, под которым файл должен храниться на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *file_name* обязателен, если указан аргумент *file_bits*, и необязателен, если указан аргумент *client_file_specifier*. Если аргумент *file_name* не указан, часть file_name в аргументе *client_file_specifier* указывается как *file_name*.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных или Базе данных SQL Azure.  
  
## <a name="remarks"></a>Remarks  
 Инструкция ALTER ASSEMBLY не нарушает сеансы, в которых в настоящий момент работает код изменяемой сборки. Текущие сеансы завершают выполнение с неизмененной сборкой.  
  
 Если указывается предложение FROM, инструкция ALTER ASSEMBLY обновляет сборку в соответствии с последними предоставленными копиями модулей. Так как в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут быть функции среды CLR, хранимые процедуры, триггеры, типы данных и пользовательские агрегатные функции, уже использующие сборку, инструкция ALTER ASSEMBLY привязывает их к последней реализации сборки. Для выполнения этой привязки в измененной сборке должны существовать методы с подписями, с которыми сопоставлены функции CLR, хранимые процедуры и триггеры. Классы, реализующие определяемые пользователем типы данных CLR и пользовательские агрегатные функции, должны удовлетворять требованиям для определяемых пользователем типов или агрегатных функций.  
  
> [!CAUTION]  
>  Если не указан параметр WITH UNCHECKED DATA, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] старается избежать выполнения ALTER ASSEMBLY, если сборка новой версии изменяет существующие данные в таблицах, индексах и т. д. Однако при обновлении сборки в среде CLR [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не обеспечивает согласованности вычисляемых столбцов, указателей, индексированных представлений или выражений с базовыми процедурами или типами. Выполняйте инструкции ALTER ASSEMBLY с осторожностью, чтобы избежать несоответствия результата выражения и его значения, хранящегося в сборке.  
  
 Инструкция ALTER ASSEMBLY изменяет версию сборки. Культура и токен открытого ключа сборки не изменяются.  
  
 С помощью инструкции ALTER ASSEMBLY нельзя изменить:  
  
-   Подписи функций CLR, агрегатных функций, хранимых процедур и триггеров в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на которые ссылается сборка. Инструкция ALTER ASSEMBLY не выполняется, если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может связать объекты базы данных [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с новой версией сборки.  
  
-   Подписи методов сборки, вызываемых из других сборок.  
  
-   Список сборок, зависящих от этой сборки, указанный в свойстве **DependentList** сборки.  
  
-   Возможность использования индексов в методе и в том числе, если только не существует индексов или постоянных вычисляемых столбцов, прямо или косвенно зависящих от этого метода.  
  
-   Атрибут имени метода **FillRow** для функций CLR с табличным значением.  
  
-   Подпись методов **Accumulate** и **Terminate** для определяемых пользователем статистических выражений.  
  
-   Системные сборки.  
  
-   Владение сборкой. Вместо этого используйте [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Кроме того, для сборок, реализующих определяемые пользователем типы, с помощью инструкции ALTER ASSEMBLY можно выполнять только следующие изменения:  
  
-   изменение общих методов класса определяемого пользователем типа, если только не изменяются подписи или атрибуты;  
  
-   добавление новых общих методов;  
  
-   изменение приватных методов любым образом.  
  
 Поля в определяемом пользователем типе с собственной сериализацией, в том числе элементы данных или основные классы, с помощью инструкции ALTER ASSEMBLY изменить нельзя. Любые другие изменения не поддерживаются.  
  
 Если предложение ADD FILE FROM не указано, инструкция ALTER ASSEMBLY удаляет все файлы, связанные со сборкой.  
  
 Если инструкция ALTER ASSEMBLY выполняется без предложения UNCHECKED для данных, выполняются проверки того, что новая версия сборки не влияет на существующие данные в таблицах. В зависимости от объема данных, для которых необходима проверка, это может повлиять на производительность.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER на сборку. Дополнительные требования.  
  
-   Для изменения сборки с указанным разрешением EXTERNAL_ACCESS требуется разрешение **EXTERNAL ACCESS ASSEMBLY** на сервере.  
  
-   Для изменения сборки с указанным разрешением UNSAFE требуется разрешение **UNSAFE ASSEMBLY** на сервере.  
  
-   Чтобы изменить разрешение для сборки на EXTERNAL_ACCESS, требуется разрешение **EXTERNAL ACCESS ASSEMBLY** на сервере.  
  
-   Чтобы изменить разрешение для сборки на UNSAFE, требуется разрешение **UNSAFE ASSEMBLY** на сервере.  
  
-   Для указания WITH UNCHECKED DATA требуется разрешение **ALTER ANY SCHEMA**.  


### <a name="permissions-with-clr-strict-security"></a>Разрешения со строгой безопасностью среды CLR    
Для изменения сборки среды CLR при включении `CLR strict security` требуются следующие разрешения:

- Пользователь должен иметь разрешение `ALTER ASSEMBLY`.  
- Кроме того, должно выполняться одно из следующих условий:  
  - Сборка должна была подписана сертификатом или асимметричным ключом, имеющим соответствующее имя входа с разрешением `UNSAFE ASSEMBLY` на сервере. Рекомендуется использовать подпись сборки.  
  - База данных имеет `TRUSTWORTHY` свойство со значением `ON` и базы данных, принадлежащие имени входа, имеющем разрешение `UNSAFE ASSEMBLY` на сервере. Использовать этот параметр не рекомендуется.  
  
  
 Дополнительные сведения о наборах разрешений сборки см. в разделе [Проектирование сборок](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-refreshing-an-assembly"></a>A. Обновление сборки  
 На следующем примере показано, как сборка `ComplexNumber` обновляется до последней копии модулей [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], содержащих ее реализацию.  
  
> [!NOTE]  
>  Сборка `ComplexNumber` может быть создана при выполнении образцов скриптов UserDefinedDataType. Дополнительные сведения см. в статье [Определяемый пользователем тип](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130)).  
  
 ```sql
 ALTER ASSEMBLY ComplexNumber 
 FROM 'C:\Program Files\Microsoft SQL Server\130\Tools\Samples\1033\Engine\Programmability\CLR\UserDefinedDataType\CS\ComplexNumber\obj\Debug\ComplexNumber.dll' 
  ```

> [!IMPORTANT]
> База данных SQL Azure не поддерживает добавление ссылок на файлы.

### <a name="b-adding-a-file-to-associate-with-an-assembly"></a>Б. Добавление файла, связанного со сборкой  
 На следующем примере показано, как производится передача файла с исходным кодом `Class1.cs`, связанного со сборкой `MyClass`. При этом предполагается, что сборка `MyClass` уже создана в базе данных.  
  
```sql  
ALTER ASSEMBLY MyClass   
ADD FILE FROM 'C:\MyClassProject\Class1.cs';  
```  

> [!IMPORTANT]
> База данных SQL Azure не поддерживает добавление ссылок на файлы.

### <a name="c-changing-the-permissions-of-an-assembly"></a>В. Изменение разрешений сборки  
 На следующем примере показано, как набор разрешений сборки `ComplexNumber` меняется с SAFE на `EXTERNAL ACCESS`.  
  
```sql  
ALTER ASSEMBLY ComplexNumber WITH PERMISSION_SET = EXTERNAL_ACCESS;  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
