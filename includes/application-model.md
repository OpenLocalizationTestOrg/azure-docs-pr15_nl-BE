# <a name="compute"></a>Berekenen

Azure kunt u implementeren en bewaken van uw toepassingscode uitgevoerd in een datacenter van Microsoft. Wanneer u een toepassing maken en uitvoeren op Azure, de code en configuratie samen heet een Azure gehoste service. Hosted services zijn eenvoudig te beheren, omhoog en omlaag te schalen, configureren en bijwerken met nieuwe versies van de code van uw toepassing. Dit artikel richt zich op de Azure gehoste service application-model.<a id="compare" name="compare"></a>

## Inhoudsopgave<a id="_GoBack" name="_GoBack"></a>

-   [Voordelen van Azure toepassing Model][]
-   [Kernconcepten gehoste Service][]
-   [Overwegingen bij het ontwerpen van gehoste Service][]
-   [Ontwerpen van uw toepassing voor schaal][]
-   [Definitie van gehoste Service en configuratie][]
-   [Het definitiebestand voor][]
-   [Het configuratiebestand van de Service][]
-   [Maken en implementeren van een gehoste Service][]
-   [Verwijzingen][]

## <a id="benefits"> </a>Toepassing azure Model voordelen

Wanneer u uw toepassing als een gehoste service implementeert, Azure maakt u een of meer virtuele machines (VMs) die van uw toepassing code bevat en de VMs op fysieke machines die woonachtig zijn in een van de datacenters Azure wordt opgestart. Clientaanvragen naar uw gehoste toepassing invoert het datacenter, taakverdeling worden deze aanvragen evenredig aan het VMs. Terwijl uw toepassing wordt gehost in Azure, haalt deze drie belangrijke voordelen:

-   **Hoge beschikbaarheid.** Hoge beschikbaarheid betekent dat Azure zorgt ervoor dat de toepassing wordt uitgevoerd, zoveel mogelijk en kan reageren op aanvragen van clients. Als de toepassing wordt beëindigd (als gevolg van een niet-verwerkte uitzondering, bijvoorbeeld), en vervolgens Azure wordt dit gedetecteerd en automatisch wordt opnieuw de toepassing te starten. Als de computer uw toepassing wordt uitgevoerd op ervaringen een soort hardwarefout, vervolgens Azure ook dit detecteren en automatisch een nieuwe VM maken op een andere fysieke machine van werken en van daaruit uitvoeren van de code. Opmerking: Om uw toepassing te krijgen van Microsoft Service Level Agreement van 99.95% beschikbaar, hebt u ten minste twee VMs de toepassingscode uitgevoerd. Hierdoor kan een VM voor het verwerken van clientaanvragen terwijl Azure uw code van een mislukte VM naar een nieuwe, goede VM wordt.

-   **Schaalbaarheid.** Azure kunt u eenvoudig en dynamisch wijzigt het nummer van de VMs uw toepassingscode uitgevoerd voor het verwerken van de werkelijke belasting van uw toepassing. Hiermee kunt u aanpassen van de toepassing van de belasting die uw klanten erop plaatst terwijl u betaalt alleen voor de VMs wanneer u ze nodig hebt. Als u wijzigen van het aantal VMs wilt, reageert de Azure binnen enkele minuten, waardoor het mogelijk is om het aantal VMs zo vaak naar wens met dynamisch te wijzigen.

-   **Beheerbaarheid.** Azure is een Platform als een aangeboden Service (PaaS) en beheren het van de infrastructuur (de hardware zelf, elektriciteit en netwerken) voor deze computers houden. Azure beheert ook het platform, ervoor te zorgen dat een besturingssysteem up-to-date met alle de juiste patches en beveiligingsupdates, evenals bijgewerkt met de component .NET Framework en Internet Information Server. Omdat alle VMs zijn Windows Server 2008 wordt uitgevoerd, biedt Azure extra functies zoals diagnostische controle, ondersteuning voor externe bureaublad firewalls en configuratie van certificaten opslaan. Al deze functies worden geleverd zonder extra kosten. In feite is de licentie van Windows Server 2008-besturingssysteem (OS) opgenomen wanneer u uw toepassing in Azure uitvoert. Aangezien de VMs alle Windows Server 2008 wordt uitgevoerd, werkt alle code die wordt uitgevoerd op Windows Server 2008 goed wanneer uitgevoerd in Azure.

