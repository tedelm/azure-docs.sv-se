---
title: Windows Virtual Desktop, personlig Skriv bords typ – Azure
description: Så här konfigurerar du tilldelnings typen för en virtuell Windows-dator med egen Skriv bords värd.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8451dc14a7ed42aa92f9adbd5ad050936949e302
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612426"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Konfigurera tilldelnings typen egen Skriv bords värd pool

>[!IMPORTANT]
>Det här innehållet gäller för våren 2020-uppdateringen med Azure Resource Manager virtuella Windows Desktop-objekt. Om du använder den virtuella Windows-datorn med version 2019 utan Azure Resource Manager objekt, se [den här artikeln](./virtual-desktop-fall-2019/configure-host-pool-personal-desktop-assignment-type-2019.md).
>
> Den virtuella Windows-skrivbordets våren 2020-uppdateringen är för närvarande en offentlig för hands version. Den här för hands versionen tillhandahålls utan service nivå avtal och vi rekommenderar inte att du använder den för produktions arbets belastningar. Vissa funktioner kanske inte stöds eller kan vara begränsade. 
> Mer information finns i [Kompletterande villkor för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Du kan konfigurera tilldelnings typen för din personliga Skriv bords värd för att anpassa din Windows Virtual Desktop-miljö så att den bättre passar dina behov. I det här avsnittet visar vi hur du konfigurerar automatisk eller direkt tilldelning för dina användare.

>[!NOTE]
> Anvisningarna i den här artikeln gäller bara för personliga Skriv bords värd pooler, inte pooler med pooler, eftersom användare i pooler värdar inte har tilldelats till vissa värdar för fjärrskrivbordssessioner.

## <a name="prerequisites"></a>Krav

Den här artikeln förutsätter att du redan har laddat ned och installerat Windows Virtual Desktop PowerShell-modulen. Om du inte har gjort det följer du anvisningarna i [Konfigurera PowerShell-modulen](powershell-module.md).

## <a name="configure-automatic-assignment"></a>Konfigurera automatisk tilldelning

Automatisk tilldelning är standard tilldelnings typen för nya personliga Skriv bords värdar som skapats i din Windows Virtual Desktop-miljö. Automatisk tilldelning av användare kräver ingen speciell värd för sessionen.

Om du vill tilldela användare automatiskt måste du först tilldela dem till den personliga Skriv bords värds adresspoolen så att de kan se Skriv bordet i deras flöde. När en tilldelad användare startar Skriv bordet i sitt flöde ansluts de till en tillgänglig sessionsgräns om de inte redan har anslutit till den, vilket slutför tilldelnings processen.

Kör följande PowerShell-cmdlet för att konfigurera en värddator för att automatiskt tilldela användare till virtuella datorer:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Automatic
```

Kör följande PowerShell-cmdlet för att tilldela en användare till den personliga Skriv bords värd poolen:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="configure-direct-assignment"></a>Konfigurera direkt tilldelning

Till skillnad från automatisk tilldelning måste du tilldela användaren till både den personliga Skriv bords värd poolen och en speciell sessionsnyckel innan de kan ansluta till sitt personliga skriv bord när du använder direkt tilldelning. Om användaren bara är tilldelad till en adresspool utan tilldelning av en sessionsgräns, kommer de inte att kunna komma åt resurser.

Kör följande PowerShell-cmdlet för att konfigurera en adresspool så att den kräver direkt tilldelning av användare till sessionsbaserade värdar:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Direct
```

Kör följande PowerShell-cmdlet för att tilldela en användare till den personliga Skriv bords värd poolen:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Kör följande PowerShell-cmdlet om du vill tilldela en användare till en speciell värd för sessionen:

```powershell
Update-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>Nästa steg

Nu när du har konfigurerat den personliga Skriv bords typen kan du logga in på en Windows Virtual Desktop-klient för att testa den som en del av en användarsession. Följande två instruktioner visar hur du ansluter till en session med valfri klient:

- [Ansluta med Windows-skrivbordsklienten](connect-windows-7-and-10.md)
- [Ansluta med webbklienten](connect-web.md)
- [Ansluta med Android-klienten](connect-android.md)
- [Ansluta med iOS-klienten](connect-ios.md)
- [Ansluta med macOS-klienten](connect-macos.md)