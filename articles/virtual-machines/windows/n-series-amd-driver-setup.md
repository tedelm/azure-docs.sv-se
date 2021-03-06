---
title: Konfiguration av Azure N-seriens AMD GPU-drivrutin för Windows
description: Konfigurera AMD GPU-drivrutiner för virtuella datorer i N-serien som kör Windows Server eller Windows i Azure
author: vikancha
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 745ec7ebf792fe1165022516be4c83fb9e864cc9
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83799870"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Installera AMD GPU-drivrutiner för virtuella datorer i N-serien som kör Windows

För att kunna dra nytta av GPU-funktionerna i de nya virtuella Azure NVv4-serierna som kör Windows, måste AMD GPU-drivrutinerna vara installerade. [Tillägget för AMD GPU-drivrutinen](../extensions/hpccompute-amd-gpu-windows.md) installerar AMD GPU-drivrutiner på en virtuell NVv4-serie. Installera eller hantera tillägget med hjälp av Azure Portal eller verktyg som Azure PowerShell eller Azure Resource Manager mallar. Mer information om vilka operativ system och distributions steg som stöds finns i [dokumentationen för AMD GPU-drivrutinen](../extensions/hpccompute-amd-gpu-windows.md) .

Om du väljer att installera AMD GPU-drivrutiner manuellt, innehåller den här artikeln stöd för operativ system, driv rutiner och installations-och verifierings steg.

Endast GPU-drivrutiner som publicerats av Microsoft stöds på virtuella NVv4-datorer. Installera inte GPU-drivrutiner från någon annan källa.

Grundläggande specifikationer, lagrings kapacitet och disk information finns i storlekar för [GPU Windows VM](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Operativsystem och drivrutiner som stöds

| Operativsystem | Drivrutin |
| -------- |------------- |
| Windows 10-EVD – build 1903 <br/><br/>Windows 10 – build 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20. q 1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (. exe) |


## <a name="driver-installation"></a>Driv rutins installation

1. Anslut via fjärr skrivbord till varje virtuell NVv4-serien.

2. Hämta och installera den senaste driv rutinen.

3. Starta om den virtuella datorn.

## <a name="verify-driver-installation"></a>Verifiera installation av driv rutin

Du kan kontrol lera driv rutins installationen i Enhetshanteraren. I följande exempel visas lyckad konfiguration av kortet Radeon Instinct MI25 på en virtuell Azure-NVv4.
<br />
![Egenskaper för GPU-drivrutin](./media/n-series-amd-driver-setup/device-manager.png)

Du kan använda dxdiag för att kontrol lera GPU-bildskärms egenskaper inklusive video-RAM. I följande exempel visas en 1/2-partition av kortet Radeon Instinct MI25 på en virtuell Azure-NVv4.
<br />
![Egenskaper för GPU-drivrutin](./media/n-series-amd-driver-setup/dxdiag-output.png)

Om du kör Windows 10 version 1903 eller senare kommer dxdiag inte att visa någon information på fliken "Visa". Använd alternativet "Spara all information" längst ned och utdatafilen visar information om AMD MI25 GPU.

![Egenskaper för GPU-drivrutin](./media/n-series-amd-driver-setup/dxdiag-details.png)


