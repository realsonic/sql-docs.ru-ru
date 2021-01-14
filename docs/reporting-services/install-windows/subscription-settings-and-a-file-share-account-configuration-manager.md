---
title: Параметры подписки и учетная запись общей папки (диспетчер конфигураций) | Документация Майкрософт
description: На странице "Параметры подписки" диспетчера конфигурации сервера отчетов можно настроить учетную запись общей папки для серверов отчетов, работающих в собственном режиме, и подписки на общую папку.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 90e585e887e9415a8cf1f9d0104595547f2d1c87
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171986"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>Параметры подписки и учетная запись общей папки (диспетчер конфигурации сервера отчетов)
  На странице **Параметры подписки** диспетчера конфигураций [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] можно настроить учетную запись общей папки для серверов отчетов, работающих в основном режиме, и подписки на общую папку. Учетная запись общей папки позволяет использовать один набор учетных данных в нескольких подписках, доставляющих отчеты в общую папку. Когда требуется изменить учетные данные, достаточно настроить изменение учетной записи общей папки, и вам не придется обновлять каждую подписку по отдельности.  
  
 Для подписок на общую папку [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] существуют два рабочих процесса:  
  
-   Новая возможность в выпуске [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] . Администратор [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] может настроить для общей папки одну учетную запись, которая будет использоваться для одной или нескольких подписок. Настройте параметр **Указать учетную запись общей папки**, а затем на страницах конфигурации отдельных подписок пользователи выберут параметр **Использовать учетную запись общей папки**.  
  
-   Настройка отдельных подписок с разными учетными данными для целевой общей папки.  
  
-   Кроме того, два этих подхода можно использовать совместно. Например, когда какие-то подписки на общую папку используют централизованную учетную запись общей папки, а другие подписки используют разные учетные данные.  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Собственный режим.  
  
## <a name="specify-a-file-share-account"></a>Указать учетную запись общей папки  
 Если выбран этот параметр, можно указать, какая учетная запись должна использоваться для доступа к общим папкам с сервера отчетов. Если настроить учетную запись общей папки, все пользователи смогут выбрать учетную запись для любых подписок, настроенных для доставки отчетов в общую папку. Если этот параметр не выбран, учетная запись общей папки **недоступна** для каких-либо подписок.  
  
 Обязательно убедитесь, что учетная запись, настраиваемая для общей папки, имеет разрешения на чтение и запись в любых общих папках, которые будут использоваться пользователями для доставки в общую папку.  
  
 На следующем рисунке показано, что видят пользователи в подписках, настроенных для доставки в общую папку. Параметр **Использовать учетную запись общей папки** отключается, если учетная запись общей папки не настроена.  
  
 ![учетная запись общей папки диспетчера конфигураций](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "учетная запись общей папки диспетчера конфигураций")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>Предотвращение эскалации привилегий или более высокого уровня привилегий  
  
> [!IMPORTANT]
> Учетная запись служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] контролирует доставку подписок и взаимодействует с учетной записью, используемой для подписок на общую папку. Функции безопасности Windows ограничивают сочетания 1) учетной записи служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и 2) учетной записи, используемой для учетных записей общих папок. Например, если для общей папки используется встроенная учетная запись операционной системы, то учетной записью служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] должна быть другая учетная запись с разрешениями на олицетворение. Если настроены явная учетная запись общей папки и пароль, то для учетной записи общей папки требуется право на вход на компьютер, где работают службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Если у учетной записи общей папки нет необходимых разрешений, произойдет сбой использующих ее подписок с сообщением об ошибке следующего вида:  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>Пример скрипта PowerShell для аудита использования учетной записи общей папки  
 Чтобы получить список всех подписок [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , настроенных на использование **учетной записи общей папки**, выполните приведенный ниже скрипт Windows PowerShell. Замените `SERVERNAME` соответствующим значением для сервера отчетов.  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 Выходные данные скрипта выглядят примерно так:  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>См. также:  
 [Доставка отчетов в общие папки с помощью служб Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Создание и администрирование подписок для серверов отчетов в собственном режиме](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
