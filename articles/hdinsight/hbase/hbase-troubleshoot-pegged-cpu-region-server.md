---
title: Peggad processor i Apache HBase-kluster – Azure HDInsight
description: Felsöka Peggad CPU på region server i Apache HBase-kluster i Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 16c994029e91d743f1c2a7e2eab51eb86fc378e8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "75887316"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>Scenario: Peggad processor på region server i Apache HBase-kluster i Azure HDInsight

Den här artikeln beskriver fel söknings steg och möjliga lösningar för problem med att interagera med Azure HDInsight-kluster.

## <a name="issue"></a>Problem

Apache HBase region Server process börjar med nära 200% processor, vilket leder till att aviseringar utlöses på HBase Master process och kluster inte fungerar med full kapacitet.

## <a name="cause"></a>Orsak

Om du kör HBase Cluster v 3.4 kan du ha träffat en möjlig bugg som orsakas av en uppgradering av JDK till version 1.7.0 _151. Det symptom som vi ser är region Server processen börjar ta upp till 200% processor (för att verifiera körningen `top` av kommandot. om det finns en process som håller nära 200% CPU får du ett PID och bekräftar att det är en region Server `ps -aux | grep` process genom att köra).

## <a name="resolution"></a>Lösning

1. Installera JDK 1,8 på alla noder i klustret enligt nedan:

    * Kör skript åtgärden `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh`. Se till att välja alternativet som ska köras på alla noder.

    * Alternativt kan du logga in på varje enskild nod och köra kommandot `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk`.

1. Gå till Ambari UI – `https://<clusterdnsname>.azurehdinsight.net`.

1. Navigera till **HBase->configs – >Advanced->Avancerat** `hbase-env configs` och ändra variabeln `JAVA_HOME` till `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`. Spara konfigurations ändringen.

1. [Valfritt men rekommenderas] [Rensa alla tabeller i klustret](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

1. Från Ambari UI igen startar du om alla HBase-tjänster som behöver startas om.

1. Beroende på data i klustret kan det ta några minuter upp till en timme innan klustret når stabilt tillstånd. Hur du bekräftar att klustret når stabilt tillstånd är genom att antingen kontrol lera HMaster-ANVÄNDARGRÄNSSNITTET (alla region servrar ska vara aktiva) från Ambari (uppdatera) eller från huvudnoden kör HBase Shell och sedan köra status kommando.

Kontrol lera att uppgraderingen lyckades genom att kontrol lera att de relevanta HBase-processerna har startats med rätt Java-version – för instans för region Server kontrol lera som:

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>Nästa steg

Om du inte ser problemet eller inte kan lösa problemet kan du gå till någon av följande kanaler för mer support:

* Få svar från Azure-experter via [Azure community support](https://azure.microsoft.com/support/community/).

* Anslut till [@AzureSupport](https://twitter.com/azuresupport) – det officiella Microsoft Azure kontot för att förbättra kund upplevelsen genom att ansluta Azure-communityn till rätt resurser: svar, support och experter.

* Om du behöver mer hjälp kan du skicka en support förfrågan från [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Välj **stöd** på Meny raden eller öppna **Hjälp + Support** Hub. Mer detaljerad information finns [i så här skapar du en support förfrågan för Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Åtkomst till prenumerations hantering och fakturerings support ingår i din Microsoft Azure prenumeration och teknisk support tillhandahålls via ett av support avtalen för [Azure](https://azure.microsoft.com/support/plans/).
