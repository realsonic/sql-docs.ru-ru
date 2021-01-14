---
title: Управление с помощью Resource Governor
description: Узнайте, как использовать Resource Governor для управления ресурсами ЦП, физическими операциями ввода-вывода и выделением ресурсов памяти для рабочих нагрузок Python и R в службах машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3e71b33eb08fd386232e992e9b47da7b5057aa32
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170726"
---
# <a name="manage-python-and-r-workloads-with-resource-governor-in-sql-server-machine-learning-services"></a>Управление рабочими нагрузками Python и R с помощью Resource Governor в службах машинного обучения SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Узнайте, как использовать [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) для управления ресурсами ЦП, физическими операциями ввода-вывода и выделением ресурсов памяти для рабочих нагрузок Python и R в службах машинного обучения SQL Server.

Алгоритмы машинного обучения в Python и R ресурсоемки. В зависимости от приоритетов рабочих нагрузок может потребоваться увеличить или уменьшить объем доступных ресурсов для служб машинного обучения.

Дополнительные сведения см. в разделе [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

> [!NOTE] 
> Resource Governor доступен в составе выпуска Enterprise.

## <a name="default-allocations"></a>Выделения по умолчанию

По умолчанию объем памяти, выделяемый для внешних сред выполнения сценариев машинного обучения, ограничен 20% от общего объема памяти компьютера. Иногда это зависит от системы, но в общем случае это ограничение не подходит для серьезных задач машинного обучения, таких как обучение моделей или прогнозирование на основе большого числа строк данных. 

## <a name="manage-resources-with-resource-governor"></a>Управление ресурсами с помощью Resource Governor
 
По умолчанию внешние процессы используют до 20% общего объема памяти узла на локальном сервере. Пул ресурсов по умолчанию можно изменить, чтобы внести изменения на уровне сервера, а процессы R и Python будут использовать любую емкость, доступную для внешних процессов.

При необходимости можно создать настраиваемые **внешние пулы ресурсов** со связанными группами рабочих нагрузок и классификаторами, чтобы определить выделение ресурсов для запросов, исходящих от конкретных программ или узлов, или в соответствии с другими указанными условиями. Внешний пул ресурсов — это тип пула ресурсов в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], который помогает управлять процессами R и Python за пределами ядра СУБД.

1. [Включите управление ресурсами](../../relational-databases/resource-governor/enable-resource-governor.md) (по умолчанию оно отключено).

2. Выполните инструкцию [CREATE EXTERNAL RESOURCE POOL](../../t-sql/statements/create-external-resource-pool-transact-sql.md), чтобы создать и настроить пул ресурсов, а затем инструкцию [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md), чтобы реализовать его.

3. Создайте группу рабочей нагрузки для детализированного выделения, например, между обработкой и оценкой.

4. Создайте классификатор для перехвата вызовов для внешней обработки.

5. Выполняйте запросы и процедуры с помощью созданных объектов.

Пошаговые инструкции см. в статье [Создание пула ресурсов для Служб машинного обучения SQL Server](create-external-resource-pool.md).

Общие сведения о терминологии и основных понятиях см. в разделе [Пул ресурсов Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

## <a name="processes-under-resource-governance"></a>Процессы, для которых применяется управление ресурсами
  
 С помощью *внешнего пула ресурсов* вы можете управлять ресурсами, используемыми следующими исполняемыми файлами в экземпляре ядра СУБД:

+ Rterm.exe при вызове локально из SQL Server или при удаленном вызове из SQL Server в качестве удаленного контекста вычислений;
+ Python.exe при вызове локально из SQL Server или при удаленном вызове из SQL Server в качестве удаленного контекста вычислений;
+ BxlServer.exe и вспомогательными процессами;
+ вспомогательными процессами, запущенными с помощью панели запуска, например, PythonLauncher.dll;
  
> [!NOTE]
> Прямое управление службой панели запуска с помощью Resource Governor не поддерживается. Панель запуска — это доверенная служба, которая может размещать только те запуски, которые предоставляются корпорацией Майкрософт. Доверенные средства запуска настраиваются явным образом, чтобы избежать чрезмерного использования ресурсов.
  
## <a name="next-steps"></a>Дальнейшие действия

+ [Создание пула ресурсов для машинного обучения](create-external-resource-pool.md)
+ [Пулы ресурсов Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)