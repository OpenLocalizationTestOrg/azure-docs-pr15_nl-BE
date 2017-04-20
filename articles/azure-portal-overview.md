<properties
    pageTitle="Microsoft Azure portal overzicht"
    description="Informatie over het gebruik van de portal voor Microsoft Azure."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Microsoft Azure portal overzicht

De portal voor Microsoft Azure is een centrale plaats waar u inrichten en beheren van uw resources Azure.  In deze zelfstudie wordt u vertrouwd met de portal en hoe u een aantal mogelijkheden voor deze sleutel gebruiken:
- Een **uitgebreide marketplace** kunt u bladeren door duizenden items van Microsoft en andere leveranciers die kunnen worden gekocht en/of ingericht.
- Een **uniforme en schaalbare bladeren ervaring** die gemakkelijk te vinden van de resources die u interesseren en uitvoeren van verschillende bewerkingen van het management.
- **Samenhangend beheer pagina 's** (of blades) waarmee u het beheer van Azure breed scala van diensten door middel van een consistente manier dat instellingen, acties, factuurgegevens, controle en het gebruik van gezondheidsgegevens, en veel meer.
- Een **persoonlijke ervaring** die u kunt maken van een aangepaste begin scherm met daarin de gegevens die u wilt zien wanneer u zich aanmeldt.  U kunt ook een van de beheer-blades met tegels.

 ![Azure Portal UI afdrukstand][UIOrientation]

## <a name="before-you-get-started"></a>Voordat u begint

U moet een geldige Azure abonnement via deze zelfstudie.  Als u nog geen, vervolgens [Aanmelden voor een gratis evaluatie van](https://azure.microsoft.com/pricing/free-trial/) vandaag hebt.  Als u een abonnement hebt, kunt u toegang tot de portal op [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Het maken van een resource

Azure is een markt met duizenden items die u vanaf één locatie kunt maken.  Stel dat u wilt maken van een nieuwe Windows Server 2012 VM.  De + nieuwe hub is uw toegangspoort tot een curated reeks beschikbare categorieën uit de markt.  Elke categorie heeft een kleine set topaanbiedingen met een koppeling op de volledige markt waarin alle categorieën en zoeken. Als u wilt dat nieuwe Windows Server 2012 VM hebt gemaakt, kunt u de volgende acties uitvoeren:  

1.  Windows Server 2012 is uitgerust, zodat u deze uit de categorie berekenen selecteren kunt.  
2.  Sommige eenvoudige invoer op een formulier invullen.
3.  Klik op 'Maak' en de VM begint onmiddellijk inrichten.

De hub meldingen wordt u gewaarschuwd wanneer de bron is gemaakt en een blade management wordt geopend (u kunt altijd Bladeren naar bronnen verderop).

![Portal-categorieën][PortalCategories]


## <a name="how-to-find-your-resources"></a>Resources zoeken

U kunt veelgebruikte bronnen altijd vastmaken aan uw startboard, maar mogelijk moet u Ga naar iets dat u niet regelmatig gebruikt.  De onderstaande bladeren-hub is de manier om aan alle resources.  U kunt filteren op abonnement kiezen/grootte van kolommen en navigeer naar de blades beheer door te klikken op de afzonderlijke items.

![Hub bladeren][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Het beheren en Gemachtigdentoegang tot een bron

Uit deze blade die kunt u verbinding maken met de virtuele machine met behulp van extern bureaublad, controleren de voornaamste prestatiestatistieken ophalen, toegang tot deze VM met toegang op basis van de rol (RBAC), VM configureren en andere belangrijke taken worden verricht.  Delegeren van toegang op basis van de rol is essentieel voor het beheer van op schaal.  Klik [hier](./active-directory/role-based-access-control-configure.md) voor meer informatie hierover. Voor het delegeren van toegang tot een bron, kunt u de volgende acties uitvoeren:

1.  Blader naar de resource.
2.  Klik op alle instellingen in de sectie Essentials.
3.  Klik op 'Users' in de instellingenlijst.
4.  Klik op 'Toevoegen' in de opdrachtbalk.
5.  Kies een gebruiker en een rol.

![Een Resource beheren][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Het aanpassen van een resource blade

Azure configureert vooraf de blades voor uw resources, maar de tegels op deze blades zijn jou om te bepalen.  U kunt eenvoudig aanpassen in de modus toevoegen, verwijderen, het formaat of het opnieuw rangschikken van de tegels gaan. Als u wilt een blade aanpassen, moet u de volgende acties uitvoeren:

1.  Blader naar de resource.
2.  Klik op de '...' aan de bovenkant van het blad dat u wilt aanpassen.
3.  Klik op "Delen toevoegen".
4.  Begin te slepen en neerzetten van onderdelen.  

![Blades aanpassen][CustomizeBlades]

## <a name="how-to-get-help"></a>Help opvragen

Hebt u ooit een probleem, we hier voor u.  De portal is een pagina help en ondersteuning kunt u in de juiste richting wijst.  Afhankelijk van uw [plan ondersteuning](https://azure.microsoft.com/support/plans/)kunt u ook support tickets rechtstreeks in de portal te maken.  U kunt na het maken van een support ticket, de levenscyclus van het ticket van de binnen de portal beheren. Kun je de Help en ondersteuningspagina door te navigeren naar Bladeren -> Help + ondersteuning.  

![Help en ondersteuning][HelpSupport]

## <a name="summary"></a>Samenvatting

Eens kijken wat u in deze zelfstudie hebt geleerd:
- Hebt u geleerd hoe u aanmeldt, krijgt een abonnement en bladert u naar de portal
- U hebt geplaatst met de gebruikersinterface van de portal en geleerd hoe u kunt maken en resources zoeken
- Hebt u geleerd hoe u een bron maken en resources zoeken
- U hebt geleerd over de structuur of het beheer van blades en hoe u kunt verschillende typen bronnen consistent beheren
- Hebt u geleerd hoe u de portal om de informatie aanpassen u geïnteresseerd naar voren en center
- Hebt u geleerd hoe u toegang tot bronnen met toegang op basis van de rol RBAC)
- U hebt geleerd hoe u help en ondersteuning

De portal voor Microsoft Azure drastisch te vereenvoudigen maken en beheren van uw toepassingen in de cloud.  Bekijk de [blog management](https://azure.microsoft.com/blog/topics/management/) aan te houden zoals we voortdurend zijn [luisteren naar feedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) en verbeteringen aanbrengen.  [Blog van ScottGu](http://weblogs.asp.net/scottgu) is een andere goede plaats om te zoeken naar updates voor alle Azure.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
