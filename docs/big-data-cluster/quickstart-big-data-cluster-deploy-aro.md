---
title: Скрипт Python для развертывания в Azure Red Hat OpenShift
titleSuffix: SQL Server Big Data Clusters
description: Узнайте, как развертывать кластеры больших данных SQL Server в Azure Red Hat OpenShift (ARO) с помощью скрипта развертывания.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c5889a9411c54312dd52ee1a5e9f2b7917da6838
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637551"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>Развертывание кластера больших данных SQL Server в Azure Red Hat OpenShift (ARO) с помощью скрипта Python

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этом руководстве используется пример скрипта Python для развертывания [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] в [Azure Red Hat OpenShift (ARO)](/azure/virtual-machines/linux/openshift-get-started). Этот вариант развертывания поддерживается начиная с SQL Server 2019 с накопительным пакетом обновления 5 (CU5).

> [!TIP]
> ARO — это единственный вариант размещения Kubernetes для кластера больших данных. Сведения о других вариантах развертывания и настройке параметров развертывания см. в статье [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md).


> [!WARNING]
> Для постоянных томов, созданных с помощью встроенных классов хранения *managed-premium*, применяется политика освобождения *Удалить*. Поэтому при удалении кластера больших данных SQL Server утверждения постоянных томов удаляются, а затем удаляются и сами тома. Вы должны создавать пользовательские классы хранения с помощью средства подготовки azure-disk с политикой освобождения *Сохранить*, как показано в разделе [Классы хранилищ](/azure/aks/concepts-storage/#storage-classes). В приведенном ниже скрипте используется класс хранения *managed-premium*. Дополнительные сведения см. в статье о [сохраняемости данных](concept-data-persistence.md).

Используемое здесь развертывание кластера больших данных по умолчанию состоит из главного экземпляра SQL, одного экземпляра пула вычислительных ресурсов, двух экземпляров пула данных и двух экземпляров пула носителей. Данные сохраняются с помощью постоянных томов Kubernetes, использующих классы хранения ARO по умолчанию. Конфигурация по умолчанию, применяемая в этом руководстве, подходит для сред разработки и тестирования.

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure.
- [oc](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Минимальная версия Python — 3.0](https://www.python.org/downloads)
- [`az` CLI](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>Вход в учетную запись Azure

Скрипт использует Azure CLI для автоматизации создания кластера ARO. Перед выполнением скрипта необходимо по крайней мере один раз войти в учетную запись Azure с помощью Azure CLI. В командной строке выполните следующую команду:

```terminal
az login
```

## <a name="instructions"></a>Instructions

1. Скачайте скрипт Python `deploy-sql-big-data-aro.py` и файл YAML `bdc-scc.yaml`.

   >Эти файлы приводятся в данной статье в следующих разделах:
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. Запустите скрипт с помощью следующей команды:

```terminal
python deploy-sql-big-data-aro.py
```

При появлении запроса введите идентификатор подписки Azure и группу ресурсов Azure, в которой будут созданы ресурсы. При необходимости можно также ввести другие данные конфигурации или использовать значения по умолчанию. Пример:

- `azure_region`
- `vm_size` для рабочих узлов OpenShift. Для оптимальной работы при проверке базовых сценариев рекомендуется использовать по крайней мере 8 виртуальных ЦП и 64 ГБ памяти во всех рабочих узлах кластера. Скрипт по умолчанию использует `Standard_D8s_v3` и 3 рабочих узла. Размер по умолчанию для кластеров больших данных также предусматривает около 24 дисков для утверждений постоянных томов во всех компонентах.
- Конфигурация сети для развертывания кластера OpenShift — дополнительные сведения о каждом параметре см. в [статье о развертывании ARO](\azure\openshift\tutorial-create-cluster).
- `cluster_name` — это значение используется как для кластера ARO, так и для кластера больших данных SQL Server, созданного поверх ARO. Обратите внимание, что именем кластера больших данных SQL будет пространство имен Kubernetes.
- `username ` — это имя пользователя для учетных записей администратора контроллера, главного экземпляра SQL Server и шлюза, подготавливаемых во время развертывания. Обратите внимание, что в соответствии с рекомендациями учетная запись `sa` SQL Server автоматически отключается.
- `password` — одно и то же значение будет использоваться для всех учетных записей.

Кластер больших данных SQL Server теперь развернут в ARO. Вы можете подключиться к кластеру с помощью Azure Data Studio. Дополнительные сведения см. в статье [Подключение к кластеру больших данных SQL Server с помощью Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Очистка

Если вы тестируете [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Azure, по завершении следует удалить кластер ARO, чтобы избежать непредвиденных расходов. Не удаляйте кластер, если планируете продолжать использовать его.

> [!WARNING]
> Приведенные ниже инструкции служат для удаления кластера ARO, что также приводит к удалению кластера больших данных SQL Server. Если вы хотите сохранить какие-либо базы данных или данные HDFS, перед удалением кластера следует выполнить их резервное копирование.

Выполните следующую команду в Azure CLI, чтобы удалить кластер больших данных и службу ARO в Azure (замените `<resource group name>` на **имя группы ресурсов Azure**, указанное в скрипте развертывания):

```terminal
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

Скрипт в этом разделе развертывает кластер больших данных SQL Server в Azure Red Hat OpenShift. Скопируйте скрипт на рабочую станцию и сохраните его как `deploy-sql-big-data-aro.py` перед началом развертывания.

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

В приведенном ниже манифесте YAML определяются пользовательские ограничения контекста безопасности (SCC) для развертывания кластера больших данных. Скопируйте его в тот же каталог, что и `deploy-sql-big-data-aro.py`.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>Дальнейшие действия

Скрипт развертывания настроил Службу Azure Kubernetes, а также развернул кластер больших данных SQL Server 2019. Вы также можете настраивать будущие развертывания путем установки вручную. Дополнительные сведения о развертывании кластеров больших данных и настройке развертывания см. в статье [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md).

Теперь, когда кластер больших данных SQL Server развернут, вы можете загрузить демонстрационные данные и изучить другие руководства:

> [!div class="nextstepaction"]
> [Руководство. Загрузка примера данных в кластер больших данных SQL Server 2019](tutorial-load-sample-data.md)