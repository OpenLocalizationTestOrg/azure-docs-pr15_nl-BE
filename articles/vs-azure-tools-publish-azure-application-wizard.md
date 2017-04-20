<properties 
   pageTitle="Azure Application Wizard publiceren | Microsoft Azure"
   description="Azure Application Wizard publiceren"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Azure Application Wizard publiceren

## <a name="overview"></a>Overzicht

Nadat u een webtoepassing in Visual Studio ontwikkelt, kunt u gemakkelijker een Azure cloud service toepassing publiceren met behulp van de wizard **publiceren Azure** . Het eerste gedeelte worden de stappen beschreven die u uitvoeren moet voordat u de wizard en de overige secties wordt uitgelegd dat de functies van de wizard.

>[AZURE.NOTE] Dit onderwerp is over het implementeren van cloud-diensten, niet naar websites. Zie [het implementeren van een Azure-website](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false)voor meer informatie over het implementeren van websites.

## <a name="prerequisites"></a>Vereisten

Voordat u uw webtoepassing naar Azure publiceren kunt, moet u een Microsoft-account en een Azure-abonnement hebt en u hebt uw webtoepassing koppelen aan een Azure cloud-service. Als u al deze taken hebt voltooid, kunt u naar de volgende sectie overslaan.

1. Een Microsoft-account en een abonnement op Azure opvragen. U kunt proberen een gratis een maand gratis Azure abonnement [hier](https://azure.microsoft.com/pricing/free-trial/)

1. Maak een cloud-service en een account voor opslag op Azure. U kunt dit doen vanuit Server Explorer in Visual Studio, of met de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Uw webtoepassing voor Azure inschakelen. Als u wilt dat uw webtoepassing naar Azure vanuit Visual Studio worden gepubliceerd, moet u koppelen aan een Azure cloud service-project in Visual Studio. De bijbehorende cloud service om project te maken, opent u het snelmenu voor het project voor de webtoepassing en kies vervolgens converteren, **omzetten in Azure Cloud Service Project**.

1. Nadat het project cloud service is toegevoegd aan uw oplossing, opnieuw hetzelfde snelmenu openen en kies vervolgens **publiceren**. Zie voor meer informatie over het inschakelen van toepassingen voor Azure [procedure: migreren en publiceren van webtoepassingen naar een Azure Cloud-Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Zorg ervoor dat Visual Studio starten met beheerdersreferenties (als Administrator uitvoeren).

1. Wanneer u voor het publiceren van uw toepassing, opent u het snelmenu voor het project Azure cloud-service, en kies vervolgens **publiceren**. De volgende stappen weergeven de wizard Publiceren Azure toepassing.

## <a name="choosing-your-subscription"></a>Uw abonnement kiezen

### <a name="to-choose-a-subscription"></a>Een abonnement kiezen

1. Voordat u de wizard voor de eerste keer gebruikt, moet u zijn aangemeld. Kies de koppeling **Aanmelden** . Aanmelden bij de Azure portal wanneer daarom wordt gevraagd en uw Azure gebruikersnaam en wachtwoord opgeven. 

    ![Dit is een van de schermen van de wizard Publiceren](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    De lijst met abonnementen wordt gevuld met de abonnementen aan uw account gekoppeld. U ziet ook abonnementen van abonnementsbestanden die u eerder hebt geïmporteerd.

1. Kies het abonnement moet worden gebruikt voor deze installatie in de lijst **Kies je abonnement** .

   Als u **< beheren >**kiest, verschijnt het dialoogvenster **Abonnementen beheren** en kunt u het abonnement en de gebruiker account die u wilt gebruiken. Het tabblad **Accounts** kunt u al uw accounts en op het tabblad **abonnementen** worden alle abonnementen die zijn gekoppeld aan de rekeningen. U kunt ook een regio van waaruit Azure bronnen, alsmede maken of importeren van certificaten voor uw abonnement via de portal Azure. Als u abonnementen geïmporteerd uit een abonnementsbestand, weergegeven de bijbehorende certificaten onder het tabblad **certificaten** . Wanneer u klaar bent, kiest u de knop **sluiten** .

    ![Abonnementen beheren](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Een abonnementsbestand bevat meer dan één abonnement.

1. Kies de knop **volgende** om door te gaan. 

    Als er geen cloud-services niet op uw abonnement, moet u een cloud-service in Azure als host voor uw project maken. Het dialoogvenster **Cloud-Service maken en opslag Account** wordt weergegeven.

    Geef een nieuwe naam voor de service cloud. De naam moet uniek zijn in Azure. Geef een regio of een groep voor een datacenter bij u of uw klanten de meeste affiniteit. Deze naam wordt ook gebruikt voor een nieuwe account voor opslag die Azure voor uw service cloud maakt.

1. Wijzig de gewenste instellingen voor deze installatie en vervolgens publiceren op de knop **publiceren** (het volgende gedeelte vindt u meer informatie over de verschillende instellingen) kiezen. Om de instellingen voor het publiceren wilt bekijken, kiest u de knop **volgende** .

    >[AZURE.NOTE] Als u in deze stap publiceren hebt gekozen, kunt u de status van deze installatie in Visual Studio kunt controleren.

U kunt algemene en geavanceerde instellingen voor een implementatie met behulp van de wizard **Publiceren Azure toepassing** wijzigen. U kunt bijvoorbeeld een instelling voor de implementatie van de toepassing naar een testomgeving voordat u het verstuurt. De volgende afbeelding ziet u het tabblad **Algemene instellingen** voor een Azure-implementatie.

![Algemene instellingen](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Configureren van uw publicatie-instellingen

### <a name="to-configure-the-publish-settings"></a>De publicatie-instellingen configureren

1. Voer een van de volgende reeksen stappen in de lijst **service Cloud** :

   1. Kies in de vervolgkeuzelijst een bestaande cloud-service. De locatie van de data center voor de service verschijnt. U moet deze locatie en zorg ervoor dat uw opslaglocatie in het midden met dezelfde gegevens.

    1. Kies **Nieuw** Azure waarop voor een cloud-service maken. Geef een naam voor de service in het dialoogvenster **Cloud-Service maken** en geeft u een regio of affiniteit groep geeft u de locatie van de gewenste host voor deze service cloud Datacenter. De naam moet uniek zijn in Azure.

1. Kies in de lijst **omgeving** **productie** of **klaarzetten**. Kies de staging-omgeving als u wilt dat uw toepassing een testomgeving te implementeren. U kunt uw toepassing naar de productieomgeving gaan.

1. Kies in de lijst **configuration bouwen** **Debug** of **Release**.

1. Kies in de lijst **Service** **Cloud** of **lokale**.

    Schakel het selectievakje **Extern bureaublad inschakelen voor alle rollen** als u op afstand verbinding maken met de service. Deze optie wordt voornamelijk gebruikt voor het oplossen van problemen. Als u dit selectievakje inschakelt, verschijnt het dialoogvenster **Configuratie voor extern bureaublad** . Kies de koppeling instellingen wijzigen van de configuratie.

    Schakel het selectievakje **Inschakelen Web implementeren voor alle web-rollen** in web-implementatie voor de service inschakelen. U kunt Extern bureaublad voor deze functie moet inschakelen. Zie [[een Cloud-Service met behulp van de hulpprogramma's voor Azure publiceren](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)voor meer informatie. Zie [[een Cloud-Service met behulp van de hulpprogramma's voor Azure publiceren](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)voor meer informatie over het implementeren van webonderdelen.

1. Kies het tabblad **Geavanceerde instellingen** . Accepteer de standaardnaam in het veld **label implementatie** of voer een naam in van uw keuze. Als u wilt de datum toevoegen aan het label implementatie, laat u het selectievakje ingeschakeld.

    ![Derde scherm van de Wizard publiceren](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Kies in de lijst met **accounts voor opslag** , de opslag-account moet worden gebruikt voor deze installatie. Vergelijk de locaties van de datacenters voor uw cloud-service en de account van uw opslag. In het ideale geval moeten deze locaties hetzelfde zijn.

    >[AZURE.NOTE] De opslag van Azure rekening slaat het pakket voor de implementatie van toepassingen. Nadat de toepassing is geïnstalleerd, wordt het pakket verwijderd uit de opslag-account.

1. Schakel het selectievakje **implementatie bijwerken** in als u alleen bijgewerkte onderdelen implementeren. Dit type implementatie is sneller dan een volledige implementatie. Kies de koppeling **Instellingen** voor het openen van het dialoogvenster **implementatie-instellingen bijwerken** in de volgende afbeelding wordt weergegeven. 

    ![Implementatie-instellingen](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    U kunt een van de twee opties voor de implementatie van de incrementele of gelijktijdige. Een stapsgewijze implementatie werkt een geïmplementeerde exemplaar tegelijk, zodat uw toepassing online en beschikbaar voor gebruikers blijft. Een gelijktijdige distributie bijgewerkt geïmplementeerde overal tegelijk. Gelijktijdig bijwerken is sneller dan een incrementele update, maar als u deze optie kiest, uw toepassing mogelijk niet beschikbaar tijdens het bijwerken.

    Moet u het selectievakje voor als implementatie kan niet worden bijgewerkt, een volledige implementatie te doen als u wilt dat de installatie volledig automatisch laten plaatsvinden als een update-installatie mislukt. Een volledige implementatie wordt het virtuele IP-(VIP)-adres voor de cloud-service. Zie voor meer informatie [hoe: behouden een constante virtueel IP-adres voor een Cloud-Service](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Schakel het selectievakje **IntelliTrace inschakelen** om te debuggen uw service, of als u een configuratie **voor foutopsporing** implementeren wilt en wilt opsporen in Azure het cloud-service, schakel het selectievakje **Externe foutopsporing inschakelen voor alle rollen** op de externe debugging services implementeren.

2. Schakel het selectievakje **Schakel profilering** in om het profiel van de toepassing, en kies vervolgens de koppeling **Instellingen** te profileren weergaveopties. 


    >[AZURE.NOTE] Moet u Visual Studio Ultimate IntelliTrace of laag interactie profilering (TIP) inschakelen en u niet beide tegelijkertijd inschakelen.

    Zie [Foutopsporing in een Cloud-Service gepubliceerd met IntelliTrace en Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) en [testen van de prestaties van een Cloud-Service](https://msdn.microsoft.com/library/azure/hh369930.aspx)voor meer informatie.

1. Kies **volgende** om de overzichtspagina voor de toepassing weer te geven.

## <a name="publishing-your-application"></a>Uw toepassing te publiceren

1. U kunt een publicatie profiel maken van de instellingen die u hebt gekozen. U kunt bijvoorbeeld een profiel voor een testomgeving en een andere voor de productie maken. Sla dit profiel, kies het pictogram **Opslaan** . De wizard profiel wordt gemaakt en opgeslagen in de Visual Studio-project. U wijzigt de profielnaam, opent u de lijst **doelprofiel** en kies vervolgens **< beheren >**.

    ![Samenvatting scherm van de Wizard publiceren](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Profiel voor publiceren wordt weergegeven in Solution Explorer in Visual Studio en de profielinstellingen worden geschreven naar een bestand met de extensie .azurePubxml. Instellingen worden opgeslagen als kenmerken van XML-codes.

1. Kies **publiceren** voor het publiceren van uw toepassing. U kunt de processtatus in het venster **uitvoer** van Visual Studio kunt controleren.

## <a name="see-also"></a>Zie ook

[Procedure: migreren en publiceren van webtoepassingen naar een Azure Cloud-Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Een Cloud-service met behulp van de hulpprogramma's voor Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Opsporen van fouten in een gepubliceerde Cloud Service met IntelliTrace en Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[De prestaties van een Cloud-Service testen](https://msdn.microsoft.com/library/azure/hh369930.aspx)

