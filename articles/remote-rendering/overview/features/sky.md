---
title: Reflektioner av himmel
description: Beskriver hur du konfigurerar miljö kartor för himmelsblå-reflektioner
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: be3dc2b113cb21c2dfb54a29e7f426e0d925c6d9
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83759123"
---
# <a name="sky-reflections"></a>Reflektioner av himmel

I Azure Remote rendering används en himmel-textur för att lätta objekt realistiska. För förhöjda verklighets program bör den här texturen likna din verkliga miljö, för att göra objekt övertygande. I den här artikeln beskrivs hur du ändrar luft rummets struktur.

> [!NOTE]
> Den himmelsblå texturen kallas även för en *miljö karta*. Dessa villkor används utbytbart.

## <a name="object-lighting"></a>Objekt belysning

Med Azure Remote rendering används PBR ( *fysiskt baserad åter givning* ) för realistiska belysnings beräkningar. Även om du kan lägga till [ljusa källor](lights.md) i din scen, har den största effekten att använda en lämplig luft rummets struktur.

Bilderna nedan visar resultat av ljus olika ytor med en himmel-struktur:

| Grovhet  | 0                                        | 0.25                                          | 0,5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| Icke-metall  | ![Dielectric0](media/dielectric-0.png)   | ![GreenPointPark](media/dielectric-0.25.png)  | ![GreenPointPark](media/dielectric-0.5.png)  | ![GreenPointPark](media/dielectric-0.75.png)  | ![GreenPointPark](media/dielectric-1.png)  |
| BMR      | ![GreenPointPark](media/metallic-0.png)  | ![GreenPointPark](media/metallic-0.25.png)    | ![GreenPointPark](media/metallic-0.5.png)    | ![GreenPointPark](media/metallic-0.75.png)    | ![GreenPointPark](media/metallic-1.png)    |

Mer information om belysnings modellen finns i kapitlet om [material](../../concepts/materials.md) .

> [!IMPORTANT]
> Azure fjärrrendering använder endast den himmelsblå texturen för belysnings modeller. Den återger inte luft rummet som en bakgrund eftersom förhöjda verklighets program redan har en korrekt bakgrund – den verkliga världen.

## <a name="changing-the-sky-texture"></a>Ändra luft rummets struktur

Om du vill ändra miljö kartan behöver du bara [läsa in en struktur](../../concepts/textures.md) och ändra sessionens `SkyReflectionSettings` :

```cs
LoadTextureAsync _skyTextureLoad = null;
void ChangeEnvironmentMap(AzureSession session)
{
    _skyTextureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap));

    _skyTextureLoad.Completed += (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                try
                {
                    session.Actions.SkyReflectionSettings.SkyReflectionTexture = res.Result;
                }
                catch (RRException exception)
                {
                    System.Console.WriteLine($"Setting sky reflection failed: {exception.Message}");
                }
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
        };
}
```

```cpp
void ChangeEnvironmentMap(ApiHandle<AzureSession> session)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::CubeMap;
    params.TextureUrl = "builtin://VeniceSunset";
    ApiHandle<LoadTextureAsync> skyTextureLoad = *session->Actions()->LoadTextureFromSASAsync(params);

    skyTextureLoad->Completed([&](ApiHandle<LoadTextureAsync> res)
    {
        if (res->IsRanToCompletion())
        {
            ApiHandle<SkyReflectionSettings> settings = *session->Actions()->SkyReflectionSettings();
            settings->SkyReflectionTexture(*res->Result());
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}

```

Observera att `LoadTextureFromSASAsync` varianten används ovan eftersom en inbyggd textur har lästs in. Om du läser in från [länkade BLOB-lagringar](../../how-tos/create-an-account.md#link-storage-accounts)använder du `LoadTextureAsync` varianten.

## <a name="sky-texture-types"></a>Typer av luft rummets struktur

Du kan använda både *[cubemaps](https://en.wikipedia.org/wiki/Cube_mapping)* -och *2D-texturer* som miljö kartor.

Alla texturer måste vara i ett [textur format som stöds](../../concepts/textures.md#supported-texture-formats). Du behöver inte ange mipmaps för himmelsblå texturer.

### <a name="cube-environment-maps"></a>Kub miljö kartor

För referens är här en cubemap som inte är figursatt:

![En figursatt cubemap](media/Cubemap-example.png)

Använd `AzureSession.Actions.LoadTextureAsync` /  `LoadTextureFromSASAsync` med `TextureType.CubeMap` för att läsa in cubemap texturer.

### <a name="sphere-environment-maps"></a>Sfär miljö kartor

När du använder en 2D-struktur som en miljö karta måste bilden vara i [sfäriskt koordinat utrymme](https://en.wikipedia.org/wiki/Spherical_coordinate_system).

![En himmel-bild i sfäriska koordinater](media/spheremap-example.png)

Använd `AzureSession.Actions.LoadTextureAsync` med `TextureType.Texture2D` för att läsa in sfäriska miljö kartor.

## <a name="built-in-environment-maps"></a>Inbyggda miljö kartor

Azure Remote rendering innehåller några inbyggda miljö kartor som alltid är tillgängliga. Alla inbyggda miljö kartor är cubemaps.

|Identifierare                         | Beskrivning                                              | Exemplet                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Olika rand lampor, ljus inomhus bas belysning    | ![Shoppa](media/autoshop.png)
|builtin://BoilerRoom               | Ljus inomhus-inställning, flera fönster lampor      | ![BoilerRoom](media/boiler-room.png)
|builtin://ColorfulStudio           | Variera färg lampor i mellanliggande inlednings inställning  | ![ColorfulStudio](media/colorful-studio.png)
|builtin://Hangar                   | Måttligt ljus omgivnings ljus                     | ![SmallHangar](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Dim-inlednings inställning med ljust mörk kontrast              | ![IndustrialPipeAndValve](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Dagtid, ljust fönster Area ljust     | ![Lebombo](media/lebombo.png)
|builtin://SataraNight              | Mörkt natt himmel och mark med många omgivande lampor   | ![SataraNight](media/satara-night.png)
|builtin://SunnyVondelpark          | Ljus solljus och skugga kontrast                      | ![SunnyVondelpark](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Ta bort luft rummets ljus med måttlig belysning            | ![Syferfontein](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Måttligt varierande sol och skugga                         | ![TearsOfSteelBridge](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Kväll solnedgång tänd Dusk                    | ![VeniceSunset](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Ljusa, Lush och vita ljusa toner, nedtonad mark | ![WhippleCreekRegionalPark](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Dagtid med ljust omgivande underlag                 | ![WinterRiver](media/winter-river.png)
|builtin://DefaultSky               | Samma som TearsOfSteelBridge                               | ![DefaultSky](media/tears-of-steel-bridge.png)

## <a name="next-steps"></a>Nästa steg

* [Lampor](../../overview/features/lights.md)
* [Material](../../concepts/materials.md)
* [Bakgrunder](../../concepts/textures.md)
* [Kommando rads verktyget TexConv](../../resources/tools/tex-conv.md)
