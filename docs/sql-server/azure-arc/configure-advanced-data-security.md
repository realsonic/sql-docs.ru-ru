---
title: Настройка расширенной защиты данных
titleSuffix: Azure Arc
description: Настройка расширенной защиты данных для экземпляра SQL Server с поддержкой Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: e0c68752d21de5945c06433af8c08f6208322f7c
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103177"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Настройка расширенной защиты данных для экземпляра SQL Server с поддержкой Azure Arc

Вы можете включить расширенную защиту данных для экземпляров SQL Server локально, выполнив следующие действия.

## <a name="prerequisites"></a>Предварительные требования

* Экземпляр SQL Server должен быть подключен к SQL Server с поддержкой Arc. Выполните инструкции по [подключению экземпляра SQL Server к SQL Server с поддержкой Arc](connect.md).

* Вашей учетной записи должна быть назначена одна из [ролей Центра безопасности (RBAC)](/azure/security-center/security-center-permissions)

## <a name="create-a-log-analytics-workspace"></a>Создание рабочей области Log Analytics

1. Найдите тип ресурса __Рабочие области Log Analytics__ и добавьте новый такой ресурс в колонке создания.

   ![Создать рабочую область](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > Вы можете использовать рабочую область Log Analytics в любом регионе, в том числе уже имеющуюся у вас область. Однако мы рекомендуем создать ее в том же регионе, где создается ресурс __Компьютер — Azure Arc__.

1. Перейдите на страницу обзора ресурса рабочей области Log Analytics и выберите "Windows, Linux и другие источники". Скопируйте ИД рабочей области и первичный ключ для использования позже.

   ![Колонка рабочей области Log Analytics](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Установите Microsoft Monitoring Agent (MMA)

Следующий шаг необходим, только если вы еще не настроили агент MMA на удаленном компьютере.

1. Выберите ресурс __Компьютер — Azure Arc__ для виртуального или физического сервера, где установлен экземпляр SQL Server, и добавьте расширение __Microsoft Monitoring Agent — Azure Arc__ с помощью функции **Расширения**. При запросе о настройке рабочей области Log Analytics используйте ее ИД и первичный ключ, сохраненные на предыдущем шаге.

   ![Установка MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. После прохождения проверки щелкните **Создать**, чтобы запустить рабочий процесс развертывания для расширения MMA Arc. После завершения развертывания состояние изменится на **Выполнено**.

1. Дополнительные сведения: [Управление расширениями с помощью Azure Arc](/azure/azure-arc/servers/manage-vm-extensions).

## <a name="enable-azure-defender"></a>Включение Azure Defender

Далее необходимо включить Azure Defender для экземпляра SQL Server.

1. Перейдите в Центр безопасности и откройте страницу **Цены и параметры** на боковой панели.

1. Выберите рабочую область, настроенную для расширения MMA на предыдущем шаге.

1. Выберите **Включить Azure Defender**. Обязательно включите параметр **Cерверы SQL на компьютере**.

   ![Обновление рабочей области](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > Первое сканирование для создания оценки уязвимостей будет выполнено в течение 24 часов после включения расширенной защиты данных. После этого автоматическое сканирование будет выполняться каждую неделю по воскресеньям.

## <a name="explore"></a>Анализ

Анализируйте аномалии и угрозы безопасности в Центре безопасности Azure.

1. Откройте ресурс SQL Server — Azure Arc и выберите **Безопасность** в меню слева, чтобы просмотреть рекомендации и оповещения по текущему экземпляру.

   ![Выбор заголовка "Безопасность"](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. Щелкните любую из рекомендаций, чтобы просмотреть сведения об уязвимостях в __Центре безопасности__ .

   ![Отчет об уязвимостях](media/configure-advanced-data-security/vulnerabilities-report.png)

1. Щелкните любое оповещение системы безопасности, чтобы получить полные сведения и подробнее проанализировать атаку в [Azure Sentinel](/azure/sentinel/overview). На следующем изображении показан пример оповещения об атаке методом подбора.

   ![Оповещение об атаке методом подбора](media/configure-advanced-data-security/brute-force-alert.png)

1. Щелкните **Принять меры** для устранения указанной в оповещении проблемы.

   ![Обработка оповещения](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> Основная ссылка __Центр безопасности__ вверху страницы не ведет на URL-адрес портала предварительной версии, поэтому ваши ресурсы __SQL Server — Azure Arc__ по ней отображаться не будут. Рекомендуем переходить по ссылкам из конкретных рекомендаций и оповещений.

## <a name="next-steps"></a>Дальнейшие действия

Вы можете подробнее анализировать оповещения системы безопасности и атаки с помощью [Azure Sentinel](/azure/sentinel/overview). Воспользуйтесь инструкциями по [подключению Azure Sentinel](/azure/sentinel/connect-data-sources).