## <a id="concepts"> </a>Gehoste Service kernconcepten

Wanneer uw toepassing is geïmplementeerd als een gehoste service in Azure, deze wordt uitgevoerd als een of meer *rollen.* Een *functie* verwijst simpelweg naar toepassingsbestanden en -configuratie. U kunt een of meer rollen definiëren voor uw toepassing, elk met een eigen set van toepassingsbestanden en -configuratie. Voor elke rol in uw toepassing, kunt u het nummer van het VMs of *exemplaren van de rol*, om uit te voeren. De onderstaande afbeelding twee eenvoudige voorbeelden van een toepassing die wordt gezien als een gehoste service met behulp van rollen en rol exemplaren weergeven.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Figuur 1: Één functie met drie exemplaren (VMs) uitgevoerd in een datacenter Azure

![afbeelding][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Afbeelding 2: Twee rollen, elk met twee instanties (VMs), uitgevoerd in een datacenter Azure

![afbeelding][1]

Rol gevallen verwerken meestal Internet clientaanvragen Datacenter zogenaamde een *eindpunt invoer*invoeren. 0 of meer invoer eindpunten hebben één rol. Elk eindpunt is een protocol (HTTP, HTTPS of TCP) en een poort. Het is gebruikelijk dat een rol invoer eindpunten configureren: HTTP luisteren op poort 80 en HTTPS-luisteren op poort 443. De onderstaande figuur toont een voorbeeld van twee verschillende rollen met andere invoer eindpunten voor het routeren van clientaanvragen naar hen.

![afbeelding][2]

Als u een gehoste service in Azure maakt, krijgt het een openbaar adresseerbare IP-adres waarmee clients toegang tot het. Bij het maken van de gehoste service moet u een URL-voorvoegsel dat naar dat IP-adres is toegewezen. Dit voorvoegsel uniek zijn als u in feite het *voorvoegsel*reserveert. URL cloudapp.net zodanig dat niemand anders deze kan hebben. Clients communiceren met uw rol sessies met behulp van de URL. Meestal u zal niet verspreidt of publiceert de Azure *voorvoegsel*. URL cloudapp.net. In plaats daarvan u een DNS-naam van uw DNS registratieservice van keuze kopen en configureert u de DNS-aanvragen van clients omleiden naar de URL Azure. Zie [een aangepaste domeinnaam in Azure configureren][]voor meer informatie.

## <a id="considerations"> </a>Gehoste Service, ontwerpoverwegingen

Als u een toepassing wilt uitvoeren in een cloud-omgeving ontwerpt, zijn verschillende denken zoals latentie, hoge beschikbaarheid en schaalbaarheid.

Bepalen waar u uw toepassingscode is een belangrijke overweging bij het uitvoeren van een gehoste service in Azure. Het is gebruikelijk voor de implementatie van uw toepassing datacenters die zich het dichtst bij uw clients beperken latentie en geniet van de beste prestaties mogelijk.
U kunt een datacenter dichter bij uw bedrijf of dichter bij uw gegevens, hebt u bepaalde bevoegdheid of juridische bezwaren over uw gegevens en waar het zich bevindt. Er zijn zes datacenters over de hele wereld kan uw toepassingscode die als host fungeert. In de onderstaande tabel ziet u de beschikbare locaties:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Land/regio**

</td>
<td style="width: 200px;">
**Subgebieden**

</td>
</tr>
<tr>
<td>
Verenigde Staten

</td>
<td>
Zuid-midden- en Noord-centrale

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Noord & West

</td>
</tr>
<tr>
<td>
Azië

</td>
<td>
Zuidoost- en Oost-

</td>
</tr>
</tbody>
</table>
Als een gehoste service maakt, selecteert u een subregio die de locatie waar u wilt dat uw code uit te voeren.

Voor hoge beschikbaarheid en schaalbaarheid, is het zeer belangrijk dat de gegevens van uw toepassing worden bewaard in een centrale locatie die toegankelijk is voor meerdere exemplaren van de rol. Met deze biedt, Azure verschillende opties voor opslag als BLOB's, tabellen en SQL-Database. Zie het artikel [Gegevens opslag aanbiedingen in Azure][] voor meer informatie over deze opslagtechnologieën. In de volgende afbeelding ziet u hoe de taakverdeling binnen het datacenter Azure verdeelt clientaanvragen voor de rol van de verschillende instanties die toegang tot dezelfde gegevensopslag hebben.

![afbeelding][3]

Meestal u wilt dat uw toepassingscode en de gegevens in het midden met dezelfde gegevens als lage latentie (betere prestaties) Hiermee kunnen vinden wanneer de code van de toepassing toegang heeft tot de gegevens. Ook u zijn niet in rekening gebracht voor bandbreedte wanneer gegevens worden verplaatst om binnen hetzelfde Datacenter.

## <a id="scale"> </a>Voor het ontwerpen van uw toepassing voor schaal

Soms kunt u één toepassing (zoals een eenvoudige website) en wordt gehost in Azure. Maar vaak uw toepassing kan bestaan uit verschillende rollen die samenwerken. In de onderstaande afbeelding zijn er bijvoorbeeld twee exemplaren van de rol van de Website, drie exemplaren van de rol van orderverwerking en één exemplaar van de rol van het rapport-Generator. Deze rollen worden alle werken samen en de code voor al deze worden samen verpakt en gedistribueerd als één eenheid tot Azure.

![afbeelding][4]

De belangrijkste reden voor het splitsen van een toepassing in verschillende rollen elk een eigen set rol exemplaren (dat wil zeggen, VMs) waarop is de rollen onafhankelijk schaal. Bijvoorbeeld tijdens de feestdagen kunnen veel klanten bestellen producten van uw bedrijf, zodat u kunt verhogen van het aantal exemplaren van rol met de rol van uw Website, evenals het aantal exemplaren van rol met uw rol orderverwerking. Na de feestdagen krijgt u mogelijk een groot aantal producten geretourneerd, dus u nog steeds een groot aantal exemplaren van de Website, maar minder exemplaren orderverwerking moet. Tijdens de rest van het jaar hoeft u misschien maar een paar Website- en Verkoopadministratie exemplaren. In dit alles moet u slechts één exemplaar van het rapport-Generator. De flexibiliteit van implementaties op basis van rollen in Azure kunt u gemakkelijk uw toepassing aan de behoeften van uw bedrijf passen.

Meestal wordt de rol binnen uw gehoste service met elkaar communiceren. Bijvoorbeeld de rol van de website accepteert order van een klant, maar vervolgens het compatibele de order verwerken van de Order Processing rol exemplaren. De beste manier om het werk formulier één set exemplaren rol naar een andere set exemplaren met behulp van de queuing technologie geleverd door Azure, geven de Queue-Service of de Service Bus wachtrijen. Het gebruik van een wachtrij is een essentieel onderdeel van het verhaal hier. De wachtrij kan de gehoste service voor het schalen van de rollen die onafhankelijk van elkaar zodat u kunt de belasting tegen de kosten. Als het aantal berichten in de wachtrij na verloop van tijd stijgt, kunt u het aantal exemplaren van orderverwerking rol schaalt. Als het aantal berichten in de wachtrij na verloop van tijd afneemt, kunt u het aantal exemplaren van orderverwerking rol verkleinen. Op deze manier u alleen betaalt voor de exemplaren die zijn vereist voor het verwerken van de werkelijke belasting.

De wachtrij biedt ook betrouwbaarheid. Bij het verkleinen van het aantal exemplaren van orderverwerking rol, besluit Azure welke exemplaren te beëindigen. Kan besluiten een exemplaar in het midden van de verwerking van een wachtrij te beëindigen. Echter, omdat de verwerking van berichten niet wordt voltooid, het bericht zichtbaar opnieuw naar een andere Order Processing rol die het opvangt en verwerkt deze. Vanwege de zichtbaarheid van de message queue, berichten gegarandeerd uiteindelijk verwerkt. De wachtrij fungeert ook als een load balancer door effectief verspreiden onder de berichten op de rol van alle instanties die berichten aanvraagt.

Voor de exemplaren van de rol van Website kunt u het verkeer die naar deze monitor en besluiten voor het schalen van het aantal naar boven of beneden ook. De wachtrij kunt u het aantal exemplaren van de Website rol onafhankelijk van de rol van orderverwerking exemplaren. Dit is zeer krachtig en biedt u een grote flexibiliteit. Als de toepassing uit extra functies bestaat, kunt u aanvullende wachtrijen natuurlijk toevoegen als de leiding voor de communicatie tussen deze gebruikmaken van dezelfde schaal en de kosten van verstrekkingen.

## <a id="defandcfg"> </a>Gehoste servicedefinitie en configuratie

Implementeren van een gehoste service op Azure, moet u ook een definitiebestand voor service en een configuratiebestand service hebben. Beide bestanden zijn XML-bestanden, en u kunt implementatie-opties voor uw gehoste service declaratief opgeven. Het definitiebestand voor beschrijving van alle van de rollen van de gehoste service en hoe ze met elkaar communiceren. Het configuratie-bestand wordt het aantal exemplaren van elke rol en de instellingen die worden gebruikt voor het configureren van elke instantie rol beschreven.

## <a id="def"> </a>Het definitiebestand

Zoals ik eerder heb gezegd, de definitie van service (CSDEF) is-bestand een XML-bestand dat beschrijft de verschillende rollen van de volledige toepassing. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx] [].
Het bestand CSDEF bevat een WebRole of WorkerRole-element voor elke rol die u opnemen in uw toepassing wilt. Implementatie van een rol als een Webrol (met behulp van het element WebRole) betekent dat de code wordt uitgevoerd op een rol exemplaar met Windows Server 2008 en Internet Information Server (IIS).
Implementatie van een rol als een functie van de werknemer (met behulp van het element WorkerRole) betekent dat het exemplaar van de functie van Windows Server 2008 op het (IIS niet geïnstalleerd).

