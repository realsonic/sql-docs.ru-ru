---
title: Руководство по разработке прогнозной модели в R
titleSuffix: SQL machine learning
description: В этом цикле учебников, состоящем из четырех частей, вы подготовите данные для обучения прогнозной модели в R с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 12441de331661e5edbf40ca678963b80a70764a2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101865"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>Руководство по Разработка прогнозной модели в R с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R и модель машинного обучения в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md) для прогнозирования количества прокатов лыж.
::: moniker-end
::: moniker range="=sql-server-2017"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R и модель машинного обучения в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) для прогнозирования количества прокатов лыж.
::: moniker-end
::: moniker range="=sql-server-2016"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R и модель машинного обучения в службах [SQL Server R Services](../r/sql-server-r-services.md) для прогнозирования количества прокатов лыж.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R и модель машинного обучения в [Службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) для прогнозирования количества прокатов лыж.
::: moniker-end

Представьте, что вы являетесь владельцем компании по прокату лыж и хотите спрогнозировать количество прокатов за некоторый будущий период. Эта информация поможет вам подготовить инвентарь, персонал и пункты проката.

В первой части учебника вы установите необходимые компоненты. Во второй и третьей частях вы создадите сценарии R в записной книжке для подготовки данных и обучения модели машинного обучения. Из третьей части вы узнаете, как выполнить эти скрипты на языке R в базе данных с помощью хранимых процедур T-SQL.

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Восстановление примера базы данных 

Во [второй части](r-predictive-model-prepare-data.md) вы узнаете, как загрузить данные из базы данных в кадр данных Python, а также подготовить данные в R.

В [третьей части](r-predictive-model-train.md) вы узнаете, как обучить модель машинного обучения в R.

В [четвертой части](r-predictive-model-deploy.md) вы узнаете, как сохранить модель в базе данных, а затем создать хранимые процедуры на основе сценариев R, разработанных во второй и третьей частях. Хранимые процедуры будут запускаться на сервере, чтобы формировать прогнозы на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* Службы машинного обучения SQL Server — сведения об установке служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Можно также [включить Службы машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* Службы машинного обучения SQL Server — сведения об установке служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
* SQL Server 2016 R Services. Сведения об установке служб R Services см. в [руководстве по установке для Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Службы машинного обучения в управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) для восстановления образца базы данных в Управляемый экземпляр SQL Azure.
::: moniker-end

* IDE R. В этом учебнике используется [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC — данный драйвер используется в сценариях R, разрабатываемых в рамках этого учебника. Установите его с помощью команды R `install.packages("RODBC")`, если этот драйвер еще не установлен. Дополнительные сведения о RODBC см. в разделе [CRAN - Package RODBC](https://CRAN.R-project.org/package=RODBC) (CRAN: пакет RODBC).

* Инструмент SQL-запросов — в этом учебнике предполагается, что вы используете [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Дополнительные сведения см. в статье [Использование записных книжек в Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

## <a name="restore-the-sample-database"></a>Восстановление примера базы данных

Пример базы данных, используемый в этом учебнике, сохранен в файл резервной копии базы данных **BAK**, чтобы его можно было скачать и использовать.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Если вы используете Службы машинного обучения в Кластерах больших данных, ознакомьтесь со статьей [Восстановление базы данных на главном экземпляре кластера больших данных SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Скачайте файл [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Следуйте инструкциям из раздела [Восстановление базы данных из файла резервной копии](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) в Azure Data Studio, используя следующие сведения:

   * Выполните импорт из скачанного файла **TutorialDB.bak**.
   * Назовите целевую базу данных "TutorialDB".

1. Чтобы убедиться, что восстановленная база данных существует, выполните запрос к таблице **dbo.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Скачайте файл [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Следуйте инструкциям в разделе [Восстановление базы данных в Управляемый экземпляр](/azure/sql-database/sql-database-managed-instance-get-started-restore) в SQL Server Management Studio, используя следующие сведения.

   * Выполните импорт из скачанного файла **TutorialDB.bak**.
   * Назовите целевую базу данных "TutorialDB".

1. Чтобы убедиться, что восстановленная база данных существует, выполните запрос к таблице **dbo.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим учебником, удалите базу данных TutorialDB.
## <a name="next-steps"></a>Дальнейшие действия

В первой части этого учебника вы выполнили следующие действия:

* Установка необходимых компонентов
* Восстановленный пример базы данных

Чтобы подготовить данные из для модели машинного обучения, перейдите ко второй части этого учебника:

> [!div class="nextstepaction"]
> [Подготовка данных для обучения прогнозной модели в R](r-predictive-model-prepare-data.md)