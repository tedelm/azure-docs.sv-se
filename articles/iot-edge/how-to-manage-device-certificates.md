---
title: Hantera enhets certifikat – Azure IoT Edge | Microsoft Docs
description: Skapa test certifikat, installera och hantera dem på en Azure IoT Edge enhet för att förbereda för produktions distribution.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c18c3d560adb3c3cae54bda808ee5842c260fd6b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79539213"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Hantera certifikat på en IoT Edge enhet

Alla IoT Edge enheter använder certifikat för att skapa säkra anslutningar mellan körningen och alla moduler som körs på enheten. IoT Edge enheter som fungerar som gateways använder samma certifikat för att ansluta till deras underordnade enheter också.

## <a name="install-production-certificates"></a>Installera produktionscertifikat

När du först installerar IoT Edge och etablerar enheten, konfigureras enheten med tillfälliga certifikat så att du kan testa tjänsten.
Dessa tillfälliga certifikat upphör att gälla om 90 dagar eller kan återställas genom att starta om datorn.
När du är redo att flytta dina enheter till ett produktions scenario, eller om du vill skapa ett Gateway-scenario, måste du ange dina egna certifikat.
Den här artikeln visar stegen för att installera certifikat på dina IoT Edge enheter.

Mer information om de olika typerna av certifikat och deras roller i ett IoT Edge scenario finns i [förstå hur Azure IoT Edge använder certifikat](iot-edge-certs.md).

>[!NOTE]
>Termen "rot certifikat utfärdare" som används i den här artikeln refererar till det offentliga utfärdade certifikat kedjan för din IoT-lösning. Du behöver inte använda certifikat roten för en insyndikerad certifikat utfärdare eller roten för organisationens certifikat utfärdare. I många fall är det faktiskt ett offentligt certifikat för certifikat utfärdare.

### <a name="prerequisites"></a>Krav

* En IoT Edge enhet som körs antingen på [Windows](how-to-install-iot-edge-windows.md) eller [Linux](how-to-install-iot-edge-linux.md).
* Ha ett certifikat från en rot certifikat utfärdare (CA), antingen självsignerat eller köpt från en betrodd kommersiell certifikat utfärdare som Baltimore, VeriSign, DigiCert eller GlobalSign.