U kunt zeker maken en implementeren van de rol van een werknemer die gebruikmaakt van een andere manier om te luisteren naar binnenkomende webverzoeken (bijvoorbeeld de code kan maken en gebruiken een HttpListener .NET). Aangezien de rol exemplaren alle Windows Server 2008 uitvoert zijn, kunt uw code bewerkingen die normaal gesproken beschikbaar zijn voor een toepassing wordt uitgevoerd op Windows Server uitvoeren
2008.

Voor elke rol, geeft u aan de gewenste grootte voor VM die exemplaren van die rol moeten worden gebruikt. In de onderstaande tabel ziet u de verschillende VM formaten beschikbaar vandaag en de kenmerken van elk:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**VM-grootte**

</td>
<td>
**CPU**

</td>
<td>
**RAM-GEHEUGEN**

</td>
<td>
**Schijf**

</td>
<td>
**Piek netwerk-i/o-**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Extra klein**

</td>
<td>
1 x 1,0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kleine**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Gemiddeld**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3.5 GB

</td>
<td>
490GB

</td>
<td>
\~200 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Grote**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Extra groot**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
Er worden in rekening gebracht per uur voor elke VM u als een exemplaar van de rol en er zijn ook in rekening gebracht voor alle gegevens die de rol van instanties buiten het datacenter verzendt. U bent niet in rekening gebracht voor gegevens invoeren van het datacenter. Zie [Azure prijzen] [] voor meer informatie. In het algemeen is het aan te raden te veel exemplaren van kleine rol in plaats van een paar grote exemplaren gebruiken, zodat uw toepassing beter bestand tegen storingen is. Nadat alle exemplaren minder rol hebben, meer desastreuze een storing in een van deze in uw algemene toepassing. Zoals eerder beschreven, moet u ten minste twee exemplaren voor elke rol implementeren om de 99.95% serviceovereenkomst die Microsoft biedt.

