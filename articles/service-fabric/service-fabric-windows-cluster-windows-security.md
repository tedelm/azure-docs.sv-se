---
title: Skydda ett kluster som körs på Windows med hjälp av Windows-säkerhet
description: Lär dig hur du konfigurerar nod-till-nod-och klient-till-nod-säkerhet i ett fristående kluster som körs på Windows med hjälp av Windows-säkerhet.
author: dkkapur
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 46be6acc1ef08770826a2e020c8930eba0787791
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "76774448"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Skydda ett fristående kluster i Windows med hjälp av Windows-säkerhet
För att förhindra obehörig åtkomst till ett Service Fabric-kluster måste du skydda klustret. Säkerhet är särskilt viktigt när klustret kör produktions arbets belastningar. Den här artikeln beskriver hur du konfigurerar säkerhet för nod-till-nod-och klient-till-nod med hjälp av Windows-säkerhet i *ClusterConfig. JSON* -filen.  Processen motsvarar steget Konfigurera säkerhets steg i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md). Mer information om hur Service Fabric använder Windows-säkerhet finns i [kluster säkerhets scenarier](service-fabric-cluster-security.md).

> [!NOTE]
> Du bör fundera över valet av nod-till-nod-säkerhet noggrant eftersom det inte finns någon kluster uppgradering från ett säkerhets val till en annan. Om du vill ändra säkerhets valet måste du återskapa det fullständiga klustret.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Konfigurera Windows-säkerhet med gMSA  
Konfigurations filen sample *ClusterConfig. gMSA. Windows. MultiMachine. JSON* som hämtats med [Microsoft. Azure. ServiceFabric. Windows Server\< . version>. zip](https://go.microsoft.com/fwlink/?LinkId=730690) fristående kluster paket innehåller en mall för att konfigurera Windows-säkerhet med [grupphanterat tjänst konto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Konfigurationsuppsättning** | **Beskrivning** |
| --- | --- |
| ClusterCredentialType |Ange till *Windows* om du vill aktivera Windows-säkerhet för kommunikation på nod-noden.  | 
| ServerCredentialType |Ange till *Windows* om du vill aktivera Windows-säkerhet för kommunikation på klient-noden. |
| WindowsIdentities |Innehåller kluster-och klient identiteter. |
| ClustergMSAIdentity |Konfigurerar säkerhet för nod-till-nod. Ett grupphanterat tjänst konto. |
| ClusterSPN |Registrerat SPN för gMSA-konto|
| ClientIdentities |Konfigurerar säkerhet på klient-till-nod. En matris med klient användar konton. |
| Identitet |Lägg till domän användaren, domän \ användar namn, för klient identiteten. |
| IsAdmin |Ange till true för att ange att domän användaren har administratörs åtkomst eller falskt för användar klient åtkomst. |

> [!NOTE]
> ClustergMSAIdentity-värdet måste vara i formatetmysfgmsa@mydomain"".

[Nod-till-nod-säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras genom att ange **ClustergMSAIdentity** när Service Fabric måste köras under gMSA. För att kunna bygga förtroende relationer mellan noder måste de göras medvetna om varandra. Detta kan åstadkommas på två olika sätt: Ange det grupphanterade tjänst kontot som innehåller alla noder i klustret eller ange domän dator gruppen som innehåller alla noder i klustret. Vi rekommenderar starkt att du använder [gMSA-metoden (Group Managed Service Account)](https://technet.microsoft.com/library/hh831782.aspx) , särskilt för större kluster (fler än 10 noder) eller för kluster som sannolikt kommer att växa eller krympa.  
Den här metoden kräver inte att en domän grupp skapas för vilken kluster administratörer har beviljats behörighet att lägga till och ta bort medlemmar. Dessa konton är också användbara för automatisk lösen ords hantering. Mer information finns i [komma igång med grupphanterade tjänst konton](https://technet.microsoft.com/library/jj128431.aspx).  
 
[Säkerhet för klient till nod](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**. För att upprätta förtroende mellan en klient och klustret måste du konfigurera klustret för att veta vilka klient identiteter som det kan lita på. Detta kan göras på två olika sätt: Ange de domän grupps användare som kan ansluta eller ange de domänanslutna användare som kan ansluta. Service Fabric stöder två olika åtkomst kontroll typer för klienter som är anslutna till ett Service Fabric-kluster: administratör och användare. Åtkomst kontroll ger kluster administratören möjlighet att begränsa åtkomsten till vissa typer av kluster åtgärder för olika grupper av användare, vilket gör klustret säkrare.  Administratörer har fullständig åtkomst till hanterings funktioner (inklusive Läs-och skriv funktioner). Användare har som standard endast Läs behörighet till hanterings funktioner (till exempel fråge funktioner) och möjligheten att lösa program och tjänster. Mer information om åtkomst kontroller finns i [rollbaserad åtkomst kontroll för Service Fabric klienter](service-fabric-cluster-security-roles.md).  
 
I följande exempel **säkerhets** avsnitt konfigureras Windows-säkerhet med gMSA och anger att datorerna i *ServiceFabric.clusterA.contoso.com* -gMSA är en del av klustret och att *CONTOSO\usera* har administratörs åtkomst till klienten:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Konfigurera Windows-säkerhet med en dator grupp  
Den här modellen är inaktuell. Rekommendationen är att använda gMSA enligt beskrivningen ovan. Konfigurations filen sample *ClusterConfig. Windows. MultiMachine. JSON* som hämtades med [Microsoft. Azure. ServiceFabric. Windows Server\< . version>. zip](https://go.microsoft.com/fwlink/?LinkId=730690) fristående kluster paket innehåller en mall för att konfigurera Windows-säkerhet.  Windows-säkerhet konfigureras i avsnittet **Egenskaper** : 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Konfigurationsuppsättning** | **Beskrivning** |
| --- | --- |
| ClusterCredentialType |Ange till *Windows* om du vill aktivera Windows-säkerhet för kommunikation på nod-noden.  |
| ServerCredentialType |Ange till *Windows* om du vill aktivera Windows-säkerhet för kommunikation på klient-noden. |
| WindowsIdentities |Innehåller kluster-och klient identiteter. |
| ClusterIdentity |Använd ett dator grupp namn, domain\machinegroup, om du vill konfigurera nod-till-nod-säkerhet. |
| ClientIdentities |Konfigurerar säkerhet på klient-till-nod. En matris med klient användar konton. |  
| Identitet |Lägg till domän användaren, domän \ användar namn, för klient identiteten. |  
| IsAdmin |Ange till true för att ange att domän användaren har administratörs åtkomst eller falskt för användar klient åtkomst. |  

[Nod-till-nod-säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras genom att konfigurera med **ClusterIdentity** om du vill använda en dator grupp i en Active Directory-domän. Mer information finns i [skapa en dator grupp i Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Klient-till-nod-säkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**. Om du vill upprätta förtroende mellan en klient och klustret måste du konfigurera klustret för att känna till de klient identiteter som klustret kan lita på. Du kan upprätta förtroende på två olika sätt:

- Ange de domän grupps användare som kan ansluta.
- Ange de domän-Node-användare som kan ansluta.

Service Fabric stöder två olika åtkomst kontroll typer för klienter som är anslutna till ett Service Fabric-kluster: administratör och användare. Med åtkomst kontroll kan kluster administratören begränsa åtkomsten till vissa typer av kluster åtgärder för olika grupper av användare, vilket gör klustret säkrare.  Administratörer har fullständig åtkomst till hanterings funktioner (inklusive Läs-och skriv funktioner). Användare har som standard endast Läs behörighet till hanterings funktioner (till exempel fråge funktioner) och möjligheten att lösa program och tjänster.  

I följande exempel **säkerhets** avsnitt konfigureras Windows-säkerhet, anger att datorerna i *ServiceFabric/clustera. contoso. com* ingår i klustret och anger att *CONTOSO\usera* har administratörs åtkomst till klienten:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric bör inte distribueras på en domänkontrollant. Se till att ClusterConfig. JSON inte innehåller IP-adressen för domänkontrollanten när du använder en dator grupp eller ett grupphanterat tjänst konto (gMSA).
>
>

## <a name="next-steps"></a>Nästa steg
När du har konfigurerat Windows-säkerhet i *ClusterConfig. JSON* -filen, återupptar du skapande processen för kluster i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).

Mer information om hur nod-till-nod-säkerhet, klient-till-nod-säkerhet och rollbaserad åtkomst kontroll finns i [kluster säkerhets scenarier](service-fabric-cluster-security.md).

Se [ansluta till ett säkert kluster](service-fabric-connect-to-secure-cluster.md) för exempel på att ansluta med PowerShell eller FabricClient.
