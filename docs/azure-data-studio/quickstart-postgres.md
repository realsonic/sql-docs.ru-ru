---
title: Краткое руководство. Подключение и отправка запроса PostgreSQL
description: Выполните краткое руководство, в котором вы используете Azure Data Studio для подключения к PostgreSQL, а затем с помощью инструкций SQL создадите базу данных и отправите к ней запрос.
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: 389c8b745a3768f63e2f702a7f48e8c110262fb7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637711"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>Краткое руководство. Использование Azure Data Studio для подключения и запросов к PostgreSQL

В этом кратком руководстве показано, как использовать Azure Data Studio для подключения к PostgreSQL и как с помощью инструкций SQL создать базу данных *tutorialdb* и выполнять к ней запросы.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам потребуется Azure Data Studio, расширение PostgreSQL для Azure Data Studio, а также доступ к серверу PostgreSQL.

- [Установите Azure Data Studio](./download-azure-data-studio.md).
- [Установите расширение PostgreSQL для Azure Data Studio](./extensions/postgres-extension.md).
- [Установите PostgreSQL](https://www.postgresql.org/download/). (Кроме того, вы можете создать базу данных Postgres в облаке с помощью команды [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli).) 

## <a name="connect-to-postgresql"></a>Подключение к PostgreSQL

1. Запустите **Azure Data Studio**.

2. При первом запуске Azure Data Studio открывается диалоговое окно **Подключение**. Если диалоговое окно **Подключение** не открылось, щелкните значок **Создать подключение** на странице **Серверы**.

   ![Значок нового подключения](media/quickstart-postgresql/new-connection-icon.png)

3. В открывшейся форме перейдите в область **Тип подключения** и выберите **PostgreSQL** в раскрывающемся списке.


4. Заполните остальные поля, указав имя сервера, имя пользователя и пароль для вашего сервера PostgreSQL. 

   ![Экран нового подключения](media/quickstart-postgresql/new-connection-screen.png)  

   | Параметр       | Пример значения | Описание |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Имя сервера** | localhost | Полное имя сервера |
   | **User name** | postgres | Имя пользователя, которое требуется использовать для входа. |
   | **Пароль (имя входа SQL)** | *password* | Пароль для учетной записи, используемой для входа. |
   | **Пароль** | *Проверка* | Установите этот флажок, если не хотите вводить пароль при каждом подключении. |
   | **Имя базы данных** | \<Default\> | Укажите этот параметр, если нужно, чтобы соединение указывало на базу данных. |
   | **Группа серверов** | \<Default\> | Этот параметр позволяет назначить данное подключение определенной группе серверов, которую вы создаете. | 
   | **Имя (необязательно)** | *Оставьте пустым* | Этот параметр позволяет указать понятное имя для сервера. | 

5. Выберите **Подключиться**. 

После успешного подключения сервер откроется на боковой панели **Серверы**.


## <a name="create-a-database"></a>Создание базы данных

Ниже приведены инструкции по созданию базы данных с именем **tutorialdb**.

1. Щелкните сервер PostgreSQL в боковой панели **Серверы** правой кнопкой мыши и выберите **Создать запрос**.

2. Вставьте эту инструкцию SQL в открывшемся редакторе запросов.

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. На панели инструментов выберите **Выполнить**, чтобы выполнить запрос. На панели **Сообщения** отображаются уведомления о ходе выполнения запроса.

>[!TIP]
> Для выполнения инструкции вместо элемента **Выполнить** можно нажать клавишу **F5** на клавиатуре.

После завершения запроса щелкните правой кнопкой мыши элемент **Базы данных** и выберите команду **Обновить**, чтобы **tutorialdb** появилась в списке внутри узла **Базы данных**.


## <a name="create-a-table"></a>Создание таблицы

 Следующие шаги создают таблицу в **tutorialdb**.

1. Измените контекст подключения на **tutorialdb** с помощью раскрывающегося списка в редакторе запросов. 

   ![Изменение контекста](media/quickstart-postgresql/change-context.png)

2. Вставьте в редакторе запросов следующую инструкцию SQL и нажмите кнопку **Выполнить**. 

   > [!NOTE]
   > Ее можно добавить в предыдущий запрос, либо можно перезаписать предыдущий запрос в редакторе. При нажатии кнопки **Выполнить** выполняется только выделенный запрос. Если ничего не выделено, при нажатии кнопки **Выполнить** выполняются все запросы в редакторе.

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>Вставка строк

Вставьте в окно запроса следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>Запрос данных

1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. Будут отображены результаты запроса:

   ![Просмотр результатов](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>Next Steps

См. сведения о [сценариях, доступных для Postgres в Azure Data Studio](./extensions/postgres-extension.md).