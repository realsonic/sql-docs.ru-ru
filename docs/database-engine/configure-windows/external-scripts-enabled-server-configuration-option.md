---
title: Параметр конфигурации сервера external scripts enabled
description: Сведения о параметре external scripts enable в SQL Server. После его включения вы сможете выполнять внешние скрипты на поддерживаемых языках, таких как R или Python.
ms.date: 06/30/2020
ms.prod: sql
ms.technology: machine-learning-services
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- external scripts enabled
- external_scripts_enabled_TSQL
helpviewer_keywords:
- external scripts enabled option
ms.assetid: 9d0ce165-8719-4007-9ae8-00f85cab3a0d
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 90a24ba11ce482e8d8608e1641d64ace067ad41b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170976"
---
# <a name="external-scripts-enabled-server-configuration-option"></a>Параметр конфигурации сервера external scripts enabled
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Параметр **external scripts enabled** позволяет включить выполнение скриптов с некоторыми удаленными расширениями языка. По умолчанию это свойство отключено. Программа установки может при необходимости присвоить этому свойству значение true при установке **служб машинного обучения**.

## <a name="remarks"></a>Remarks

Параметр external scripts enabled необходимо включить перед выполнением внешнего скрипта с помощью процедуры [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) . Используйте **sp_execute_external_script**, чтобы выполнять в скрипты, написанные на нескольких поддерживаемых языках, таких как R или Python. 

+ При анализе служб [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]

    [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] включает поддержку языка R в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], а также набор инструментов рабочей станции R и библиотек подключений.

    Установите компонент **службы R** в ходе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы включить выполнение скриптов R.

+ Для [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] и более поздних версий

    [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] поддерживает одновременно языки R и Python.

    Установите компонент **Службы машинного обучения** в ходе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы включить выполнение внешних скриптов R. Во время начальной настройки нужно выбрать хотя бы один язык: R, Python или оба.
    
+ Для [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] и более поздних версий [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] включает поддержку всех языков R, Python, Java и других языков от сторонних производителей.

Установите компоненты Службы машинного обучения и Расширения языка в ходе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы включить выполнение внешних скриптов на любом поддерживаемом языке.

## <a name="additional-requirements"></a>Дополнительные требования

Чтобы после установки включить внешние скрипты, выполните следующий скрипт:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

Дополнительные сведения см. в статье [Установка служб машинного обучения SQL Server (Python и R) в Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) или [Linux](../../linux/sql-server-linux-setup-machine-learning-docker.md?toc=/sql/machine-learning/toc.json).

## <a name="see-also"></a>См. также раздел

+ [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
+ [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)
+ [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [Документация по машинному обучению на SQL](../../machine-learning/index.yml)
