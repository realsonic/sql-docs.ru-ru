---
title: Выбор алгоритма шифрования | Документация Майкрософт
description: Используйте это руководство, чтобы выбрать алгоритм шифрования для защиты экземпляра SQL Server, поддерживающего несколько распространенных алгоритмов.
ms.custom: ''
ms.date: 08/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fb31365f95cddc23739db02df9ba4ad9db0345e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171816"
---
# <a name="choose-an-encryption-algorithm"></a>Выбор алгоритма шифрования
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Шифрование — одно из нескольких эффективных средств защиты, позволяющих администраторам обеспечивать безопасность экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Алгоритмы шифрования определяют преобразования данных, которые не могут с легкостью отменить неавторизованные пользователи. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] позволяет администраторам и разработчикам выбирать из нескольких алгоритмов, в том числе DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 со 128-разрядным ключом, DESX, AES со 128-разрядным ключом, AES со 192-разрядным ключом и AES с 256-разрядным ключом.  
  
> [!NOTE]  
>  Начиная с версии [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)]все алгоритмы, отличные от AES_128, AES_192 и AES_256, являются нерекомендуемыми. Чтобы использовать старые алгоритмы (что не рекомендуется), необходимо установить уровень совместимости базы данных 120 или ниже.  
  
 Не существует одного алгоритма, идеально подходящего для всех случаев. Информация по качеству каждого из них лежит за пределами электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Однако можно руководствоваться следующими общими принципами:  
  
-   Надежные алгоритмы шифрования обычно потребляют больше ресурсов ЦП, чем менее надежные средства шифрования.  
  
-   Использование длинных ключей, как правило, дает более надежные результаты, чем шифрование с помощью коротких ключей.  
  
-   Асимметричное шифрование медленнее симметричного.  
  
-   Длинные сложные пароли надежнее, чем короткие пароли.  

-   Симметричное шифрование обычно рекомендуется использовать, только если ключ хранится локально; асимметричное — если ключи должны передаваться по каналу связи.
  
-   При необходимости шифровать большие объемы данных, шифруйте данные с помощью симметричного ключа, а симметричный ключ — с помощью асимметричного ключа.  
  
-   Зашифрованные данные не поддаются сжатию, но сжатые данные могут быть зашифрованы. При использовании сжатия данных, выполняйте эту операцию до их шифрования.  
  
> [!IMPORTANT]  
>  Алгоритм RC4 поддерживается только в целях обратной совместимости. Когда база данных имеет уровень совместимости 90 или 100, новые материалы могут шифроваться только с помощью алгоритмов RC4 или RC4_128. (Не рекомендуется.) Используйте вместо этого более новые алгоритмы, например AES. В [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] и более поздних версиях материалы, зашифрованные с помощью алгоритмов RC4 или RC4_128, могут быть расшифрованы на любом уровне совместимости.  
>   
>  Многократное использование одного и того же RC4 или RC4_128 KEY_GUID для различных блоков данных приведет к одному и тому же ключу RC4, так как [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не предоставляет рассеивание автоматически. Повторное использование ключа RC4 является типичной ошибкой, снижающей надежность шифра. Таким образом, ключевые слова RC4 и RC4_128 являются устаревшими. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 Дополнительные сведения об алгоритмах шифрования и о технологии шифрования см. в разделе [Основные понятия безопасности](/previous-versions/aa720225(v=vs.71)) руководства разработчика для платформы .NET Framework в сети MSDN.  
  
 **Пояснение к алгоритмам DES:**  
  
-   DESX был именован неправильно. Симметричные ключи, созданные с параметром ALGORITHM = DESX, в действительности используют шифр TRIPLE DES с 192-битным ключом. Алгоритм DESX не предоставляется. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES_3KEY, используют шифр TRIPLE DES с 192-битным ключом.  
  
-   Симметричные ключи, созданные с параметром ALGORITHM = TRIPLE_DES, используют шифр TRIPLE DES с 128-битным ключом.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
| Задача | Тип |
| ---- | ---- |
|Шифрование с помощью симметричного ключа.|[CREATE SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Шифрование с помощью асимметричного ключа.|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|Кодирование с использованием сертификата.|[CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|Шифрование файлов базы данных с помощью прозрачного шифрования данных.|[Прозрачное шифрование данных (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|Как зашифровать столбец таблицы.|[Шифрование столбца данных](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a>См. также:  
 [Шифрование SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Иерархия средств шифрования](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
