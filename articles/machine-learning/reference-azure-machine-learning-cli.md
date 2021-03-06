---
title: Installera & Använd Azure Machine Learning CLI
description: Lär dig hur du installerar och använder Azure Machine Learning CLI-tillägget för att skapa och hantera resurser, till exempel din arbets yta, data uppsättningar, data uppsättningar, pipelines, modeller och distributioner.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: d401522ffc45e2e7ea20de70a59ed967dd7623ab
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659790"
---
# <a name="install--use-the-cli-extension-for-azure-machine-learning"></a>Installera & Använd CLI-tillägget för Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning CLI är ett tillägg till [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), ett plattforms oberoende kommando rads gränssnitt för Azure-plattformen. Det här tillägget innehåller kommandon för att arbeta med Azure Machine Learning. Det gör att du kan automatisera dina Machine Learning-aktiviteter. Följande lista innehåller några exempel åtgärder som du kan göra med CLI-tillägget:

+ Köra experiment för att skapa maskin inlärnings modeller

+ Registrera maskin inlärnings modeller för kund användning

+ Paketera, distribuera och spåra livs cykeln för dina Machine Learning-modeller

CLI är inte en ersättning för Azure Machine Learning SDK. Det är ett komplett verktyg som är optimerat för att hantera mycket parameterstyrda uppgifter som passar bra för automatisering.

## <a name="prerequisites"></a>Krav

