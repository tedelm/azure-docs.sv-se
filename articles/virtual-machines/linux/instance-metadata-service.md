---
title: Azure-Instance Metadata Service
description: RESTful-gränssnitt för att hämta information om virtuella Linux-datorer beräkning, nätverk och kommande underhålls händelser.
services: virtual-machines-linux
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines-linux
ms.subservice: monitoring
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/29/2020
ms.author: sukumari
ms.reviewer: azmetadata
ms.openlocfilehash: ce3463d39b17e7099f85945caa92d1f009b696c0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649849"
---
# <a name="azure-instance-metadata-service"></a>Azure-instansens metadatatjänst

Azure-Instance Metadata Service (IMDS) innehåller information om de virtuella dator instanser som körs och kan användas för att hantera och konfigurera dina virtuella datorer.
Detta omfattar SKU, lagring, nätverkskonfigurationer och kommande underhålls händelser. En fullständig lista över tillgängliga data finns i [API: er för metadata](#metadata-apis). Instance Metadata Service är tillgängligt för instanser av skalnings uppsättningar för virtuella datorer och virtuella datorer. Det är bara tillgängligt för att köra virtuella datorer som skapats/hanterats med [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). 

Azures Instance Metadata Service är en REST-slutpunkt som är tillgänglig på en välkänd icke-flyttbar IP-adress ( `169.254.169.254` ), men den kan bara nås från den virtuella datorn.

## <a name="security"></a>Säkerhet

Instance Metadata Service slut punkten kan bara nås från den virtuella dator instansen som körs på en IP-adress som inte är dirigerbart. Dessutom avvisas alla begär Anden med en `X-Forwarded-For` rubrik av tjänsten.
Begär Anden måste också innehålla en `Metadata: true` rubrik för att säkerställa att den faktiska begäran var direkt avsedd och inte en del av oavsiktlig omdirigering.

> [!IMPORTANT]
> Instance Metadata Service är inte en kanal för känsliga data. Slut punkten är öppen för alla processer på den virtuella datorn. Information som exponeras genom den här tjänsten bör betraktas som delad information till alla program som körs i den virtuella datorn.

## <a name="usage"></a>Användning

### <a name="accessing-azure-instance-metadata-service"></a>Åtkomst till Azure-Instance Metadata Service

För att komma åt Instance Metadata Service skapar du en virtuell dator från [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) eller [Azure Portal](https://portal.azure.com)och följer sedan exemplen nedan.
Fler exempel på hur du kan fråga IMDS finns på [Azure instance metadata-exempel](https://github.com/microsoft/azureimds).

Nedan visas exempel koden för att hämta alla metadata för en instans, för att få åtkomst till en speciell data källa, se avsnittet [metadata-API](#metadata-apis) . 

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2019-06-01"
```

**Svar**

> [!NOTE]
> Svaret är en JSON-sträng. Följande exempel svar är ganska utskrivet för läsbarhet.

```json
{
  "compute": {
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
        "dataDisks": [
          {
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
              "uri": ""
            },
            "lun": "0",
            "managedDisk": {
              "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
              "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
              "uri": ""
            },
            "writeAcceleratorEnabled": "false"
          }
        ],
        "imageReference": {
          "id": "",
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04.0-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": "30",
          "diffDiskSettings": {
            "option": "Local"
          },
          "encryptionSettings": {
            "enabled": "false"
          },
          "image": {
            "uri": ""
          },
          "managedDisk": {
            "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
            "storageAccountType": "Standard_LRS"
          },
          "name": "exampleosdiskname",
          "osType": "Linux",
          "vhd": {
            "uri": ""
          },
          "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Prod;Role:WorkerRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

### <a name="data-output"></a>Data utdata

Som standard returnerar Instance Metadata Service data i JSON-format ( `Content-Type: application/json` ). Olika API: er returnerar dock data i olika format om det begärs.
Följande tabell är en referens till andra API: er för data format som kan ha stöd för.

API | Standard data format | Andra format
--------|---------------------|--------------
/instance | json | text
/scheduledevents | json | inget
/attested | json | inget

Om du vill komma åt ett svar som inte är standardformat anger du det begärda formatet som en frågesträngparametern i begäran. Till exempel:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> För löv-noder `format=json` fungerar det inte. För dessa frågor `format=text` måste anges explicit om standardformatet är JSON.

### <a name="versioning"></a>Versionshantering

Instance Metadata Service har versions hantering och angett att API-versionen i HTTP-begäran är obligatorisk.

Följ de service versioner som stöds: 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15.

Obs! när en ny version släpps, tar det en stund innan den distribueras till alla regioner. För närvarande är version 2019-11-01 fortfarande distribuerad och är kanske inte tillgänglig i alla regioner.

När nya versioner läggs till kan äldre versioner fortfarande nås för kompatibilitet om skripten har beroenden för vissa data format.

Om ingen version anges returneras ett fel med en lista över de senaste versionerna som stöds.

> [!NOTE]
> Svaret är en JSON-sträng. I följande exempel visas fel tillståndet när ingen version anges, svaret är ganska utskrivet för läsbarhet.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance"
```

**Svar**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

## <a name="metadata-apis"></a>API: er för metadata
IMDS innehåller flera API-gränssnitt som representerar olika data källor. 

Data | Description | Version introducerad
-----|-------------|-----------------------
session | Se [instans-API](#instance-api) | 2017-04-02
godkänd | Se [attesterade data](#attested-data) | 2018-10-01
identity | Se [Hämta en åtkomsttoken](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
scheduledevents | Se [schemalagda händelser](scheduled-events.md) | 2017-08-01

## <a name="instance-api"></a>Instans-API

Instans-API: t visar viktiga metadata för VM-instanser, inklusive VM, nätverk och lagring. Följande kategorier kan nås via instans/Compute:

Data | Description | Version introducerad
-----|-------------|-----------------------
azEnvironment | Azure-miljö där den virtuella datorn körs i | 2018-10-01
customData | Den här funktionen är för närvarande inaktive rad och vi kommer att uppdatera den här dokumentationen när den blir tillgänglig | 2019-02-01
location | Azure-regionen som den virtuella datorn körs i | 2017-04-02
name | Namn på den virtuella datorn | 2017-04-02
offer | Erbjudande information för den virtuella dator avbildningen och finns bara för avbildningar som distribuerats från Azures avbildnings Galleri | 2017-04-02
osType | Linux eller Windows | 2017-04-02
placementGroupId | [Placerings grupp](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) för den virtuella datorns skalnings uppsättning | 2017-08-01
planera | [Planera](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) som innehåller namn, produkt och utgivare för en virtuell dator om det är en Azure Marketplace-avbildning | 2018-04-02
platformUpdateDomain |  [Uppdatera den domän](manage-availability.md) som den virtuella datorn körs i | 2017-04-02
platformFaultDomain | [Feldomän](manage-availability.md) som den virtuella datorn körs i | 2017-04-02
CSP | Provider för den virtuella datorn | 2018-10-01
publicKeys | [Samling offentliga nycklar](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey) som har tilldelats den virtuella datorn och sökvägar | 2018-04-02
utgivare | Utgivare av VM-avbildningen | 2017-04-02
resourceGroupName | [Resurs grupp](../../azure-resource-manager/management/overview.md) för den virtuella datorn | 2017-08-01
resourceId | Resursens [fullständigt kvalificerade](https://docs.microsoft.com/rest/api/resources/resources/getbyid) ID | 2019-03-11
sku | En speciell SKU för VM-avbildningen | 2017-04-02
storageProfile | Se [lagrings profil](#storage-metadata) | 2019-06-01
subscriptionId | Azure-prenumeration för den virtuella datorn | 2017-08-01
tags | [Taggar](../../azure-resource-manager/management/tag-resources.md) för den virtuella datorn  | 2017-08-01
tagsList | Taggar formaterade som en JSON-matris för enklare programmerings parsning  | 2019-06-04
version | Version av VM-avbildningen | 2017-04-02
vmId | [Unikt ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) för den virtuella datorn | 2017-04-02
vmScaleSetName | [Skal uppsättnings namn för virtuell](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) dator för den virtuella datorns skalnings uppsättning | 2017-12-01
vmSize | [Storlek på virtuell dator](sizes.md) | 2017-04-02
zon | [Tillgänglighets zon](../../availability-zones/az-overview.md) för den virtuella datorn | 2017-12-01

### <a name="sample-1-tracking-vm-running-on-azure"></a>Exempel 1: spåra virtuell dator som körs på Azure

Som tjänst leverantör kan du behöva spåra antalet virtuella datorer som kör program varan eller ha agenter som behöver spåra unika virtuella datorer. Om du vill kunna hämta ett unikt ID för en virtuell dator använder du `vmId` fältet från instance metadata service.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Svar**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="sample-2-placement-of-containers-data-partitions-based-faultupdate-domain"></a>Exempel 2: placering av behållare, data-partitioner som är baserade fel/uppdaterings domän

För vissa scenarier är placeringen av olika data repliker av primär betydelse. Till exempel, [HDFS-replik placering](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) eller container placering via en [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kan du behöva känna till `platformFaultDomain` och `platformUpdateDomain` den virtuella datorn körs på.
Du kan också använda [Tillgänglighetszoner](../../availability-zones/az-overview.md) för instanserna för att fatta beslut.
Du kan fråga dessa data direkt via Instance Metadata Service.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Svar**

```text
0
```

### <a name="sample-3-getting-more-information-about-the-vm-during-support-case"></a>Exempel 3: Hämta mer information om den virtuella datorn under support ärendet

Som tjänst leverantör kan du få ett support samtal där du vill ha mer information om den virtuella datorn. Att be kunden att dela dina beräknings-metadata kan ge grundläggande information om support teknikern för att veta om vilken typ av virtuell dator som finns i Azure.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2019-06-01"
```

**Svar**

> [!NOTE]
> Svaret är en JSON-sträng. Följande exempel svar är ganska utskrivet för läsbarhet.

```json
{
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
        "dataDisks": [
          {
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
              "uri": ""
            },
            "lun": "0",
            "managedDisk": {
              "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
              "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
              "uri": ""
            },
            "writeAcceleratorEnabled": "false"
          }
        ],
        "imageReference": {
          "id": "",
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04.0-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": "30",
          "diffDiskSettings": {
            "option": "Local"
          },
          "encryptionSettings": {
            "enabled": "false"
          },
          "image": {
            "uri": ""
          },
          "managedDisk": {
            "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
            "storageAccountType": "Standard_LRS"
          },
          "name": "exampleosdiskname",
          "osType": "Linux",
          "vhd": {
            "uri": ""
          },
          "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
}
```

### <a name="sample-4-getting-azure-environment-where-the-vm-is-running"></a>Exempel 4: få Azure-miljö där den virtuella datorn körs

Azure har olika suveräna moln som [Azure Government](https://azure.microsoft.com/overview/clouds/government/). Ibland behöver du Azure-miljön för att kunna göra vissa körnings beslut. I följande exempel visas hur du kan få det här beteendet.

**Förfrågan**
```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Svar**

```bash
AzurePublicCloud
```

Molnet och värdena i Azure-miljön visas nedan.

 Molnet   | Azure-miljö
---------|-----------------
[Alla allmänt tillgängliga globala Azure-regioner](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure Kina 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | AzureChinaCloud
[Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

## <a name="network-metadata"></a>Metadata för nätverk 

Metadata för nätverket är en del av instans-API: et. Följande nätverks kategorier är tillgängliga via instansen/nätverks slut punkten.

Data | Description | Version introducerad
-----|-------------|-----------------------
IPv4/privateIpAddress | Lokal IPv4-adress för den virtuella datorn | 2017-04-02
IPv4/publicIpAddress | Offentlig IPv4-adress för den virtuella datorn | 2017-04-02
undernät/adress | Den virtuella datorns under näts adress | 2017-04-02
undernät/prefix | Undernätsprefixet, exempel 24 | 2017-04-02
IPv6/ipAddress | Lokal IPv6-adress för den virtuella datorn | 2017-04-02
macAddress | Mac-adress för virtuell dator | 2017-04-02

> [!NOTE]
> Alla API-svar är JSON-strängar. Alla följande exempel svar är ganska utskrivna för läsbarhet.

#### <a name="sample-1-retrieving-network-information"></a>Exempel 1: hämtar nätverks information

***Förfrågan***

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

***Svar***

> [!NOTE]
> Svaret är en JSON-sträng. Följande exempel svar är ganska utskrivet för läsbarhet.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="sample-2-retrieving-public-ip-address"></a>Exempel 2: hämtar offentlig IP-adress

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"

```

## <a name="storage-metadata"></a>Metadata för lagring

Lagrings-metadata ingår i instans-API: t under instans/Compute/storageProfile-slutpunkt.
Den innehåller information om de lagrings diskar som är kopplade till den virtuella datorn. 

Lagrings profilen för en virtuell dator är uppdelad i tre kategorier-avbildnings referens, OS-disk och data diskar.

Objektet bild referens innehåller följande information om operativ system avbildningen:

Data    | Description
--------|-----------------
id      | Resurs-ID
offer   | Erbjudande för plattformen eller Marketplace-avbildningen
utgivare | Avbildnings utgivare
sku     | Bild-SKU
version | Version av plattformen eller Marketplace-avbildningen

Objektet OS-disk innehåller följande information om den OS-disk som används av den virtuella datorn:

Data    | Description
--------|-----------------
cachelagring | Krav för cachelagring
createOption | Information om hur den virtuella datorn skapades
diffDiskSettings | Inställningar för tillfälliga diskar
diskSizeGB | Disk storlek i GB
image   | Virtuell hård disk för käll användar avbildning
enheten     | Diskens logiska enhets nummer
managedDisk | Parametrar för hanterade diskar
name    | Disknamn
disken     | Virtuell hårddisk
writeAcceleratorEnabled | Huruvida writeAccelerator har Aktiver ATS på disken

Data disks-matrisen innehåller en lista över data diskar som är anslutna till den virtuella datorn. Varje data disk objekt innehåller följande information:

Data    | Description
--------|-----------------
cachelagring | Krav för cachelagring
createOption | Information om hur den virtuella datorn skapades
diffDiskSettings | Inställningar för tillfälliga diskar
diskSizeGB | Disk storlek i GB
encryptionSettings | Krypterings inställningar för disken
image   | Virtuell hård disk för käll användar avbildning
managedDisk | Parametrar för hanterade diskar
name    | Disknamn
osType  | Typ av operativ system som ingår i disken
disken     | Virtuell hårddisk
writeAcceleratorEnabled | Huruvida writeAccelerator har Aktiver ATS på disken

Följande är ett exempel på hur du frågar den virtuella datorns lagrings information.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01"
```

**Svar**

> [!NOTE]
> Svaret är en JSON-sträng. Följande exempel svar är ganska utskrivet för läsbarhet.

```json
{
    "dataDisks": [
      {
        "caching": "None",
        "createOption": "Empty",
        "diskSizeGB": "1024",
        "image": {
          "uri": ""
        },
        "lun": "0",
        "managedDisk": {
          "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
          "storageAccountType": "Standard_LRS"
        },
        "name": "exampledatadiskname",
        "vhd": {
          "uri": ""
        },
        "writeAcceleratorEnabled": "false"
      }
    ],
    "imageReference": {
      "id": "",
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04.0-LTS",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "diskSizeGB": "30",
      "diffDiskSettings": {
        "option": "Local"
      },
      "encryptionSettings": {
        "enabled": "false"
      },
      "image": {
        "uri": ""
      },
      "managedDisk": {
        "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
        "storageAccountType": "Standard_LRS"
      },
      "name": "exampleosdiskname",
      "osType": "Linux",
      "vhd": {
        "uri": ""
      },
      "writeAcceleratorEnabled": "false"
    }
}
```

## <a name="vm-tags"></a>VM-Taggar

VM-Taggar ingår i instans-API: t under slut punkt för instans/beräkning/taggar.
Taggar kan ha tillämpats på den virtuella Azure-datorn för att logiskt organisera dem i en taxonomi. Taggarna som tilldelas till en virtuell dator kan hämtas med hjälp av begäran nedan.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Svar**

```text
Department:IT;Environment:Test;Role:WebRole
```

`tags`Fältet är en sträng med taggarna avgränsade med semikolon. Detta kan vara ett problem om semikolon används i själva taggarna. Om en parser skrivs för att program mässigt extrahera taggarna bör du förlita dig på `tagsList` fältet som är en JSON-matris utan avgränsare och därmed lättare att parsa.

**Förfrågan**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04&format=json"
```

**Svar**

```json
[
  {
    "name": "Department",
    "value": "IT"
  },
  {
    "name": "Environment",
    "value": "Test"
  },
  {
    "name": "Role",
    "value": "WebRole"
  }
]
```

## <a name="attested-data"></a>Bestyrkade data

En del av scenariot som hanteras av Instance Metadata Service är att tillhandahålla garantier att de data som tillhandahålls kommer från Azure. Vi loggar en del av den här informationen så att Marketplace-avbildningar kan vara säker på att de är en image som körs på Azure.

### <a name="sample-1-getting-attested-data"></a>Exempel 1: Hämta attestering av data

> [!NOTE]
> Alla API-svar är JSON-strängar. Följande exempel svar är ganska utskrivna för läsbarhet.

***Förfrågan***

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"

```

API-version är ett obligatoriskt fält. Se [användnings avsnittet](#usage) för API-versioner som stöds.
Nonce är en valfri sträng med 10 siffror. Om den inte anges, returnerar IMDS den aktuella UTC-tidsstämpeln i sitt ställe. På grund av IMDS-mekanismen kan ett tidigare cachelagrat nonce-värde returneras.

 ***Svar***

> [!NOTE]
> Svaret är en JSON-sträng. Följande exempel svar är ganska utskrivet för läsbarhet.

```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

Signatur-bloben är en [PKCS7](https://aka.ms/pkcs7) -signerad version av dokumentet. Det innehåller det certifikat som används för att signera tillsammans med den virtuella dator informationen som vmId, SKU, nonce, subscriptionId, timeStamp för att skapa och upphör ande av dokumentet och plan informationen om avbildningen. Plan informationen är bara ifylld för Azure Marketplace-avbildningar. Certifikatet kan extraheras från svaret och används för att verifiera att svaret är giltigt och kommer från Azure.

### <a name="sample-2-validating-that-the-vm-is-running-in-azure"></a>Exempel 2: verifierar att den virtuella datorn körs i Azure

Marketplace-leverantörer vill se till att deras program vara licensieras att köras endast i Azure. Om någon kopierar den virtuella hård disken till en lokal plats, ska de ha möjlighet att identifiera den. Genom att ringa till Instance Metadata Service kan Marketplace-leverantörer Hämta signerade data som endast garanterar svar från Azure.

> [!NOTE]
> Kräver att JQ installeras.

***Förfrågan***

```bash
  # Get the signature
   curl  --silent -H Metadata:True http://169.254.169.254/metadata/attested/document?api-version=2019-04-30 | jq -r '.["signature"]' > signature
  # Decode the signature
  base64 -d signature > decodedsignature
  #Get PKCS7 format
  openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
  # Get Public key out of pkc7
  openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
  #Get the intermediate certificate
  wget -q -O intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
  openssl x509 -inform der -in intermediate.cer -out intermediate.pem
  #Verify the contents
  openssl smime -verify -in sign.pk7 -inform pem -noverify
 ```

 **Svar**

```json
Verification successful
{"nonce":"20181128-001617",
  "plan":
    {
     "name":"",
     "product":"",
     "publisher":""
    },
"timeStamp":
  {
    "createdOn":"11/28/18 00:16:17 -0000",
    "expiresOn":"11/28/18 06:16:17 -0000"
  },
"vmId":"d3e0e374-fda6-4649-bbc9-7f20dc379f34",
"subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
"sku": "RS3-Pro"
}
```

Data | Description
-----|------------
Nnär | Användaren angav valfri sträng med begäran. Om inget nonce angavs i begäran returneras den aktuella UTC-tidsstämpeln
planera | [Planera](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) för en virtuell dator i den är en Azure Marketplace-avbildning som innehåller namn, produkt och utgivare
Timestamp/createdOn | UTC-tidsstämpeln då det första signerade dokumentet skapades
tidsstämpel/expiresOn | UTC-tidsstämpeln då det signerade dokumentet upphör att gälla
vmId |  [Unikt ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) för den virtuella datorn
subscriptionId | Azure-prenumerationen för den virtuella datorn som introducerades i`2019-04-30`
sku | En speciell SKU för VM-avbildningen som introducerades i`2019-11-01`

#### <a name="sample-3-verifying-the-signature"></a>Exempel 3: verifiera signaturen

När du har hämtat signaturen ovan kan du kontrol lera att signaturen är från Microsoft. Du kan också kontrol lera mellanliggande certifikat och certifikat kedjan. Slutligen kan du kontrol lera att prenumerations-ID: t är korrekt.

> [!NOTE]
> Certifikatet för det offentliga molnet och det suveräna molnet är annorlunda.

 Molnet | Certifikat
---------|-----------------
[Alla allmänt tillgängliga globala Azure-regioner](https://azure.microsoft.com/regions/)     | *. metadata.azure.com
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | *. metadata.azure.us
[Azure Kina 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | *. metadata.azure.cn
[Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/)                    | *. metadata.microsoftazure.de

Det finns ett känt problem runt certifikatet som används för signering. Certifikaten kanske inte har en exakt matchning av `metadata.azure.com` för det offentliga molnet. Certifierings valideringen bör därför tillåta ett eget namn från valfri `.metadata.azure.com` under domän.

```bash

# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain, for Azure China 21Vianet the intermediate certificate will be from DigiCert Global Root CA
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

I de fall där det mellanliggande certifikatet inte kan laddas ned på grund av nätverks begränsningar under verifieringen, kan det mellanliggande certifikatet fästas. Azure kommer dock att återställas över certifikaten enligt PKI-praxis enligt standard. De fästa certifikaten måste uppdateras när förnyelsen sker. När en ändring av uppdateringen av mellanliggande certifikat planeras uppdateras Azure-bloggen och Azure-kunder meddelas. Du hittar de mellanliggande certifikaten [här](https://www.microsoft.com/pki/mscorp/cps/default.htm). De mellanliggande certifikaten för varje region kan vara olika.

> [!NOTE]
>Mellanliggande certifikat för Azure Kina 21Vianet kommer från DigiCert globala rot certifikat utfärdare i stället för Baltimore.
Om du har fäst mellanliggande certifikat för Azure Kina som en del av ändringen av rot kedjan måste de mellanliggande certifikaten uppdateras.

## <a name="managed-identity-via-metadata-service"></a>Hanterad identitet via Metadata Service
Användaren kan aktivera den hanterade identiteten på en virtuell dator och sedan använda Instance Metadata Service för att skicka token för åtkomst till Azure-tjänster. Program som körs på en virtuell dator kan nu begära en token från Azure instance metadata service-slutpunkten och sedan använda token för att autentisera till moln tjänster, inklusive Key Vault.
Detaljerade anvisningar om hur du aktiverar den här funktionen finns i [Hämta en](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md)åtkomsttoken.

## <a name="scheduled-events-via-metadata-service"></a>Schemalagda händelser via Metadata Service
Du kan hämta status för schemalagda händelser via metadatatjänst och sedan kan användaren ange en uppsättning åtgärder som ska utföras vid dessa händelser.  Se [schemalagda händelser](scheduled-events.md) för mer information. 

## <a name="regional-availability"></a>Regional tillgänglighet

Tjänsten är **allmänt tillgänglig** i alla Azure-regioner. Det här omfattar: 
1. [Alla allmänt tillgängliga globala Azure-regioner](https://azure.microsoft.com/regions/)
2. [Azure Government](https://azure.microsoft.com/overview/clouds/government/)  
3. [Azure Kina 21Vianet](https://www.azure.cn/) 
4. [Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/) 

## <a name="sample-code-in-different-languages"></a>Exempel kod på olika språk

Exempel på hur du anropar metadata service med olika språk i den virtuella datorn:

Språk | Exempel
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Go  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Puppet | https://github.com/keirans/azuremetadata

## <a name="error-and-debugging"></a>Fel och fel sökning

Om det inte går att hitta ett data element eller en felaktig begäran, returnerar Instance Metadata Service vanliga HTTP-fel. Till exempel:

HTTP-statuskod | Anledning
----------------|-------
200 OK |
400 Felaktig begäran | `Metadata: true`Sidhuvudet saknas eller så saknas formatet vid fråga till en lövnod
404 – Hittades inte | Det begärda elementet finns inte
metoden 405 tillåts inte | Endast `GET` begär Anden stöds
410 borta | Försök igen om en stund i högst 70 sekunder
429 För många förfrågningar | API: et stöder för närvarande högst 5 frågor per sekund
500-tjänst fel     | Försök igen om en stund

### <a name="known-issues-and-faq"></a>Kända problem och vanliga frågor och svar

1. Jag får felet `400 Bad Request, Required metadata header not specified` . Vad betyder detta?
   * Den Instance Metadata Service kräver att rubriken `Metadata: true` skickas i begäran. Om du skickar den här rubriken i REST-anropet får du till gång till Instance Metadata Service.
2. Varför får jag inte beräknings information för min virtuella dator?
   * För närvarande stöder Instance Metadata Service endast instanser som skapats med Azure Resource Manager. I framtiden kan stöd för virtuella datorer i moln tjänsten läggas till.
3. Jag har skapat min virtuella dator genom att Azure Resource Manager en och tillbaka. Varför visas inte information om att beräkna metadata?
   * För virtuella datorer som skapats efter sep 2016 lägger du till en [tagg](../../azure-resource-manager/management/tag-resources.md) för att börja se Compute metadata. För äldre virtuella datorer (som skapats före sep 2016) lägger du till/tar bort tillägg eller data diskar till den virtuella datorn för att uppdatera metadata.
4. Jag ser inte alla data som är ifyllda för nya versioner
   * För virtuella datorer som skapats efter sep 2016 lägger du till en [tagg](../../azure-resource-manager/management/tag-resources.md) för att börja se Compute metadata. För äldre virtuella datorer (som skapats före sep 2016) lägger du till/tar bort tillägg eller data diskar till den virtuella datorn för att uppdatera metadata.
5. Varför får jag fel meddelandet `500 Internal Server Error` ?
   * Gör om din begäran baserat på exponentiellt system. Kontakta Azure-supporten om problemet kvarstår.
6. Var kan jag dela fler frågor/kommentarer?
   * Skicka dina kommentarer om https://feedback.azure.com .
7. Skulle detta fungera för den virtuella datorns skalnings uppsättnings instans?
   * Tjänsten Yes metadata är tillgänglig för instanser av skalnings uppsättningar.
8. Hur gör jag för att få support för tjänsten?
   * Om du vill få support för tjänsten skapar du ett support ärende i Azure Portal för den virtuella datorn där du inte kan få svar på metadata efter långa återförsök.
9. Jag får en timeout för mitt samtal till tjänsten?
   * Metadata-anrop måste göras från den primära IP-adress som tilldelats till det primära nätverkskortet på den virtuella datorn, förutom om du har ändrat dina vägar måste det finnas en väg för 169.254.0.0/16-adressen från nätverkskortet.
10. Jag uppdaterade mina taggar i den virtuella datorns skalnings uppsättning men de visas inte i de här instanserna till skillnad från virtuella datorer?
    * För närvarande för ScaleSets-taggar visas bara för den virtuella datorn vid en omstart/återavbildning/eller en disk ändring till instansen.

## <a name="support-and-feedback"></a>Support och feedback

Skicka feedback och kommentarer om https://feedback.azure.com .
Om du vill få support för tjänsten skapar du ett support ärende i Azure Portal för den virtuella datorn där du inte kan få svar på metadata efter långa återförsök.

![Stöd för instansen metadata](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Nästa steg

Läs mer om:
1.  [Hämta en åtkomsttoken för den virtuella datorn](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).
2.  [Schemalagda händelser](scheduled-events.md) 