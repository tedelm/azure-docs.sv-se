---
title: Mallens struktur och syntax
description: Beskriver strukturen och egenskaperna för Azure Resource Manager mallar med deklarativ JSON-syntax.
ms.topic: conceptual
ms.date: 04/20/2020
ms.openlocfilehash: 60d800eb5251fb3454ba60a67bd109261c6ff9d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687872"
---
# <a name="understand-the-structure-and-syntax-of-arm-templates"></a>Förstå strukturen och syntaxen för ARM-mallar

I den här artikeln beskrivs strukturen för en Azure Resource Manager-mall (ARM). Den visar de olika avsnitten i en mall och de egenskaper som är tillgängliga i dessa avsnitt.

Den här artikeln är avsedd för användare som har en viss välbekanthet med ARM-mallar. Den innehåller detaljerad information om mallens struktur. En stegvis själv studie kurs som vägleder dig genom processen för att skapa en mall finns i [Självstudier: skapa och distribuera din första Azure Resource Manager-mall](template-tutorial-create-first-template.md).

## <a name="template-format"></a>Mallformat

I sin enklaste struktur har en mall följande element:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| $schema |Ja |Platsen för JSON-schemafilen som beskriver versionen av mallens språk. Vilket versions nummer du använder beror på distributionens omfattning och JSON-redigeraren.<br><br>Om du använder [vs Code med Azure Resource Manager Tools-tillägget](use-vs-code-to-create-template.md)använder du den senaste versionen för resurs grupps distributioner:<br>`https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#`<br><br>Andra redigerare (inklusive Visual Studio) kanske inte kan bearbeta schemat. För dessa redigerare använder du:<br>`https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>För prenumerations distributioner använder du:<br>`https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`<br><br>För distributioner av hanterings grupper använder du:<br>`https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#`<br><br>För klient distributioner använder du:<br>`https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#` |
| contentVersion |Ja |Version av mallen (till exempel 1.0.0.0). Du kan ange valfritt värde för det här elementet. Använd det här värdet om du vill dokumentera viktiga ändringar i mallen. När du distribuerar resurser med hjälp av mallen kan det här värdet användas för att se till att rätt mall används. |
| apiProfile |Nej | En API-version som fungerar som en samling av API-versioner för resurs typer. Använd det här värdet för att undvika att behöva ange API-versioner för varje resurs i mallen. När du anger en API-profil version och inte anger en API-version för resurs typen, använder Resource Manager API-versionen för den resurs typ som definieras i profilen.<br><br>Egenskapen API Profile är särskilt användbar när du distribuerar en mall till olika miljöer, till exempel Azure Stack och globala Azure. Använd API Profile-versionen för att kontrol lera att din mall automatiskt använder versioner som stöds i båda miljöerna. En lista över aktuella API-profiler och de resurs-API-versioner som definierats i profilen finns i [API-profil](https://github.com/Azure/azure-rest-api-specs/tree/master/profile).<br><br>Mer information finns i [spåra versioner med hjälp av API-profiler](templates-cloud-consistency.md#track-versions-using-api-profiles). |
| [komponentparametrar](#parameters) |Nej |Värden som anges när distributionen körs för att anpassa resurs distributionen. |
| [användarvariabler](#variables) |Nej |Värden som används som JSON-fragment i mallen för att förenkla mallarnas språk uttryck. |
| [funktionen](#functions) |Nej |Användardefinierade funktioner som är tillgängliga i mallen. |
| [resurser](#resources) |Ja |Resurs typer som distribueras eller uppdateras i en resurs grupp eller prenumeration. |
| [utdata](#outputs) |Nej |Värden som returneras efter distribution. |

Varje element har egenskaper som du kan ange. I den här artikeln beskrivs mallens avsnitt mer detaljerat.

## <a name="parameters"></a>Parametrar

I avsnittet parametrar i mallen anger du vilka värden som du kan ange när du distribuerar resurserna. Du är begränsad till 256 parametrar i en mall. Du kan minska antalet parametrar genom att använda objekt som innehåller flera egenskaper.

Tillgängliga egenskaper för en parameter är:

```json
"parameters": {
  "<parameter-name>" : {
    "type" : "<type-of-parameter-value>",
    "defaultValue": "<default-value-of-parameter>",
    "allowedValues": [ "<array-of-allowed-values>" ],
    "minValue": <minimum-value-for-int>,
    "maxValue": <maximum-value-for-int>,
    "minLength": <minimum-length-for-string-or-array>,
    "maxLength": <maximum-length-for-string-or-array-parameters>,
    "metadata": {
      "description": "<description-of-the parameter>"
    }
  }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| parameter-Name |Ja |Parameterns namn. Måste vara ett giltigt JavaScript-ID. |
| typ |Ja |Typ av parameter värde. De tillåtna typerna och värdena är **String**, **SecureString**, **int**, **bool**, **Object**, **secureObject**och **array**. Se [data typer](#data-types). |
| Standar |Nej |Standardvärdet för parametern, om det inte finns något värde för parametern. |
| allowedValues |Nej |Matris med tillåtna värden för parametern för att kontrol lera att rätt värde har angetts. |
| minValue |Nej |Det minsta värdet för int-typ parametrar, detta värde är inkluderat. |
| maxValue |Nej |Det maximala värdet för int-typ parametrar, detta värde är inkluderat. |
| minLength |Nej |Den minsta längden för parametrar av typen sträng, säker sträng och mat ris typ, detta värde är inkluderat. |
| Max |Nej |Den maximala längden för parametrar av typen sträng, säker sträng och mat ris typ, detta värde är inkluderat. |
| description |Nej |Beskrivning av den parameter som visas för användarna via portalen. Mer information finns i [kommentarer i mallar](#comments). |

Exempel på hur du använder parametrar finns [i parametrar i Azure Resource Manager mallar](template-parameters.md).

### <a name="data-types"></a>Datatyper

För heltal som skickas som infogade parametrar kan värde intervallet begränsas av SDK eller kommando rads verktyget som du använder för distribution. När du till exempel använder PowerShell för att distribuera en mall kan heltals typer vara mellan-2147483648 och 2147483647. Undvik den här begränsningen genom att ange stora heltals värden i en [parameter fil](parameter-files.md). Resurs typer tillämpar egna gränser för heltals egenskaper.

När du anger booleska värden och heltals värden i mallen omger du värdet med citat tecken. Start-och slut sträng värden med dubbla citat tecken.

Objekt börjar med en vänster klammerparentes och slutar med en höger klammerparentes. Matriser börjar med en vänsterparentes och slutar med en höger hak paren tes.

Det går inte att läsa säkra strängar och säkra objekt efter resurs distributionen.

Exempel på format data typer finns i [parameter typ format](parameter-files.md#parameter-type-formats).

## <a name="variables"></a>Variabler

I avsnittet variabler skapar du värden som kan användas i hela mallen. Du behöver inte definiera variabler, men de fören klar ofta mallen genom att minska komplexa uttryck.

I följande exempel visas tillgängliga alternativ för att definiera en variabel:

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": {
    <variable-complex-type-value>
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

Information om hur du `copy` använder för att skapa flera värden för en variabel finns i [variabel iteration](copy-variables.md).

Exempel på hur du använder variabler finns [i variabler i Azure Resource Manager mall](template-variables.md).

## <a name="functions"></a>Functions

I mallen kan du skapa egna funktioner. Dessa funktioner är tillgängliga för användning i din mall. Vanligt vis definierar du komplicerade uttryck som du inte vill upprepa i hela mallen. Du kan skapa användardefinierade funktioner från uttryck och [funktioner](template-functions.md) som stöds i mallar.

När du definierar en användar funktion finns det vissa begränsningar:

* Funktionen kan inte komma åt variabler.
* Funktionen kan bara använda parametrar som har definierats i funktionen. När du använder [funktionen parametrar](template-functions-deployment.md#parameters) i en användardefinierad funktion är du begränsad till parametrarna för den funktionen.
* Funktionen kan inte anropa andra användardefinierade funktioner.
* Funktionen kan inte använda funktionen [Reference](template-functions-resource.md#reference).
* Parametrar för funktionen kan inte ha standardvärden.

```json
"functions": [
  {
    "namespace": "<namespace-for-functions>",
    "members": {
      "<function-name>": {
        "parameters": [
          {
            "name": "<parameter-name>",
            "type": "<type-of-parameter-value>"
          }
        ],
        "output": {
          "type": "<type-of-output-value>",
          "value": "<function-return-value>"
        }
      }
    }
  }
],
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| namnområde |Ja |Namn område för anpassade funktioner. Används för att undvika namn konflikter med mall funktioner. |
| funktions namn |Ja |Namnet på den anpassade funktionen. När du anropar funktionen kombinerar du funktions namnet med namn området. Om du till exempel vill anropa en funktion med namnet uniqueName i namn området contoso `"[contoso.uniqueName()]"`, använder du. |
| parameter-Name |Nej |Namnet på parametern som ska användas i den anpassade funktionen. |
| parameter-värde |Nej |Typ av parameter värde. De tillåtna typerna och värdena är **String**, **SecureString**, **int**, **bool**, **Object**, **secureObject**och **array**. |
| typ av utdata |Ja |Typ av utmatnings värde. Utmatnings värden har stöd för samma typer som indataparametrarna för funktionen. |
| utmatnings värde |Ja |Mallens språk uttryck som utvärderas och returneras från funktionen. |

Exempel på hur du använder anpassade funktioner finns i [användardefinierade funktioner i Azure Resource Manager mall](template-user-defined-functions.md).

## <a name="resources"></a>Resurser

I avsnittet resurser definierar du de resurser som distribueras eller uppdateras.

Du definierar resurser med följande struktur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "apiVersion": "<api-version-of-resource>",
      "name": "<name-of-the-resource>",
      "comments": "<your-reference-notes>",
      "location": "<location-of-resource>",
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| moduletype | Nej | Booleskt värde som anger om resursen ska tillhandahållas under distributionen. När `true`skapas resursen under distributionen. När `false`hoppas resursen över för den här distributionen. Se [villkor](conditional-resource-deployment.md). |
| typ |Ja |Typ av resurs. Det här värdet är en kombination av resurs leverantörens namn område och resurs typ (till exempel **Microsoft. Storage/storageAccounts**). Information om vilka värden som är tillgängliga finns i [referens för mallar](/azure/templates/). För en underordnad resurs är formatet för typen beroende av om den är kapslad i den överordnade resursen eller definieras utanför den överordnade resursen. Se [Ange namn och typ för underordnade resurser](child-resource-name-type.md). |
| apiVersion |Ja |Den version av REST API som ska användas för att skapa resursen. Information om vilka värden som är tillgängliga finns i [referens för mallar](/azure/templates/). |
| name |Ja |Namn på resursen. Namnet måste följa de URI-komponentparametrar som definierats i RFC3986. Azure-tjänster som visar resurs namnet för utomstående parter verifierar namnet för att se till att det inte är ett försök att använda en annan identitet. För en underordnad resurs är formatet på namnet beroende av om det är kapslat i den överordnade resursen eller definierats utanför den överordnade resursen. Se [Ange namn och typ för underordnade resurser](child-resource-name-type.md). |
| Comment |Nej |Dina anteckningar om att dokumentera resurserna i mallen. Mer information finns i [kommentarer i mallar](template-syntax.md#comments). |
| location |Varierar |Geo-platser som stöds för den angivna resursen. Du kan välja någon av de tillgängliga platserna, men vanligt vis är det bra att välja en som är nära dina användare. Vanligt vis är det också bra att placera resurser som interagerar med varandra i samma region. De flesta resurs typer kräver en plats, men vissa typer (till exempel en roll tilldelning) kräver ingen plats. Se [Ange resurs plats](resource-location.md). |
| dependsOn |Nej |Resurser som måste distribueras innan den här resursen distribueras. Resource Manager utvärderar beroenden mellan resurser och distribuerar dem i rätt ordning. När resurserna inte är beroende av varandra distribueras de parallellt. Värdet kan vara en kommaavgränsad lista över resurs namn eller resurs unika identifierare. Endast List resurser som har distribuerats i den här mallen. Resurser som inte har definierats i den här mallen måste redan finnas. Undvik att lägga till onödiga beroenden eftersom de kan minska distributionen och skapa cirkulära beroenden. Anvisningar om hur du ställer in beroenden finns i [definiera beroenden i Azure Resource Manager mallar](define-resource-dependency.md). |
| tags |Nej |Taggar som är associerade med resursen. Använd taggar för att logiskt organisera resurser i din prenumeration. |
| sku | Nej | Vissa resurser tillåter värden som definierar SKU: n som ska distribueras. Du kan till exempel ange typen av redundans för ett lagrings konto. |
| metod | Nej | Vissa resurser tillåter ett värde som definierar vilken typ av resurs du distribuerar. Du kan till exempel ange vilken typ av Cosmos DB som ska skapas. |
| exemplar |Nej |Om fler än en instans behövs, antalet resurser som ska skapas. Standard läget är parallellt. Ange serie läge när du inte vill att alla eller resurserna ska distribueras samtidigt. Mer information finns i [skapa flera instanser av resurser i Azure Resource Manager](copy-resources.md). |
| planera | Nej | Vissa resurser tillåter värden som definierar den plan som ska distribueras. Du kan till exempel ange Marketplace-avbildningen för en virtuell dator. |
| properties |Nej |Resurs-/regionsspecifika konfigurations inställningar. Värdena för egenskaperna är desamma som de värden som du anger i begär ande texten för åtgärden REST API (metoden sätt) för att skapa resursen. Du kan också ange en kopierad matris för att skapa flera instanser av en egenskap. Information om vilka värden som är tillgängliga finns i [referens för mallar](/azure/templates/). |
| resources |Nej |Underordnade resurser som är beroende av den resurs som definieras. Ange endast resurs typer som tillåts av schemat för den överordnade resursen. Beroendet av den överordnade resursen är inte underförstådd. Du måste uttryckligen definiera det beroendet. Se [Ange namn och typ för underordnade resurser](child-resource-name-type.md). |

## <a name="outputs"></a>Utdata

I avsnittet utdata anger du värden som returneras från distributionen. Normalt returnerar du värden från resurser som har distribuerats.

I följande exempel visas strukturen för en utmatnings definition:

```json
"outputs": {
  "<output-name>": {
    "condition": "<boolean-value-whether-to-output-value>",
    "type": "<type-of-output-value>",
    "value": "<output-value-expression>",
    "copy": {
      "count": <number-of-iterations>,
      "input": <values-for-the-variable>
    }
  }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| utmatnings namn |Ja |Namn på utdata. Måste vara ett giltigt JavaScript-ID. |
| moduletype |Nej | Booleskt värde som anger om det här värdet returneras. När `true`är värdet inkluderat i utdata för distributionen. När `false`ignoreras värdet för utdata för den här distributionen. Om inget värde anges är `true`standardvärdet. |
| typ |Ja |Typ av utmatnings värde. Utmatnings värden har stöd för samma typer som indataparametrarna för mallar. Om du anger **SecureString** för utdatatypen visas inte värdet i distributions historiken och kan inte hämtas från en annan mall. Om du vill använda ett hemligt värde i fler än en mall lagrar du hemligheten i en Key Vault och refererar till hemligheten i parameter filen. Mer information finns i [använda Azure Key Vault för att skicka ett säkert parameter värde under distributionen](key-vault-parameter.md). |
| värde |Nej |Mallens språk uttryck som utvärderas och returneras som utdata-värde. Ange antingen **Value** eller **copy**. |
| exemplar |Nej | Används för att returnera fler än ett värde för utdata. Ange **värde** eller **Kopiera**. Mer information finns i [utmatnings iteration i Azure Resource Manager mallar](copy-outputs.md). |

Exempel på hur du använder utdata finns [i utdata i Azure Resource Manager mall](template-outputs.md).

<a id="comments" />

## <a name="comments-and-metadata"></a>Kommentarer och metadata

Du har några alternativ för att lägga till kommentarer och metadata till din mall.

### <a name="comments"></a>Kommentarer

För infogade kommentarer kan du använda antingen `//` eller, `/* ... */` men den här syntaxen fungerar inte med alla verktyg. Du kan inte använda Portal mal len redigeraren för att arbeta med mallar med infogade kommentarer. Om du lägger till den här typen av kommentar måste du se till att de verktyg du använder stöder infogade JSON-kommentarer.

> [!NOTE]
> Om du vill distribuera mallar med kommentarer med hjälp av Azure CLI med version 2.3.0 eller äldre måste du `--handle-extended-json-format` använda växeln.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "dependsOn": [ /* storage account and network interface must be deployed first */
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

I Visual Studio Code kan [Azure Resource Manager Tools-tillägget](use-vs-code-to-create-template.md#install-resource-manager-tools-extension) automatiskt identifiera Resource Manager-mall och ändra språk läget. Om du ser **Azure Resource Manager mall** i det nedre högra hörnet av vs Code kan du använda de infogade kommentarerna. De infogade kommentarerna markeras inte längre som ogiltiga.

![Visual Studio Code Azure Resource Manager Template mode](./media/template-syntax/resource-manager-template-editor-mode.png)

### <a name="metadata"></a>Metadata

Du kan lägga till `metadata` ett objekt nästan var som helst i mallen. Resource Manager ignorerar objektet, men JSON-redigeraren kan varna dig om att egenskapen inte är giltig. I objektet definierar du de egenskaper du behöver.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

För **parametrar**lägger du till `metadata` ett objekt med `description` en egenskap.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

När du distribuerar mallen via portalen används den text som du anger i beskrivningen automatiskt som ett tips för den parametern.

![Visa parameter tips](./media/template-syntax/show-parameter-tip.png)

För **resurser**lägger du till `comments` ett-element eller ett metadataobjekt. I följande exempel visas både ett kommentars element och ett metadataobjekt.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2018-07-01",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "comments": "Storage account used to store VM disks",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

För **utdata**lägger du till ett metadataobjekt till utdata-värdet.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

Det går inte att lägga till ett metadataobjekt i användardefinierade funktioner.

## <a name="multi-line-strings"></a>Strängar med flera rader

Du kan dela upp en sträng i flera rader. Se till exempel egenskapen location och en av kommentarerna i följande JSON-exempel.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Om du vill distribuera mallar med strängar med flera rader med hjälp av Azure CLI med version 2.3.0 eller äldre måste du `--handle-extended-json-format` använda växeln.

## <a name="next-steps"></a>Nästa steg

* Om du vill visa kompletta mallar för många olika typer av lösningar kan du se [Azure-snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).
* Mer information om vilka funktioner du kan använda i en mall finns i [Azure Resource Manager Template Functions](template-functions.md).
* Information om hur du kombinerar flera mallar under distributionen finns i [använda länkade mallar med Azure Resource Manager](linked-templates.md).
* Rekommendationer om hur du skapar mallar finns i [metod tips för Azure Resource Manager mallar](template-best-practices.md).
* Rekommendationer för att skapa Resource Manager-mallar som du kan använda i alla Azure-miljöer och Azure Stack finns i [utveckla Azure Resource Manager mallar för moln konsekvens](templates-cloud-consistency.md).
