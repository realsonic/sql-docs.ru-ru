---
title: Свойства сервера (страница "Процессоры")
description: Изучите параметры процессора в SQL Server. Узнайте, какие параметры управляют числом рабочих потоков, назначением процессора и другими свойствами.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684218"
---
# <a name="server-properties-processors-page"></a>Свойства сервера (страница «Процессоры»)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Используйте эту страницу, чтобы просмотреть или изменить параметры процессоров. Настройки соответствия процессоров доступны только в случае, если в системе установлено более одного процессора.  

## <a name="options"></a>Параметры

### <a name="processor-affinity"></a>Соответствие процессоров
Связывает процессоры с определенными потоками, чтобы устранить чрезмерную нагрузку на процессоры и уменьшить количество переходов потоков между процессорами. Дополнительные сведения см. в разделе [Параметр конфигурации сервера "affinity mask"](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

### <a name="io-affinity"></a>Привязка ввода-вывода
Связывает операции дискового ввода-вывода [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server с определенным подмножеством ЦП. Дополнительные сведения см. в разделе [Параметр конфигурации сервера "affinity Input-Output mask"](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>Автоматически устанавливать маску соответствия для всех процессоров
Позволяет SQL Server устанавливать сопоставление процессоров.

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>Автоматически устанавливать маску схожести ввода-вывода для всех процессоров
Позволяет SQL Server устанавливать сопоставление ввода-вывода.

### <a name="maximum-worker-threads"></a>Максимальное число потоков исполнителя.
Значение 0 позволяет SQL Server динамически устанавливать количество рабочих потоков. Эта настройка является наиболее подходящей для большинства систем. Однако в зависимости от конфигурации системы, присвоение этому параметру определенного значения иногда улучшает производительность. Дополнительные сведения см. в статье [Настройка параметра конфигурации сервера max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  

### <a name="boost-sql-server-priority"></a>Повысить приоритет SQL Server
Указывает следует ли SQL Server выставить более высокий приоритет планирования Microsoft Windows по сравнению с другими процессами на том же компьютере. Дополнительные сведения см. в статье [Настройка параметра конфигурации сервера priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  

> [!Note]
> Этот параметр недоступен в SSMS 18.x и более поздних версиях.

### <a name="use-windows-fibers-lightweight-pooling"></a>Использовать волокна Windows (использование упрощенных пулов)
Вы можете использовать легковесные потоки (волокна) Windows вместо обычных потоков для службы SQL Server. Такая возможность доступна только в Windows 2003 Server Edition. Дополнительные сведения см. в разделе [Параметр конфигурации сервера «использование упрощенных пулов»](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

> [!Note]
> Этот параметр недоступен в SSMS 18.x и более поздних версиях.

### <a name="configured-values"></a>Настроенные значения
Отображает настроенные значения для параметров на этой панели. В случае изменения этих значений выберите пункт **Текущие значения** и посмотрите, вступили ли в силу внесенные изменения. В противном случае первым должен быть перезапущен экземпляр SQL Server.

### <a name="running-values"></a>Текущие значения
Просмотр текущих значений для параметров на этой панели. Эти значения доступны только для чтения.

## <a name="see-also"></a>См. также:
[Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


