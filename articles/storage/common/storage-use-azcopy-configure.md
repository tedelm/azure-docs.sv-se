---
title: Konfigurera, optimera och Felsök AzCopy med Azure Storage | Microsoft Docs
description: Konfigurera, optimera och Felsök AzCopy.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: c3ee0f335741c171c3a7ee1df3eea6dea9c4b728
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/27/2020
ms.locfileid: "82176166"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Konfigurera, optimera och felsöka AzCopy

AzCopy är ett kommando rads verktyg som du kan använda för att kopiera blobbar eller filer till eller från ett lagrings konto. Den här artikeln hjälper dig att utföra avancerade konfigurations åtgärder och hjälper dig att felsöka problem som kan uppstå när du använder AzCopy.

> [!NOTE]
> Om du letar efter innehåll som hjälper dig att komma igång med AzCopy kan du läsa följande artiklar:
> - [Kom igång med AzCopy](storage-use-azcopy-v10.md)
> - [Överföra data med AzCopy och Blob Storage](storage-use-azcopy-blobs.md)
> - [Överföra data med AzCopy och fil lagring](storage-use-azcopy-files.md)
> - [Överföra data med AzCopy och Amazon S3-buckets](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Konfigurera proxyinställningar

Om du vill konfigurera proxyinställningarna för AzCopy anger du `https_proxy` miljövariabeln. Om du kör AzCopy i Windows identifierar AzCopy automatiskt proxyinställningar, så du behöver inte använda den här inställningen i Windows. Om du väljer att använda den här inställningen i Windows kommer den att åsidosätta automatisk identifiering.

| Operativsystem | Kommando  |
|--------|-----------|
| **Windows** | Använd följande i en kommando tolk:`set https_proxy=<proxy IP>:<proxy port>`<br> I PowerShell använder du:`$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

För närvarande stöder AzCopy inte proxyservrar som kräver autentisering med NTLM eller Kerberos.

## <a name="optimize-performance"></a>Optimera prestanda

Du kan använda prestanda mätning och sedan använda kommandon och miljövariabler för att hitta en optimal kompromiss mellan prestanda och resursförbrukning.

Det här avsnittet hjälper dig att utföra följande optimerings aktiviteter:

> [!div class="checklist"]
> * Kör benchmark-tester
> * Optimera data flödet
> * Optimera minnes användning 
> * Optimera filsynkronisering

### <a name="run-benchmark-tests"></a>Kör benchmark-tester

Du kan köra ett prestandatest på vissa BLOB-behållare för att visa allmän prestanda statistik och för att identifiera Flask halsar i identiteter. 

Använd följande kommando för att köra ett prestandatest.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy bench 'https://<storage-account-name>.blob.core.windows.net/<container-name>'` |
| **Exempel** | `azcopy bench 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

> [!TIP]
> I det här exemplet omges Sök vägs argument med enkla citat tecken (' '). Använd enkla citat tecken i alla kommando gränssnitt utom Windows Command Shell (cmd. exe). Om du använder ett Windows Command Shell (cmd. exe), omger Sök vägs argument med dubbla citat tecken ("") i stället för enkla citat tecken ().

Det här kommandot kör prestanda mätning genom att överföra test data till ett angivet mål. Test data genereras i minnet, överförs till målet och tas sedan bort från målet när testet har slutförts. Du kan ange hur många filer som ska genereras och vilken storlek du vill att de ska vara med hjälp av valfria kommando parametrar.

Detaljerade referens dokument finns i [AzCopy bänk](storage-ref-azcopy-bench.md).

Om du vill visa detaljerad hjälp guide för det här `azcopy bench -h` kommandot skriver du och trycker sedan på RETUR-tangenten.

### <a name="optimize-throughput"></a>Optimera data flödet

Du kan använda `cap-mbps` flaggan i dina kommandon för att placera ett tak på data flödets data hastighet. Följande kommando återupptar till exempel ett jobb-och Caps-genomflöde till `10` megabyte (MB) per sekund. 

```azcopy
azcopy jobs resume <job-id> --cap-mbps 10
```

Data flödet kan minska vid överföring av små filer. Du kan öka data flödet genom att ställa in `AZCOPY_CONCURRENCY_VALUE` miljövariabeln. Den här variabeln anger antalet samtidiga begär Anden som kan utföras.  

Om datorn har färre än 5 processorer anges värdet för den här variabeln till `32`. Annars är standardvärdet lika med 16 multiplicerat med antalet processorer. Det maximala standardvärdet för den här `3000`variabeln är, men du kan manuellt ange det här värdet högre eller lägre. 

| Operativsystem | Kommando  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Använd `azcopy env` för att kontrol lera det aktuella värdet för den här variabeln. Om värdet är tomt kan du läsa vilket värde som används genom att titta i början av en AzCopy logg fil. Det valda värdet, och orsaken till det valdes, rapporteras där.

Innan du anger den här variabeln rekommenderar vi att du kör ett benchmark-test. Test processen för benchmark rapporterar det rekommenderade samtidiga värdet. Om ditt nätverks villkor och dina nytto laster varierar kan du ange den här variabeln `AUTO` till ordet i stället för ett visst tal. Detta gör att AzCopy alltid kör samma automatiska justerings process som används i benchmark-tester.

### <a name="optimize-memory-use"></a>Optimera minnes användning

`AZCOPY_BUFFER_GB` Ange miljövariabeln för att ange den maximala mängden system minne som du vill att AzCopy ska använda när du laddar ned och laddar upp filer.
Express detta värde i gigabyte (GB).

| Operativsystem | Kommando  |
|--------|-----------|
| **Windows** | `set AZCOPY_BUFFER_GB=<value>` |
| **Linux** | `export AZCOPY_BUFFER_GB=<value>` |
| **MacOS** | `export AZCOPY_BUFFER_GB=<value>` |

### <a name="optimize-file-synchronization"></a>Optimera filsynkronisering

[Sync](storage-ref-azcopy-sync.md) -kommandot identifierar alla filer vid målet och jämför sedan fil namn och senast ändrade tidsstämplar innan synkroniseringen startades. Om du har ett stort antal filer kan du förbättra prestandan genom att ta bort klient bearbetningen. 

För att åstadkomma detta använder du i stället [AzCopy Copy](storage-ref-azcopy-copy.md) -kommandot och anger `--overwrite` flaggan till `ifSourceNewer`. AzCopy kommer att jämföra filer när de kopieras utan att utföra några startgenomsökningar och jämförelser. Detta ger en prestanda gräns i fall där det finns ett stort antal filer att jämföra.

[AzCopy Copy](storage-ref-azcopy-copy.md) -kommandot tar inte bort filer från målet, så om du vill ta bort filer på målet när de inte längre finns på källan använder du kommandot [AzCopy Sync](storage-ref-azcopy-sync.md) med `--delete-destination` flaggan inställd på värdet `true` eller. `prompt` 

## <a name="troubleshoot-issues"></a>Felsöka problem

AzCopy skapar logg-och plan-filer för varje jobb. Du kan använda loggarna för att undersöka och felsöka eventuella problem. 

Loggarna innehåller status för felen (`UPLOADFAILED`, `COPYFAILED`, och `DOWNLOADFAILED`), den fullständiga sökvägen och orsaken till problemet.

Som standard finns logg-och plan-filerna i katalogen på `%USERPROFILE%\.azcopy` Windows eller `$HOME$\.azcopy` i en katalog på Mac och Linux, men du kan ändra platsen om du vill.

Det relevanta felet är inte nödvändigt vis det första fel som visas i filen. För fel som nätverks fel, timeout-fel och Server upptaget, kommer AzCopy att försöka igen till 20 gånger, och normalt lyckas processen igen.  Det första fel du ser kan vara något ofarligt som har gjorts om.  Så i stället för att titta på det första felet i filen söker du efter de fel som finns `UPLOADFAILED`nära `COPYFAILED`, eller `DOWNLOADFAILED`. 

> [!IMPORTANT]
> När du skickar en begäran till Microsoft Support (eller fel sökning av problemet som berör tredje part) delar du den avvisade versionen av kommandot som du vill köra. Detta säkerställer att SAS inte delas av misstag med vem. Du kan hitta den avvisade versionen i början av logg filen.

### <a name="review-the-logs-for-errors"></a>Granska loggarna för fel

Följande kommando får alla fel med `UPLOADFAILED` status från `04dc9ca9-158f-7945-5933-564021086c79` loggen:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Visa och återuppta jobb

Varje överförings åtgärd kommer att skapa ett AzCopy-jobb. Använd följande kommando för att visa jobb historiken:

```
azcopy jobs list
```

Om du vill visa jobb statistiken använder du följande kommando:

```
azcopy jobs show <job-id>
```

Använd följande kommando för att filtrera överföringarna efter status:

```
azcopy jobs show <job-id> --with-status=Failed
```

Använd följande kommando för att återuppta ett misslyckat/avbrutet jobb. Det här kommandot använder sitt ID tillsammans med SAS-token eftersom det inte är beständigt av säkerhets skäl:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

> [!TIP]
> Omge Sök vägs argument som SAS-token med enkla citat tecken (). Använd enkla citat tecken i alla kommando gränssnitt utom Windows Command Shell (cmd. exe). Om du använder ett Windows Command Shell (cmd. exe), omger Sök vägs argument med dubbla citat tecken ("") i stället för enkla citat tecken ().

När du återupptar ett jobb tittar AzCopy på jobb Plans filen. Plan filen visar en lista över alla filer som identifierades för bearbetning när jobbet först skapades. När du återupptar ett jobb kommer AzCopy att försöka överföra alla filer som anges i den plan fil som inte redan har överförts.

## <a name="change-the-location-of-the-plan-and-log-files"></a>Ändra placeringen av plan-och loggfilerna

Som standard finns plan-och loggfiler i `%USERPROFILE%\.azcopy` katalogen på Windows eller i `$HOME$\.azcopy` katalogen på Mac och Linux. Du kan ändra den här platsen.

### <a name="change-the-location-of-plan-files"></a>Ändra placeringen av plan filer

Använd något av dessa kommandon.

| Operativsystem | Kommando  |
|--------|-----------|
| **Windows** | `set AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **Linux** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |

Använd `azcopy env` för att kontrol lera det aktuella värdet för den här variabeln. Om värdet är tomt skrivs planera filer till standard platsen.

### <a name="change-the-location-of-log-files"></a>Ändra platsen för loggfiler

Använd något av dessa kommandon.

| Operativsystem | Kommando  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Använd `azcopy env` för att kontrol lera det aktuella värdet för den här variabeln. Om värdet är tomt skrivs loggar till standard platsen.

## <a name="change-the-default-log-level"></a>Ändra standard logg nivån

Som standard är logg nivån för AzCopy inställd på `INFO`. Om du vill minska loggens utförlighet för att spara disk utrymme skriver du över den här inställningen med hjälp av ``--log-level`` alternativet. 

Tillgängliga logg nivåer är: `NONE`, `DEBUG`, `INFO` `WARNING` `ERROR` `PANIC`,,, och `FATAL`.

## <a name="remove-plan-and-log-files"></a>Ta bort plan-och loggfiler

Om du vill ta bort alla plan-och loggfiler från den lokala datorn för att spara disk utrymme, `azcopy jobs clean` använder du kommandot.

Om du vill ta bort plan-och loggfilerna som är associerade med `azcopy jobs rm <job-id>`endast ett jobb använder du. Ersätt `<job-id>` plats hållaren i det här exemplet med jobb-ID: t för jobbet.