Om du inte har en rot certifikat utfärdare än, men vill testa IoT Edge funktioner som kräver produktions certifikat (till exempel Gateway-scenarier) kan du [skapa demonstrations certifikat för att testa IoT Edge enhets funktioner](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Skapa produktions certifikat

Du bör använda din egen certifikat utfärdare för att skapa följande filer:

* Rotcertifikatutfärdare
* Enhetens CA-certifikat
* Privat nyckel för enhets certifikat utfärdare

I den här artikeln refererar vi till som *rot certifikat utfärdare* är inte den översta certifikat utfärdaren för en organisation. Det är den översta certifikat utfärdaren för IoT Edge scenariot, som IoT Edge Hub-modulen, användarattribut och eventuella underordnade enheter använder för att upprätta förtroende mellan varandra.

Om du vill se ett exempel på dessa certifikat granskar du skripten som skapar demo certifikat i [Hantera test CA-certifikat för exempel och självstudier](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

### <a name="install-certificates-on-the-device"></a>Installera certifikat på enheten

Installera certifikat kedjan på den IoT Edge enheten och konfigurera IoT Edge runtime så att den refererar till de nya certifikaten.

Om du till exempel använde exempel skripten för att [skapa demo certifikat](how-to-create-test-certificates.md), kopierar du följande filer till din IoT-Edge-enhet:

* Enhetens CA-certifikat:`<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Privat nyckel för enhets certifikat utfärdare:`<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Rot certifikat utfärdare:`<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Kopiera de tre certifikat-och nyckelfilerna till din IoT Edge-enhet.

   Du kan använda en tjänst som [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) eller en funktion som [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) för att flytta certifikatfiler.  Om du har genererat certifikaten på själva enheten för IoT Edge kan du hoppa över det här steget och använda sökvägen till arbets katalogen.

1. Öppna konfigurations filen för IoT Edge Security daemon.

   * Aktivitets`C:\ProgramData\iotedge\config.yaml`
   * Linux`/etc/iotedge/config.yaml`

1. Ange **certifikat** egenskaperna i filen config. yaml till den fullständiga sökvägen till certifikatet och nyckelfilen på den IoT Edge enheten. Ta bort `#` tecknen innan du tar bort dem från certifikat egenskaperna för att ta bort kommentarer till de fyra raderna. Se till att det inte finns några föregående blank steg i raden **certifikat:** rad och att kapslade objekt är indragna med två blank steg. Ett exempel:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "c:\\<path>\\device-ca.cert.pem"
        device_ca_pk: "c:\\<path>\\device-ca.key.pem"
        trusted_ca_certs: "c:\\<path>\\root-ca.root.ca.cert.pem"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "<path>/device-ca.cert.pem"
        device_ca_pk: "<path>/device-ca.key.pem"
        trusted_ca_certs: "<path>/root-ca.root.ca.cert.pem"
      ```

1. På Linux-enheter ser du till att användar **iotedge** har Läs behörighet för den katalog som innehåller certifikaten.

1. Om du har använt andra certifikat för IoT Edge på enheten tidigare tar du bort filerna i följande två kataloger innan du startar eller startar om IoT Edge:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` och`C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` och`/var/lib/iotedge/hsm/cert_keys`

## <a name="customize-certificate-lifetime"></a>Anpassa certifikatets livstid

IoT Edge automatiskt genererar certifikat på enheten i flera fall, inklusive:

* Om du inte anger dina egna produktions certifikat när du installerar och etablerar IoT Edge, genererar IoT Edge Security Manager automatiskt ett **ca-certifikat för enhet**. Detta självsignerade certifikat är endast avsett för utvecklings-och testnings scenarier, inte produktion. Det här certifikatet upphör att gälla efter 90 dagar.
* IoT Edge Security Manager genererar också ett **ca-certifikat för arbets belastning** signerat av ENHETens CA-certifikat

Mer information om funktionen för de olika certifikaten på en IoT Edge-enhet finns i [förstå hur Azure IoT Edge använder certifikat](iot-edge-certs.md).

För dessa två automatiskt genererade certifikat har du möjlighet att ställa in **auto_generated_ca_lifetime_days** -flaggan i config. yaml för att konfigurera antalet dagar för certifikatens livs längd.

>[!NOTE]
>Det finns ett tredje automatiskt genererat certifikat som IoT Edge Security Manager skapar, **IoT Edge Hub-servercertifikat**. Det här certifikatet har alltid en 90 dag, men förnyas automatiskt innan det upphör att gälla. **Auto_generated_ca_lifetime_days** -värdet påverkar inte det här certifikatet.

Om du vill konfigurera certifikatets giltighets tid till något annat än standardvärdet 90 dagar lägger du till värdet i dagar i avsnittet **certifikat** i filen config. yaml.

```yaml
certificates:
  device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
  device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
  trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
  auto_generated_ca_lifetime_days: <value>
```

Om du har angett dina egna enhets certifikat för certifikat UTFÄRDAre, gäller det här värdet fortfarande för arbets Belastningens CA-certifikat, förutsatt att det angivna livs längd svärdet är kortare än livs längden för enhetens CA-certifikat.

När du har angett flaggan i filen config. yaml utför du följande steg:

1. Ta bort innehållet i `hsm` mappen.

   Windows: `C:\ProgramData\iotedge\hsm\certs and C:\ProgramData\iotedge\hsm\cert_keys` Linux:`/var/lib/iotedge/hsm/certs and /var/lib/iotedge/hsm/cert_keys`

1. Starta om tjänsten IoT Edge.

   Windows:

   ```powershell
   Restart-Service iotedge
   ```

   Linux:

   ```bash
   sudo systemctl restart iotedge
   ```

1. Bekräfta livs längds inställningen.

   Windows:

   ```powershell
   iotedge check --verbose
   ```

   Linux:

   ```bash
   sudo iotedge check --verbose
   ```

   Kontrol lera utdata från **produktions beredskap: certifikat** kontroll, som visar antalet dagar tills de automatiskt genererade ENHETens CA-certifikat upphör att gälla.

## <a name="next-steps"></a>Nästa steg

Att installera certifikat på en IoT Edge enhet är ett nödvändigt steg innan du distribuerar din lösning i produktionen. Lär dig mer om hur du [förbereder för att distribuera IoT Edge-lösningen i produktionen](production-checklist.md).
