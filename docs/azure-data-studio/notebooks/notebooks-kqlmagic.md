---
title: Записные книжки с Kqlmagic (язык запросов Kusto) в Azure Data Studio
description: В этом руководстве показано, как создать и запустить Kqlmagic в Azure Data Studio.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: cd9145bb01350a2019f1bfaa12c5280151c35f49
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091748"
---
# <a name="kqlmagic-in-azure-data-studio"></a>Kqlmagic в Azure Data Studio

**Kqlmagic** — это команда, которая расширяет возможности ядра Python в **[записных книжках Azure Data Studio](./notebooks-guidance.md)** . Вы можете объединить Python и **[язык запросов Kusto (KQL)](/azure/data-explorer/kusto/query)** , чтобы запрашивать и визуализировать данные с использованием обширной библиотеки Plot.ly, интегрированной в команды `render`. Kqlmagic предоставляет преимущества записных книжек, анализ данных и широкие возможности Python в одном расположении. Поддерживаемые источники данных с Kqlmagic включают **[Azure Data Explorer](/azure/data-explorer/data-explorer-overview)** , **[Application Insights](/azure/azure-monitor/app/app-insights-overview)** и **[журналы Azure Monitor](/azure/azure-monitor/platform/data-platform-logs)** .

В этой статье показано, как создать и запустить записную книжку в Azure Data Studio с помощью расширения Kqlmagic для кластера Azure Data Explorer, журнала Application Insights и журналов Azure Monitor.

## <a name="prerequisites"></a>Предварительные требования

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-kqlmagic-in-a-notebook"></a>Установка и настройка Kqlmagic в записной книжке

Действия, описанные в этом разделе, выполняются в записной книжке Azure Data Studio.

1. Создайте новую записную книжку и измените **ядро** на *Python 3*.

   ![Новая записная книжка](media/notebooks-kqlmagic/install-new-notebook.png)

2. При появлении запроса выберите **Да**, чтобы обновить пакеты Python.

   ![Да](media/notebooks-kqlmagic/install-python-yes.png)

3. Установите библиотеку Kqlmagic.

   ```python
   !pip install Kqlmagic --no-cache-dir --upgrade
   ```

   Убедитесь, что она установлена:

   ```python
   !pip list
   ```

   ![Список](media/notebooks-kqlmagic/install-list.png)

