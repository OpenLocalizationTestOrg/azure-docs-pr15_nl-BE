
<properties 
    pageTitle="Uw abonnement op Office 365 gebruiken met Azure RemoteApp | Microsoft Azure"
    description="Informatie over hoe u kunt uw abonnement op Office 365 in Azure RemoteApp-Office-toepassingen delen."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Uw abonnement op Office 365 gebruiken met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wist u dat kunt u uw bestaande Office 365-abonnement in Azure RemoteApp Office apps van de cloud te delen? Lees verder voor meer informatie over de Office 365 + Azure RemoteApp-opties, maken inclusief koppelingen naar artikelen over Office 365, die u helpen het meeste uit uw abonnement.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Hoe gebruik ik Office 365-accounts voor Azure RemoteApp?
Nieuw artikel voor alle informatie van Peter uitchecken: [Azure RemoteApp met Office 365-gebruikersaccounts gebruiken](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Kan ik mijn abonnement op Office 365 naar Office-toepassingen uitvoeren in Azure RemoteApp gebruiken?

Ja! Met behulp van uw abonnement op Office 365 is in feite de enige manier om uw Office-toepassingen op Azure RemoteApp.

(Opmerking: als de implementatie van RemoteApp-Azure wordt geleverd door een partner, hosting, ze mogelijk om Office-licenties op basis van een [Gebruiksrechtovereenkomst van Service Provider](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Het aardige van uw abonnement op Office 365 is dat u de gebruikerslicentie gebruiken op veel verschillende platforms en omgeving kunt, inclusief de Azure cloud. Wanneer u Office-toepassingen in Azure RemoteApp gebruikt hoeft u niet extra licenties kopen of uw bestaande licenties configureren op een speciale manier. Alles wat u nodig hebt is een abonnement op Office 365 met [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus kan [gedeelde computer activeren](https://technet.microsoft.com/library/Dn782860.aspx) - met deze functie kunt tijdelijke activering op basis van gebruiker voor Office in virtueel en cloud-omgevingen zoals Azure RemoteApp (en extern bureaublad-Services).

Welke Office 365 's Office 365 ProPlus opnemen? De tabel [beschikbaarheid van de Service in elk plan](https://technet.microsoft.com/library/office-365-plan-options.aspx) uitchecken. Houd er rekening mee dat niet alle regelingen Office 365 ProPlus (bijvoorbeeld de Office 365 Business plan omvatten). Als de indeling niet ondersteunt, kunt u upgraden naar een plan dat (bijvoorbeeld Office 365 Education E3 is).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, hoe worden mijn Office 365 ProPlus licenties gebruikt in combinatie met Azure RemoteApp?

Elke gebruiksrechtovereenkomst of licentie voor Office 365 ProPlus kunt één gebruiker activeren van Office-toepassingen op maximaal 5 computers en tablets en telefoons. Bij elke activering is geregistreerd bij de gebruiker totdat ze Office op het apparaat deactiveren. (Gebruikers kunnen hun apparaten in de [Office 365 portal](https://portal.office365.com/)beheren).

Met RemoteApp-Azure een enkele gebruiker kan in het logboekbestand naar meerdere computers op dezelfde dag zonder dit te beseffen. Dat komt omdat de service automatisch wordt beheerd en bronnen in de cloud, geschaald, terwijl de gebruiker ziet alleen de apps en programma's die u hebt gedeeld. Voor dit scenario Office 365 ProPlus een gedeelde computer-modus activeren biedt-dit betekent dat die gebruiker niet hoeft te doen een Licentiebeheer voor toegang tot de bronnen en die de afzonderlijke computers niet meegeteld in de limiet van 5 computer activeren.

Als u (admin) Office 365 ProPlus licenties aan gebruikers toewijst, kunnen ze Office gebruiken op hun persoonlijke apparaten en uw Azure RemoteApp-collectie.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Welke Office-toepassingen kan ik met Office 365 en Azure RemoteApp gebruiken?

U kunt uw Office 365-abonnement activeren en delen van Office 2013 in Azure RemoteApp-implementaties. We ondersteunen op dit moment niet het gebruik van andere versies van Office met RemoteApp-Azure. Dit omvat de Office 2003, Office 2007, Office 2010 en Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Hoe zit het met Pro Visio of Project Pro?

De Office 365 ProPlus afbeelding RemoteApp-abonnement bevat zowel Visio Pro en Pro-Project. Maar u uw abonnement op Office 365 ProPlus niet gebruiken voor het activeren van deze programma's - ze elk hun eigen licentie hebben. In de [Office 365 portal](https://portal.office365.com/)kunt u deze activeren. 

U hebt geen licentie voor deze programma's als u niet wilt gebruiken. Alleen de programma's die u wilt gebruiken - en overslaan van de andere activeren. U zult ze nog steeds zien in de afbeelding, maar u deze niet gebruiken. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hoe ga ik aan de slag met Office 365 en Azure RemoteApp?

Als u weet dat de details van een licentie voor Office 365, krijgen we u aan de slag met Azure RemoteApp - het is heel eenvoudig:

Wanneer u uw Azure RemoteApp-collectie maakt, kunt u de **Office 365 ProPlus (abonnement vereist)** afbeelding gebruiken.

![Azure RemoteApp-afbeelding met Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Deze afbeelding bevat de meest recente versie van Windows Server en Office 365 ProPlus. Nadat u uw collectie (inclusief publishing apps), de gebruikers gewoon geconfigureerd log in Azure RemoteApp (met behulp van de RemoteApp-client) en de Office 365-referenties opgeven voor alle Office-toepassingen. Licenties worden automatisch geleverd zonder een willekeurige set up of beheer vereist.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Kan ik een aangepaste afbeelding maken met Office 365 ProPlus

U kunt een aangepaste afbeelding maken voor de collectie met Office 365 ProPlus. Er zijn 2 opties - de Azure galerijafbeelding wij gebruiken of kunt u uw eigen aangepaste afbeelding maken en installeren van Office 365 ProPlus er.

### <a name="use-the-azure-gallery-image"></a>Gebruik de Azure galerijafbeelding

De eenvoudigste manier om het implementeren van Office 365 ProPlus aan een collectie is te [beginnen met een van de Azure galerijafbeeldingen](remoteapp-image-on-azurevm.md) Azure RemoteApp-abonnement. Zorg ervoor dat u de installatiekopie van **Windows Server extern bureaublad-sessiehost met Office 365 ProPlus vooraf geïnstalleerd** . Vervolgens installeert u eventuele andere gewenste apps op die afbeelding en u bent goed om te gaan.

### <a name="use-a-custom-image"></a>Een aangepaste afbeelding gebruiken

U kunt altijd een aangepaste afbeelding maken - u kunt een [Azure VM](remoteapp-image-on-azurevm.md) [maken](remoteapp-create-custom-image.md) of de afbeelding lokaal maken en uploaden naar Azure. In beide gevallen moet dat u installeren Office 365 ProPlus met het knooppunt voor gedeelde computer activeren. De [Office Deployment Tool](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) gebruiken en volg de [instructies](https://technet.microsoft.com/library/Dn782858.aspx) voor de installatie.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Automatische updates voor Office 365 ProPlus uitschakelen in uw image aangepaste - belangrijk

Uw aangepaste installatiekopie wordt gebruikt door Azure RemoteApp als een sjabloon voor het toevoegen van extra bronnen als de vraag van de stijgingen van de gebruikers. Als u wilt voorkomen dat vertragingen en problemen met de verbinding, moet u automatische updates uitschakelen voor Office in de afbeelding. Als u dit niet doet, wordt elke resource die met die sjabloon zijn gemaakt, automatisch bijgewerkt worden wanneer deze wordt gestart. Gebruik de standaard Azure RemoteApp-proces voor het bijwerken van uw aangepaste installatiekopie. Op die manier u eenmaal op de Sjabloonafbeelding in de Office-toepassingen bijwerken en laat RemoteApp Azure ervoor te zorgen dat de updates voor uw gebruikers.

Automatische updates uitschakelen, voeg de volgende in het configuratiebestand van de Office Deployment Tool:

        <Updates Enabled="FALSE" />

Het bestand moet nu deze regels bevatten:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Hoe kan ik een afbeelding bijwerken met Office 365 ProPlus?

Er zijn veel redenen voor het bijwerken van de afbeelding in uw verzameling. Hier zijn een paar:

- Download de nieuwste updates voor Windows 
- Download de nieuwste updates voor Office 365 ProPlus toepassing
- Uw aangepaste app bijwerken
- Andere configuratie-instellingen voor de afbeelding zelf wijzigen

De end-to-end stappen voor het bijwerken van uw verzameling voor het gebruik van de afbeelding die u hebt bijgewerkt, gaat u [hier](remoteapp-update.md). Maar voor informatie over het bijwerken van de afbeelding en de Office 365 ProPlus, bekijk de volgende informatie.

U hebt twee opties voor het bijwerken van uw image - vervangt de afbeelding met een volledig nieuw of handmatig bijwerken van de bestaande afbeelding.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>De afbeelding vervangen door de meest recente Azure galerijafbeelding + aanpassingen toevoegen
Met deze optie kunt u Microsoft zorgen voor de updates voor Windows Server en Office 365 ProPlus. In plaats van de bestaande afbeelding bijwerkt, maakt u een volledig nieuwe afbeelding op basis van de laatste afbeelding. Herhaal de stappen die u hebt gedaan voordat de installatiekopie aanpassen-aangepaste toepassingen, wijzigen van de configuratie van de afbeelding, enzovoort.

De afbeeldingen worden regelmatig bijgewerkt, plaatst u eenvoudig, wetende dat uw Windows Server en Office 365 ProPlus apps bijgewerkt zijn. De verhouding is natuurlijk dat hebt u uw aanpassingen van toepassing telkens wanneer u een nieuwe afbeelding. U kunt scripts voor het automatiseren van het instellen van uw aanpassingen maken.

### <a name="manually-update-your-existing-image"></a>De bestaande afbeelding handmatig bijwerken

Met deze optie kunt u standaardprogramma's van Windows updates toepassen op de afbeelding. Gebruik de Office Deployment tool downloaden en installeren van de meest recente versies van Office 365 ProPlus of updates voor Office 365 ProPlus.

> [AZURE.IMPORTANT] Vergeet niet - Office 365 ProPlus automatische updates uitschakelen.

Meer informatie over het gebruik van de Office Deployment Tool updates nodig?

- [Klik en klaar voor Office 365-producten implementeren met behulp van de Office Deployment Tool](https://technet.microsoft.com/library/JJ219423.aspx)
- [Distribueren en bijwerken van Office 365 ProPlus met behulp van de Office Deployment Tool](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
- [Update-instellingen configureren voor Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)
