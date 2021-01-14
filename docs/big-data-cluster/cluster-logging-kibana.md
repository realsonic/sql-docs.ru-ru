---
title: Извлечение журналов кластера с помощью панели мониторинга Kibana
titleSuffix: SQL Server Big Data Clusters
description: Мониторинг кластера больших данных SQL Server 2019 с помощью панели мониторинга Kibana.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 55dc3056b9f66f7a96b55ab750a5f74fe9bfd394
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091714"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Извлечение журналов кластера с помощью панели мониторинга Kibana

В этой статье описывается мониторинг приложений в кластерах больших данных SQL Server.

## <a name="prerequisites"></a>Предварительные требования

- [Кластер больших данных SQL Server 2019](deployment-guidance.md)
- [Служебная программа командной строки azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Возможности

В SQL Server 2019 можно создать, удалить, описать, инициализировать, перечислить, запустить и обновить приложение. В следующей таблице описаны команды развертывания приложения, которые можно использовать с **azdata**.

|Get-Help |Описание |
|:---|:---|
|`azdata bdc endpoint list` | Список конечных точек для кластера больших данных. |


Следующий пример можно использовать для вывода конечной точки **панели мониторинга Kibana**:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

В выходных данных будет предоставлена конечная точка, в которой можно использовать имя пользователя и пароль для входа в кластер. 

![Панель мониторинга Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Ссылка на панель мониторинга Kibana:

![Панель мониторинга Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Браузер Microsoft Edge (старой версии) несовместим с Kibana. Чтобы эта панель мониторинга отображалась корректно, необходимо использовать браузер на базе Chromium. При загрузке панелей мониторинга в неподдерживаемом браузере вы увидите пустую страницу. Поддерживаемые браузеры для Kibana см. здесь.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).