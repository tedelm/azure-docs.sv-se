---
title: Aktivera loggning i Azure Machine Learning
description: Lär dig hur du aktiverar loggning i Azure Machine Learning att använda både standard-python-loggnings paketet, samt att använda SDK-/regionsspecifika funktioner.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 03/05/2020
ms.openlocfilehash: 73b9ae6bc3c15526bfdafd74330c7b86286631b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "78396151"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Aktivera loggning i Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Med Azure Machine Learning python SDK kan du aktivera loggning med hjälp av både standard-python-loggningsdatabasen, samt använda SDK-/regionsspecifika funktioner både för lokal loggning och loggning till din arbets yta i portalen. Loggar ger utvecklare information om programmets tillstånd i real tid och kan hjälpa till att diagnostisera fel eller varningar. I den här artikeln får du lära dig olika sätt att aktivera loggning i följande områden:

> [!div class="checklist"]
> * Tränings modeller och beräknings mål
> * Skapa bild
> * Distribuerade modeller
> * Python `logging` -inställningar

[Skapa en Azure Machine Learning-arbetsyta](how-to-manage-workspace.md). Använd [guiden](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) för mer information SDK.

## <a name="training-models-and-compute-target-logging"></a>Tränings modeller och loggning av beräknings mål

Det finns flera sätt att aktivera loggning under modell inlärnings processen och exemplen som visas illustrerar vanliga design mönster. Du kan enkelt logga körnings relaterade data till din arbets yta i molnet med hjälp `start_logging` av funktionen i `Experiment` klassen.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

I referens dokumentationen för [körnings](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) klassen finns ytterligare loggnings funktioner.

Om du vill aktivera lokal loggning av program tillstånd under utbildning, använder `show_output` du parametern. Genom att aktivera utförlig loggning kan du se information från inlärnings processen samt information om eventuella fjär resurser eller beräknings mål. Använd följande kod för att aktivera loggning vid experiment överföring.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Du kan också använda samma parameter i `wait_for_completion` funktionen på den resulterande körningen.

```python
run.wait_for_completion(show_output=True)
```

SDK stöder också användning av standard-python-loggnings paketet i vissa scenarier för utbildning. `INFO` I följande exempel aktive ras loggnings nivån i `AutoMLConfig` ett objekt.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

Du kan också använda- `show_output` parametern när du skapar ett beständigt beräknings mål. Ange parametern i `wait_for_completion` funktionen för att aktivera loggning vid skapande av beräknings mål.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Loggning för distribuerade modeller

Om du vill hämta loggar från en tidigare distribuerad webb tjänst läser du in tjänsten och `get_logs()` använder funktionen. Loggarna kan innehålla detaljerad information om eventuella fel som uppstod under distributionen.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Du kan också logga anpassade stack spårningar för din webb tjänst genom att aktivera Application Insights, vilket gör att du kan övervaka begär ande-/svars tider, fel frekvenser och undantag. Anropa `update()` funktionen på en befintlig webb tjänst för att aktivera Application Insights.

```python
service.update(enable_app_insights=True)
```

Mer information finns i [övervaka och samla in data från ml webb tjänst slut punkter](how-to-enable-app-insights.md).

## <a name="python-native-logging-settings"></a>Inställningar för python-intern loggning

Vissa loggar i SDK kan innehålla ett fel som uppmanar dig att ange loggnings nivå för fel sökning. Om du vill ange loggnings nivå lägger du till följande kod i skriptet.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Nästa steg

* [Övervaka och samla in data från ML webb tjänst slut punkter](how-to-enable-app-insights.md)