4. Загрузите Kqlmagic.

   ```python
   %reload_ext Kqlmagic
   ```

   > [!Note]
   > Если этот шаг не удался, закройте файл и снова откройте его.

   ![Загрузка расширения Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

5. Чтобы проверить, правильно ли загружен Kqlmagic, просмотрите справочную документацию или проверьте версию.

   ```python
   %kql --help "help"
   ```

   > [!Note]
   > Если `Samples@help` запрашивает пароль, вы можете не указывать его и нажать клавишу **ВВОД**.

   ![Справка](media/notebooks-kqlmagic/install-help.png)

   Чтобы узнать, какая версия Kqlmagic установлена, выполните следующую команду.

   ```python
   %kql --version
   ```

## <a name="kqlmagic-with-an-azure-data-explorer-cluster"></a>Kqlmagic с кластером Azure Data Explorer

В этом разделе объясняется, как запустить анализ данных с помощью Kqlmagic с кластером Azure Data Explorer.

### <a name="load-and-authenticate-kqlmagic-for-azure-data-explorer"></a><a name="ade-load-auth"></a> Загрузка и проверка подлинности Kqlmagic для Azure Data Explorer

   > [!Note]
   > Каждый раз, когда вы создаете новую записную книжку в Azure Data Studio, необходимо загрузить расширение Kqlmagic.

1. Убедитесь, что для параметра **Ядро** установлено *Python3*.

   ![Изменение ядра](media/notebooks-kqlmagic/change-kernel.png)

2. Загрузите Kqlmagic.

   ```python
   %reload_ext Kqlmagic
   ```

   ![Загрузка расширения Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

3. Подключитесь к кластеру и выполните проверку подлинности.

   ```python
   %kql azureDataExplorer://code;cluster='help';database='Samples'
   ```

    > [!Note]
    > Если вы используете собственный кластер ADX, включите в строку подключения регион следующим образом:   
    ```%kql azuredataexplorer://code;cluster='mycluster.westus';database='mykustodb'```

   Для проверки подлинности используется имя входа устройства. Скопируйте код из выходных данных и выберите **проверка подлинности**, чтобы открыть браузер, где необходимо вставить код. После успешного выполнения проверки подлинности можно вернуться к Azure Data Studio, чтобы продолжить выполнение остальной части скрипта.

   ![Проверка подлинности Azure Data Explorer](media/notebooks-kqlmagic/ade-auth.png)

### <a name="query-and-visualize-for-azure-data-explorer"></a>Запрос и визуализация для Azure Data Explorer

Запрашивайте данные с помощью [оператора отображения](/azure/data-explorer/kusto/query/renderoperator) и визуализируйте их с помощью библиотеки ploy.ly. Для запрашивания и визуализации данных используется интегрированный интерфейс, использующий собственный KQL.

1. Анализ 10 главных гроз по штату и частоте

   ```python
   %kql StormEvents | summarize count() by State | sort by count_ | limit 10
   ```

   Если вы знакомы с языком запросов Kusto (KQL), введите запрос после `%kql`.

   ![Анализ гроз](media/notebooks-kqlmagic/ade-analyze-storm-events.png)

2. Визуализация диаграммы временной шкалы:

   ```python
   %kql StormEvents \
   | summarize event_count=count() by bin(StartTime, 1d) \
   | render timechart title= 'Daily Storm Events'
   ```

   ![визуализация временной шкалы](media/notebooks-kqlmagic/ade-visualize-timechart.png)

3. Пример многострочного запроса с использованием `%%kql`.

   ```python
   %%kql
   StormEvents
   | summarize count() by State
   | sort by count_
   | limit 10
   | render columnchart title='Top 10 States by Storm Event count'
   ```

   ![Пример многострочного запроса](media/notebooks-kqlmagic/ade-multiline-query-sample.png)

## <a name="kqlmagic-with-application-insights"></a>Kqlmagic с Application Insights

### <a name="load-and-authenticate-kqlmagic-for-application-insights"></a><a name="appin-load-auth"></a> Загрузка и проверка подлинности Kqlmagic для Application Insights

1. Убедитесь, что для параметра **Ядро** установлено *Python3*.

   ![Ядро](media/notebooks-kqlmagic/change-kernel.png)

2. Загрузите Kqlmagic.

   ```python
   %reload_ext Kqlmagic
   ```

   ![Загрузка расширения Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > Каждый раз, когда вы создаете новую записную книжку в Azure Data Studio, необходимо загрузить расширение Kqlmagic.

3. Подключение и проверка подлинности.

   Сначала необходимо создать ключ API для ресурса Application Insights. Затем используйте идентификатор приложения и ключ API для подключения к Application Insights из записной книжки:

   ```python
   %kql appinsights://appid='DEMO_APP';appkey='DEMO_KEY'
   ```

### <a name="query-and-visualize-for-application-insights"></a>Запрос и визуализация для Application Insights

Запрашивайте данные с помощью [оператора отображения](/azure/data-explorer/kusto/query/renderoperator) и визуализируйте их с помощью библиотеки ploy.ly. Для запрашивания и визуализации данных используется интегрированный интерфейс, использующий собственный KQL.

1. Показать просмотры страниц

   ```python
   %%kql
   pageViews
   | limit 10
   ```

   ![Просмотры страницы](media/notebooks-kqlmagic/appin-page-views.png)

   > [!Note]
   > С помощью мыши перетащите область диаграммы, чтобы увеличить конкретные даты.

2. Показывать просмотры страниц на диаграмме временной шкалы:

   ```python
   %%kql
   pageViews
   | summarize event_count=count() by name, bin(timestamp, 1d)
   | render timechart title= 'Daily Page Views'
   ```

   ![Диаграмма временной шкалы](media/notebooks-kqlmagic/appin-timechart.png)

## <a name="kqlmagic-with-azure-monitor-logs"></a>Kqlmagic с журналами Azure Monitor

### <a name="load-and-authenticate-kqlmagic-for-azure-monitor-logs"></a><a name="aml-load-auth"></a> Загрузка и проверка подлинности Kqlmagic для журналов Azure Monitor

1. Убедитесь, что для параметра **Ядро** установлено *Python3*.

   ![Change](media/notebooks-kqlmagic/change-kernel.png)

2. Загрузите Kqlmagic.

   ```python
   %reload_ext Kqlmagic
   ```

   ![Загрузка расширения Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > Каждый раз, когда вы создаете новую записную книжку в Azure Data Studio, необходимо загрузить расширение Kqlmagic.

3. Подключение и проверка подлинности:

   ```python
   %kql loganalytics://workspace='DEMO_WORKSPACE';appkey='DEMO_KEY';alias='myworkspace'
   ```

   ![проверка подлинности Log Analytics](media/notebooks-kqlmagic/aml-auth.png)

### <a name="query-and-visualize-for-azure-monitor-logs"></a>Запрос и визуализация для журналов Azure Monitor

Запрашивайте данные с помощью [оператора отображения](/azure/data-explorer/kusto/query/renderoperator) и визуализируйте их с помощью библиотеки ploy.ly. Для запрашивания и визуализации данных используется интегрированный интерфейс, использующий собственный KQL.

1. Просмотр диаграммы временной шкалы:

   ```python
   %%kql
   KubeNodeInventory
   | summarize event_count=count() by Status, bin(TimeGenerated, 1d)
   | render timechart title= 'Daily Kubernetes Nodes'
   ```

   ![временная диаграмма узлов Kubernetes по дням в Log Analytics](media/notebooks-kqlmagic/aml-timechart-daily-kubernetes-nodes.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о записных книжках и Kqlmagic

- [Расширение Kusto (KQL) для Azure Data Studio (предварительная версия)](../extensions/kusto-extension.md)
- [Создание и запуск записной книжки Kusto (KQL) (предварительная версия)](./notebooks-kusto-kernel.md)
- [Использование расширения Jupyter Notebook и Kqlmagic для анализа данных в Azure Data Explorer](/azure/data-explorer/Kqlmagic)
- [Расширение (Magic) для Jupyter Notebook и Jupyter Lab](https://github.com/Microsoft/jupyter-Kqlmagic), обеспечивающее работу с данными Kusto, Application Insights и Log Analytics в записных книжках.
- [Kqlmagic](https://pypi.org/project/Kqlmagic/)
- [Использование записных книжек в Azure Data Studio](./notebooks-guidance.md)