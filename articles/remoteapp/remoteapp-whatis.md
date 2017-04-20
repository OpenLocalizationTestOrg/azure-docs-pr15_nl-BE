<properties 
    pageTitle="Wat is Azure RemoteApp? | Microsoft Azure" 
    description="Informatie over het delen van toepassingen en bronnen aan elk apparaat via RemoteApp Azure." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Wat is Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp wordt de functionaliteit van het programma op het bedrijf Microsoft RemoteApp, ondersteund door extern bureaublad-Services op Azure. Azure RemoteApp kunt u een veilige, externe toegang bieden tot toepassingen van veel apparaten van andere gebruiker. Azure RemoteApp in feite fungeert als host voor niet-permanente Terminal Server-sessies in de cloud en krijgt u ze gebruiken en deze delen met uw gebruikers.

U kunt met Azure RemoteApp-toepassingen en bronnen delen met gebruikers op vrijwel elk apparaat. We hosten uw toepassingen in de cloud, wat betekent dat wij zorgen voor de hardware en schaalbaarheid om te voldoen aan de vereisten van gebruikers. Hoeft u uploaden van de apps die u wilt delen en vervolgens gebruikers gebruiken deze apps is. [Gebruikers krijgen om hun eigen apparaten](remoteapp-clients.md), terwijl u alles via de Azure portal beheren. Ook hebt u de mogelijkheid van het gebruik van uw zakelijke gegevens kunt u de beveiliging van gegevens en toepassingen.

Lees verder voor meer informatie over RemoteApp Azure, of, als we u, [Probeer het nu](https://azure.microsoft.com/services/remoteapp/)al hebben overtuigd.

Hebt u vragen over Azure RemoteApp? Bekijk onze [Veelgestelde vragen](remoteapp-faq.md).

Azure RemoteApp is onderdeel van de [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nieuw!** Wilt u meer informatie over RemoteApp Azure? Of wilt u RemoteApp Azure op schaal valideren? Lid worden van onze wekelijkse [vraagt de deskundigen webinar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp-collecties
Er zijn twee soorten [Azure RemoteApp collecties](remoteapp-collections.md):


- Een **collectie van de wolk** wordt gehost in en gegevens voor de programma's worden opgeslagen in de cloud. Gebruikers hebben toegang tot apps door aan te melden met hun Microsoft-account of zakelijke referenties gesynchroniseerd of federatieve met Azure Active Directory.

    Kies een cloud-collectie als de toepassing die u wilt delen geen een verbinding met een bron vereist van uw particuliere netwerk (bijvoorbeeld via een VPN-apparaat). Als de toepassing gebruikmaakt van bronnen op het Internet, OneDrive of Azure, werkt een wolk collectie voor u. Het is ook de snelste manier om te maken.

- Een **collectie van hybride** wordt gehost in en worden gegevens opgeslagen in Azure cloud maar kunnen gebruikers toegang tot gegevens en bronnen die zijn opgeslagen op uw lokale netwerk. Gebruikers hebben toegang tot apps door aan te melden met hun zakelijke referenties gesynchroniseerd of federatieve met Azure Active Directory.

    Kies een hybride-collectie als u een verbinding met de bronnen op het particuliere netwerk van uw bedrijf. Bijvoorbeeld, als de toepassing moet toegang hebben tot een van de volgende handelingen uit:

    - Bestandsservers die zich op uw intranet
    - Quicken
    - Databases achter een firewall

    Dit is over het algemeen meer handig voor grote bedrijven met een groot aantal bronnen op hun particuliere netwerken die niet worden verplaatst naar de cloud.

De verschillende collecties hebben verschillende opties, waaronder netwerken, zodat u uitzoeken [welke collectie](remoteapp-collections.md) werkt het beste voor u. 


### <a name="updating-your-collection"></a>De collectie bijwerken
Een van de belangrijkste verschillen tussen de hybride en cloud collecties is hoe software-updates worden verwerkt. Met een cloud-collectie die de vooraf geïnstalleerde Office 365 ProPlus of Office 2013 afbeelding er geen zorgen te maken over eventuele updates. De service zelf onderhoudt en updates worden doorlopend, toepassingen en het besturingssysteem.

Hybride collecties, zowel voor als cloud-collecties die de afbeelding van een aangepaste sjabloon gebruiken, bent u verantwoordelijk voor het onderhouden van de afbeelding en apps. U kunt updates bepalen met behulp van hulpprogramma's zoals Windows Update, Groepsbeleid of System Center voor afbeeldingen in een domein behoren.

Na het bijwerken van uw image aangepaste sjabloon, kunt u de nieuwe afbeelding te uploaden naar de cloud Azure en werk vervolgens de collectie voor het gebruik van de nieuwe afbeelding. (U kunt dit doen vanuit de Azure RemoteApp- **Quick Start** -pagina of het Dashboard.)

Zie [de collectie bijwerken](remoteapp-update.md) voor meer informatie.

## <a name="supported-remoteapp-clients"></a>Ondersteunde RemoteApp-clients
Azure RemoteApp wordt ondersteund op de apps RemoteApp-client voor Windows en Windows RT, als de Microsoft Remote Desktop apps voor Mac, iOS en Android. Uw gebruikers kunnen deze apps gebruiken op hun mobiele of apparaten voor toegang tot de nieuwe Azure RemoteApp-programma's te berekenen.

Zie [toegang krijgen tot uw toepassingen in Azure RemoteApp](remoteapp-clients.md) voor meer informatie over de client.

## <a name="next-steps"></a>Volgende stappen
Go! Probeer het zelf! Deze artikelen te krijgen u aan de slag met Azure RemoteApp:

- [Wat voor soort collectie hebt u nodig voor Azure RemoteApp?](remoteapp-collections.md)
- [Maak een afbeelding van Azure RemoteApp](remoteapp-imageoptions.md)
- [Het maken van een wolk Azure RemoteApp-collectie](remoteapp-create-cloud-deployment.md)
- [Het maken van een hybride Azure RemoteApp-collectie](remoteapp-create-hybrid-deployment.md)
- [Hoe werkt licentieverlening samen in Azure RemoteApp?](remoteapp-licensing.md)
- [Aanbevolen procedures voor het gebruik van Azure RemoteApp](remoteapp-bestpractices.md)
- [Azure RemoteApp-Veelgestelde vragen](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Help ons u te helpen 
Wist u dat naast de beoordeling van dit artikel en het toevoegen van opmerkingen naar beneden onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreekt? Iets mis? Schrijf iets dat is gewoon verwarrend? Omhoog schuiven en klik op **bewerken op GitHub** of **bewerken** als u wijzigingen wilt - die voor revisie naar ons komen, en vervolgens, wanneer we deze afmelden, ziet u de wijzigingen en verbeteringen hier.