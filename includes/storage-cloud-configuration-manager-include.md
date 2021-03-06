---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 5102866cdda51ef545fd95b32946cb17c6e40a3c
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "72038164"
---
[Microsoft Azure-konfigurationshanterarens bibliotek för .NET](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/) erbjuder en klass för parsning av en anslutningssträng från en konfigurationsfil. [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) -klassen parsar konfigurations inställningar. Den tolkar inställningar för klient program som körs på Skriv bordet, på en mobil enhet, i en virtuell Azure-dator eller i en Azure-moln tjänst.

Lägg till följande `CloudConfigurationManager` `using` direktiv för att referera till paketet:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage;
```

Här följer ett exempel som visar hur du kan hämta en anslutningssträng från en konfigurationsfil:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Du måste inte använda Azure Configuration Manager. Du kan också använda ett API som .NET Framework [ConfigurationManager-klass](/dotnet/api/system.configuration.configurationmanager).
