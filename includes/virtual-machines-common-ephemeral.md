---
title: ta med fil
description: ta med fil
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d848b92da5d4181832adff8499b3531d020c30c9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "78155454"
---
Tillfälliga OS-diskar skapas på den lokala virtuella datorns lagrings plats (VM) och sparas inte på den fjärranslutna Azure Storage. Tillfälliga OS-diskar fungerar bra för tillstånds lösa arbets belastningar, där program är toleranta av enskilda VM-fel, men de påverkas mer av den virtuella datorns distributions tid eller avbildning av de enskilda VM-instanserna. Med en tillfällig OS-disk får du mindre Läs-/skriv fördröjning till operativ system disken och en snabbare avbildning av den virtuella datorn. 
 
Huvud funktionerna för tillfälliga diskar är: 
- Idealisk för tillstånds lösa program.
- De kan användas med både Marketplace och anpassade avbildningar.
- Möjlighet att snabbt återställa eller återställa avbildningar av virtuella datorer och skalnings uppsättnings instanser till det ursprungliga start läget.  
- Lägre latens, ungefär som en tillfällig disk. 
- De tillfälliga OS-diskarna är kostnads fria, du debiteras ingen lagrings kostnad för OS-disken.
- De är tillgängliga i alla Azure-regioner. 
- Den tillfälliga OS-disken stöds av det [delade avbildnings galleriet](/azure/virtual-machines/linux/shared-image-galleries). 
 

 
Viktiga skillnader mellan beständiga och tillfälliga OS-diskar:

