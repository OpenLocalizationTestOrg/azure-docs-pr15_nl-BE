<properties
    pageTitle="Overzicht van de schema's en de Enterprise Integration Pack | Microsoft Azure"
    description="Informatie over het gebruik van schema's met de apps Enterprise Integration Pack en logica"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Meer informatie over schema's en de Enterprise Integration Pack  

## <a name="why-use-a-schema"></a>Waarom een schema gebruiken?
Schema's gebruiken om te bevestigen dat de XML-documenten die u ontvangt, met de verwachte gegevens in een vooraf gedefinieerde notatie geldig. Schema's worden gebruikt voor het valideren van berichten die worden uitgewisseld in een B2B-scenario.

## <a name="add-a-schema"></a>Een schema toevoegen
Vanuit de Azure portal:  

1. Selecteer **meer services**.  
![Screenshot van Azure portal, met 'Meer services' gemarkeerd](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. **Integratie**invoeren in het vak filter zoeken en **Integratie Accounts** selecteren uit de lijst met resultaten.     
![Screenshot van zoekvak filter](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Selecteer de **account integratie** waaraan u het schema toevoegt.    
![Screenshot van de lijst met accounts voor integratie](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Selecteer de **schema's** naast elkaar.  
![Screenshot van IEP integratie rekening met 'Schema' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Toevoegen van een schemabestand dat kleiner is dan 2 MB  

1. Selecteer in de **schema's** blade openen (met de voorgaande stappen), **toevoegen**.  
![Screenshot van schema's blade, met de knop 'Toevoegen' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Voer een naam voor het schema. Selecteer vervolgens het om schemabestand te uploaden, het mappictogram naast het tekstvak **Schema** . Nadat het uploaden is voltooid, klik op **OK**.    
![Screenshot van "Schema toevoegen", "klein bestand' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Toevoegen van een schemabestand dat groter is dan 2 MB (maximaal 8 MB)  

Het proces is afhankelijk van het toegangsniveau van de blob-container: **openbaar** of **geen anonieme toegang**. Om te bepalen dit toegangsniveau in **Opslagverkenner Azure** **Blob Containers**, selecteert u de gewenste blob-container. Selecteer **beveiliging**en klik op het tabblad **Toegangsniveau** .

1. Als het niveau van toegang blob **openbaar is**, volg deze stappen.  
  ![Screenshot van Azure Opslagverkenner met 'Blob Containers', 'Beveiliging' en 'Openbaar' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    een. Het schema voor opslag uploadt en kopieer de URI.  
    ![Screenshot van opslag-account met URI gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. In het **Schema toevoegen**, **groot bestand**selecteren en de URI die in het vak **URI inhoud** bieden.  
    ![Screenshot van schema's met de knop 'Toevoegen' en 'Groot bestand' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Als het niveau van toegang blob **geen anonieme toegang is**, volg deze stappen.  
  ![Screenshot van Azure Opslagverkenner met 'Blob Containers', 'Beveiliging' en 'Geen anonieme toegang' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    een. Het schema om opslag te uploaden.  
    ![Screenshot van opslag account](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Een handtekening voor gedeelde toegang voor het schema genereren.  
    ![Screenshot van opslag, sccount met gedeelde toegang handtekeningen tabblad gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. In het **Schema toevoegen**, **groot bestand**selecteren en de gedeelde toegang handtekening URI in het vak **URI inhoud** bieden.  
    ![Screenshot van schema's met de knop 'Toevoegen' en 'Groot bestand' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. In het blad **schema's** van de Account EIP integratie ziet u nu het zojuist toegevoegde schema.  
![Screenshot van EIP integratie rekening met "Schema's" en het nieuwe schema gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Schema's bewerken
1. Selecteer de **schema's** naast elkaar.  
2. Selecteer het schema dat u wilt bewerken in de **schema's** blade dat wordt geopend.
3. Selecteer **bewerken**op het blad **schema's** .  
![Screenshot van schema's blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Selecteer het schemabestand dat u bewerken wilt met behulp van het bestand kiezen in het dialoogvenster dat wordt geopend.
5. Selecteer **openen** in de bestandskiezer.  
![Screenshot van het kiezen van bestanden](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. U ontvangt een melding dat het uploaden is voltooid.  

## <a name="delete-schemas"></a>Schema's verwijderen
1. Selecteer de **schema's** naast elkaar.  
2. Selecteer het schema dat u wilt verwijderen uit het **schema's** blade dat wordt geopend.  
3. Selecteer op het blad **schema's** **verwijderen**.
![Screenshot van schema's blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Selecteer **Ja**om uw keuze te bevestigen.  
![Screenshot van het bevestigingsbericht "Schema verwijderen"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Merk ook op dat de lijst met schema's in de **schema's** blade vernieuwd en het schema dat u hebt verwijderd, wordt niet meer vermeld.  
![Screenshot van EIP integratie rekening met 'Schema' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over de enterprise integration pack").  
