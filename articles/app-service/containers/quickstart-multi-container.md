---
title: 'Snabb start: skapa en app med flera behållare'
description: Kom igång med appar för flera behållare på Azure App Service genom att distribuera din första app med flera behållare.
keywords: Azure App Service, webbapp, Linux, Docker, skapa, flera behållare, flera behållare, webbapp för behållare, flera behållare, behållare, WordPress, Azure dB för MySQL, produktions databas med behållare
author: msangapu-msft
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: ab8e416e460546dcf758c83aa8b357139c6dbefb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82085883"
---
# <a name="create-a-multi-container-preview-app-using-a-docker-compose-configuration"></a>Skapa en app med flera containrar (förhandsversion) med hjälp av en Docker Compose-konfiguration

> [!NOTE]
> Flera behållare är i för hands version.

Med [Web App for Containers](app-service-linux-intro.md) får du ett flexibelt sätt att använda Docker-avbildningar. Den här snabb starten visar hur du distribuerar en app med flera behållare (för hands version) till Web App for Containers i [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) med hjälp av en Docker-konfiguration.

Du genomför den här snabbstarten i Cloud Shell, men du kan även köra de här kommandona lokalt med [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 eller senare). 

![Exempelapp med flera behållare i Web App for Containers][1]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Hämta exemplet

För den här snabbstarten använder du compose-filen från [Docker](https://docs.docker.com/compose/wordpress/#define-the-project). Konfigurationsfilen finns på [Azure Samples](https://github.com/Azure-Samples/multicontainerwordpress) (Azure-exempel).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

Skapa en snabbstartskatalog i Cloud Shell och ändra sedan till den.

```bash
mkdir quickstart

cd $HOME/quickstart
```

Kör sedan följande kommando för att klona lagringsplatsen för exempelprogrammet till din snabbstartskatalog. Ändra sedan till katalogen `multicontainerwordpress`.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

I Cloud Shell skapar du en resurs grupp med [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) kommandot. I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *South Central US* (USA, södra centrala). Om du vill se alla platser som stöds för App Service på Linux på **Standard**-nivån kör du kommandot [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations).

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Du skapar vanligtvis din resursgrupp och resurserna i en region nära dig.

När kommandot har slutförts visar JSON-utdata resursgruppens egenskaper.

## <a name="create-an-azure-app-service-plan"></a>Skapa en Azure App Service-plan

I Cloud Shell skapar du en App Service plan i resurs gruppen med [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) kommandot.

I följande exempel skapas en App Service-plan med namnet `myAppServicePlan` i prisnivån **Standard** (`--sku S1`) och i en Linux-containrar (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

När App Service-planen har skapats visas information av Azure CLI. Informationen ser ut ungefär som i följande exempel:

<pre>
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
</pre>

## <a name="create-a-docker-compose-app"></a>Skapa en Docker Compose-app

> [!NOTE]
> Docker-sammanställning för Azure App Services har för närvarande en gräns på 4 000 tecken för tillfället.

I Cloud Shell-terminalen skapar du en [webbapp](app-service-linux-intro.md) med flera containrar i `myAppServicePlan` App Service-planen med kommandot [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Glöm inte att ersätta _ \<APP_NAME>_ med ett unikt namn på appen (giltiga tecken `a-z`är `0-9`, och `-`).

```azurecli
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

När webbappen har skapats visar Azure CLI utdata liknande den i följande exempel:

<pre>
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app_name&gt;.azurewebsites.net",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

### <a name="browse-to-the-app"></a>Bläddra till appen

Bläddra till den distribuerade appen på (`http://<app_name>.azurewebsites.net`). Det kan ta några minuter att läsa in appen. Om du får ett fel väntar du ytterligare ett par minuter och uppdaterar sedan webbläsaren.

![Exempelapp med flera behållare i Web App for Containers][1]

**Grattis!** Du har skapat en app med flera behållare i Web App for Containers.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Självstudie: WordPress-app med flera behållare](tutorial-multi-container-app.md)

> [!div class="nextstepaction"]
> [Konfigurera en anpassad behållare](configure-custom-container.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
