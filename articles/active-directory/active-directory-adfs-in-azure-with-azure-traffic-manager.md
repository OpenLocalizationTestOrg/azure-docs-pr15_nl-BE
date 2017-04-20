<properties
    pageTitle="Hoge beschikbaarheid cross-geografische AD FS-implementatie in Azure Azure verkeer Manager | Microsoft Azure"
    description="In dit document leert u het implementeren van AD FS in Azure voor hoge beschikbaarheid."
    keywords="AD fs met Azure verkeer manager, adfs met Azure verkeer Manager, geografische, multi-datacenter, geografische datacenters, meerdere geografische datacenters, implementeren van AD FS in azure, azure adfs, azure adfs, azure ad fs implementeren, implementeren van AD FS, implementeren van ad fs, adfs in azure, implementeren van AD FS in azure, implementeren van AD FS in azure, adfs azure, inleiding tot AD FS, Azure, AD FS in Azure, iaas , ADFS, adfs verplaatsen naar azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Hoge beschikbaarheid cross-geografische AD FS-implementatie in Azure Azure verkeer Manager

[AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md) biedt stapsgewijze richtlijnen over hoe u een eenvoudige AD FS-infrastructuur voor uw organisatie in Azure implementeren kunt. Dit artikel bevat de volgende stappen voor het maken van een cross-geografische implementatie van AD FS in Azure met [Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md). Azure verkeer Manager helpt bij het maken van een geografisch verspreid hoge beschikbaarheid en hoge prestaties AD FS-infrastructuur voor uw organisatie door gebruik van verschillende methoden beschikbaar voor verschillende behoeften van de infrastructuur voor routering.

Er kunnen maximaal beschikbare cross-geografische AD FS-infrastructuur:

* **Eliminatie van potentieel risico:** Met failover-mogelijkheden van Azure verkeer Manager, kunt u een AD FS-infrastructuur maximaal beschikbaar bereiken zelfs als een van de datacenters in een deel van de wereld afgesloten wordt
* **Verbeterde prestaties:** U kunt de voorgestelde implementatie in dit artikel bieden een krachtige AD FS-infrastructuur waarmee gebruikers sneller te verifiëren. 

##<a name="design-principles"></a>Principes van databaseontwerp

