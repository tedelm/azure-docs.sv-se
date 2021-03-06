---
title: Distribuera Windows 7 virtuell dator Windows Virtual Desktop – Azure
description: Så här konfigurerar och distribuerar du en virtuell Windows 7-dator på Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0cb5b2ee8b8391dc4fcb78cc1d3bd212c44f1803
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615414"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Distribuera en virtuell Windows 7-dator på Windows Virtual Desktop

>[!IMPORTANT]
>Det här innehållet gäller för hösten 2019-versionen som inte stöder Azure Resource Manager virtuella Windows Desktop-objekt.

Processen för att distribuera en virtuell Windows 7-dator (VM) på virtuella Windows-datorer skiljer sig något från för virtuella datorer som kör senare versioner av Windows. I den här guiden får du lära dig hur du distribuerar Windows 7.

## <a name="prerequisites"></a>Krav

Innan du börjar följer du anvisningarna i [skapa en adresspool med PowerShell](create-host-pools-powershell-2019.md) för att skapa en adresspool. Efter det följer du instruktionerna i [skapa lagringspooler på Azure Marketplace](create-host-pools-azure-marketplace-2019.md#optional-assign-additional-users-to-the-desktop-application-group) för att tilldela en eller flera användare till program gruppen Skriv bord.

## <a name="configure-a-windows-7-virtual-machine"></a>Konfigurera en virtuell Windows 7-dator

När du har genomfört kraven är du redo att konfigurera din virtuella Windows 7-dator för distribution på Windows Virtual Desktop.

Konfigurera en virtuell Windows 7-dator på Windows Virtual Desktop:

1. Logga in på Azure Portal och Sök antingen efter Windows 7 Enterprise-avbildningen eller ladda upp din egen anpassade Windows 7 Enterprise-avbildning (x64).  
2. Distribuera en eller flera virtuella datorer med Windows 7 Enterprise som värd operativ system. Se till att de virtuella datorerna tillåter Remote Desktop Protocol (RDP) (TCP/3389-porten).
3. Anslut till Windows 7 Enterprise-värden med hjälp av RDP och autentisera med de autentiseringsuppgifter som du definierade när du konfigurerade distributionen. 
4. Lägg till det konto som du använde när du anslöt till värden med RDP till gruppen "fjärr skrivbords användare". Om du inte gör det kanske du inte kan ansluta till den virtuella datorn när du har anslutit den till din Active Directory-domän.
5. Gå till Windows Update på den virtuella datorn.
6. Installera alla Windows-uppdateringar i den viktiga kategorin.
7. Installera alla Windows-uppdateringar i den valfria kategorin (exklusive språk paket). Detta installerar Remote Desktop Protocol 8,0-uppdateringen ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35387)) som du behöver för att slutföra de här instruktionerna.
8. Öppna redigerare för lokalt grupprincipobjekt och navigera till **dator konfiguration** > **administrativa mallar** > **Windows-komponenter** > **Fjärrskrivbordstjänster** > **fjärrskrivbordssessionen** > för**fjärrskrivbordssessioner.**
9. Aktivera Remote Desktop Protocol 8,0-principen.
10. Anslut den här virtuella datorn till din Active Directory-domän.
11. Starta om den virtuella datorn genom att köra följande kommando:
    
     ```cmd
     shutdown /r /t 0
     ```
    
12. Följ anvisningarna [här](/powershell/module/windowsvirtualdesktop/export-rdsregistrationinfo/) för att hämta en registrerings-token.
13. [Hämta Windows Virtual Desktop-agenten för Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
14. [Ladda ned Windows Virtual Desktop Agent Manager för Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
15. Öppna installations programmet för Windows Virtual Desktop agent och följ instruktionerna. När du uppmanas till det, ger du registrerings nyckeln som du skapade i steg 12.
16. Öppna installations programmet för Windows Virtual Desktop och följ anvisningarna.
17. Du kan också blockera TCP/3389-porten för att ta bort direkt Remote Desktop Protocol åtkomst till den virtuella datorn.

## <a name="next-steps"></a>Nästa steg

Din Windows-distribution för virtuella datorer är nu redo att användas. [Hämta den senaste versionen av Windows Virtual Desktop-klienten](https://aka.ms/wvd/clients/windows) för att komma igång.

En lista med kända problem och fel söknings anvisningar för Windows 7 på Windows Virtual Desktop finns i vår fel söknings artikel på [Felsöka Windows 7 Virtual Machines i Windows Virtual Desktop](troubleshoot-windows-7-vm.md).
