---
title: Подключение к Azure Arc
titleSuffix: ''
description: Подключение экземпляра SQL Server к Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 5f0401081e6d437bcc0290c111bb5325e476650e
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103197"
---
# <a name="connect-your-sql-server-to-azure-arc"></a>Подключение SQL Server к Azure Arc

Вы можете подключить локальный экземпляр SQL Server к службе Azure Arc, выполнив следующие действия.

## <a name="prerequisites"></a>Предварительные требования

* На компьютере должен быть установлен хотя бы один экземпляр SQL Server.
* Поставщик ресурсов **Microsoft.AzureArcData** зарегистрирован с помощью одного из следующих методов:  
    * Используя портал Azure, выполните указанные ниже действия.
        - Выбор пункта **Подписки** 
        - Выберите свою подписку
        - В разделе **Параметры** с помощью пункта **Поставщики ресурсов**
        - Посредством поиска по `Microsoft.AzureArcData` и выбора **Зарегистрировать**
    * В PowerShell с помощью команды `Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData`
    * В интерфейсе командной строки с помощью команды `az provider register --namespace 'Microsoft.AzureArcData`

## <a name="generate-a-registration-script-for-sql-server"></a>Создание скрипта регистрации для SQL Server

На этом шаге вы создадите скрипт, который обнаруживает все экземпляры SQL Server, установленные на компьютере, и регистрирует их как ресурсы __SQL Server — Azure Arc__. Если физическая или виртуальная машина, на которой размещен экземпляр SQL Server, не зарегистрирована в службе Azure Arc, скрипт сделает это автоматически.

1. Найдите тип ресурса __SQL Server — Azure Arc__ и добавьте новый ресурс в колонке создания.

![Начало создания](media/join/start-creation-of-sql-server-azure-arc-resource.png)

2. Проверьте предварительные требования и перейдите на вкладку **Сведения о сервере**.  

3. Выберите подписку, группу ресурсов, регион Azure и операционную систему хост-компьютера. При необходимости укажите также прокси-сервер, используемый вашей сетью для подключения к Интернету.

> [!IMPORTANT]
> Если компьютер, на котором размещен экземпляр SQL Server, уже [подключен к службе Azure Arc](/azure/azure-arc/servers/onboard-portal), убедитесь, что выбрана та же группа ресурсов, которая содержит соответствующий ресурс __Компьютер — Azure Arc__.

![Сведения о сервере](media/join/server-details-sql-server-azure-arc.png)

4. Перейдите на вкладку **Запуск скрипта** и скачайте скрипт регистрации. Портал создает скрипт для указанной вами ОС размещения.

![Скачивание скрипта](media/join/download-script-sql-server-azure-arc.png)

## <a name="connect-the-installed-sql-server-instances-to-azure-arc"></a>Подключение установленных экземпляров SQL Server к службе Azure Arc

На этом шаге вы выполните скрипт, скачанный с портала Azure, на целевой физической или виртуальной машине. В результате каждый установленный экземпляр SQL Server на этом компьютере будет зарегистрирован как ресурс __SQL Server — Azure Arc__. Кроме того, если на компьютерах не установлен гостевой агент конфигурации, он будет установлен автоматически и зарегистрирован как ресурс __Компьютер — Azure Arc__.

> [!NOTE]
> Сценарий необходимо выполнять с использованием учетной записи, которая соответствует минимальным требованиям к разрешениям, описанным в разделе [Предварительные требования](overview.md#prerequisites).

### <a name="windows"></a>Windows

1. Запустите экземпляр администратора __powershell.exe__ и войдите в модуль PowerShell с помощью своих учетных данных Azure. Следуйте [инструкциям по входу](/powershell/azure/install-az-ps#sign-in).

2. Выполнение скачанного скрипта

   ```powershell
   & '.\RegisterSqlServerArc.ps1'
   ```

   > [!NOTE]
   > Если модуль AZ для PowerShell не был предварительно установлен, то при первом запуске возникнут проблемы. В таком случае выполните инструкции в скрипте, чтобы установить этот модуль и подключить свою учетную запись, а затем выполните скрипт снова.

### <a name="linux"></a>Linux

1. Используйте Azure CLI для входа со своими учетными данными Azure. Следуйте [инструкциям по входу](/cli/azure/authenticate-azure-cli).

2. Предоставьте скачанному скрипту разрешение на выполнение и выполните его.

   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="register-sql-server-instances-on-multiple-machines"></a>Регистрация экземпляров SQL Server на нескольких компьютерах

Вы можете подключить к Azure Arc несколько экземпляров SQL Server, установленных на нескольких компьютерах Windows или Linux, с помощью того же скрипта, который был создан для одного компьютера. Выполните инструкции по [масштабному подключению экземпляров SQL Server к Azure Arc](connect-at-scale.md).

## <a name="validate-the-sql-server---azure-arc-resources"></a>Проверка ресурсов SQL Server — Azure Arc

Перейдите на [портал Azure](https://ms.portal.azure.com/#home) и откройте только что зарегистрированный ресурс __SQL Server — Azure Arc__, чтобы проверить его.

![Проверка подключенного экземпляра SQL Server ](media/join/validate-sql-server-azure-arc.png)

## <a name="disconnect-your-sql-server-instance"></a>Отключение экземпляра SQL Server

Чтобы отключить экземпляр SQL Server от Azure Arc, перейдите на портал Azure, откройте ресурс __SQL Server — Azure Arc__ для этого экземпляра и нажмите кнопку **Отменить регистрацию**.

![Отмена регистрации экземпляра SQL Server](media/join/unregister-sql-server-azure-arc.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка расширенной защиты данных для экземпляра SQL Server](configure-advanced-data-security.md)

* [Настройка оценки SQL по запросу для экземпляра SQL Server](assess.md)