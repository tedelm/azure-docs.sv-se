---
title: NCv3-serien – Azure Virtual Machines
description: Specifikationer för virtuella datorer i NCv3-serien.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 9ae3604a9ea82e6e50ba4d639d36572f7b052e4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302805"
---
# <a name="ncv3-series"></a>NCv3-serien

Virtuella datorer i NCv3-serien drivs av [NVIDIA Tesla V100](https://www.nvidia.com/data-center/tesla-v100/) -GPU: er. Dessa GPU: er kan ge 1,5 x beräknings prestanda i NCv2-serien. Kunderna kan dra nytta av dessa uppdaterade GPU: er för traditionella HPC-arbetsbelastningar som till exempel behållar modellering, DNA-sekvensering, protein analys, Monte Carlo-simuleringar och andra. NC24rs v3-konfigurationen ger ett nätverks gränssnitt med låg fördröjning och hög genom strömning som är optimerat för tätt sammansatta parallella data behandlings arbets belastningar. Förutom GPU: er är virtuella datorer i NCv3-serien också baserade på Broadwell-processorer (Intel Xeon E5-2690 v4).

Premium Storage: stöds

Premium Storage caching: stöds

Direktmigrering: stöds inte

Minnes bebetjänings uppdateringar: stöds inte

> [!IMPORTANT]
> För den här VM-serien ställs vCPU-kvoten (kärnor) i din prenumeration från början till 0 i varje region. [Begär en vCPU-kvot ökning](../azure-supportability/resource-manager-core-quotas-request.md) för den här serien i en [tillgänglig region](https://azure.microsoft.com/regions/services/).
>
| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | GPU | GPU-minne: GiB | Maximalt antal datadiskar | Maximalt antal cachelagrade diskar: IOPS/MBps | Maximalt antal nätverkskort |
|---|---|---|---|---|---|---|---|---|
| Standard_NC6s_v3    | 6  | 112 | 736  | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3   | 12 | 224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3   | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v3 * | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = ett V100-kort.

*RDMA-stöd

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Operativsystem och drivrutiner som stöds

För att kunna dra nytta av GPU-funktionerna i virtuella datorer i Azure N-serien måste du installera NVIDIA GPU-drivrutiner.

[NVidia GPU-drivrutinen](./extensions/hpccompute-gpu-windows.md) installerar lämpliga NVIDIA-CUDA eller rutnäts driv rutiner på en virtuell dator i N-serien. Installera eller hantera tillägget med hjälp av Azure Portal eller verktyg som Azure PowerShell eller Azure Resource Manager mallar. Mer information om vilka operativ system och distributions steg som stöds finns i [dokumentationen för NVIDIA GPU-drivrutins tillägget](./extensions/hpccompute-gpu-windows.md) . Allmän information om VM-tillägg finns i [tillägg och funktioner för virtuella Azure-datorer](./extensions/overview.md).

Om du väljer att installera NVIDIA GPU-drivrutiner manuellt, se [N-seriens installations program för GPU-drivrutiner för Windows](./windows/n-series-driver-setup.md) eller [N-serien GPU-drivrutin installation för Linux](./linux/n-series-driver-setup.md) för operativ system, driv rutiner, installation och verifierings steg som stöds.

## <a name="other-sizes"></a>Andra storlekar

- [Generellt syfte](sizes-general.md)
- [Minnesoptimerad](sizes-memory.md)
- [Lagringsoptimerad](sizes-storage.md)
- [GPU-optimerad](sizes-gpu.md)
- [Databehandling med höga prestanda](sizes-hpc.md)
- [Tidigare generationer](sizes-previous-gen.md)

## <a name="next-steps"></a>Nästa steg

Lär dig mer om hur [Azure Compute Units (ACU)](acu.md) kan hjälpa dig att jämföra beräknings prestanda i Azure SKU: er.
