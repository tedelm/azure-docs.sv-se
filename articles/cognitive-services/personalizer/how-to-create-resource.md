---
title: Skapa en personanpassa resurs
description: Tjänst konfigurationen omfattar hur tjänsten behandlar förmåner, hur ofta tjänsten utforskar, hur ofta modellen omtränas och hur mycket data som lagras.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: adb97db53d1fc0b6f0cdb14b697c82ec52501b84
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80336063"
---
# <a name="create-a-personalizer-resource"></a>Skapa en personanpassa resurs

En personanpassa resurs är samma sak som en personanpassa inlärnings slinga. En enskild resurs eller inlärnings slinga skapas för varje ämnes domän eller innehålls område som du har. Använd inte flera innehålls områden i samma slinga eftersom detta förvirrar inlärnings slingan och ger dåliga förutsägelser.

Om du vill att Personanpassaren ska välja det bästa innehållet för mer än ett innehålls områden på en webb sida använder du en annan inlärnings slinga för var och en.


## <a name="create-a-resource-in-the-azure-portal"></a>Skapa en resurs i Azure Portal

Skapa en personanpassa resurs för varje feedback-slinga.

1. Logga in på [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer). Föregående länk tar dig till sidan **skapa** för tjänsten personanpassa.
1. Ange tjänstens namn, Välj en prenumeration, plats, pris nivå och resurs grupp.

    > [!div class="mx-imgBorder"]
    > ![Använd Azure Portal för att skapa en personanpassa resurs, även kallat en inlärnings slinga.](./media/how-to-create-resource/how-to-create-personalizer-resource-learning-loop.png)

1. Välj **skapa** för att skapa resursen.

1. När resursen har distribuerats väljer du knappen **gå till resurs** för att gå till din personanpassa resurs.

1. Välj sidan **snabb start** för resursen och kopiera sedan värdena för din slut punkt och nyckel. Du behöver både resurs slut punkten och nyckeln för att använda API: erna rang och belöning.

1. Välj **konfigurations** sidan för den nya resursen för att [Konfigurera inlärnings slingan](how-to-settings.md).

## <a name="create-a-resource-with-the-azure-cli"></a>Skapa en resurs med Azure CLI

1. Logga in på Azure CLI med följande kommando:

    ```azurecli-interactive
    az login
    ```

1. Skapa en resurs grupp, en logisk gruppering för att hantera alla Azure-resurser som du tänker använda med personanpassa resursen.


    ```azurecli-interactive
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Skapa en ny personanpassa resurs, _inlärnings slinga_med följande kommando för en befintlig resurs grupp.

    ```azurecli-interactive
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```

    Detta returnerar ett JSON-objekt, som innehåller **resurs slut punkten**.

1. Använd följande Azure CLI-kommando för att hämta din **resurs nyckel**.

    ```azurecli-interactive
        az cognitiveservices account keys list \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group
    ```

    Du behöver både resurs slut punkten och nyckeln för att använda API: erna rang och belöning.

## <a name="next-steps"></a>Nästa steg

* [Konfigurera](how-to-settings.md) Inlärnings slinga för personanpassa