---
title: Регистрация компьютера в службе защиты узла
description: Регистрация компьютера SQL Server в службе защиты узла для Always Encrypted с безопасными анклавами.
ms.custom: ''
ms.date: 11/15/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d1b2a7209de25b1ce5c988ec9a46b77369dcf70
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101836"
---
# <a name="register-computer-with-host-guardian-service"></a>Регистрация компьютера в службе защиты узла

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

В этой статье описывается, как зарегистрировать компьютеры [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] для прохождения аттестации с помощью службы защиты узла (HGS).

Перед началом работы нужно развернуть по крайней мере один компьютер HGS и настроить службу аттестации.
Дополнительные сведения см. в статье [Развертывание службы защиты узла для [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md).

## <a name="step-1-install-the-attestation-client-components"></a>Шаг 1. Установка клиентских компонентов для аттестации

Чтобы клиент SQL мог убедиться в том, что он взаимодействует с надежным компьютером [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], компьютер [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] должен успешно пройти аттестацию в службе защиты узла.
Процесс аттестации управляется дополнительным компонентом Windows, который называется "клиент HGS".
Выполните приведенные ниже шаги, чтобы установить этот компонент и начать аттестацию.

1. Убедитесь в том, что компьютер [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] соответствует [предварительным требованиям, изложенным в документе по планированию HGS](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites).

2. Выполните приведенную ниже команду в консоли PowerShell с повышенными привилегиями, чтобы установить компонент "Служба защиты узла — поддержка Hyper-V", который включает в себя клиент HGS и компоненты аттестации.

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. Чтобы завершить установку, перезагрузите компьютер.

## <a name="step-2-verify-virtualization-based-security-is-running"></a>Шаг 2. Проверка выполнения функции безопасности на основе виртуализации

При установке компонента "Служба защиты узла — поддержка Hyper-V" автоматически настраивается и включается функция безопасности на основе виртуализации (VBS).
Анклавы для [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted защищаются средой VBS и выполняются в ней.
Функция VBS может не запуститься, если на компьютере не установлено и не включено устройство IOMMU.
Чтобы проверить, работает ли VBS, откройте средство "Сведения о системе", запустив файл `msinfo32.exe`, и найдите элементы `Virtualization-based security` ближе к концу списка "Сведения о системе".

![Снимок экрана: состояние и конфигурация безопасности на основе виртуализации в средстве "Сведения о системе"](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

Первый элемент, который нужно проверить, — `Virtualization-based security`. Он может иметь одно из трех состояний.

- `Running` означает, что функция VBS настроена правильно и была успешно запущена. Если вы видите это состояние, то можете перейти к шагу 3.
- `Enabled but not running` означает, что запуск функции VBS настроен, но оборудование не отвечает минимальным требованиям для запуска VBS. Может потребоваться изменить конфигурацию оборудования в BIOS или UEFI, включив дополнительные функции процессора, такие как IOMMU, или, если оборудование действительно не поддерживает необходимые функции, может потребоваться снизить требования для VBS. Дополнительные сведения см. далее в этом разделе.
- `Not enabled` означает, что запуск функции VBS не настроен. Компонент "Служба защиты узла — поддержка Hyper-V" автоматически включает функцию VBS, поэтому, если вы видите это состояние, рекомендуется повторить шаг 1.

Если функция VBS не выполняется на компьютере, проверьте свойства `Virtualization-based security`. Сравните значения элемента `Required Security Properties` со значениями элемента `Available Security Properties`.
Чтобы функция VBS выполнялась, требуемые свойства должны быть равны доступным свойствам безопасности или являться их подмножеством.

В контексте аттестации анклавов [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] свойства безопасности имеют следующую важность.

- `Base virtualization support` требуется всегда, так как представляет минимальный набор компонентов оборудования, необходимый для запуска гипервизора.
- `Secure Boot` рекомендуется, но не требуется для [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. Безопасная загрузка защищает от программ rootkit, требуя запуска подписанного корпорацией Майкрософт загрузчика после завершения инициализации UEFI. Если вы используете аттестацию доверенного платформенного модуля (TPM), безопасная загрузка включается после соответствующей оценки независимо от того, требует ли функция VBS ее включения.
- `DMA Protection` рекомендуется, но не требуется для [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. Защита DMA использует IOMMU для защиты VBS и памяти анклава от атак с прямым доступом к памяти. В рабочей среде следует всегда использовать компьютеры с защитой DMA. В среде разработки или тестирования можно сделать защиту DMA необязательной. Если экземпляр [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] виртуализирован, скорее всего, защита DMA будет недоступна и потребуется снять требование обязательного запуска VBS. Сведения о более низких гарантиях безопасности при выполнении в виртуальной машине см. в описании [модели доверия](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model).

Прежде чем понижать требования к функциям безопасности для VBS, обратитесь к изготовителю оборудования или поставщику облачных служб, чтобы узнать, можно ли каким-либо образом обеспечить выполнение недостающих требований платформы в UEFI или BIOS (например, включить безопасную загрузку, Intel VT-d или AMD IOV).

Чтобы изменить требуемые функции безопасности платформы для VBS, выполните следующую команду в консоли PowerShell с повышенными привилегиями:

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

После изменения реестра перезагрузите компьютер [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] и проверьте, запустилась ли функция VBS снова.

Если компьютер управляется вашей компанией, групповая политика или Microsoft Endpoint Manager могут переопределить изменения, внесенные в эти разделы реестра, после перезагрузки.
Чтобы узнать, развернуты ли политики, управляющие конфигурацией VBS, обратитесь в свою службу технической ИТ-поддержки.

## <a name="step-3-configure-the-attestation-url"></a>Шаг 3. Настройка URL-адреса аттестации

Далее вы настроите для компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] URL-адрес службы аттестации HGS.

В консоли Windows PowerShell с повышенными привилегиями выполните приведенную ниже команду, чтобы настроить URL-адрес аттестации, изменив ее нужным образом.

- Замените `hgs.bastion.local` именем кластера HGS.
- Чтобы узнать имя кластера, можно выполнить командлет `Get-HgsServer` на любом компьютере HGS.
- URL-адрес аттестации всегда должен заканчиваться на `/Attestation`.
- SQL Server не использует функции защиты ключей HGS, поэтому укажите для `-KeyProtectionServerUrl` любой фиктивный URL-адрес, например `http://localhost`.

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

Если вы не зарегистрировали этот компьютер в службе HGS ранее, команда сообщит об ошибке аттестации. Это нормальный результат.

В поле `AttestationMode` в выходных данных командлета указывается режим аттестации, используемый в HGS.

Перейдите к [шагу 4А](#step-4a-register-a-computer-in-tpm-mode), чтобы зарегистрировать компьютер в режиме TPM, или [шагу 4Б](#step-4b-register-a-computer-in-host-key-mode), чтобы зарегистрировать компьютер в режиме ключа узла.

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>Шаг 4А. Регистрация компьютера в режиме TPM

На этом шаге вы собираете сведения о состоянии модуля TPM компьютера и регистрируете их в службе HGS.

Если для службы аттестации HGS настроен режим ключа узла, вместо этого перейдите к [шагу 4Б](#step-4b-register-a-computer-in-host-key-mode).

Прежде чем приступить к сбору показателей TPM, убедитесь в том, что вы работаете с известной конфигурацией компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
На компьютере должно быть установлено все необходимое оборудование, а также применены последние обновления встроенного ПО и программного обеспечения.
Служба HGS измеряет показатели компьютеров относительно этих базовых показателей при аттестации, поэтому при сборе показателей модуля TPM важно иметь максимально безопасное состояние.

Для аттестации модуля TPM собираются три файла данных, некоторые из которых можно использовать повторно при наличии одинаково настроенных компьютеров.

| Артефакт аттестации | Что измеряется | Уникальность |
| -------------------- | ---------------- | ---------- |
| Идентификатор платформы  | Открытый ключ подтверждения в модуле TPM компьютера и сертификат ключа подтверждения от производителя модуля TPM. | 1 на каждый компьютер |
| Базовые показатели TPM | Регистры конфигурации платформы (PCR) в модуле TPM, которые измеряют показатели конфигурации встроенного ПО и ОС, загруженной в процессе загрузки. К примерам относятся состояние безопасной загрузки и шифрование аварийных дампов. | Один базовый план на уникальную конфигурацию компьютера (для одинакового оборудования и программного обеспечения можно использовать один и тот же базовый план) |
| Политика целостности кода | Надежная политика [управления приложениями в Защитнике Windows](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) для защиты компьютеров | Одна на уникальную политику непрерывной интеграции, развернутую на компьютерах. |

В HGS можно настроить более одного артефакта аттестации каждого типа для поддержки смешанного парка оборудования и программного обеспечения.
Служба HGS требует, чтобы проходящий аттестацию компьютер соответствовал хотя бы одной политике из каждой категории.
Например, если в службе HGS зарегистрированы три базовых плана TPM, компьютер будет считаться соответствующим политике, если его показатели соответствуют любому из них.

### <a name="configure-a-code-integrity-policy"></a>Настройка политики целостности кода

Служба HGS требует, чтобы к каждому компьютеру, проходящему аттестацию в режиме TPM, применялась политика управления приложениями в Защитнике Windows (WDAC).
Политики целостности кода WDAC ограничивают программное обеспечение, которое может выполняться на компьютере, проверяя каждый процесс, пытающийся выполнить код, по списку доверенных издателей и хэшей файлов.
В варианте использования [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] анклавы защищены функцией безопасности на основе виртуализации и не могут изменяться из операционной системы узла, поэтому строгость политики WDAC не влияет на безопасность зашифрованных запросов.
По этой причине с целью удовлетворения требований для аттестации на компьютерах [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] рекомендуется развернуть простую политику с режимом аудита, не налагая дополнительные ограничения на систему.

Если вы уже используете настраиваемую политику целостности кода WDAC на компьютерах для защиты конфигурации ОС, можно перейти к разделу [Получение сведений для аттестации TPM](#collect-tpm-attestation-information).

1. В операционных системах Windows Server 2019, Windows 10 версии 1809 и более поздних версий имеются готовые примеры политик. Политика `AllowAll` позволяет запускать на компьютере любое программное обеспечение без ограничений. Чтобы использовать политику, преобразуйте ее в двоичную форму, понятную операционной системе и службе HGS. Чтобы скомпилировать политику `AllowAll`, в консоли PowerShell с повышенными привилегиями выполните следующие команды.

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. Чтобы развернуть файл `allowall_cipolicy.bin` на компьютерах [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] с помощью [групповой политики](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy), выполните инструкции в [руководстве по развертыванию управления приложениями в Защитнике Windows](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide). Для компьютеров из рабочей группы выполните тот же процесс в редакторе локальных групповых политик (`gpedit.msc`).

3. На компьютерах [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] выполните команду `gpupdate /force`, чтобы настроить новую политику целостности кода, а затем перезагрузите компьютеры, чтобы применить политику.

### <a name="collect-tpm-attestation-information"></a>Сбор сведений для аттестации TPM

Выполните указанные ниже действия для каждого компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], который будет проходить аттестацию с помощью HGS.

1. На компьютере, который находится в достоверно хорошем состоянии, выполните следующие команды в PowerShell, чтобы собрать сведения для аттестации TPM:

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. Скопируйте три файла аттестации на сервер HGS.

3. На сервере HGS в консоли PowerShell с повышенными привилегиями выполните следующие команды для регистрации компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > Если при попытке зарегистрировать уникальный идентификатор модуля TPM возникла ошибка, убедитесь в том, что вы [импортировали промежуточные и корневые сертификаты TPM](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation) на используемый компьютер HGS.

Помимо идентификатора платформы, базовых показателей TPM и политики целостности кода, может потребоваться изменить встроенные политики, настраиваемые и применяемые службой HGS.
Эти встроенные политики измеряются относительно базовых показателей модуля TPM, которые собираются с сервера, и представляют различные параметры безопасности, которые должны быть включены для защиты компьютера.
Если имеются компьютеры, на которых нет блока IOMMU для защиты от атак путем прямого доступа к памяти (например, виртуальные машины), необходимо отключить политику IOMMU.

Чтобы отключить требование наличия IOMMU, выполните следующую команду на сервере HGS:

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> Если отключить политику IOMMU, блоки IOMMU не будут требоваться на компьютерах, проходящих аттестацию в службе HGS.
> Политику IOMMU нельзя отключить для отдельного компьютера.

Список зарегистрированных узлов и политик TPM можно просмотреть с помощью следующих команд PowerShell:

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>Шаг 4Б. Регистрация компьютера в режиме ключа узла

В этом разделе приводятся пошаговые инструкции по созданию уникального ключа для узла и его регистрации в службе HGS.
Если для службы аттестации HGS настроен режим TPM, вместо этого выполните инструкции в [шаге 4А](#step-4a-register-a-computer-in-tpm-mode).

Для аттестации ключа узла на компьютере [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] создается пара асимметричных ключей, и службе HGS предоставляется ее открытая половина.
Чтобы создать пару ключей, в консоли PowerShell с повышенными привилегиями выполните следующую команду:

```powershell
Set-HgsClientHostKey
Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
```

Если вы уже создали ключ узла и хотите создать новую пару ключей, вместо этого используйте следующие команды:

```powershell
Remove-HgsClientHostKey
Set-HgsClientHostKey
Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
```

После создания ключа узла скопируйте файл сертификата на сервер HGS и выполните следующую команду в консоли PowerShell с повышенными привилегиями, чтобы зарегистрировать компьютер [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

```powershell
Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
```

Повторите шаг 4Б для каждого компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], который будет проходить аттестацию в службе HGS.

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>Шаг 5. Проверка успешной аттестации узла

После регистрации компьютера [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] в службе HGS ([шаг 4А](#step-4a-register-a-computer-in-tpm-mode) для режима TPM или [шаг 4Б](#step-4b-register-a-computer-in-host-key-mode) для режима ключа узла) необходимо проверить возможность его успешной аттестации.

Вы можете проверить конфигурацию клиента аттестации HGS и выполнить попытку аттестации в любое время с помощью командлета [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration).
Выходные данные команды будут выглядеть следующим образом:

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

Два наиболее важных поля в выходных данных — `AttestationStatus` (показывает, прошел ли компьютер аттестацию) и `AttestationSubStatus` (показывает, какие политики не соблюдаются, если компьютер не прошел аттестацию).

Ниже описываются наиболее распространенные значения поля `AttestationStatus`.

| `AttestationStatus` | Объяснение |
| ----------------- | ----------- |
| Срок действия истек | Узел проходил аттестацию ранее, но истек срок действия выданного ему сертификата работоспособности. Проверьте, синхронизировано ли время узла и службы HGS. |
| `InsecureHostConfiguration` | Компьютер не соответствует одной или нескольким политикам аттестации, настроенным на сервере HGS. Дополнительные сведения см. в разделе `AttestationSubStatus`. |
| NotConfigured | На компьютере не настроен URL-адрес аттестации. [Настройте URL-адрес](#step-3-configure-the-attestation-url). |
| Passed | Компьютер прошел аттестацию и является доверенным для выполнения анклавов [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |
| `TransientError` | Попытка аттестации завершилась сбоем из-за временной ошибки. Эта ошибка обычно означает, что возникла проблема при обращении к службе HGS по сети. Проверьте сетевое подключение и возможность разрешения имени службы HGS и маршрутизации к нему. |
| `TpmError` | Устройство TPM компьютера сообщило об ошибке во время попытки аттестации. Дополнительные сведения см. в журналах TPM. Очистка модуля TPM может помочь устранить эту проблему, но перед этим не забудьте приостановить работу BitLocker и других служб, использующих модуль TPM. |
| `UnauthorizedHost` | Ключ узла неизвестен службе HGS. Чтобы зарегистрировать компьютер в службе HGS, выполните инструкции из [шага 4Б](#step-4b-register-a-computer-in-host-key-mode). |

Если поле `AttestationStatus` содержит значение `InsecureHostConfiguration`, в поле `AttestationSubStatus` будет указано имя одной или нескольких политик, приведших к сбою.
В таблице ниже описываются наиболее распространенные значения и способы устранения ошибок.

| AttestationSubStatus | Пояснение и требуемые меры |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | Политика целостности кода на компьютере не зарегистрирована в службе HGS, *или* на компьютере в настоящее время не используется политика целостности кода. Инструкции см. в разделе [Настройка политики целостности кода](#configure-a-code-integrity-policy). |
| DumpsEnabled | На компьютере разрешены аварийные дампы, но политика Hgs_DumpsEnabled запрещает их. Отключите дампы на этом компьютере или отключите политику Hgs_DumpsEnabled, чтобы продолжить. |
| FullBoot | Компьютер вышел из спящего режима или гибернации, что привело к изменениям в показателях TPM. Перезапустите компьютер, чтобы создать изначальные показатели TPM. |
| HibernationEnabled | На компьютере разрешена гибернация с незашифрованными файлами гибернации. Чтобы устранить эту проблему, отключите гибернацию на компьютере. |
| HypervisorEnforcedCodeIntegrityPolicy | На компьютере не настроено использование политики целостности кода. Проверьте узел "Групповая политика" или "Локальная групповая политика" > "Конфигурация компьютера" > "Административные шаблоны" > "Система" > Device Guard > "Включить средство обеспечения безопасности на основе виртуализации" > "Защита проверки целостности кода на основе виртуализации". Этот элемент политики должен иметь значение "Включено без блокировки UEFI". |
| Iommu | На этом компьютере не включено устройство IOMMU. Если это физический компьютер, включите IOMMU в меню конфигурации UEFI. Если это виртуальная машина и блок IOMMU недоступен, выполните команду `Disable-HgsAttestationPolicy Hgs_IommuEnabled` на сервере HGS. |
| SecureBoot | На этом компьютере не включена безопасная загрузка. Чтобы устранить эту ошибку, включите безопасную загрузку в меню конфигурации UEFI. |
| VirtualSecureMode | На этом компьютере не запущена функция безопасности на основе виртуализации. Следуйте инструкциям в разделе [Шаг 2. Проверка выполнения функции безопасности на основе виртуализации](#step-2-verify-virtualization-based-security-is-running). |