---
title: Snabb start – skapa register i portalen
description: Lär dig snabbt att skapa ett privat Docker-register i Azure Container Registry med Azure Portal.
ms.topic: quickstart
ms.date: 03/03/2020
ms.custom: seodec18, mvc
ms.openlocfilehash: 0875e5be628ddfe47696a9d4fc537a8a07122804
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682799"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>Snabb start: skapa ett privat behållar register med hjälp av Azure Portal

Ett Azure Container Registry är ett privat Docker-register i Azure där du kan lagra och hantera privata Docker-behållar avbildningar och relaterade artefakter. I den här snabbstarten skapar du ett containerregister med Azure-portalen. Använd därefter Docker-kommandon för att skicka en containeravbildning till registret, och hämta och kör avbildningen slutligen från registret.

För att logga in i registret för att arbeta med containeravbildningar kräver den här snabbstarten att du kör Azure CLI (version 2.0.55 eller senare rekommenderas). Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI][azure-cli].

Du måste också ha Docker installerat lokalt. Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac][docker-mac]-, [Windows][docker-windows]- eller [Linux][docker-linux]-dator.

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på https://portal.azure.com.

## <a name="create-a-container-registry"></a>Skapa ett containerregister

Välj **skapa en resurs**  >  **behållare**  >  **container Registry**.

![Skapa ett containerregister i Azure-portalen][qs-portal-01]

På fliken **grundläggande** anger du värden för **resurs grupp** och **register namn**. Registernamnet måste vara unikt i Azure och innehålla 5–50 alfanumeriska tecken. För den här snabbstarten skapar du en ny resursgrupp på platsen `West US` med namnet `myResourceGroup`, och för **SKU** väljer du ”Basic”. 

![Skapa ett containerregister med Azure-portalen][qs-portal-03]

Acceptera standardvärden för återstående inställningar. Välj sedan **Granska + skapa**. När du har granskat inställningarna väljer du **skapa**.

I den här snabbstarten skapar du ett *Basic*-register, vilket är ett kostnadsoptimerat alternativ för utvecklare som lär sig om Azure Container Registry. Mer information om tillgängliga tjänst nivåer finns i [tjänste nivåer för container Registry][container-registry-skus].

När meddelandet **Distribueringen lyckades** visas väljer du containerregistret i portalen. 

![Översikt över Container Registry i Azure-portalen][qs-portal-05]

Anteckna **inloggningsserverns** värde. Du använder det här värdet i följande steg när du push-överför och hämtar bilder med Docker.

## <a name="log-in-to-registry"></a>Logga in till registret

Innan du skickar och hämtar containeravbildningar måste du logga in på ACR-instansen. Öppna en kommandotolk i ditt operativsystem och använd kommandot [az acr login][az-acr-login] i Azure CLI. (Ange bara register namnet vid inloggning. Ta inte med "azurecr.io"-suffixet.)

```azurecli
az acr login --name <acrName>
```

Kommandot returnerar `Login Succeeded` när det har slutförts. 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Visa lista över containeravbildningar

Om du vill visa en lista över avbildningarna i registret navigerar du till registret i portalen och väljer **Centrallager**. Välj sedan det lager du skapade med `docker push`.

I det här exemplet väljer vi databasen **Hello-World** och vi kan se bilden med märkord `v1` under **taggar**.

![Visa lista över containeravbildningar i Azure-portalen][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill rensa dina resurser går du till resursgruppen **myResourceGroup** i portalen. När resursgruppen har lästs in klickar du på **Ta bort resursgrupp** för att ta bort resursgruppen, containerregistret och alla containeravbildningar som finns lagrade där.

![Ta bort resursgrupp i Azure-portalen][qs-portal-08]

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten skapade du ett Azure Container Registry med Azure-portalen, push-överförde en containeravbildning och hämtade och körde avbildningen från registret. Fortsätt till självstudien om Azure Container Registry om du vill titta närmare på ACR.

> [!div class="nextstepaction"]
> [Självstudier för Azure Container Registry][container-registry-tutorial-quick-task]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-login]: /cli/azure/acr#az-acr-login