* Du måste ha en Azure-prenumeration för att kunna använda CLI. Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto innan du börjar. Prova den [kostnads fria eller betalda versionen av Azure Machine Learning](https://aka.ms/AMLFree) idag.

* Om du vill använda CLI-kommandona i det här dokumentet från din **lokala miljö**behöver du [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

    Om du använder [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/)kan CLI nås via webbläsaren och finns i molnet.

## <a name="full-reference-docs"></a>Fullständiga referens dokument

Hitta [fullständiga referens dokument för Azure-CLI-ml-tillägget för Azure CLI](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest).

## <a name="connect-the-cli-to-your-azure-subscription"></a>Ansluta CLI till din Azure-prenumeration

> [!IMPORTANT]
> Om du använder Azure Cloud Shell kan du hoppa över det här avsnittet. Cloud Shell autentiserar automatiskt dig med det konto som du loggar in i din Azure-prenumeration.

Det finns flera sätt som du kan autentisera till din Azure-prenumeration från CLI. Den mest grundläggande är att interaktivt autentisera med hjälp av en webbläsare. För att autentisera interaktivt, öppna en kommando rad eller Terminal och Använd följande kommando:

```azurecli-interactive
az login
```

Om CLI kan öppna din standardwebbläsare så sker det och en inloggningssida läses in. Annars måste du öppna en webbläsare och följa anvisningarna på kommando raden. Anvisningarna omfattar att bläddra till [https://aka.ms/devicelogin](https://aka.ms/devicelogin) och ange en auktoriseringskod.

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

För andra metoder för autentisering, se [Logga in med Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

## <a name="install-the-extension"></a>Installera tillägget

Om du vill installera Machine Learning CLI-tillägget använder du följande kommando:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Exempel på filer som du kan använda med kommandona nedan hittar du [här](https://aka.ms/azml-deploy-cloud).

När du uppmanas väljer `y` du att installera tillägget.

Kontrol lera att tillägget har installerats genom att använda följande kommando för att visa en lista över ML-Specific-kommandon:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Uppdatera tillägget

Om du vill uppdatera Machine Learning CLI-tillägget använder du följande kommando:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Ta bort tillägget

Om du vill ta bort CLI-tillägget använder du följande kommando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Resurshantering

Följande kommandon visar hur du använder CLI för att hantera resurser som används av Azure Machine Learning.

+ Om du inte redan har en, skapar du en resurs grupp:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Skapa en Azure Machine Learning arbets yta:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    > [!TIP]
    > Det här kommandot skapar en Basic Edition-arbetsyta. Om du vill skapa en företags arbets yta använder du `--sku enterprise` växeln med `az ml workspace create` kommandot. Mer information om Azure Machine Learning-versioner finns i [Azure Machine Learning](overview-what-is-azure-ml.md#sku).

    Mer information finns i [skapa en AZ ml-arbetsyta](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ Koppla en arbets ytans konfiguration till en mapp för att aktivera CLI-kontext medvetenhet.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Det här kommandot skapar en `.azureml` under katalog som innehåller exempel på runconfig-och Conda-miljöfiler. Den innehåller också en `config.json` fil som används för att kommunicera med din Azure Machine Learning-arbetsyta.

    Mer information finns i [AZ ml Folder Attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Anslut en Azure Blob-behållare som ett data lager.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Mer information finns i [AZ ml data lager Attach-BLOB](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

+ Ladda upp filer till ett data lager.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Mer information finns i [AZ ml data lager uppladdning](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload).

+ Anslut ett AKS-kluster som ett beräknings mål.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Mer information finns i [AZ ml computetarget Attach AKS](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Skapa ett nytt AMLcompute-mål.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Mer information finns i [AZ ml computetarget Create amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a name="run-experiments"></a><a id="experiments"></a>Köra experiment

* Starta en körning av experimentet. När du använder det här kommandot anger du namnet på runconfig-filen (texten före \* . runconfig om du tittar på ditt fil system) mot parametern-c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach`Kommandot skapar en `.azureml` under katalog som innehåller två exempel på runconfig-filer. 
    >
    > Om du har ett Python-skript som skapar ett kör konfigurations objekt program mässigt kan du använda [RunConfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) för att spara det som en RunConfig-fil.
    >
    > Du hittar det fullständiga runconfig-schemat i den här [JSON-filen](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json). Schemat är självdokumenterande genom `description` nyckeln för varje objekt. Det finns dessutom uppräkningar för möjliga värden och ett mallkod i slutet.

    Mer information finns i [AZ ml Run Submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Visa en lista över experiment:

    ```azurecli-interactive
    az ml experiment list
    ```

    Mer information finns i [AZ ml experiment List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="dataset-management"></a>Hantering av data uppsättningar

Följande kommandon visar hur du arbetar med data uppsättningar i Azure Machine Learning:

+ Registrera en data uppsättning:

    ```azurecli-interactive
    az ml dataset register -f mydataset.json
    ```

    Information om formatet på den JSON-fil som används för att definiera data uppsättningen använder du `az ml dataset register --show-template` .

    Mer information finns i [AZ ml data data register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Arkivera en aktiv eller föråldrad data uppsättning:

    ```azurecli-interactive
    az ml dataset archive -n dataset-name
    ```

    Mer information finns i [AZ ml data uppsättnings Arkiv](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Föråldra en data uppsättning:

    ```azurecli-interactive
    az ml dataset deprecate -d replacement-dataset-id -n dataset-to-deprecate
    ```

    Mer information finns i [AZ ml dataset reåldrad](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Lista alla data uppsättningar på en arbets yta:

    ```azurecli-interactive
    az ml dataset list
    ```

    Mer information finns i [AZ ml dataset List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Hämta information om en data uppsättning:

    ```azurecli-interactive
    az ml dataset show -n dataset-name
    ```

    Mer information finns i [AZ ml data uppsättning show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Återaktivera en arkiverad eller inaktuell data uppsättning:

    ```azurecli-interactive
    az ml dataset reactivate -n dataset-name
    ```

    Mer information finns i [AZ ml dataset reaktivera](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Avregistrera en data uppsättning:

    ```azurecli-interactive
    az ml dataset unregister -n dataset-name
    ```

    Mer information finns i [avregistrering av AZ ml-datauppsättning](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

## <a name="environment-management"></a>Miljö hantering

Följande kommandon visar hur du skapar, registrerar och listar Azure Machine Learning [miljöer](how-to-configure-environment.md) för din arbets yta:

+ Skapa ramverk-filer för en miljö:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Mer information finns i [AZ ml Environment Autogenerera](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold).

+ Registrera en miljö:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Mer information finns i [AZ ml miljö register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register).

+ Lista registrerade miljöer:

    ```azurecli-interactive
    az ml environment list
    ```

    Mer information finns i [AZ ml Environment List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list).

+ Ladda ned en registrerad miljö:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Mer information finns i [AZ ml-miljö hämtning](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download).

### <a name="environment-configuration-schema"></a>Konfigurations schema för miljö

Om du använde `az ml environment scaffold` kommandot genererar det en mallfil `azureml_environment.json` som kan ändras och används för att skapa anpassade miljö konfigurationer med cli. Objektet på den översta nivån mappas löst till [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) klassen i python SDK. 

```json
{
    "name": "testenv",
    "version": null,
    "environmentVariables": {
        "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
    },
    "python": {
        "userManagedDependencies": false,
        "interpreterPath": "python",
        "condaDependenciesFile": null,
        "baseCondaEnvironment": null
    },
    "docker": {
        "enabled": false,
        "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
        "baseDockerfile": null,
        "sharedVolumes": true,
        "shmSize": "2g",
        "arguments": [],
        "baseImageRegistry": {
            "address": null,
            "username": null,
            "password": null
        }
    },
    "spark": {
        "repositories": [],
        "packages": [],
        "precachePackages": true
    },
    "databricks": {
        "mavenLibraries": [],
        "pypiLibraries": [],
        "rcranLibraries": [],
        "jarLibraries": [],
        "eggLibraries": []
    },
    "inferencingStackVersion": null
}
```

Följande tabell innehåller information om varje översta fält i JSON-filen, dess typ och en beskrivning. Om en objekt typ är länkad till en klass från python SDK finns det en lös 1:1-matchning mellan varje JSON-fält och namnet på den offentliga variabeln i python-klassen. I vissa fall kan fältet mappas till ett konstruktor-argument i stället för en klass variabel. Till exempel `environmentVariables` mappas fältet till `environment_variables` variabeln i- [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) klassen.

| JSON-fält | Typ | Description |
|---|---|---|
| `name` | `string` | Miljöns namn. Starta inte namn med **Microsoft** eller **azureml**. |
| `version` | `string` | Version av miljön. |
| `environmentVariables` | `{string: string}` | En hash-mappning av miljö variabel namn och-värden. |
| `python` | [`PythonSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py) | Objekt som definierar den python-miljö och-tolk som ska användas på mål beräknings resurser. |
| `docker` | [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) | Definierar inställningar för att anpassa Docker-avbildningen som är byggd för miljöns specifikationer. |
| `spark` | [`SparkSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.sparksection?view=azure-ml-py) | I avsnittet konfigureras Spark-inställningar. Den används endast när Framework är inställt på PySpark. |
| `databricks` | [`DatabricksSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.databricks.databrickssection?view=azure-ml-py) | Konfigurerar beroenden för Databricks-bibliotek. |
| `inferencingStackVersion` | `string` | Anger den inferencing stack-version som läggs till i avbildningen. Lämna det här fältet för att undvika att lägga till en inferencing-stack `null` . Giltigt värde: "senaste". |

## <a name="ml-pipeline-management"></a>ML pipeline-hantering

Följande kommandon visar hur du arbetar med maskin inlärnings pipeliner:

+ Skapa en pipeline för maskin inlärning:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Mer information finns i [AZ ml pipeline Create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create).

    Mer information om pipeline YAML-filen finns i [definiera pipelines i Machine Learning i yaml](reference-pipeline-yaml.md).

+ Kör en pipeline:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Mer information finns i [AZ ml Run Submit-pipeline](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-pipeline).

    Mer information om pipeline YAML-filen finns i [definiera pipelines i Machine Learning i yaml](reference-pipeline-yaml.md).

+ Schemalägg en pipeline:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexpereiment -i mypipelineid -y myschedule.yml
    ```

    Mer information finns i [AZ ml pipeline Create-Schedule](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create-schedule).

    Mer information om YAML-filen för pipelinen finns i [definiera pipelines för maskin inlärning i yaml](reference-pipeline-yaml.md#schedules).

## <a name="model-registration-profiling-deployment"></a>Modell registrering, profilering, distribution

Följande kommandon visar hur du registrerar en utbildad modell och sedan distribuerar den som en produktions tjänst:

+ Registrera en modell med Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Mer information finns i [AZ ml modell register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **Valfritt** Profilera din modell för att få optimala processor-och minnes värden för distribution.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Mer information finns i [AZ ml modell Profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Distribuera din modell till AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Mer information om schema för konfigurations filen för konfigurations filen finns i [konfigurations schema för energischemat](#inferenceconfig).
    
    Mer information om schemat för distributions konfigurations filen finns i [schema för distributions konfiguration](#deploymentconfig).

    Mer information finns i [AZ ml modell Deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Schema för konfigurations härledning

[!INCLUDE [inferenceconfig](../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Schema för distributions konfiguration

### <a name="local-deployment-configuration-schema"></a>Konfigurations schema för lokal distribution

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Konfigurations schema för distribution av Azure Container instance 

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Konfigurations schema för Azure Kubernetes service-distribution

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Nästa steg

* [Kommando referens för Machine Learning CLI-tillägget](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Träna och distribuera maskin inlärnings modeller med Azure-pipelines](/azure/devops/pipelines/targets/azure-machine-learning)
