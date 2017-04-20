<properties
    pageTitle="Azure AD gezondheid Veelgestelde vragen over verbinding"
    description="Deze Veelgestelde vragen antwoorden op vragen over gezondheid verbinding Azure AD. Deze Veelgestelde vraag heeft betrekking op vragen over het gebruik van de service, inclusief de facturatie model, mogelijkheden, beperkingen en ondersteuning."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD verbinding gezondheid Veelgestelde vragen (FAQ)

Deze Veelgestelde vragen antwoorden op vragen over gezondheid verbinding Azure AD. Deze Veelgestelde vraag heeft betrekking op vragen over het gebruik van de service, inclusief de facturatie model, mogelijkheden, beperkingen en ondersteuning.

## <a name="general-questions"></a>Algemene vragen



**V: ik beheren meerdere AD Azure mappen. Hoe schakel ik een met Azure Active Directory Premium**

U kunt schakelen tussen verschillende Azure AD huurders door te selecteren op dit moment ondertekend in naam van de gebruiker in de rechterbovenhoek en kiezen van de juiste account. Als de account hier niet wordt vermeld, selecteert u afmelden en vervolgens de globale admin referenties gebruikt van de map met Azure Active Directory Premium aanmelden is ingeschakeld.

## <a name="installation-questions"></a>Vragen betreffende de installatie



**V: Wat is de impact van de Azure AD verbinding Health Agent installeren op afzonderlijke servers?**

Het effect van het installeren van de Microsoft Azure AD verbinding Health Agents ADFS, Web Application Proxy servers, Azure AD verbinden (sycn)-servers domeincontrollers is minimaal voor de CPU, geheugen verbruik netwerkbandbreedte en opslag.

De onderstaande getallen zijn een benadering.

- CPU-verbruik: ~ 1% verhogen
- Geheugengebruik: maximaal 10% van de totale hoeveelheid systeemgeheugen

>[AZURE.NOTE] Bij de agent kan niet communiceren met Azure wordt, slaat de agent lokaal, tot een opgegeven maximum. De agent wordt overschreven door de gegevens in de "cache" op basis van 'laatst verwerkt'.

- Opslag van lokale buffer voor Azure AD verbinding Health Agents: ~ 20 MB
- Voor AD FS-servers verdient het inrichten van een ruimte op de schijf van 1024 MB (1 GB) voor AD FS controleren kanaal voor Azure AD verbinding Health Agents voor het verwerken van de gecontroleerde gegevens voordat deze wordt overschreven.

**V: heb ik over mijn servers opnieuw wordt opgestart tijdens de installatie van de Azure AD verbinding Health Agents?**

Nr. De server opnieuw opstarten is niet nodig voor de installatie van de agents zijn vereist. Installatie van enkele van de vereiste stappen kan echter opnieuw starten van de server.

In Windows Server 2008 R2 de installatie van .net vereist Framework 4.5 bijvoorbeeld een server opnieuw opstarten.


**V: is Azure AD verbinding gezondheidsdiensten werken door middel van een Pass Through-HTTP-proxy?**

Ja.  Voor op gaan bewerkingen, kunt u de Health Agent voor het doorsturen van uitgaande HTTP-aanvragen via een HTTP-Proxy. Zie voor meer informatie [Azure configureren AD verbinding Health Agents HTTP-Proxy gebruikt.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Als u een proxy configureren tijdens de registratie van de Agent, moet u mogelijk vooraf de proxyinstellingen van Internet Explorer wijzigen.
1. Open Internet Explorer-instellingen > Internet -> Opties -> verbindingen -> LAN-instellingen.
2. Selecteer een proxyserver voor het LAN-netwerk gebruiken.
3. Selecteer Geavanceerd als u verschillende proxyservers poorten voor HTTP en HTTPS/Secure.

**V: Azure AD verbinding gezondheidsdiensten Basisverificatie wel ondersteunt bij het verbinden met de HTTP-proxy's?**

Nr. Een mechanisme voor het opgeven van willekeurige gebruikersnaam en wachtwoord voor basisverificatie wordt momenteel niet ondersteund.


**Q: welke versie van AD DS worden ondersteund door gezondheid verbinding Azure AD voor AD DS?**

Controle van AD DS, wordt geïnstalleerd op de volgende versies van OS ondersteund:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Bewerkingen vragen



**V: moet ik op mijn AD FS Application Proxy Servers of mijn Web Application Proxy Servers controle inschakelen?**

Nee, controle hoeft niet te worden ingeschakeld op de Servers Web Application Proxy (WAP). Inschakelen op de AD FS-servers.


**V: hoe Azure AD verbinding gezondheid waarschuwingen krijgen opgelost?**

Azure AD verbinding gezondheid waarschuwingen krijgen opgelost op een voorwaarde voor succes. Azure AD verbinding Health Agents detecteren en rapporteren van de voorwaarden voor succes met de service op periodieke basis. Voor enkele waarschuwingen is de onderdrukking op basis van tijd. Met andere woorden, als het dezelfde fout niet wordt waargenomen binnen 72 uur na het genereren van waarschuwingen, de waarschuwing automatisch opgelost.




**V: welke firewallpoorten moet ik open voor de Azure AD verbinding Health Agent om te werken?**

U moet het TCP/UDP-poort 443 en open de Azure AD verbinding Health Agent kan communiceren met de service-endpoints Azure AD gezondheid 5671 hebben.


**V: Waarom zie ik twee servers met dezelfde naam in Azure AD verbinding gezondheid Portal?**

Wanneer u een agent van een server verwijdert, wordt de server niet automatisch verwijderd van Azure AD verbinding Portal.  Als u handmatig een agent verwijderd van een server of de server zelf verwijderd, moet u de serververmelding handmatig te verwijderen uit de portal Azure AD verbinding maken met de gezondheid. Ga voor meer informatie naar [een exemplaar van de server of service verwijderen.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Als u een server reimaged of een nieuwe server met de dezelfde gegevens (zoals de naam van de computer gemaakt) en de reeds geregistreerde server niet uit de portal Azure AD verbinding maken met de gezondheid verwijderen heeft, de agent geïnstalleerd op de nieuwe server, kunnen er twee vermeldingen met dezelfde naam.  
In dit geval moet u de post die deel uitmaken van de oudere server handmatig verwijderen. De gegevens voor deze server moet verlopen.

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD Health Agent-installatie aansluiten](active-directory-aadconnect-health-agent-install.md)
* [Azure AD verbinding gezondheid bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van gezondheid Azure AD verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Met behulp van Azure AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