Het service-definitiebestand (CSDEF) is ook waar u veel kenmerken over elke rol geeft in uw toepassing. Hier volgen enkele van de meer nuttige artikelen beschikbaar zijn:

-   **Certificaten**. U kunt certificaten gebruiken voor het coderen van gegevens, of als uw web-service SSL ondersteunt. Alle certificaten moeten worden geüpload naar Azure. Voor meer informatie, Zie [Beheren van certificaten in Azure][]. Deze XML-instelling worden certificaten hebt geüpload geïnstalleerd in het certificaatarchief van de rol van de instantie zodat deze kan worden gebruikt door uw toepassingscode zijn.

-   De **namen van de configuratie-instelling**. Voor de waarden die u wilt dat uw toepassingen te lezen tijdens het uitvoeren van een exemplaar van de rol. De werkelijke waarde van de configuratie-instellingen wordt in het bestand van de configuratie (CSCFG) die kan worden bijgewerkt op elk gewenst moment, zonder dat u uw code opnieuw ingesteld. In feite kunt u uw toepassingen zodanig voor het detecteren van de gewijzigde configuratiewaarden zonder enige uitvaltijd code.

-   De **eindpunten worden ingevoerd**. Hier geeft u de HTTP-, HTTPS- of TCP-eindpunten (met poorten) die u wilt weergeven met de buitenwereld via uw *voorvoegsel*. URL cloadapp.net. Wanneer Azure geïmplementeerd voor uw rol, zal het automatisch de firewall configureren op het exemplaar van de rol.

