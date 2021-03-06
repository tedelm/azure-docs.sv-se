---
title: Självstudie för att filtrera, analysera data för avancerad distribution med beräkning på Azure Stack Edge | Microsoft Docs
description: Lär dig hur du konfigurerar Compute-rollen på Azure Stack Edge och använder den för att transformera data för avancerade distributions flöden innan du skickar dem till Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge for advanced deployment flow so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: eb7bb7933303aaf16f320e219ad3673bf1efde72
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82570771"
---
# <a name="tutorial-transform-data-with-azure-stack-edge-for-advanced-deployment-flow"></a>Självstudie: transformera data med Azure Stack Edge för avancerade distributions flöde

I den här självstudien beskrivs hur du konfigurerar en beräknings roll för ett avancerat distributions flöde på din Azure Stack Edge-enhet. När du har konfigurerat Compute-rollen kan Azure Stack Edge transformera data innan de skickas till Azure.

Compute kan konfigureras för enkla eller avancerade distributions flöden på enheten.

|                  | Enkel distribution                                | Avancerad distribution                   |
|------------------|--------------------------------------------------|---------------------------------------|
| Avsett för     | IT-administratörer                                | Utvecklare                            |
| Typ             | Använd Azure Stack Edge-tjänst för att distribuera moduler      | Använda IoT Hub tjänst för att distribuera moduler |
| Distribuerade moduler | Enkel                                           | Länkade eller flera moduler           |


Den här proceduren kan ta cirka 20 till 30 minuter att slutföra.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Konfigurera beräkning
> * Lägg till resurser
> * Lägga till en utlösare
> * Lägga till en beräkningsmodul
> * Verifiera datatransformering och överföring

 
## <a name="prerequisites"></a>Krav

Innan du ställer in en beräknings roll på Azure Stack Edge-enhet ser du till att:

