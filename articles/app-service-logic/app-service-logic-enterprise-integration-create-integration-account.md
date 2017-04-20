<properties 
    pageTitle="Overzicht van de rekeningen van de integratie en de Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
    description="Lees alle informatie over de integratie van accounts, Enterprise Integration Pack en logica apps" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Overzicht van de rekeningen van de integratie

## <a name="what-is-an-integration-account"></a>Wat is een account integratie?
Een account integratie is een Azure account waarmee apps integratie in de onderneming beheren artefacten waaronder schema's, kaarten, certificaten, partners en overeenkomsten. Eventuele integratie app u maakt moet een account integratie gebruiken voor toegang tot een schema, kaart of een certificaat, bijvoorbeeld.

## <a name="create-an-integration-account"></a>Maak een account integratie 
1. Selecteer **Bladeren**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. **Integratie** invoeren in het vak filter zoeken en **Integratie Accounts** selecteren uit de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Selecteer knop *toevoegen* in het menu aan de bovenkant van de pagina      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Voer de **naam**, het **abonnement** dat u gebruiken wilt, selecteert u een nieuwe **resourcegroep** maken of een bestaande resourcegroep selecteren, selecteer een **locatie** waar uw account integratie wordt beheerd, selecteer een **prijzen laag**en selecteer vervolgens de knop **maken** .   

  Op dit moment wordt de integratie rekening worden ingericht op de locatie die u hebt geselecteerd. Dit moet worden voltooid binnen 1 minuut.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Vernieuw de pagina. U ziet uw nieuwe integratie rekening vermeld. Gefeliciteerd!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Hoe een integratie-account te koppelen aan een app logica
Om uw logica apps toegang tot kaarten, schema's, overeenkomsten en andere artefacten die zich in uw account integratie bevinden, moet u eerst de integratie-account koppelen aan uw app logica.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Hier zijn de stappen een integratie-account te koppelen aan een app logica 

#### <a name="prerequisites"></a>Vereisten
- Een account integratie
- Een app logica

>[AZURE.NOTE]Zorg ervoor dat uw account integratie en logica app in de **Azure locatie** voordat u begint

1. Klik op de link **Instellingen** in het menu van uw app logica  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Selecteer de optie **Account integratie** in de blade instellingen  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Selecteer de integratie rekening die u wilt koppelen aan de app van de **Selecteer een account voor de integratie** van het vak vervolgkeuzelijst logica  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Uw werk opslaan  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Er verschijnt een melding waarin wordt aangegeven dat uw integratie-account is gekoppeld aan uw app logica en alle artefacten in uw account integratie zijn nu beschikbaar voor uw app logica.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Nu dat uw integratie-account is gekoppeld aan uw logica app, kunt u Ga naar uw app logica en B2B-verbindingen, zoals XML-validatie, plat bestand coderen/decoderen of transformatie gebruiken om apps te maken met B2B-functies.  
    
## <a name="how-to-delete-an-integration-account"></a>Hoe kan ik een integratie-account verwijderen?
1. Selecteer **Bladeren**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integratie** invoeren in het vak filter zoeken en **Integratie Accounts** selecteren uit de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Selecteer de **integratie-account** die u wilt verwijderen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Selecteer de koppeling **verwijderen** in het menu   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Bevestig uw keuze    

## <a name="how-to-move-an-integration-account"></a>Het verplaatsen van een integratie-account?
Een account integratie kunt u eenvoudig verplaatsen naar een nieuw abonnement en een nieuwe bronnengroep. Volg deze stappen als u uw account integratie:

>[AZURE.IMPORTANT] U moet alle scripts voor het gebruik van de nieuwe resource-id's nadat u een integratie-account bijwerken.

1. Selecteer **Bladeren**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integratie** invoeren in het vak filter zoeken en **Integratie Accounts** selecteren uit de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Selecteer de **integratie-account** die u wilt verwijderen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Selecteer de koppeling **verplaatsen** in het menu   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Bevestig uw keuze    

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over overeenkomsten] (./app-service-logic-enterprise-integration-agreements.md "Meer informatie over enterprise integratie overeenkomsten")  


 