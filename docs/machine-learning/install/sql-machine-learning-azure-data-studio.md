---
title: Записные книжки Azure Data Studio (Python, R)
description: Узнайте, как выполнять скрипты Python и R в записных книжках Azure Data Studio с помощью служб машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: b1e3d81e0229d028b9fab6a4521ee11cb20ce1b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091557"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>Запуск скриптов Python и R в записных книжках Azure Data Studio с помощью служб машинного обучения SQL Server
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Узнайте, как запускать скрипты Python и R в записных книжках [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) с помощью [служб машинного обучения SQL Server](../sql-server-machine-learning-services.md). Azure Data Studio — это кроссплатформенный инструмент для работы с базами данных.

## <a name="prerequisites"></a>Предварительные требования

- [Скачайте и установите Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) на своем компьютере. Azure Data Studio — кроссплатформенная среда, которая работает в Windows, macOS и Linux.

- В ней установлен и включен сервер служб машинного обучения SQL Server. Службы машинного обучения можно использовать в Windows, Linux или кластерах больших данных.

  - [Установка служб машинного обучения SQL Server в Windows](sql-machine-learning-services-windows-install.md).
  - [Установка служб машинного обучения SQL Server в Linux](../../linux/sql-server-linux-setup-machine-learning.md).
  - [Выполнение скриптов Python и R с помощью служб машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).

## <a name="create-a-sql-notebook"></a>Создание записной книжки SQL

> [!IMPORTANT]
> Службы машинного обучения работают как часть SQL Server. Поэтому необходимо использовать ядро SQL, а не ядро Python.

Службы машинного обучения можно использовать в Azure Data Studio с записной книжкой SQL. Чтобы создать новую записную книжку, выполните следующие действия.

1. Щелкните **Файл** и **Создать записную книжку**, чтобы создать новую записную книжку. В записной книжке по умолчанию будет использовано **ядро SQL**.

1. Щелкните **Присоединить к** и **Изменить подключение**. 

    > [!div class="mx-imgBorder"]
    > ![Изменение подключения к записной книжке SQL в Azure Data Studio](media/ads-attach-to-connection.png)
    
1. Подключитесь к существующему или новому серверу SQL Server. Вы можете сделать одно из двух:

    1. Выберите существующее подключение в разделе **Последние подключения** или **Сохраненные подключения**.

    1. Создайте новое подключение в разделе **Сведения о подключении**. Укажите сведения о подключении к SQL Server и базе данных.

    > [!div class="mx-imgBorder"]
    > ![Сведения о подключении к записной книжке SQL в Azure Data Studio](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>Выполнение скриптов Python или R

Записные книжки SQL состоят из ячеек кода и текста. Ячейки кода используются для выполнения скриптов Python или R с помощью хранимой процедуры [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Текстовые ячейки можно использовать для документирования кода в записной книжке.

### <a name="run-a-python-script"></a>Выполнение скрипта Python

Чтобы запустить скрипт Python, сделайте следующее.

1. Щелкните **+ Код**, чтобы добавить ячейку кода.

    > [!div class="mx-imgBorder"]
    > ![Добавление блока кода в записных книжках SQL в Azure Data Studio](media/ads-add-code.png)  

1. В ячейку кода введите следующий скрипт:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Щелкните **Выполнить ячейки** (скругленная черная стрелка) или нажмите клавишу **F5**, чтобы выполнить одну ячейку.

    > [!div class="mx-imgBorder"]
    > ![Запуск кода на Python в записных книжках SQL в Azure Data Studio](media/ads-run-python.png)  

1. Результат будет отображаться под ячейкой кода.

    > [!div class="mx-imgBorder"]
    > ![Вывод кода на Python в записной книжке SQL в Azure Data Studio](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>Запуск скрипта R

Чтобы запустить скрипт на R, сделайте следующее.

1. Щелкните **+ Код**, чтобы добавить ячейку кода.

    > [!div class="mx-imgBorder"]
    > ![Добавление блока кода в записных книжках SQL в Azure Data Studio](media/ads-add-code.png)  

1. В ячейку кода введите следующий скрипт:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Щелкните **Выполнить ячейки** (скругленная черная стрелка) или нажмите клавишу **F5**, чтобы выполнить одну ячейку.

    > [!div class="mx-imgBorder"]
    > ![Запуск кода на R в записных книжках SQL в Azure Data Studio](media/ads-run-r.png)  

1. Результат будет отображаться под ячейкой кода.

    > [!div class="mx-imgBorder"]
    > ![Вывод кода на R в записной книжке SQL в Azure Data Studio](media/ads-run-r-output.png)  

## <a name="next-steps"></a>Дальнейшие действия

- [Использование записных книжек в Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [Создание и запуск записной книжки SQL Server](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [Краткое руководство. Выполнение простых скриптов Python с помощью служб машинного обучения SQL Server](../tutorials/quickstart-python-create-script.md)
- [Краткое руководство. Выполнение простых скриптов R с помощью служб машинного обучения SQL Server](../tutorials/quickstart-r-create-script.md)