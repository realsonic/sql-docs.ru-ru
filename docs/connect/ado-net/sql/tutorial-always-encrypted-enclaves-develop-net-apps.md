---
description: Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами
title: Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами | Документация Майкрософт
ms.custom: ''
ms.date: 07/09/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 59c377d5055e8eb3858e1005c50d9c4dfb8334ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771572"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

В этом руководстве содержатся сведения о разработке простого приложения, которое выполняет запросы к базе данных, использующие безопасный анклав на стороне сервера для [Always Encrypted с защищенными анклавами](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted с безопасными анклавами поддерживается только в Windows.

## <a name="prerequisites"></a>Предварительные требования

Это руководство представляет собой продолжение статьи [Руководство. Начало работы с Always Encrypted с безопасными анклавами с использованием SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Убедитесь, что вы полностью изучили его, прежде чем выполнять действия ниже.

Кроме того, требуется среда Visual Studio (рекомендуется версия 2019), которую можно скачать на странице [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Среда разработки приложений должна использовать .NET Framework 4.6 или более поздней версии или .NET Core 2.1 или более поздней версии.

## <a name="step-1-set-up-your-visual-studio-project"></a>Шаг 1. Настройка проекта в Visual Studio

Чтобы использовать Always Encrypted с безопасными анклавами в приложении .NET Framework, необходимо убедиться, что приложение предназначено для .NET Framework 4.6 или более поздней версии. Чтобы использовать Always Encrypted с безопасными анклавами в приложении .NET Core, необходимо убедиться, что приложение предназначено для .NET Core 2.1 или более поздней версии.

Кроме того, если вы храните главный ключ столбца в Azure Key Vault, необходимо также интегрировать приложение с [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Запустите Visual Studio.

2. Создайте проект консольного приложения C\# (.NET Framework или Core).

3. Убедитесь, что в проекте настроена по крайней мере платформа .NET Framework 4.6. или .NET Core 2.1. Щелкните правой кнопкой мыши проект в обозревателе решений, выберите "Свойства" и установите целевую платформу.

4. Установите следующий пакет NuGet. Щелкните **Инструменты** (главное меню) > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. Выполните следующий код в консоли диспетчера пакетов.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Если вы используете Azure Key Vault для хранения главных ключей столбцов, установите следующие пакеты NuGet, щелкнув **Инструменты** (главное меню) > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. Выполните следующий код в консоли диспетчера пакетов.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Укажите `Attestation Protocol` и `Enclave Attestation Url` в строке подключения, которые будут использоваться в приложении для взаимодействия с SQL Server.

  ```cs
   Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Column Encryption Setting = Enabled
   ```

## <a name="step-2-implement-your-application-logic"></a>Шаг 2. Реализация логики приложения

Приложение подключится к базе данных **ContosoHR** из статьи [Руководство. Always Encrypted с безопасными анклавами в SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) и выполнит запрос, содержащий предикат `LIKE` в столбце **SSN**, а также проведет сравнение диапазонов в столбце **Salary**.

1. Замените содержимое файла Program.cs (созданного в Visual Studio) на приведенный ниже код. Обновите строку подключения к базе данных, указав имя сервера и URL-адрес аттестации анклава для своей среды. Можно также изменить параметры проверки подлинности базы данных.

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Выполните сборку и запустите приложение.

## <a name="see-also"></a>См. также раздел

- [Использование Always Encrypted с поставщиком данных Microsoft .NET для SQL Server](sqlclient-support-always-encrypted.md)
- [Example demonstrating use of Azure Key Vault provider with Always Encrypted](azure-key-vault-example.md) (Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted)
- [Example demonstrating use of Azure Key Vault provider with Always Encrypted enabled with Secure Enclaves](azure-key-vault-enclave-example.md) (Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted с безопасными анклавами)
