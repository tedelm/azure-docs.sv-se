---
title: Skapa parameterfil
description: Skapa parameter fil för att överföra värden under distributionen av en Azure Resource Manager-mall
ms.topic: conceptual
ms.date: 04/20/2020
ms.openlocfilehash: a9845bbb9e14288a01fb7836db260a2baf484395
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873097"
---
# <a name="create-resource-manager-parameter-file"></a>Skapa parameter fil för Resource Manager

I stället för att skicka parametrar som infogade värden i skriptet, kan det vara lättare att använda en JSON-fil som innehåller parameter värden. Den här artikeln visar hur du skapar parameter filen.

## <a name="parameter-file"></a>Parameter fil

Parameter filen har följande format:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "value": "<second-value>"
    }
  }
}
```

Observera att parametervärdena lagras som oformaterad text i parameter filen. Den här metoden fungerar för värden som inte är känsliga, till exempel att ange SKU för en resurs. Det fungerar inte för känsliga värden, t. ex. lösen ord. Om du behöver skicka ett känsligt värde som en parameter lagrar du värdet i ett nyckel valv och refererar till nyckel valvet i parameter filen. Det känsliga värdet hämtas på ett säkert sätt under distributionen.

Följande parameter fil innehåller ett oformaterat text värde och ett värde som lagras i ett nyckel valv.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "reference": {
        "keyVault": {
          "id": "<resource-id-key-vault>"
        },
        "secretName": "<secret-name>"
      }
    }
  }
}
```

Mer information om hur du använder värden från ett nyckel valv finns i [använda Azure Key Vault för att skicka ett säkert parameter värde under distributionen](key-vault-parameter.md).

## <a name="define-parameter-values"></a>Definiera parameter värden

Om du vill ta reda på hur du definierar parameter värden öppnar du mallen som du distribuerar. Titta på avsnittet parametrar i mallen. I följande exempel visas parametrarna från en mall.

```json
"parameters": {
  "storagePrefix": {
    "type": "string",
    "maxLength": 11
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS",
    "Standard_ZRS",
    "Premium_LRS"
    ]
  }
}
```

Den första informationen att notera är namnet på varje parameter. Värdena i parameter filen måste matcha namnen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Lägg märke till parameterns typ. Värdena i parameter filen måste vara av samma typ. För den här mallen kan du ange båda parametrarna som strängar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": ""
    },
    "storageAccountType": {
      "value": ""
    }
  }
}
```

Leta sedan efter ett standardvärde. Om en parameter har ett standardvärde kan du ange ett värde, men du behöver inte.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "" // This value must be provided.
    },
    "storageAccountType": {
      "value": "" // This value is optional. Template will use default value if not provided.
    }
  }
}
```

Titta slutligen på de tillåtna värdena och eventuella begränsningar som maximal längd. De anger det värde intervall som du kan ange för parametern.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "storage"
    },
    "storageAccountType": {
      "value": "Standard_ZRS"
    }
  }
}
```

## <a name="parameter-type-formats"></a>Parameter typ format

I följande exempel visas formaten för olika parameter typer.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "exampleString": {
      "value": "test string"
    },
    "exampleInt": {
      "value": 4
    },
    "exampleBool": {
      "value": true
    },
    "exampleArray": {
      "value": [
        "value 1",
        "value 2"
      ]
    },
    "exampleObject": {
      "value": {
        "property1": "value1",
        "property2": "value2"
      }
    }
   }
}
```

## <a name="file-name"></a>Filnamn

Den allmänna konventionen för att namnge parameter filen är att lägga till **. parametrar** till mallnamnet. Om din mall till exempel heter **azuredeploy. JSON**heter parameter filen **azuredeploy. Parameters. JSON**. Den här namngivnings konventionen hjälper dig att se anslutningen mellan mallen och parametrarna.

Om du vill distribuera till olika miljöer skapar du mer än en parameter fil. När du namnger parameter filen lägger du till ett sätt att identifiera användningen. Använd till exempel **azuredeploy. Parameters-dev. JSON** och **azuredeploy. Parameters-Prod. JSON**


## <a name="parameter-precedence"></a>Parameter prioritet

Du kan använda infogade parametrar och en lokal parameter fil i samma distributions åtgärd. Du kan till exempel ange vissa värden i den lokala parameter filen och lägga till andra värden i den under distributionen. Om du anger värden för en parameter i både den lokala parameter filen och den infogade värdet, prioriteras det infogade värdet.

Det går att använda en extern parameter fil, genom att tillhandahålla URI: n till filen. När du gör detta kan du inte skicka andra värden antingen infogade eller från en lokal fil. Alla infogade parametrar ignoreras. Ange alla parameter värden i den externa filen.

## <a name="parameter-name-conflicts"></a>Parameter namns konflikter

Om din mall innehåller en parameter med samma namn som en av parametrarna i PowerShell-kommandot, visar PowerShell parametern från mallen med postfix- **FromTemplate**. En parameter med namnet **ResourceGroupName** i din mall är till exempel i konflikt med **ResourceGroupName** -parametern i cmdleten [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) . Du uppmanas att ange ett värde för **ResourceGroupNameFromTemplate**. Du kan undvika denna förvirring genom att använda parameter namn som inte används för distributions kommandon.

## <a name="next-steps"></a>Nästa steg

- Information om hur du definierar parametrar i din mall finns i [parametrar i Azure Resource Manager mallar](template-parameters.md).
- Mer information om hur du använder värden från ett nyckel valv finns i [använda Azure Key Vault för att skicka ett säkert parameter värde under distributionen](key-vault-parameter.md).
- Mer information om parametrar finns [i parametrar i Azure Resource Manager mallar](template-parameters.md).