|                             | Beständig OS-disk                          | Differentierande OS-disk                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| Storleks gräns för OS-disk      | 2 TiB                                                                                        | Cachestorlek för VM-storlek eller 2TiB, beroende på vilket som är mindre. Cache- **storleken i GIB**finns i [DS](../articles/virtual-machines/linux/sizes-general.md), [es](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md)och [GS](/azure/virtual-machines/linux/sizes-previous-gen#gs-series)              |
| VM-storlekar som stöds          | Alla                                                                                          | DSv1, DSv2, DSv3, Esv3, FS, FsV2, GS, M                                               |
| Disk typs stöd           | Hanterad och ohanterad OS-disk                                                                | Endast hanterad OS-disk                                                               |
| Stöd för regioner              | Alla regioner                                                                                  | Alla regioner                              |
| Data persistens            | Operativ system disk data som skrivs till OS-disken lagras i Azure Storage                                  | Data som skrivs till OS-disken lagras på den lokala VM-lagringen och är inte kvar att Azure Storage. |
| Stopp-frigjord tillstånd      | De virtuella datorerna och skalnings uppsättnings instanserna kan stoppas och startas om från det stoppade avallokerade läget | Virtuella datorer och skalnings uppsättnings instanser kan inte stoppas eller avallokeras                                  |
| Stöd för specialiserade OS-diskar | Ja                                                                                          | Nej                                                                                 |
| Storleks ändring av OS-disk              | Stöds under skapande av virtuell dator och när den virtuella datorn har stoppats                                | Stöds endast när en virtuell dator skapas                                                  |
| Ändra storlek till en ny VM-storlek   | OS-disk data bevaras                                                                    | Data på OS-disken tas bort, OS har allokerats på nytt                                      |

## <a name="size-requirements"></a>Storleks krav

Du kan distribuera VM-och instans avbildningar upp till storleken på VM-cachen. Till exempel är standard Windows Server-avbildningar från Marketplace cirka 127 GiB, vilket innebär att du behöver en VM-storlek som har ett cacheminne som är större än 127 GiB. I det här fallet har [Standard_DS2_v2](~/articles/virtual-machines/dv2-dsv2-series.md) cache-storlek på 86 GIB, vilket inte är tillräckligt stort. Standard_DS3_v2 har cache-storleken 172 GiB, vilket är tillräckligt stort. I det här fallet är Standard_DS3_v2 den minsta storleken i DSv2-serien som du kan använda med den här avbildningen. Grundläggande Linux-avbildningar i Marketplace-och Windows Server-avbildningar som anges `[smallsize]` av tenderar att vara cirka 30 GiB och kan använda de flesta tillgängliga VM-storlekar.

Tillfälliga diskar kräver också att den virtuella dator storleken har stöd för Premium Storage. Storlekarna brukar vara (men inte alltid) har `s` ett i namnet, t. ex. DSv2 och EsV3. Mer information finns i [storlekar för virtuella Azure-datorer](../articles/virtual-machines/linux/sizes.md) för information om vilka storlekar som stöder Premium Storage.

## <a name="powershell"></a>PowerShell

Om du vill använda en tillfällig disk för en PowerShell VM-distribution använder du [set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) i din VM-konfiguration. Ange `-DiffDiskSetting` till `Local` och `-Caching` till. `ReadOnly`     

```powershell
Set-AzVMOSDisk -DiffDiskSetting Local -Caching ReadOnly
```

För distributioner av skalnings uppsättningar använder du cmdleten [set-AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) i konfigurationen. Ange `-DiffDiskSetting` till `Local` och `-Caching` till. `ReadOnly`


```powershell
Set-AzVmssStorageProfile -DiffDiskSetting Local -OsDiskCaching ReadOnly
```

## <a name="cli"></a>CLI

Om du vill använda en tillfällig disk för en distribution av CLI-VM `--ephemeral-os-disk` anger du parametern i [AZ VM Create](/cli/azure/vm#az-vm-create) `true` to `--os-disk-caching` och parametern `ReadOnly`till.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --ephemeral-os-disk true \
  --os-disk-caching ReadOnly \
  --admin-username azureuser \
  --generate-ssh-keys
```

För skalnings `--ephemeral-os-disk true` uppsättningar använder du samma parameter för [AZ-VMSS-Create](/cli/azure/vmss#az-vmss-create) och anger `--os-disk-caching` parametern till. `ReadOnly`

## <a name="portal"></a>Portalen   

I Azure Portal kan du välja att använda tillfälliga diskar när du distribuerar en virtuell dator genom att öppna avsnittet **Avancerat** på fliken **diskar** . Välj **Ja**om du vill **använda en tillfällig OS-disk** .

![Skärm bild som visar alternativ knappen för att välja att använda en tillfällig OS-disk](./media/virtual-machines-common-ephemeral/ephemeral-portal.png)

Om alternativet för att använda en tillfällig disk är nedtonat kan du ha valt en storlek på virtuell dator som inte har en cachestorlek som är större än operativ system avbildningen eller som inte stöder Premium Storage. Gå tillbaka till sidan med **grundläggande** information och försök att välja en annan storlek på den virtuella datorn.

Du kan också skapa skalnings uppsättningar med tillfälliga OS-diskar med hjälp av portalen. Se bara till att du väljer en VM-storlek med en tillräckligt stor cachestorlek och välj sedan **Ja**i **Använd tillfällig OS-disk** .

![Skärm bild som visar alternativ knappen för att välja att använda en tillfällig OS-disk för din skalnings uppsättning](./media/virtual-machines-common-ephemeral/scale-set.png)

## <a name="scale-set-template-deployment"></a>Distribution av mall för skalnings uppsättning  
Processen för att skapa en skalnings uppsättning som använder en tillfällig OS-disk är att lägga `diffDiskSettings` till egenskapen till `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` resurs typen i mallen. Dessutom måste caching-principen anges till `ReadOnly` för den tillfälliga OS-disken. 


```json
{ 
  "type": "Microsoft.Compute/virtualMachineScaleSets", 
  "name": "myScaleSet", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "sku": { 
    "name": "Standard_DS2_v2", 
    "capacity": "2" 
  }, 
  "properties": { 
    "upgradePolicy": { 
      "mode": "Automatic" 
    }, 
    "virtualMachineProfile": { 
       "storageProfile": { 
        "osDisk": { 
          "diffDiskSettings": { 
                "option": "Local" 
          }, 
          "caching": "ReadOnly", 
          "createOption": "FromImage" 
        }, 
        "imageReference":  { 
          "publisher": "Canonical", 
          "offer": "UbuntuServer", 
          "sku": "16.04-LTS", 
          "version": "latest" 
        } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvmss", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
  } 
}  
```

## <a name="vm-template-deployment"></a>Distribution av VM-mall 
Du kan distribuera en virtuell dator med en tillfällig OS-disk med hjälp av en mall. Processen för att skapa en virtuell dator som använder tillfälliga OS-diskar är att lägga `diffDiskSettings` till egenskapen till resurs typen Microsoft. Compute/virtualMachines i mallen. Dessutom måste caching-principen anges till `ReadOnly` för den tillfälliga OS-disken. 

```json
{ 
  "type": "Microsoft.Compute/virtualMachines", 
  "name": "myVirtualMachine", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "properties": { 
       "storageProfile": { 
            "osDisk": { 
              "diffDiskSettings": { 
                "option": "Local" 
              }, 
              "caching": "ReadOnly", 
              "createOption": "FromImage" 
            }, 
            "imageReference": { 
                "publisher": "MicrosoftWindowsServer", 
                "offer": "WindowsServer", 
                "sku": "2016-Datacenter-smalldisk", 
                "version": "latest" 
            }, 
            "hardwareProfile": { 
                 "vmSize": "Standard_DS2_v2" 
             } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvirtualmachine", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
 } 
```


## <a name="reimage-a-vm-using-rest"></a>Återställa avbildningen av en virtuell dator med hjälp av REST
Du kan återställa avbildningen av en virtuell dator instans med en tillfällig OS-disk med REST API enligt beskrivningen nedan och via Azure Portal genom att gå till översikts fönstret på den virtuella datorn. För skalnings uppsättningar är åter avbildning redan tillgänglig via PowerShell, CLI och portalen.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**F: Vad är storleken på de lokala OS-diskarna?**

A: vi stöder plattform och anpassade avbildningar, upp till den virtuella datorns cachestorlek, där all läsning/skrivning till OS-disken är lokalt på samma nod som den virtuella datorn. 

**F: kan storleken på den tillfälliga OS-disken ändras?**

A: Nej, det går inte att ändra storlek på operativ system disken när den tillfälliga OS-disken har allokerats. 

**F: kan jag koppla en Managed Disks till en tillfällig virtuell dator?**

A: Ja, du kan koppla en hanterad datadisk till en virtuell dator som använder en tillfällig OS-disk. 

**F: kommer alla VM-storlekar att stödjas för tillfälliga OS-diskar?**

S: Nej, alla Premium Storage VM-storlekar stöds (DS, ES, FS, GS och M) förutom storlekarna B-serien, N-serien och H-serien.  
 
**F: kan den tillfälliga OS-disken tillämpas på befintliga virtuella datorer och skalnings uppsättningar?**

A: Nej, det går bara att använda en tillfällig OS-disk när VM och skalnings uppsättning skapas. 

**F: kan du blanda tillfälliga och normala OS-diskar i en skalnings uppsättning?**

A: Nej, du kan inte ha en blandning av instanser av tillfälliga och beständiga OS-diskar inom samma skalnings uppsättning. 

**F: kan den tillfälliga OS-disken skapas med PowerShell eller CLI?**

A: Ja, du kan skapa virtuella datorer med en tillfällig OS-disk med hjälp av REST, templates, PowerShell och CLI.

**F: vilka funktioner stöds inte med den tillfälliga OS-disken?**

A: tillfälliga diskar stöder inte:
- Samla in VM-avbildningar
- Ögonblicksbilder 
- Azure Disk Encryption 
- Azure Backup
- Azure Site Recovery  
- OS-disk växling 
