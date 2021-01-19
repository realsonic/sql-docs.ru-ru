---
description: CREATE SYMMETRIC KEY (Transact-SQL)
title: CREATE SYMMETRIC KEY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE SYMMETRIC KEY
- SYMMETRIC KEP
- CREATE_SYMMETRIC_KEY_TSQL
- SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SYMMETRIC KEY statement
- temporary symmetric keys [SQL Server]
- symmetric keys [SQL Server], creating
- symmetric keys [SQL Server]
ms.assetid: b5d23572-b79d-4cf1-9eef-d648fa3b1358
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 581f49677f56c0d0c1360d5282c07205f18ee35a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170826"
---
# <a name="create-symmetric-key-transact-sql"></a>CREATE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает симметричный ключ и указывает его свойства в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Эта функция несовместима с экспортом базы данных с использованием платформы приложения уровня данных (DACFx). Необходимо удалить все симметричные ключи перед экспортом.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE SYMMETRIC KEY key_name   
    [ AUTHORIZATION owner_name ]  
    [ FROM PROVIDER provider_name ]  
    WITH 
      [
          <key_options> [ , ... n ]  
        | ENCRYPTION BY <encrypting_mechanism> [ , ... n ] 
      ]
  
<key_options> ::=  
      KEY_SOURCE = 'pass_phrase'  
    | ALGORITHM = <algorithm>  
    | IDENTITY_VALUE = 'identity_phrase'  
    | PROVIDER_KEY_NAME = 'key_name_in_provider'   
    | CREATION_DISPOSITION = {CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
    DES | TRIPLE_DES | TRIPLE_DES_3KEY | RC2 | RC4 | RC4_128  
    | DESX | AES_128 | AES_192 | AES_256   
  
<encrypting_mechanism> ::=  
      CERTIFICATE certificate_name   
    | PASSWORD = 'password'   
    | SYMMETRIC KEY symmetric_key_name   
    | ASYMMETRIC KEY asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *Key_name*  
 Уникальное имя, под которым симметричный ключ известен в базе данных. Временные ключи назначаются, если _key_name_ начинается с одного символа номера (#). Например, **#temporaryKey900007**. Нельзя создать симметричный ключ, в начале имени которого указано более одного символа #. Временный симметричный ключ невозможно создать с помощью поставщика расширенного управления ключами.  
  
 AUTHORIZATION *owner_name*  
 Указывает имя пользователя базы данных или роли приложения, которым будет принадлежать этот ключ.  
  
 FROM PROVIDER *provider_name*  
 Указывает поставщик расширенного управления ключами и его имя. Этот ключ не экспортируется с устройства расширенного управления ключами. Поставщик должен быть сначала определен с использованием инструкции CREATE PROVIDER. Дополнительные сведения о создании поставщиков внешних ключей см. в разделе [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 KEY_SOURCE **='** _pass\_phrase_ **'**  
 Указывает парольную фразу, из которой извлекается ключ.  
  
 IDENTITY_VALUE **='** _identity\_phrase_ **'**  
 Указывает идентифицирующую фразу, из которой будет формироваться идентификатор GUID для маркировки данных, зашифрованных с помощью временного ключа.  
  
 PROVIDER_KEY_NAME **='** _key\_name\_in\_provider_ **'**  
 Указывает имя, на которое ссылается поставщик расширенного управления ключами.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 CREATION_DISPOSITION **=** CREATE_NEW  
 Создает новый ключ на устройстве расширенного управления ключами.  Если ключ уже существует в устройстве, оператор завершается с ошибкой.  
  
 CREATION_DISPOSITION **=** OPEN_EXISTING  
 Сопоставляет симметричный ключ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с существующим ключом системы расширенного управления ключами. Если выражение CREATION_DISPOSITION = OPEN_EXISTING не предусмотрено, значением по умолчанию является CREATE_NEW.  
  
 *certificate_name*  
 Указывает имя сертификата, который будет использоваться для шифрования симметричного ключа. Сертификат уже должен существовать в базе данных.  
  
 **'** *password* **'**  
 Указывает пароль, из которого извлекается ключ TRIPLE_DES для защиты симметричного ключа. *password* должен соответствовать требованиям политики паролей Windows применительно к компьютеру, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Всегда используйте надежные пароли.  
  
 *symmetric_key_name*  
 Указывает симметричный ключ, используемый для шифрования создаваемого ключа. Указанный ключ должен уже существовать в базе данных, и этот ключ должен быть открытым.  
  
 *asym_key_name*  
 Указывает асимметричный ключ, используемый для шифрования создаваемого ключа. Асимметричный ключ должен уже существовать в базе данных.  
  
 \<algorithm>  
Указывает алгоритм шифрования.   
> [!WARNING]  
> Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]все алгоритмы, отличные от AES_128, AES_192 и AES_256, являются нерекомендуемыми. Чтобы использовать старые алгоритмы (что не рекомендуется), необходимо установить уровень совместимости базы данных 120 или ниже.  
  
## <a name="remarks"></a>Remarks  
 После создания симметричный ключ должен быть зашифрован с помощью хотя бы одного из следующих средств: сертификат, пароль, симметричный ключ, асимметричный ключ или PROVIDER. Ключ может быть зашифрован более чем один раз для каждого типа шифрования. Другими словами, один симметричный ключ может быть зашифрован с использованием нескольких сертификатов, симметричных ключей и асимметричных ключей одновременно.  
  
> [!CAUTION]  
>  Если симметричный ключ шифруется с использованием пароля вместо сертификата (или другого ключа), для шифрования пароля используется алгоритм шифрования TRIPLE DES. Поэтому ключи, созданные с помощью сильных алгоритмов шифрования, таких как AES, защищены с помощью более слабого алгоритма.  
  
 Необязательный пароль можно использовать для шифрования симметричного ключа перед распространением ключа нескольким пользователям.  
  
 Владельцем временных ключей является пользователь, который создает их. Временные ключи действительны только в текущем сеансе.  
  
 Аргумент IDENTITY_VALUE формирует идентификатор GUID, с помощью которого маркируются данные, зашифрованные новым симметричным ключом. Маркирование может быть использовано для сопоставления ключей шифрованным данным. Идентификатор GUID, формируемый указанной фразой, будет всегда одним и тем же. Фраза, использованная для создания идентификатора GUID, не может быть повторно использована до тех пор, пока активен хотя бы один сеанс, использующий эту фразу. Предложение IDENTITY_VALUE является необязательным, но рекомендуется использовать его для хранения данных, зашифрованных с применением временного ключа.  
  
 Не существует алгоритма шифрования по умолчанию.  
  
> [!IMPORTANT]  
>  Защищать конфиденциальные данные с помощью потоковых шифров RC4 и RC4_128 не рекомендуется. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает дальнейшее шифрование данных с помощью таких ключей.  
  
 Сведения о симметричных ключах доступны в представлении каталога [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md).  
  
 Симметричные ключи не могут быть зашифрованы с помощью симметричных ключей, созданных поставщиком шифрования.  
  
 **Пояснение к алгоритмам DES:**  
  
-   DESX был именован неправильно. Симметричные ключи, созданные с параметром ALGORITHM = DESX, в действительности используют шифр TRIPLE DES с 192-битным ключом. Алгоритм DESX не предоставляется. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES_3KEY, используют шифр TRIPLE DES с 192-битным ключом.  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES, используют шифр TRIPLE DES с 128-битным ключом.  
  
 **Отмена алгоритма RC4:**  
  
 Многократное использование одного и того же RC4 или RC4_128 KEY_GUID для различных блоков данных приведет к одному и тому же ключу RC4, так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не предоставляет соль автоматически. Повторное использование одного и того же ключа RC4 является типичной ошибкой, становящейся причиной очень слабого шифрования. Таким образом, ключевые слова RC4 и RC4_128 являются устаревшими. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!WARNING]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY SYMMETRIC KEY на базу данных. Если указан аргумент AUTHORIZATION, то требуется разрешение IMPERSONATE для пользователя базы данных или разрешение ALTER для роли приложения. Если для шифрования использовался сертификат или асимметричный ключ, то требуется разрешение VIEW DEFINITION для сертификата или асимметричного ключа. Симметричные ключи могут принадлежать только именам входа Windows, именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и ролям приложений. Симметричные ключи не могут принадлежать группам и ролям.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-symmetric-key"></a>A. Создание симметричного ключа  
 В следующем примере создается симметричный ключ с именем `JanainaKey09` с помощью алгоритма `AES 256`, а затем новый ключ шифруется с применением сертификата `Shipping04`.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey09   
WITH ALGORITHM = AES_256  
ENCRYPTION BY CERTIFICATE Shipping04;  
GO  
```  
  
### <a name="b-creating-a-temporary-symmetric-key"></a>Б. Создание временного симметричного ключа  
 В следующем примере создается временный симметричный ключ с именем `#MarketingXXV` из парольной фразы: `The square of the hypotenuse is equal to the sum of the squares of the sides`. Ключу назначается идентификатор GUID, формируемый из строки `Pythagoras` и зашифрованный сертификатом `Marketing25`.  
  
```sql 
CREATE SYMMETRIC KEY #MarketingXXV   
WITH ALGORITHM = AES_128,  
KEY_SOURCE   
     = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
IDENTITY_VALUE = 'Pythagoras'  
ENCRYPTION BY CERTIFICATE Marketing25;  
GO  
```  
  
### <a name="c-creating-a-symmetric-key-using-an-extensible-key-management-ekm-device"></a>В. Создание симметричного ключа с помощью устройства расширенного управления ключами (EKM)  
 В следующем примере демонстрируется создание симметричного ключа с именем `MySymKey` при помощи поставщика с именем `MyEKMProvider` и имени ключа `KeyForSensitiveData`. Назначается авторизация для пользователя `User1`; при этом подразумевается, что системным администратором уже зарегистрирован поставщик с именем `MyEKMProvider` в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
CREATE SYMMETRIC KEY MySymKey  
AUTHORIZATION User1  
FROM PROVIDER EKMProvider  
WITH  
PROVIDER_KEY_NAME='KeyForSensitiveData',  
CREATION_DISPOSITION=OPEN_EXISTING;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Расширенное управление ключами с помощью хранилища ключей Azure (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
