---
title: Учебник по Python. Классификация клиентов
titleSuffix: SQL machine learning
description: В этом цикле учебников из четырех частей вы выполните кластеризацию клиентов методом k-средних в базе данных, используя Python и машинное обучение SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/26/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: dc119f4afe15606dc0d97e69abf0e3f055235bd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101964"
---
# <a name="python-tutorial-categorizing-customers-using-k-means-clustering-with-sql-machine-learning"></a>Учебник по Python. Классификация клиентов на основе кластеризации методом k-средних с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этом цикле учебников из четырех частей вы будете использовать Python для разработки и развертывания модели кластеризации методом k-средних в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md) для классификации данных клиентов.
::: moniker-end
::: moniker range="=sql-server-2017"
В этом учебнике из четырех частей вы будете использовать Python для разработки и развертывания модели кластеризации методом k-средних в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) для кластеризации данных клиентов.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать Python для разработки и развертывания модели кластеризации методом K-средних в [Службах машинного обучения Управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) для кластеризации данных клиентов.
::: moniker-end

В первой части этого цикла учебников вы настроите необходимые компоненты, а затем восстановите пример набора данных в базе данных. Далее в этом цикле вы будете использовать полученные данные для обучения и развертывания модели кластеризации в Python с помощью машинного обучения SQL.

Во второй и третьей частях вы напишите скрипты Python в записной книжке Azure Data Studio для анализа и подготовки данных и обучения модели машинного обучения. Затем, в четвертой части, вы запустите эти сценарии Python в базе данных с помощью хранимых процедур.

*Кластеризацию* можно описать как организацию данных по группам, где члены группы каким-либо образом похожи друг на друга. В рамках этой серии руководств вы можете представить себя владельцем розничного предприятия. Вы будете использовать метод **k-средних** для кластеризации клиентов в наборе данных о покупках и возвратах продуктов. Благодаря кластеризации клиентов вы можете более эффективно осуществлять маркетинговую деятельность, ориентируясь на конкретные группы. Кластеризация методом k-средних — это алгоритм *неконтролируемого обучения*, который ищет закономерности в данных на основе сходства.

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Восстановление примера базы данных

Во [второй части](python-clustering-model-prepare-data.md) вы узнаете, как подготовить данные из базы данных для выполнения кластеризации.

В [третьей части](python-clustering-model-build.md) вы узнаете, как создать и обучить модель кластеризации на основе k-средних в Python.

В [четвертой части](python-clustering-model-deploy.md) вы узнаете, как создать в базе данных хранимую процедуру, которая может выполнять кластеризацию в Python на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [Службы машинного обучения SQL Server с языком Python](../sql-server-machine-learning-services.md) — следуйте инструкциям по установке в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). Можно также [включить Службы машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md) с языком Python — следуйте инструкциям по установке в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Службы машинного обучения в Управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) для восстановления образца базы данных в Управляемый экземпляр SQL Azure.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Записную книжку Azure Data Studio вы будете использовать как для Python, так и для SQL. Дополнительные сведения о записных книжках см. в статье [Использование записных книжек в Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Дополнительные пакеты Python — в примерах этой серии учебников используются пакеты Python, которые могут быть как установлены, так и не установлены вами.

  Откройте **командную строку** и измените путь установки для версии Python, используемой в Azure Data Studio. Например, `cd %LocalAppData%\Programs\Python\Python37-32`. Выполните следующие команды, чтобы импортировать пакеты, которые еще не установлены.

  ```console
  pip install matplotlib
  pip install pandas
  pip install pyodbc
  pip install scipy
  pip install sklearn
  ```

## <a name="restore-the-sample-database"></a>Восстановление примера базы данных

Пример набора данных, используемый в этом учебнике, был сохранен в файл резервной копии базы данных **BAK**, чтобы его можно было скачать и использовать. Этот набор данных является производным от набора данных [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp), предоставляемого [Советом по оценке производительности обработки транзакций (TPC)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Если вы используете Службы машинного обучения в Кластерах больших данных, ознакомьтесь со статьей [Восстановление базы данных на главном экземпляре кластера больших данных SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Скачайте файл [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Следуйте инструкциям из раздела [Восстановление базы данных из файла резервной копии](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) в Azure Data Studio, используя следующие сведения:

   * Выполните импорт из скачанного файла **tpcxbb_1gb.bak**.
   * Назовите целевую базу данных "tpcxbb_1gb".

1. Чтобы убедиться, что набор данных существует после восстановления базы данных, выполните запрос к таблице **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Скачайте файл [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Следуйте инструкциям в разделе [Восстановление базы данных в Управляемый экземпляр](/azure/sql-database/sql-database-managed-instance-get-started-restore) в SQL Server Management Studio, используя следующие сведения.

   * Выполните импорт из скачанного файла **tpcxbb_1gb.bak**.
   * Назовите целевую базу данных "tpcxbb_1gb".

1. Чтобы убедиться, что набор данных существует после восстановления базы данных, выполните запрос к таблице **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим учебником, удалите базу данных tpcxbb_1gb.

## <a name="next-steps"></a>Дальнейшие действия

В первой части этого учебника вы выполнили следующие действия:

* Восстановление примера базы данных

Чтобы подготовить данные из для модели машинного обучения, перейдите ко второй части этого учебника:

> [!div class="nextstepaction"]
> [Учебник по Python. Подготовка данных к выполнению кластеризации](python-clustering-model-prepare-data.md)