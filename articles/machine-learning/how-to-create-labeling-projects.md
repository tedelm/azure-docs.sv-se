---
title: Skapa ett data etiketts projekt
titleSuffix: Azure Machine Learning
description: Lär dig hur du skapar och kör etiketter för projekt för att tagga data för Machine Learning.  Verktygen omfattar ml-etikettering eller mänsklig i slingan för att hjälpa till med uppgiften.
author: sdgilley
ms.author: sgilley
ms.service: machine-learning
ms.topic: tutorial
ms.date: 04/09/2020
ms.openlocfilehash: 40c31d4dd4a6c675691f75d3717f7865d6b847f7
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171574"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Skapa ett projekt med data etiketter och exportera etiketter 

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Att märka Voluminous-data i Machine Learning-projekt är ofta ett problem. Projekt som har en dator vision komponent, till exempel bild klassificering eller objekt identifiering, kräver vanligt vis etiketter för tusentals avbildningar.
 
[Azure Machine Learning](https://ml.azure.com/) ger dig en central plats för att skapa, hantera och övervaka etikett projekt (offentlig för hands version). Använd den för att koordinera data, etiketter och team medlemmar för att effektivt hantera etikett uppgifter. Machine Learning stöder bild klassificering, antingen flera etiketter eller flera klasser och objekt identifiering med gränser rutor.

Azure Machine Learning spårar förloppet och underhåller kön med ofullständiga etikett uppgifter.

Du kan starta och stoppa projektet och övervaka etikettens förlopp. Du kan exportera märkta data i COCO-format eller som en Azure Machine Learning data uppsättning.

> [!Important]
> Det finns för närvarande stöd för att märka projekt med bild klassificering och objekt identifiering. Dessutom måste data avbildningarna vara tillgängliga i ett Azure Blob-datalager. (Om du inte har ett befintligt data lager kan du ladda upp bilder när projektet skapas.)

I den här artikeln får du lära dig att:

> [!div class="checklist"]
> * Skapa ett projekt
> * Ange projektets data och struktur
> * Köra och övervaka projektet
> * Exportera etiketterna


## <a name="prerequisites"></a>Förutsättningar


* De data som du vill märka, antingen i lokala filer eller i Azure Blob Storage.
* Den uppsättning etiketter som du vill använda.
* Anvisningarna för att märka.
* En Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du [skapa ett kostnadsfritt konto](https://aka.ms/AMLFree) innan du börjar.
* En Machine Learning-arbetsyta. Se [skapa en Azure Machine Learning-arbetsyta](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Skapa ett etikett projekt

Att märka projekt administreras från Azure Machine Learning. Du kan använda sidan **Märk projekt** för att hantera dina projekt.

Om dina data redan finns i Azure Blob Storage bör du göra det tillgängligt som ett data lager innan du skapar ett etikettande projekt. Ett exempel på hur du använder ett data lager finns i [Självstudier: skapa ditt första bild klassificerings projekt](tutorial-labeling.md).

Välj **Lägg till projekt**om du vill skapa ett projekt. Ge projektet ett lämpligt namn och välj **uppgifts typ för etiketter**.

:::image type="content" source="media/how-to-create-labeling-projects/labeling-creation-wizard.png" alt-text="Guiden skapa etikett för projekt":::

* Välj **bild klassificering flera klasser** för projekt när du bara vill använda en *enda klass* från en uppsättning klasser till en avbildning.
* Välj **bild klassificering med flera etiketter** för projekt om du vill tillämpa *en eller flera* etiketter från en uppsättning klasser i en bild. Till exempel kan ett foto av en hund vara märkt med både *hund* och *dagtid*.
* Välj **objekt identifiering (markerings ram)** för projekt när du vill tilldela en klass och en avgränsnings ruta till varje objekt i en bild.

Välj **Nästa** när du är redo att fortsätta.

## <a name="specify-the-data-to-label"></a>Ange de data som ska etiketteras

Om du redan har skapat en data uppsättning som innehåller dina data väljer du den från List rutan **Välj en befintlig data uppsättning** . Eller Välj **skapa en data uppsättning** för att använda ett befintligt Azure-datalager eller för att ladda upp lokala filer.


### <a name="create-a-dataset-from-an-azure-datastore"></a>Skapa en data uppsättning från ett Azure-datalager

I många fall är det bra att bara ladda upp lokala filer. Men [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) ger ett snabbare och mer robust sätt att överföra en stor mängd data. Vi rekommenderar Storage Explorer som standard sätt att flytta filer.

Så här skapar du en data uppsättning från data som du redan har lagrat i Azure Blob Storage:

1. Välj **skapa en data uppsättning**  >  **från data lagret**.
1. Tilldela ett **namn** till din data uppsättning.
1. Välj **fil** som **data uppsättnings typ**.  
1. Välj data lagret.
1. Om dina data finns i en undermapp i Blob Storage väljer du **Bläddra** för att välja sökvägen.
    * Lägg till "/* *" i sökvägen om du vill inkludera alla filer i undermappar för den valda sökvägen.
    * Lägg till "* */* . *" för att inkludera alla data i den aktuella behållaren och dess undermappar.
1. Ange en beskrivning för din data uppsättning.
1. Välj **Nästa**.
1. Bekräfta informationen. Välj **tillbaka** om du vill ändra inställningarna eller **skapa** för att skapa data uppsättningen.

> [!NOTE]
> De data du väljer läses in i projektet.  Att lägga till mer data i data lagret visas inte i det här projektet när projektet har skapats.  

### <a name="create-a-dataset-from-uploaded-data"></a>Skapa en data uppsättning från överförda data

Så här överför du dina data direkt:

1. Välj **skapa en data uppsättning**  >  **från lokala filer**.
1. Tilldela ett **namn** till din data uppsättning.
1. Välj "Arkiv" som **data uppsättnings typ**.
1. *Valfritt:* Välj **Avancerade inställningar** för att anpassa data lagret, behållaren och sökvägen till dina data.
1. Välj **Bläddra** för att välja de lokala filer som ska laddas upp.
1. Ange en beskrivning av din data uppsättning.
1. Välj **Nästa**.
1. Bekräfta informationen. Välj **tillbaka** om du vill ändra inställningarna eller **skapa** för att skapa data uppsättningen.

Data överförs till standard-BLOB-arkivet ("workspaceblobstore") på din Machine Learning-arbetsyta.

## <a name="specify-label-classes"></a>Ange etikett klasser

På sidan **etikett klasser** anger du uppsättningen klasser för att kategorisera dina data. Gör detta noggrant eftersom etiketternas precision och hastighet kommer att påverkas av deras möjlighet att välja bland klasserna. I stället för att bokstavera hela släktet och arterna för växter eller djur använder du exempelvis en fältkod eller förkorta släktet.

Ange en etikett per rad. Använd **+** knappen för att lägga till en ny rad. Om du har fler än 3 eller 4 etiketter men färre än 10 kanske du vill lägga till prefixet i namnen med siffror ("1:", "2:") så att etiketterna kan använda sig av siffer tangenterna för att påskynda sitt arbete.

## <a name="describe-the-labeling-task"></a>Beskriv etikettens uppgift

Det är viktigt att tydligt förklara etikettens uppgift. På sidan **etiketting-instruktioner** kan du lägga till en länk till en extern plats för etikett instruktioner eller ange instruktioner i redigerings rutan på sidan. Se till att instruktionerna är orienterade och lämpligt för mål gruppen. Tänk på följande frågor:

* Vilka etiketter ser de ut och hur kommer de att välja mellan dem? Finns det en referens text att referera till?
* Vad ska de göra om ingen etikett verkar vara lämplig?
* Vad ska de göra om flera etiketter förefaller lämpliga?
* Vilket konfidens tröskelvärde bör gälla för en etikett? Vill du ha deras "bästa gissning" om de inte är säkra?
* Vad ska de göra med delvis Occluded eller överlappande objekt av intresse?
* Vad ska de göra om ett objekt av intresse klipps av bildens kant?
* Vad ska de göra när de skickar en etikett om de tror att de gör ett misstag?

För avgränsnings rutor är viktiga frågor:

* Hur definieras avgränsnings rutan för den här uppgiften? Ska det vara helt på insidan av objektet eller ska det vara på utsidan? Bör den beskäras så tätt som möjligt, eller är vissa godkännanden godtagbara?
* Vilken nivå av vård och konsekvens förväntar du dig att etiketterna ska tillämpas i definitions rutor?
* Hur gör jag för att märka objektet som visas delvis i bilden? 
* Hur gör jag för att märka objektet som delvis täcks av andra objekt?

>[!NOTE]
> Observera att etiketterna kan välja de första 9 etiketterna genom att använda siffer nycklar 1-9.

## <a name="use-ml-assisted-labeling"></a>Använd ML-etikettering

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Med hjälp av sidan **ml-märkning** kan du utlösa automatiska maskin inlärnings modeller för att påskynda etiketten. I början av ditt projekt som du har märkt, sorteras bilderna i en slumpmässig ordning för att minska potentiell kompensation. Men eventuella förskjutningar som förekommer i data uppsättningen visas i den tränade modellen. Om till exempel 80% av dina avbildningar är av en enda klass kommer cirka 80% av de data som används för att träna modellen att vara av klassen. Den här utbildningen omfattar inte aktiv inlärning.

Välj *Enable ml-etikettering* och ange en GPU för att aktivera assisterad märkning, som består av två faser:
* Klustring
* För markering

Det exakta antalet etiketterade bilder som krävs för att kunna starta assisterad etikettering är inte ett fast nummer.  Detta kan variera markant från ett etikett projekt till ett annat. För vissa projekt är det ibland möjligt att se förmärknings-eller kluster aktiviteter efter att 300-bilder har märkts manuellt. ML-märkning använder en teknik som kallas *överförings Inlärning*, som använder en förtränad modell för att komma igång med inlärnings processen. Om din data uppsättnings klasser liknar dem i den förtränade modellen kan det finnas etiketter som är tillgängliga efter några hundra manuellt märkta bilder. Om din data uppsättning skiljer sig avsevärt från de data som används för att förträna modellen, kan det ta mycket längre tid.

Eftersom de slutliga etiketterna fortfarande är beroende av inmatade Labeler, kallas den här tekniken ibland *mänsklig i slingan* .

### <a name="clustering"></a>Klustring

När ett visst antal etiketter har skickats börjar Machine Learning-modellen för bild klassificering att gruppera liknande bilder.  Dessa liknande bilder presenteras för etiketter på samma skärm för att påskynda manuell taggning. Klustring är särskilt användbart när Labeler visar ett rutnät med 4, 6 eller 9 bilder. 

När en maskin inlärnings modell har tränats på dina manuellt märkta data, trunkeras modellen till det sista fullständigt anslutna lagret. Omärkta bilder skickas sedan genom den trunkerade modellen i en process som kallas "inbäddning" eller "funktionalisering". Detta bäddar in varje bild i ett högt dimensionellt utrymme som definieras av detta modell lager. Bilder som är närmsta grannar i utrymmet används för kluster uppgifter. 

Kluster fasen visas inte för objekt identifierings modeller.

### <a name="prelabeling"></a>För markering

När tillräckligt med bild etiketter har skickats används en klassificerings modell för att förutsäga bild taggar. Eller en objekt identifierings modell används för att förutsäga avgränsnings rutor. Labeler ser nu sidor som innehåller förväntade etiketter som redan finns på varje bild. För objekt identifiering visas även de förväntade rutorna. Uppgiften granskar sedan dessa förutsägelser och korrigerar eventuella felmärkta bilder innan sidan skickas.  

När en maskin inlärnings modell har tränats på dina manuellt märkta data, utvärderas modellen i en test uppsättning med manuellt märkta bilder för att fastställa dess exakthet på en rad olika konfidens trösklar. Den här utvärderings processen används för att fastställa ett konfidens tröskelvärde över vilket modellen är tillräckligt korrekt för att Visa före-etiketter. Modellen utvärderas sedan mot omärkta data. Bilder med förutsägelser mer tryggare än det här tröskelvärdet används för för märkning.

> [!NOTE]
> ML-märkta etiketter är **bara** tillgängligt i Enterprise Edition-arbetsytor.

## <a name="initialize-the-labeling-project"></a>Initiera ett etikettande projekt

När du har initierat projektet är vissa delar av projektet oföränderliga. Du kan inte ändra aktivitets typen eller data uppsättningen. Du *kan* ändra etiketter och URL: en för uppgifts beskrivningen. Granska inställningarna noggrant innan du skapar projektet. När du har skickat projektet kommer du tillbaka till start sidan för **data etiketter** som visar projektet som **initieras**. Den här sidan uppdateras inte automatiskt. Efter en paus uppdaterar du sidan manuellt för att se projektets status som **skapats**.

## <a name="run-and-monitor-the-project"></a>Köra och övervaka projektet

När du har initierat projektet börjar Azure att köra det. Välj projektet på huvud sidan för **data etiketter** för att gå till **projekt information**. Fliken **instrument panel** visar förloppet för etikett uppgiften.

På fliken **data** kan du se din data uppsättning och granska etiketterade data. Om du ser felaktigt märkta data markerar du den och väljer **avvisa**, vilket tar bort etiketterna och sätter tillbaka dem i den omärkta kön.

Om du vill pausa eller starta om projektet väljer du knappen **pausa** / **Start** . Du kan bara märka data när projektet körs.

Du kan märka data direkt från sidan **projekt information** genom att välja **etikett data**.

## <a name="add-new-label-class-to-a-project"></a>Lägg till en ny etikett klass i ett projekt

Under märkningen kanske du upptäcker att ytterligare etiketter behövs för att klassificera dina avbildningar.  Du kanske exempelvis vill lägga till etiketten "okänd" eller "annan" för att ange förvirrande bilder.

Följ dessa steg om du vill lägga till en eller flera etiketter i ett projekt:

1. Välj projektet på huvud sidan för **data etiketter** .
1. Längst upp på sidan väljer du **pausa** för att stoppa etiketter från deras aktivitet.
1. Välj fliken **Information**.
1. I listan till vänster väljer du **etikett klasser**.
1. Överst i listan väljer du **+ Lägg till etiketter** ![ Lägg till en etikett](media/how-to-create-labeling-projects/add-label.png)
1. I formuläret lägger du till din nya etikett och väljer hur du vill fortsätta.  Eftersom du har ändrat de tillgängliga etiketterna för en bild väljer du hur du vill behandla redan märkta data:
    * Börja om och ta bort alla befintliga etiketter.  Välj det här alternativet om du vill starta etiketter från början med den nya fullständiga uppsättningen etiketter. 
    * Börja om och Behåll alla befintliga etiketter.  Välj det här alternativet om du vill markera alla data som omärkta, men Behåll de befintliga etiketterna som en standardtagg för bilder som tidigare har märkts.
    * Fortsätt och Behåll alla befintliga etiketter. Välj det här alternativet om du vill behålla alla data som redan är märkta som de är och börja använda den nya etiketten för data som ännu inte har märkts.
1. Ändra sidan instruktioner efter behov för de nya etiketterna.
1. När du har lagt till alla nya etiketter överst på sidan väljer du **Starta** för att starta om projektet.  

## <a name="export-the-labels"></a>Exportera etiketterna

Du kan när som helst exportera etikett data för Machine Learning experimentering. Bild etiketter kan exporteras i [Coco-format](http://cocodataset.org/#format-data) eller som en Azure Machine Learning data uppsättning. Använd knappen **Exportera** på sidan **projekt information** i ditt projekt med etiketter.

COCO-filen skapas i standard-BLOB-arkivet för Azure Machine Learning arbets ytan i en mapp i *export-/Coco*. Du kan komma åt den exporterade Azure Machine Learning data uppsättningen i avsnittet **data uppsättningar** i Machine Learning. På sidan data uppsättnings information finns också exempel kod för att få åtkomst till dina etiketter från python.

![Exporterad data uppsättning](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Nästa steg

* [Självstudie: skapa din första bild klassificerings etikett för projekt](tutorial-labeling.md).
* Etikett bilder för [bild klassificering eller objekt identifiering](how-to-label-images.md)
* Läs mer om [Azure Machine Learning och Machine Learning Studio (klassisk)](compare-azure-ml-to-studio-classic.md)