-   **Interne eindpunten**. Hier geeft u de HTTP- of TCP-eindpunten die blootgesteld aan een andere rol exemplaren die zijn geïmplementeerd als onderdeel van uw toepassing. Interne eindpunten ervoor dat alle exemplaren van een rol binnen de toepassing te praten met elkaar, maar zijn niet toegankelijk voor elke rol exemplaren die buiten de toepassing.

-   **Modules importeren**. Deze installeren eventueel nuttig onderdelen op de exemplaren van uw rol. Onderdelen bestaan voor diagnostische controle van extern bureaublad en Azure verbinding (waarmee uw exemplaar rol toegang te krijgen tot bronnen op het bedrijf via een beveiligd kanaal).

-   **Lokale opslag**. Dit wijst een submap op de rol van exemplaar voor uw toepassing te gebruiken. In het artikel [Gegevens opslag aanbiedingen in Azure][] uitgebreider beschreven.

-   **Taken starten**. Van opstarttaken kunnen u vereiste onderdelen installeren op een exemplaar van de rol als deze wordt opgestart. De taken kunnen uitvoeren als beheerder indien nodig verhoogd.

## <a id="cfg"> </a>Het configuratie-bestand

De service-configuratiebestand (CSCFG) is een XML-bestand met een beschrijving van instellingen die kunnen worden gewijzigd zonder de toepassing opnieuw te implementeren. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Het bestand CSCFG bevat een element Role voor elke rol in uw toepassing. Hier volgen enkele van de items die u in het bestand CSCFG opgeven kunt:

-   **Versie van het besturingssysteem**. Dit kenmerk kunt u het besturingssysteem (OS) versie die wordt gebruikt voor de functie overal met uw toepassingscode selecteren. Dit besturingssysteem is bekend als de *Gast OS*en elke nieuwe versie bevat de meest recente beveiligingspatches en updates die beschikbaar zijn op het moment dat de Gast OS wordt vrijgegeven. Als u de waarde van het kenmerk Versie_besturing ingesteld op "\*", en vervolgens de Gast OS op elk van uw rol exemplaren Azure automatisch bijgewerkt zodra nieuwe Gast OS versies beschikbaar. Echter u kunt niet deelnemen aan automatische updates door Gast specifieke versie van het besturingssysteem te selecteren. Bijvoorbeeld het kenmerk Versie_besturing instellen op de waarde "WA GAST-OS 2.8\_201109-01 ' zorgt ervoor dat uw rol sessies om wat wordt beschreven op deze pagina: [http://msdn.microsoft.com/library/hh560567.aspx][]. Zie voor meer informatie over Gast OS versies [Beheren van Upgrades van het besturingssysteem Azure gasten].

