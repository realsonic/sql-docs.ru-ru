---
description: Настройка шифрования столбцов с помощью Always Encrypted с пакетом приложения уровня данных
title: Настройка шифрования столбцов с помощью Always Encrypted с пакетом приложения уровня данных | Документация Майкрософт
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 506abf476e04ab1a40059d04031949fa80b31acf
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559326"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>Настройка шифрования столбцов с помощью Always Encrypted с пакетом приложения уровня данных 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

[Пакет приложения уровня данных (DAC)](../../data-tier-applications/data-tier-applications.md), также известный как DACPAC, является переносимой единицей развертывания базы данных SQL Server, определяющей все объекты SQL Server, включая таблицы и столбцы внутри таблиц. При публикации DACPAC в базу данных (при обновлении базы данных с помощью DACPAC) схема целевой базы данных обновляется в соответствии со схемой в DACPAC. DACPAC можно опубликовать с помощью [мастера обновления приложения уровня данных](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard) в SQL Server Management Studio, [PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) или [sqlpackage](../../../tools/sqlpackage/sqlpackage-publish.md).

В этой статье рассматриваются особые замечания относительно обновления базы данных, когда DACPAC или (и) целевая база данных содержит столбцы, защищенные с помощью [Always Encrypted](always-encrypted-database-engine.md). Если схема шифрования столбца в DACPAC отличается от схемы шифрования существующего столбца в целевой базе данных, при публикации DACPAC происходит шифрование, расшифровка или повторное шифрование хранящихся в столбце данных. Подробные сведения приведены в представленной ниже таблице.

| Условие|Действие|
|:---|:---|
|Столбец зашифрован в DACPAC и не зашифрован в базе данных.| Данные в столбце не будут зашифрованы.|
|Столбец зашифрован в DACPAC и не зашифрован в базе данных.| Данные в столбце будут расшифрованы (для столбца будет отменено шифрование).|
| Столбец зашифрован в DACPAC и базе данных, но столбец в DACPAC использует другой тип шифрования и (или) другой ключ шифрования столбца, чем соответствующий столбец в базе данных.|Данные в столбце будет расшифрованы и повторно зашифрованы в соответствии с конфигурацией шифрования в DACPAC.|

Развертывание пакета DAC может также привести к созданию или удалению объектов метаданных для главных ключей столбцов или ключей шифрования столбцов в Always Encrypted.

## <a name="performance-considerations"></a>Рекомендации по производительности
Для выполнения криптографических операций средство, используемое для развертывания DACPAC, должно переместить данные за пределы базы данных. Средство создает в базе данных новую таблицу (или таблицы) с требуемой конфигурацией, загружает все данные из исходных таблиц, выполняет запрошенные криптографические операции, отправляет данные в новую таблицу, а затем меняет местами исходную и новую таблицы. Выполнение криптографических операций может занимать много времени. В течение этого периода база данных недоступна для записи транзакций. 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Если вы используете [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] и для экземпляра SQL Server настроен безопасный анклав, можно выполнять криптографические операции на месте без перемещения данных из базы данных. См. статью [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md). Обратите внимание, что шифрование на месте недоступно для развертываний DACPAC.

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>Разрешения для публикации пакета DAC, если функция Always Encrypted настроена

Для публикации пакета DAC в случае, если функция Always Encrypted настроена в DACPAC или (и) целевой базе данных, могут потребоваться некоторые или все перечисленные ниже разрешения, которые зависят от различий между схемой в DACPAC и схемой целевой базы данных.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Если операция обновления запускает операцию шифрования данных, также необходим доступ к главным ключам столбцов, настроенным для затрагиваемых столбцов.

- **Хранилище сертификатов — локальный компьютер** — требуется доступ на чтение к сертификату, который используется в качестве главного ключа столбца, либо необходимо быть администратором компьютера.
- **Хранилище ключей Azure** — требуются разрешения *create*, *get*, *unwrapKey*, *wrapKey*, *sign* и *verify* к хранилищу, содержащему главный ключ столбца.
- **Поставщик хранилища ключей (CNG)** — необходимые разрешения и учетные данные, которые могут быть запрошены при использовании хранилища ключей или ключа, зависят от конфигурации хранилища и KSP.
- **Поставщик служб шифрования (CAPI)** — необходимые разрешения и учетные данные, которые могут быть запрошены при использовании хранилища ключей или ключа, зависят от конфигурации хранилища и CSP.

Дополнительные сведения см. в разделе [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Создание и хранение главных ключей столбцов (постоянное шифрование)). 

 
## <a name="next-steps"></a>Next Steps
- [Разработка приложений с помощью Always Encrypted](always-encrypted-client-development.md)
- [Выполнение запросов к столбцам с помощью Always Encrypted с использованием SQL Server Management Studio](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>См. также:  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Общие сведения об управлении ключами для Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Настройка функции Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Настройка шифрования столбцов с помощью мастера Always Encrypted](always-encrypted-wizard.md)
 - [Настройка шифрования столбцов с помощью Always Encrypted с PowerShell](configure-column-encryption-using-powershell.md)
 
