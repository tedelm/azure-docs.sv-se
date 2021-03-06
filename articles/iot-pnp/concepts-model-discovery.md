---
title: Implementera identifiering av IoT Plug and Play Preview-modell | Microsoft Docs
description: Som en lösnings utvecklare kan du läsa mer om hur du kan implementera IoT Plug and Play modell identifiering i din lösning.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 74eb38269a3c7fbdc6d95554a8a8cef14eb0b787
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81770461"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>Implementera IoT Plug and Play för hands versions modell identifiering i en IoT-lösning

I den här artikeln beskrivs hur, som en lösnings utvecklare, kan du implementera IoT Plug and Play Preview-modell identifiering i en IoT-lösning.  Identifiering av IoT Plug and Play-modellen är hur IoT Plug and Play-enheter identifierar stöd modeller och gränssnitt och hur en IoT-lösning hämtar dessa kapacitets modeller och gränssnitt.

Det finns två breda kategorier av IoT-lösningar: syftes inbyggda lösningar som fungerar med en känd uppsättning IoT Plug and Play-enheter och modell drivna lösningar som fungerar med alla IoT Plug and Play-enheter.

Den här artikeln beskriver hur du implementerar modell identifiering i båda typerna av lösning.

## <a name="model-discovery"></a>Modellidentifiering

När en IoT Plug and Play-enhet först ansluter till din IoT-hubb skickar den ett telemetri om modell information. Det här meddelandet innehåller ID: n för de gränssnitt som enheten implementerar. För att din lösning ska fungera med enheten måste den matcha dessa ID: n och hämta definitionerna för varje gränssnitt.

Här följer de steg som en IoT Plug and Play-enhet tar när den använder enhets etablerings tjänsten (DPS) för att ansluta till en hubb:

1. När enheten är aktive rad ansluter den till den globala slut punkten för DPS och autentiserar med hjälp av en av de tillåtna metoderna.
1. DPS autentiserar sedan enheten och letar upp regeln som talar om för den vilken IoT-hubb som enheten ska tilldelas. DPS registrerar sedan enheten med den hubben.
1. DPS returnerar en IoT Hub anslutnings sträng till enheten.
1. Enheten skickar sedan ett meddelande om identifierings telemetri till din IoT Hub. Meddelandet identifiering av telemetri innehåller ID: n för de gränssnitt som enheten implementerar.
1. IoT Plug and Play-enheten är nu redo att fungera med en lösning som använder IoT Hub.

Om enheten ansluter direkt till din IoT-hubb ansluter den med hjälp av en anslutnings sträng som är inbäddad i enhets koden. Enheten skickar sedan ett meddelande om identifierings telemetri till din IoT Hub.

I [ModelInformation](concepts-common-interfaces.md) -gränssnittet kan du läsa mer om meddelandet om modell information.

### <a name="purpose-built-iot-solutions"></a>Syfte – inbyggda IoT-lösningar

En syfte – konstruerad IoT-lösning fungerar med en känd uppsättning IoT Plug and Play enhets kapacitets modeller och gränssnitt.

Du har kapacitets modellen och gränssnitten för de enheter som ska ansluta till din lösning i förväg. Använd följande steg för att förbereda din lösning:

1. Lagra gränssnitts-JSON-filerna i ett [modell lager](./howto-manage-models.md) där din lösning kan läsa dem.
1. Skriv logik i din IoT-lösning baserat på de förväntade IoT-Plug and Play kapacitets modeller och-gränssnitt.
1. Prenumerera på meddelanden från IoT Hub som din lösning använder.

Följ dessa steg när du får ett meddelande om en ny enhets anslutning:

1. Läs meddelandet om identifiering av telemetri för att hämta ID: n för kapacitets modellen och gränssnitt som implementeras av enheten.
1. Jämför ID för kapacitets modellen mot ID: n för de kapacitets modeller som du sparade i förväg.
1. Nu vet du vilken typ av enhet som är ansluten. Använd den logik du skrev tidigare för att göra det möjligt för användare att interagera med enheten på rätt sätt.

### <a name="model-driven-solutions"></a>Modell drivna lösningar

En modell driven IoT-lösning kan fungera med alla IoT Plug and Play-enheter. Att skapa en modell driven IoT-lösning är mer komplex, men fördelen är att din lösning fungerar med alla enheter som lagts till i framtiden.

Om du vill bygga en modell driven IoT-lösning måste du bygga logik mot IoT Plug and Play-gränssnittets primitiver: telemetri, egenskaper och kommandon. Din IoT-lösnings logik representerar en enhet genom att kombinera flera telemetri-, egenskaps-och kommando funktioner.

Lösningen måste också prenumerera på meddelanden från IoT-hubben som används.

Följ dessa steg när din lösning tar emot ett meddelande för en ny enhets anslutning:

1. Läs meddelandet om identifiering av telemetri för att hämta ID: n för kapacitets modellen och gränssnitt som implementeras av enheten.
1. För varje ID läser du den fullständiga JSON-filen för att hitta enhetens funktioner.
1. Kontrol lera om varje gränssnitt finns i alla cacheminnen som du har skapat för att lagra de JSON-filer som hämtades tidigare av din lösning.
1. Kontrol lera sedan om ett gränssnitt med det ID: t finns i den offentliga modellens lagrings plats. Mer information finns i [lagrings plats för offentlig modell](howto-manage-models.md).
1. Om gränssnittet inte finns i den offentliga modellens lagrings plats, kan du försöka att söka efter det i alla företags modell databaser som är kända för din lösning. Du behöver en anslutnings sträng för att få åtkomst till en företags modell lagrings plats. Mer information finns i [företags modellens lagrings plats](howto-manage-models.md).
1. Om du inte kan hitta alla gränssnitt i den offentliga modellens lagrings plats, eller i en företags modell databas, kan du kontrol lera om enheten kan tillhandahålla gränssnitts definitionen. En enhet kan implementera standard [ModelDefinition](concepts-common-interfaces.md) -gränssnittet för att publicera information om hur du hämtar gränssnitts filer med ett kommando.
1. Om du har hittat JSON-filer för varje gränssnitt som implementerats av enheten kan du räkna upp enhetens funktioner. Använd den logik du skrev tidigare för att göra det möjligt för användare att interagera med enheten.
1. Du kan när som helst anropa det digitala dubbla API: et för att hämta kapacitets modell-ID: t och gränssnitts-ID: n för enheten.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig om modell identifiering av en IoT-lösning kan du läsa mer om [Azure IoT-plattformen](overview-iot-plug-and-play.md) för att utnyttja andra funktioner för din lösning.
