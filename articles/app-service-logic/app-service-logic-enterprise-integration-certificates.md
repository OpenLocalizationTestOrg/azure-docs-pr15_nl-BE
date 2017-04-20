
<properties
    pageTitle="Met behulp van certificaten met Enterprise Integration Pack | Microsoft Azure"
    description="Informatie over het gebruik van certificaten met de Enterprise Integration Pack en logica Apps"
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
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Meer informatie over certificaten en Enterprise Integration Pack

## <a name="overview"></a>Overzicht
Integratie in de onderneming gebruikt certificaten B2B-communicatie beveiligen. U kunt twee typen certificaten in uw enterprise integration apps:

- Certificaten voor openbare, dat moeten worden ingekocht bij een certificeringsinstantie (CA).
- Persoonlijke certificaten, kunt u zelf uitgeven. Deze certificaten worden soms aangeduid als zelfondertekende certificaten.


## <a name="what-are-certificates"></a>Wat zijn certificaten?
Certificaten zijn digitale documenten die bij het controleren van de identiteit van de deelnemers in elektronische communicatie en dat ook elektronische communicatie beveiligen.

## <a name="why-use-certificates"></a>Waarom certificaten gebruiken?
Soms moet B2B-communicatie vertrouwelijk worden behandeld. Integratie in de onderneming gebruikt certificaten voor het beveiligen van deze communicatie op twee manieren:

- Door de inhoud van berichten coderen
- Door berichten digitaal te ondertekenen  

## <a name="how-do-you-upload-certificates"></a>Hoe kan u certificaten uploaden?

### <a name="public-certificates"></a>Certificaten voor openbare
Als u een *certificaat met openbare* in uw logica apps met B2B-mogelijkheden, moet u eerst het certificaat uploaden naar je account integratie. Om een *zelf-ondertekend certificaat*gebruikt, aan de andere kant, moet u eerst uploaden het [Azure sleutel kluis](../key-vault/key-vault-get-started.md "informatie over sleutel kluis").

Nadat u een certificaat hebt geüpload, is het beschikbaar waarmee u uw B2B-berichten beveiligen bij het definiëren van de eigenschappen in de [overeenkomsten](./app-service-logic-enterprise-integration-agreements.md) die u maakt.  

Hier zijn de gedetailleerde stappen voor het uploaden van uw certificaten met openbare de integratie rekening nadat u bij de portal Azure inlogt:

1. Selecteer **Bladeren**.  
    ![Selecteer Bladeren](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. **Integratie** invoeren in het vak filter zoeken en selecteer **Accounts integratie** uit de lijst met resultaten.     
    ![Selecteer de rekeningen integratie](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Selecteer de integratie-account die u wilt toevoegen van het certificaat.  
    ![Selecteer de integratie-account die u wilt toevoegen het certificaat](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Selecteer de tegel **certificaten** .  
    ![Selecteer de tegel certificaten](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. In het blad voor **certificaten** die wordt geopend, selecteert u de knop **toevoegen** .
    ![Klik op de knop toevoegen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Voer een **naam** voor het certificaat en selecteer vervolgens het certificaattype. (In dit voorbeeld hebben we gebruikt het type certificaat voor openbare.) Selecteer het pictogram van de map aan de rechterkant van het tekstvak van het **certificaat** .

7. Wanneer het kiezen van bestanden wordt geopend, zoek en selecteer het certificaatbestand dat u wilt uploaden naar je account integratie.

8. Selecteer het certificaat en selecteert u **OK** in de bestandskiezer. Dit wordt gevalideerd en uploadt u het certificaat op uw account integratie.

8. Ten slotte terug op het blad **certificaat toevoegen** selecteert u de knop **OK** .  
    ![Klik op OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. In ongeveer een minuut ziet u een melding die aangeeft dat het certificaat uploaden voltooid is.

10. Selecteer de tegel **certificaten** . Hier ziet u het nieuwe certificaat.  
    ![Het nieuwe certificaat bekijken](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Persoonlijke certificaten
Persoonlijke certificaten kunt u uploaden naar uw rekening integratie door middel van de volgende stappen uit:  

1. [Upload uw persoonlijke sleutel sleutel kluis] (../key-vault/key-vault-get-started.md "Meer informatie over de sleutel kluis").  

    > [AZURE.TIP] U moet de functie logica Apps van App-Service voor het uitvoeren van bewerkingen op de sleutel kluis Azure machtigen. U kunt toegang verlenen aan de logica Apps service principal met behulp van de volgende PowerShell-opdracht:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Een persoonlijk certificaat maken.  

3. De persoonlijke certificaat uploaden naar je account integratie.

Nadat u de vorige stappen hebt gemaakt, kunt u het certificaatarchief voor het maken van overeenkomsten.

Hieronder vindt u de gedetailleerde stappen voor het uploaden van uw persoonlijke certificaten in uw account integratie wanneer u bent aangemeld bij de Azure portal:  

1. Selecteer **Bladeren**.  
    ![Upload uw persoonlijke certificaten naar uw account integratie](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. **Integratie** invoeren in het vak filter zoeken en selecteer **Accounts integratie** uit de lijst met resultaten.     
    ![Selecteer de rekeningen integratie](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Selecteer de integratie-account die u wilt toevoegen van het certificaat.  
    ![Selecteer de integratie-account die u wilt toevoegen het certificaat](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Selecteer de tegel **certificaten** .  
    ![Selecteer de tegel certificaten](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. In het blad voor **certificaten** die wordt geopend, selecteert u de knop **toevoegen** .
    ![Klik op de knop toevoegen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Voer een **naam** voor het certificaat en selecteer vervolgens het certificaattype. (In dit voorbeeld hebben we gebruikt het type certificaat voor openbare.) Selecteer het pictogram van de map aan de rechterkant van het tekstvak van het **certificaat** .

7. Wanneer het kiezen van bestanden wordt geopend, zoek en selecteer het certificaatbestand dat u wilt uploaden naar je account integratie.

8. Nadat u het certificaat hebt geselecteerd, klik op **OK** in de bestandskiezer. Deze actie wordt het certificaat gevalideerd en geüpload naar uw account integratie.

9. Ten slotte terug op het blad **certificaat toevoegen** selecteert u de knop **OK** .  
    ![Certificaat toevoegen](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. In ongeveer een minuut ziet u een melding die aangeeft dat het certificaat uploaden voltooid is.

11. Selecteer de tegel **certificaten** . Hier ziet u het nieuwe certificaat.
    ![Het nieuwe certificaat bekijken](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Nadat u een certificaat hebt geüpload, is het beschikbaar waarmee u uw B2B-berichten beveiligen bij het definiëren van de eigenschappen in [overeenkomsten](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Volgende stappen
- [Maken van een app logica die B2B-functies gebruikt](./app-service-logic-enterprise-integration-b2b.md)  
- [Een B2B overeenkomst maken](./app-service-logic-enterprise-integration-agreements.md)  
- [Meer informatie over de sleutel kluis] (../key-vault/key-vault-get-started.md "Meer informatie over de sleutel kluis")  