- Du har aktiverat din Azure Stack Edge-enhet enligt beskrivningen i [ansluta, konfigurera och aktivera Azure Stack Edge](azure-stack-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>Konfigurera beräkning

Om du vill konfigurera Compute på Azure Stack Edge skapar du en IoT Hub resurs.

1. I Azure Portal av Azure Stack Edge-resursen går du till **Översikt**. Välj **Kom igång**i den högra rutan på **beräknings** panelen.

    ![Kom igång med Compute](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-1.png)

2. På panelen **Konfigurera Edge Compute** väljer du **Konfigurera beräkning**.

    ![Kom igång med Compute](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-2.png)

3. Ange följande på bladet **Konfigurera Edge Compute** :

   
    |Field  |Värde  |
    |---------|---------|
    |IoT Hub     | Välj från **ny** eller **befintlig**. <br> Som standard används en standard-nivå (S1) för att skapa en IoT-resurs. Om du vill använda en IoT-resurs på kostnads fri nivå skapar du en och väljer sedan den befintliga resursen. <br> I varje fall använder IoT Hub resursen samma prenumeration och resurs grupp som används av Azure Stack Edge-resursen.     |
    |Name     |Ange ett namn för din IoT Hub-resurs.         |

    ![Kom igång med Compute](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-3.png)

4. Välj **Skapa**. Det tar några minuter att skapa en IoT Hub-resurs. När IoT Hub resurs har skapats, uppdaterar konfigurations panelen för **Edge** för att Visa beräknings konfigurationen. Bekräfta att Edge Compute-rollen har kon figurer ATS genom att välja **Visa konfiguration** på panelen **Konfigurera beräkning** .
    
    ![Kom igång med Compute](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-4.png)

    När Edge-beräkningsrollen har konfigurerats på Edge-enheten så skapas två enheter: en IoT-enhet och en IoT Edge-enhet. Bägge enheter kan visas i IoT Hub-resursen. En IoT Edge runtime körs också på den här IoT Edge enheten.

    För tillfället är det bara Linux-plattformen tillgänglig för din IoT Edge-enhet.


## <a name="add-shares"></a>Lägg till resurser

För den avancerade distributionen i den här självstudien behöver du två resurser: en Edge-resurs och en annan Edge-lokal resurs.

1. Lägg till en Edge-resurs på enheten genom att utföra följande steg:

    1. I din Azure Stack Edge-resurs går du till **Edge compute > kom igång**.
    2. Välj **Lägg till**på panelen **Lägg till resurs (er)** .
    3. På bladet **Lägg till resurs** anger du resurs namnet och väljer resurs typ.
    4. Om du vill montera Edge-resursen markerar du kryss rutan för att **använda resurs med Edge Compute**.
    5. Välj **lagrings kontot**, **lagrings tjänsten**, en befintlig användare och välj sedan **skapa**.

        ![Lägg till en Edge-resurs](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-1.png)

    <!--If you created a local NFS share, use the following remote sync (rsync) command option to copy files onto the share:

    `rsync <source file path> < destination file path>`

    For more information about the rsync command, go to [Rsync documentation](https://www.computerhope.com/unix/rsync.htm).-->

    När Edge-resursen har skapats får du ett meddelande om att det har skapats. Delnings listan uppdateras för att avspegla den nya resursen.

2. Lägg till en lokal Edge-resurs på gräns enheten genom att upprepa alla steg i föregående steg och markera kryss rutan för att **Konfigurera som Edge-lokal resurs**. Data i den lokala resursen förblir på enheten.

    ![Lägg till en lokal Edge-resurs](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-2.png)

3. På bladet **resurser** ser du den uppdaterade listan med resurser.

    ![Uppdaterad lista över resurser](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-3.png)

4. Om du vill visa egenskaperna för den nyligen skapade lokala resursen väljer du resursen i listan. I rutan **Lokal monteringspunkt för Edge-beräkningsmoduler** kopierar du det värde som motsvarar den här resursen.

    Du använder den här lokala monteringspunkten när du distribuerar modulen.

    ![Rutan Lokal monteringspunkt för Edge-beräkningsmodul](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-4.png)
 
5. Om du vill visa egenskaperna för kant resursen som du har skapat väljer du resursen i listan. I rutan **Lokal monteringspunkt för Edge-beräkningsmoduler** kopierar du det värde som motsvarar den här resursen.

    Du använder den här lokala monteringspunkten när du distribuerar modulen.

    ![Lägga till anpassad modul](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-5.png)


## <a name="add-a-trigger"></a>Lägga till en utlösare

1. Gå till **Edge compute >-utlösare**. Välj **+ Lägg till utlösare**.

    ![Lägg till utlösare](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-1.png)

2. Ange följande värden i bladet **Lägg till utlösare** .

    |Field  |Värde  |
    |---------|---------|
    |Utlösarens namn     | Ett unikt namn för utlösaren.         |
    |Utlösartyp     | Välj **fil** utlösare. En fil utlösare utlöses när en fil händelse inträffar, till exempel att en fil skrivs till den angivna resursen. En schemalagd utlösare å andra sidan, utlöses utifrån ett schema som definierats av dig. I det här exemplet behöver vi en fil utlösare.    |
    |Inmatad resurs     | Välj en inmatad resurs. Den lokala Edge-resursen är den ingående resursen i det här fallet. Modulen som används här flyttar filer från den lokala Edge-resursen till en Edge-resurs där de överförs till molnet.        |

    ![Lägg till utlösare](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-2.png)

3. Du får ett meddelande när utlösaren har skapats. Listan över utlösare uppdateras för att visa den nyligen skapade utlösaren. Välj den utlösare som du nyss skapade.

    ![Lägg till utlösare](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-3.png)

4. Kopiera och spara exempel vägen. Du kommer att ändra den här exempel vägen och använda den senare i IoT Hub.

    `"sampleroute": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/modulename/inputs/input1\")"`

    ![Lägg till utlösare](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-4.png)

## <a name="add-a-module"></a>Lägg till en modul

Det finns inga anpassade moduler på den här Edge-enheten. Du kan lägga till en anpassad eller en fördefinierad modul. Om du vill lära dig hur du skapar en anpassad modul går du till [utveckla en C#-modul för din Azure Stack Edge-enhet](azure-stack-edge-create-iot-edge-module.md).

I det här avsnittet lägger du till en anpassad modul till den IoT Edge enhet som du skapade i [utveckla en C#-modul för Azure Stack Edge](azure-stack-edge-create-iot-edge-module.md). Den här anpassade modulen tar filer från en lokal Edge-resurs på gräns enheten och flyttar dem till en gräns (moln) resurs på enheten. Molnresursen pushar sedan filerna till det Azure-lagringskonto som är associerat med molnresursen.

1. Gå till **Edge compute > kom igång**. Välj scenario typen som **Avancerat**på panelen **Lägg till moduler** . Välj **gå till IoT Hub**.

    ![Välj avancerad distribution](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-1.png)

<!--2. In the **Configure and add module** blade, input the following values:  

    |Input share     | Select an input share. The Edge local share is the input share in this case. The module used here moves files from the Edge local share to an Edge share where they are uploaded into the cloud.        |
    |Output share     | Select an output share. The Edge share is the output share in this case.        |
-->

2. I IoT Hub resurs går du till **IoT Edge enhet** och väljer sedan din IoT Edge enhet.

    ![Gå till IoT Edge enhet i IoT Hub](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-2.png)

3. Välj **Ange moduler**på **enhets information**.

    ![Länken Ange moduler](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-3.png)

4. Under **Lägg till moduler** utför du följande:

    1. Ange namn, adress, användarnamn och lösenord för containerregisterinställningarna för den anpassade modulen.
    Namn, adress och de listade autentiseringsuppgifterna används för att hämta moduler med en matchande URL. Distribuera den här modulen genom att under **Distributionsmoduler** välja **IoT Edge-modul**. Den här IoT Edge modulen är en Docker-behållare som du kan distribuera till den IoT Edge enhet som är kopplad till din Azure Stack Edge-enhet.

        ![Länken Ange moduler](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-4.png) 
 
    2. Ange inställningarna för den anpassade IoT Edge-modulen. Mata in följande värden.
     
        |Field  |Värde  |
        |---------|---------|
        |Namn     | Ett unikt namn för modulen. Den här modulen är en Docker-behållare som du kan distribuera till den IoT Edge enhet som är kopplad till Azure Stack Edge.        |
        |Bild-URI     | Avbildnings-URI för motsvarande behållar avbildning för modulen.        |
        |Autentiseringsuppgifter krävs     | Om det här alternativet markeras används användar namn och lösen ord för att hämta moduler med en matchande URL.        |
    
        I rutan **alternativ för att skapa behållare** anger du de lokala monterings punkterna för de Edge-moduler som du kopierade i föregående steg för den lokala resursen Edge och Edge.

        > [!IMPORTANT]
        > De sökvägar som används här monteras i din behållare, så de måste matcha vad funktionen i din behållare förväntar sig. Om du följer [skapa en anpassad modul](azure-stack-edge-create-iot-edge-module.md#update-the-module-with-custom-code)förväntar sig koden som anges i modulen de kopierade Sök vägarna. Ändra inte dessa sökvägar.
    
        I rutan **Alternativ för containerskapande** så kan du klistra in följande exempel:
    
        ```
        {
          "HostConfig": 
          {
           "Binds": 
            [
             "/home/hcsshares/mydbesmbedgelocalshare1:/home/input",
             "/home/hcsshares/mydbesmbedgeshare1:/home/output"
            ]
           }
        }
        ```

        Ange alla miljövariabler som används för modulen. Miljövariabler ger valfri information som hjälper dig att definiera miljön där modulen körs.

        ![Rutan Alternativ för containerskapande](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-5.png) 
 
    4. Konfigurera vid behov avancerade Edge-körningsinställningar och klicka sedan på **Nästa**.

        ![Lägga till anpassad modul](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-6.png)
 
5.    Under **Ange vägar** anger du vägar mellan moduler.  
    
    ![Ange vägar](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-7.png)

    Du kan ersätta *routning* med följande väg sträng som du kopierade tidigare. I det här exemplet anger du namnet på den lokala resurs som skickar data till molnresursen. Ersätt `modulename` med namnet på modulen. Välj **Nästa**.
        
    ```
    "route": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/filemove/inputs/input1\")"
    ```

    ![Avsnittet Ange vägar](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-8.png)

6.    Under **Granska distributionen** så granskar du alla inställningarna. Om du är nöjd så **skickar** du modulen för distribution.

    ![Länken Ange moduler](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-9.png)
 
    Den här åtgärden startar modulen distribution. När distributionen är klar **körs**modulens **körnings status** .

    ![Lägga till anpassad modul](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-10.png)

## <a name="verify-data-transform-transfer"></a>Verifiera datatransformering, överföring

Det slutliga steget är att se till att modulen är ansluten och körs som förväntat. Körningsstatusen för modulen bör vara aktiv för din IoT Edge-enhet i IoT Hub-resursen.

Utför följande steg för att verifiera datatransformering och överföra till Azure.
 
1.    I Utforskaren ansluter du till både de lokala resurserna och kant resurserna som du skapade tidigare.

    ![Verifiera datatransformering](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-2.png)
 
1.    Lägg till data i den lokala resursen.

    ![Verifiera datatransformering](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-3.png)
 
    Data flyttas till molnresursen.

    ![Verifiera datatransformering](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-4.png)  

    Data pushas sedan från molnresursen till lagringskontot. Om du vill visa data går du till ditt lagrings konto och väljer sedan **Storage Explorer**. Du kan visa överförda data i ditt lagrings konto.

    ![Verifiera datatransformering](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-5.png)
 
Du har slutfört valideringsprocessen.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Konfigurera beräkning
> * Lägg till resurser
> * Lägga till en utlösare
> * Lägga till en beräkningsmodul
> * Verifiera datatransformering och överföring

Information om hur du administrerar Azure Stack Edge-enheten finns i:

> [!div class="nextstepaction"]
> [Använd lokalt webb gränssnitt för att administrera en Azure Stack Edge](azure-stack-edge-manage-access-power-connectivity-mode.md)
