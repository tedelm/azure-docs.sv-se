---
title: Ange en kundspecifik nyckel på en begäran till Blob Storage med .NET-Azure Storage
description: Lär dig hur du anger en kundanged nyckel på en begäran till Blob Storage med hjälp av .NET.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ea8254f108aed9d40e6970a27409035b1e10ab41
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "74807008"
---
# <a name="specify-a-customer-provided-key-on-a-request-to-blob-storage-with-net"></a>Ange en kundspecifik nyckel på en begäran till Blob Storage med .NET

Klienter som begär förfrågningar mot Azure Blob Storage har möjlighet att tillhandahålla en krypterings nyckel på en enskild begäran. Inklusive krypterings nyckeln på begäran ger detaljerad kontroll över krypterings inställningarna för Blob Storage-åtgärder. Kundspecifika nycklar (för hands version) kan lagras i Azure Key Vault eller i en annan nyckel lagrings plats.

Den här artikeln visar hur du anger en kunds nyckel på en begäran med .NET.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Mer information om hur du autentiserar med klient biblioteket för Azure Identity från Azure Storage finns i avsnittet **autentisera med Azure Identity Library** i [ge åtkomst till blobbar och köer med Azure Active Directory och hanterade identiteter för Azure-resurser](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).

## <a name="example-use-a-customer-provided-key-to-upload-a-blob"></a>Exempel: Använd en kundanged nyckel för att ladda upp en BLOB

I följande exempel skapas en kundspecifik nyckel som använder den nyckeln för att ladda upp en blob. Koden laddar upp ett block och genomför sedan blockeringslistan för att skriva blobben till Azure Storage.

```csharp
async static Task UploadBlobWithClientKey(string accountName, string containerName,
    string blobName, Stream data, byte[] key)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri accountUri = new Uri("https://" + accountName + blobServiceEndpointSuffix);

    // Specify the customer-provided key on the options for the client.
    BlobClientOptions options = new BlobClientOptions()
    {
        CustomerProvidedKey = new CustomerProvidedKey(key)
    };

    // Create a client object for the Blob service, including options.
    BlobServiceClient serviceClient = new BlobServiceClient(accountUri, 
        new DefaultAzureCredential(), options);

    // Create a client object for the container.
    // The container client retains the credential and client options.
    BlobContainerClient containerClient = serviceClient.GetBlobContainerClient(containerName);

    // Create a new block blob client object.
    // The blob client retains the credential and client options.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload the data using the customer-provided key.
        await blobClient.UploadAsync(data);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="next-steps"></a>Nästa steg

- [Azure Storage kryptering för vilande data](../common/storage-service-encryption.md)
- [Ge åtkomst till blobbar och köer med Azure Active Directory och hanterade identiteter för Azure-resurser](../common/storage-auth-aad-msi.md)