-   **Instanties**. De waarde van het element geeft het aantal exemplaren van rol moet worden ingericht met de code voor een bepaalde functie. Aangezien u kunt een nieuw CSCFG-bestand uploaden naar Azure (zonder opnieuw te implementeren in uw toepassing), is het gebruikelijke manier eenvoudig wijzigt u de waarde voor dit element en het uploaden van een nieuw CSCFG-bestand dynamisch vergroten of verkleinen van het aantal exemplaren van rol de toepassingscode uitgevoerd. Hiermee kunt u de toepassing gemakkelijk op schaal omhoog of omlaag om de reële werklast voldoen aan bepaalde eisen terwijl ook bepalen hoeveel er in rekening gebracht voor het uitvoeren van de exemplaren van de rol.

-   De **ingestelde waarden voor de configuratie**. Dit element geeft de waarden voor instellingen (zoals gedefinieerd in het bestand CSDEF). Uw rol kan deze waarden lezen terwijl deze wordt uitgevoerd. De waarden van deze configuratie-instellingen worden gewoonlijk gebruikt voor verbindingsreeksen Azure opslag of SQL-Database, maar ze kunnen worden gebruikt voor doeleinden die u wenst.

## <a id="hostedservices"> </a>Maken en implementeren van een gehoste Service

Een gehoste service maken, moet u eerst de [Azure Management Portal] en inrichten van een gehoste service door een DNS-voorvoegsel opgeven en het datacenter u uiteindelijk wilt dat uw code uitgevoerd in. Klik in uw ontwikkelomgeving maken uw service-definitiebestand (CSDEF), uw toepassing en deze bestanden-pakket (zip) bouwen in een service-pakketbestand (CSPKG). U moet ook uw service-configuratiebestand (CSCFG) voorbereiden. Als u wilt implementeren in uw rol, kunt u de bestanden CSPKG en CSCFG met de Azure Service Management API uploaden. Eenmaal geïmplementeerd, Azure, zal bepaling rol instanties in het datacenter (op basis van de configuratiegegevens), de toepassingscode extraheren uit het pakket, kopiëren naar de rol van exemplaren en start u de exemplaren. Uw code is nu actief.

In de volgende afbeelding toont de bestanden CSPKG en CSCFG die u op de computer maakt. Het bestand CSPKG bevat het bestand CSDEF en de code voor twee rollen. Na het uploaden van de bestanden CSPKG en CSCFG met de Azure Service Management API maakt Azure de exemplaren van de rol in het datacenter. In dit voorbeeld wordt het bestand CSCFG aangegeven dat Azure Maak drie exemplaren van de rol van \#1 en twee exemplaren van de rol van \#2.

![afbeelding][5]

Zie het artikel [distribueren en bijwerken van Azure-toepassingen][] voor meer informatie over het implementeren van het upgraden en opnieuw configureren van de rollen.<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Verwijzingen

-   [Maken van een gehoste Service voor Azure][]

-   [Gehoste Services in Azure beheren][]

-   [Azure-toepassingen migreren][]

-   [Azure-toepassingen configureren][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Geschreven door Jeffrey Richter (Wintellect)</p>

</div>

  [Voordelen van Azure toepassing Model]: #benefits
  [Kernconcepten gehoste Service]: #concepts
  [Overwegingen bij het ontwerpen van gehoste Service]: #considerations
  [Ontwerpen van uw toepassing voor schaal]: #scale
  [Definitie van gehoste Service en configuratie]: #defandcfg
  [Het definitiebestand voor]: #def
  [Het configuratiebestand van de Service]: #cfg
  [Maken en implementeren van een gehoste Service]: #hostedservices
  [Verwijzingen]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Een aangepaste domeinnaam configureren in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Data opslag aanbiedingen in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Beheren van certificaten in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.Microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.Microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [De Azure gasten OS Upgrades beheren]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure Management Portal]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Distribueren en bijwerken van Azure toepassingen]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Maken van een gehoste Service voor Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Gehoste Services in Azure beheren]: http://msdn.microsoft.com/library/gg433038.aspx
  [Azure-toepassingen migreren]: http://msdn.microsoft.com/library/gg186051.aspx
  [Azure-toepassingen configureren]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