![Algehele ontwerp](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

De fundamentele principes worden dezelfde zoals vermeld in de ontwerpprincipes in het artikel in AD FS-implementatie in Azure. Het bovenstaande diagram toont een eenvoudige uitbreiding van de eenvoudige distributie naar een andere geografische onderverdeling. Hieronder volgen enkele punten bij het uitbreiden van uw nieuwe geografische regio

* **Virtueel netwerk:** Maak een nieuwe virtuele netwerk in de geografische regio die u wilt implementeren van aanvullende AD FS-infrastructuur. In het bovenstaande diagram ziet u de Geo1 VNET en Geo2 VNET als de twee virtuele netwerken in elke regio.

* **-Domeincontrollers en AD FS-servers in het nieuwe geografische VNET:** Het wordt aanbevolen voor de implementatie van domain controllers in de nieuwe geografische regio, zodat de AD FS-servers in het nieuwe gebied niet hoeft te maken met een domeincontroller in een ander ver netwerk om een verificatie- en daarmee de prestaties te verbeteren.

* **Opslag rekeningen:** Opslag-accounts zijn gekoppeld aan een bepaalde regio. Aangezien u machines in een nieuw geografisch gebied distribueren, moet u nieuwe opslag accounts moet worden gebruikt in de regio.  

* **Network Security Groups:** Als u accounts opslag, netwerk-beveiligingsgroepen zijn gemaakt in een gebied kunnen niet worden gebruikt in een andere geografische onderverdeling. Daarom moet u maken nieuw netwerk beveiligingsgroepen overeen met die van de eerste geografische regio voor INT en DMZ subnet in de nieuwe geografische regio.

* **DNS-Labels voor openbare IP-adressen:** Azure verkeer Manager kan verwijzen naar de eindpunten alleen via DNS-labels. Daarom moet u DNS-labels voor de externe Netwerktaakverdeling openbare IP-adressen maken.

* **Azure verkeer Manager:** Microsoft Azure verkeer Manager kunt u de verdeling van het gebruikersverkeer op uw service-endpoints in verschillende datacenters over de hele wereld uitgevoerd. Azure verkeer Manager werkt op het niveau van DNS. DNS-antwoorden via directe eindgebruikers verkeer eindpunten wereldwijd verspreid. Clients vervolgens verbinding maken met deze eindpunten rechtstreeks. Met verschillende routeringsopties van prestaties, gewogen en prioriteit kunt gemakkelijk u de optie voor routering geschikt voor de behoeften van uw organisatie. 

* **V-net naar V-netto-connectiviteit tussen de twee regio's:** U hoeft niet zelf verbinding tussen de virtuele netwerken hebben. Omdat elk virtueel netwerk toegang tot domeincontrollers heeft en AD FS- en WAP-server zelf, werken ze zonder een verbinding tussen de virtuele netwerken in verschillende regio's. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Stappen voor de integratie van de Azure verkeer Manager

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Implementeren van AD FS in de nieuwe geografische regio
Volg de stappen en richtsnoeren in [AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md) te implementeren in nieuwe geografische regio dezelfde topologie.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS-labels voor openbare IP-adressen van de netwerktaakverdeling Internet Facing (openbaar)
Zoals hierboven vermeld, de Manager van Azure verkeer alleen kan verwijzen naar de DNS-labels als eindpunten en daarom is het belangrijk om DNS-etiketten voor de externe Netwerktaakverdeling openbare IP-adressen te maken. Hieronder screenshot ziet u hoe u uw DNS-label voor het openbare IP-adres kunt configureren. 

![DNS-Label](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Azure verkeer Manager implementeren

De volgende stappen voor het maken van een serviceprofiel verkeer. Voor meer informatie kunt u ook verwijzen naar [een Azure verkeer Manager profiel beheren](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Maakt u een profiel verkeer Manager:** Uw profiel verkeer manager een unieke naam geven. Deze naam van het profiel maakt deel uit van de DNS-naam en fungeert als een voorvoegsel voor het beheer van verkeer domein naamlabel. De naam / voorvoegsel wordt toegevoegd aan. trafficmanager.net voor het maken van een DNS-label voor het verkeer manager. Het screenshot hieronder toont de manager verkeer DNS-voorvoegsel wordt ingesteld als mysts en de resulterende DNS-label worden mysts.trafficmanager.net. 

    ![Verkeer Manager profiel maken](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Verkeer methode:** Er zijn drie opties voor routering in beheer van verkeer beschikbaar:

    * Prioriteit 
    * Prestaties
    * Gewogen
    
    **Prestaties** is de aanbevolen optie voor een uiterst responsieve AD FS-infrastructuur. U kunt echter de methode geschikt voor uw implementatievereisten. De AD FS-functionaliteit is niet getroffen door de optie routering is ingeschakeld. Zie [beheer van verkeer verkeer routeringsmethoden](../traffic-manager/traffic-manager-routing-methods.md) voor meer informatie. In het voorbeeld kunt screenshot hierboven u Zie de **prestaties** geselecteerd.
   
3.  **Eindpunten configureren:** Klik op de eindpunten in de pagina beheer van netwerkverkeer en selecteer toevoegen. Hiermee opent u een pagina lijkt op het screenshot hieronder eindpunt van toevoegen
 
    ![Eindpunten configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Volg de onderstaande richtsnoer voor de verschillende inputs:

    **Type:** Selecteer Azure eindpunt zullen we verwijzen naar een Azure openbare IP-adres.

    **Naam:** Maak een naam die u wilt koppelen aan het eindpunt. Dit is niet de DNS-naam en heeft geen gevolgen voor de DNS-records.

    **Richten op het resourcetype:** Selecteer openbaar IP-adres als de waarde van deze eigenschap. 

    **Doel, bron:** Hierdoor krijgt u een optie om te kiezen uit de verschillende labels van de DNS-waarover u beschikt in uw abonnement. De DNS-label voor te kiezen.

    Eindpunt voor elke geografische regio die u het routeren van netwerkverkeer naar Azure verkeer Manager wilt toevoegen.
    Raadpleeg voor meer informatie en gedetailleerde stapsgewijze instructies voor het toevoegen / eindpunten configureren in beheer van verkeer toe te [voegen, uitschakelen, inschakelen of eindpunten verwijderen](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Configureren sonde:** Klik in de pagina beheer van verkeer op configuratie. Op de configuratiepagina moet u de beeldscherminstellingen te onderzoeken op HTTP-poort 80 en het relatieve pad /adfs/probe wijzigen

    ![Sonde configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Zorg ervoor dat de status van de eindpunten on line is zodra de configuratie voltooid is**. Als alle eindpunten in 'slechtere' staat, doet Azure verkeer Manager een beste poging om het verkeer op voorwaarde dat dat de diagnostische gegevens onjuist is en alle eindpunten bereikbaar zijn.

5. **Wijzigen van DNS-records voor Azure verkeer Manager:** De federation-service moet een CNAME-record met de Azure verkeer Manager DNS-naam. Een CNAME in de openbare DNS-records maken, zodat iedereen die aan het bereiken van de federation-service probeert de Manager Azure verkeer daadwerkelijk bereikt.

    Bijvoorbeeld, de federation-service-fs.fabidentity.com, punt aan het beheer van verkeer, moet u voor het bijwerken van uw DNS-bronrecord worden de volgende:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Test de Routering en de AD FS-aanmelden   

###<a name="routing-test"></a>Test-routering

Een eenvoudige test voor het bewerkingsplan zou zijn om te proberen de federation-service DNS-naam van een computer in elke regio ping. Afhankelijk van de methode gekozen, worden het eindpunt dat daadwerkelijk wordt gepingd weergegeven in de weergave van de ping. Bijvoorbeeld, als u de prestaties van routering ingeschakeld, wordt vervolgens het eindpunt die het dichtst bij de regio van de client bereikt. Hieronder vindt u de momentopname van de twee pings van clientcomputers met twee andere regio, één in Oost-Aziatische regio en één in West VS. 

![Test-routering](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS-in test

De eenvoudigste manier om AD FS te testen is met behulp van de pagina IdpInitiatedSignon.aspx. Om te kunnen doen, is vereist voor de IdpInitiatedSignOn van de AD FS-eigenschappen. Volg de onderstaande stappen om te controleren of de installatie van AD FS
 
1. Voer de onderstaande cmdlet op de AD FS-server met PowerShell, instellen op ingeschakeld. $True set-AdfsProperties - EnableIdPInitiatedSignonPage
2. Vanaf een externe computer toegang https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. U ziet de AD FS-pagina, zoals hieronder:

    ![Test van de AD FS - verificatie uitdaging](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    en bij succesvolle aanmelden, deze krijgt u een bericht zoals hieronder wordt weergegeven:

    ![Test van de AD FS - verificatie geslaagd](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Verwante koppelingen
* [Eenvoudige implementatie van AD FS in Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md)
* [Beheer van verkeer verkeer routeringsmethoden](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Volgende stappen
* [Een serviceprofiel Azure verkeer beheren](../traffic-manager/traffic-manager-manage-profiles.md)
* [Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen](../traffic-manager/traffic-manager-endpoints.md) 

