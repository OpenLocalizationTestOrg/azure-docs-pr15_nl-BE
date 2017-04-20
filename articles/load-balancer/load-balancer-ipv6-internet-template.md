<properties
    pageTitle="Een internetverbinding taakverdeling oplossing implementeren met IPv6 via een sjabloon | Microsoft Azure"
    description="Het implementeren van IPv6-ondersteuning voor taakverdeling Azure en VMs verdeeld."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure-taakverdeling, dual-stack, openbaar IP-, systeemeigen ipv6, mobiele, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Een internetverbinding taakverdeling oplossing geïmplementeerd met IPv6 met behulp van een sjabloon

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Azure-taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt een hoge beschikbaarheid door binnenkomend verkeer over gezonde service-exemplaren in de cloud-services of virtuele machines in een load balancer verdelen. Azure taakverdeling kan ook presenteren die diensten op meerdere poorten of meerdere IP-adressen.

## <a name="example-deployment-scenario"></a>Voorbeeld van de implementatie van scenario

In het volgende diagram ziet u de oplossing voor taakverdeling wordt geïmplementeerd met behulp van het van de voorbeeldsjabloon die in dit artikel wordt beschreven.

![Load balancer scenario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende bronnen Azure:

- een virtuele netwerkinterface voor elke VM met zowel IPv4 als IPv6-adressen die zijn toegewezen
- de taakverdeling van een internetverbinding met een IPv4- en IPv6-openbare IP-adres
- twee laden balancing regels voor de openbare VIP's worden toegewezen aan de particuliere eindpunten
- een Set voor beschikbaarheid met de twee VMs
- twee virtuele machines (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>De sjabloon met de Azure portal implementeren

Dit artikel verwijst naar een sjabloon die is gepubliceerd in de galerie met [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) . U kunt downloaden van de sjabloon in de galerie of de implementatie in Azure rechtstreeks vanuit de galerie te starten. In dit artikel wordt ervan uitgegaan dat u de sjabloon hebt gedownload naar uw lokale computer.

1. Open de Azure portal en meld u aan met een account met machtigingen voor het maken van VMs en netwerken bronnen binnen een Azure-abonnement. Tenzij u gebruikmaakt van bestaande middelen, moet de account de machtiging voor het maken van een resourcegroep en een account voor de opslag.

2. Klik op "+ nieuw" uit het menu en vervolgens het type 'sjabloon' in het zoekvak. Selecteer 'sjabloonimplementatie' uit de lijst met zoekresultaten.

    ![lb ipv6-portal step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. In de alles blade, klikt u op 'sjabloonimplementatie'.

    ![lb-ipv6-portal-step3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klik op 'Maken'.

    ![lb ipv6-portal step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klik op 'Sjabloon bewerken'. De bestaande inhoud te verwijderen, kopiëren en plakken in de gehele inhoud van het sjabloonbestand (aan het begin en einde {}) en klik op 'Opslaan'.

    > [AZURE.NOTE] Als u Microsoft Internet Explorer wanneer u u plakt er een dialoogvenster waarin u toegang tot het Klembord van Windows. Klik op 'Toegang toestaan'.

    ![lb ipv6-portal step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klik op 'Parameters bewerken'. In de blade Parameters de waarden per de instructies in de sectie sjabloon parameters opgeven en klik op 'Opslaan' Sluit de blade Parameters. In de implementatie van aangepaste blade, selecteer uw abonnement, een bestaande bronnengroep of een maken. Als u een resourcegroep maakt, selecteert u een locatie voor de resourcegroep. Vervolgens klikt u op de **juridische voorwaarden**en klik vervolgens op **Inkoop** voor de juridische voorwaarden. Azure begint de implementatie van de resources. Het duurt enkele minuten voor de implementatie van alle bronnen.

    ![lb ipv6-portal step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Zie de sectie [Sjabloonparameters en variabelen](#template-parameters-and-variables) verderop in dit artikel voor meer informatie over deze parameters.

7. Klik op Bladeren, blader door de lijst totdat u Zie 'resourcegroepen' en klik vervolgens op het overzicht van de middelen die door de sjabloon zijn gemaakt.

    ![lb ipv6-portal step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Klik op de naam van de resourcegroep die u in stap 6 hebt opgegeven op de bladeserver Resource-groepen. Ziet u een lijst van alle bronnen die zijn geïmplementeerd. Als alles goed is gegaan, moet er 'Geslaagd' onder 'Laatste implementatie'. Zo niet, zorg ervoor dat de account die u machtigingen voor het maken van de nodige middelen.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Als u de bronnengroepen onmiddellijk na het voltooien van stap 6, weer 'Laatste implementatie' de status van 'Implementatie' terwijl de bronnen zijn wordt ingezet.

9. Klik op 'myIPv6PublicIP' in de lijst met resources. U ziet dat er een IPv6-adres onder IP-adres en dat de DNS-naam is de waarde die u hebt opgegeven voor de parameter dnsNameforIPv6LbIP in stap 6. Dit is de openbare IPv6-adres en hostnaam naam die toegankelijk is voor Internet-clients.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Valideren

Als de sjabloon is geïmplementeerd, kunt u verbindingen kunt valideren door de volgende taken uit te voeren:

1. Aanmelden bij de Azure portal en verbinding maken met elk van de VMs gemaakt door de sjabloonimplementatie. Als u een Windows Server-VM ipconfig uitvoeren geïmplementeerd/vanaf een opdrachtprompt. U ziet dat de VMs zowel IPv4 als IPv6-adressen hebben. Als u Linux VMs hebt geïmplementeerd, moet u het besturingssysteem Linux ontvangen dynamische IPv6-adressen met behulp van de instructies voor uw Linux-distributie te configureren.
2. Starten vanaf een client IPv6-Internet-verbinding een verbinding met het openbare IPv6-adres van de taakverdeling. Om te bevestigen dat de taakverdeling tussen de twee VMs is netwerktaakverdeling, kunt u een webserver zoals Microsoft Internet Information Services (IIS) installeren op elk van de VMs. De standaardwebpagina op elke server kan bevatten de tekst 'Server0' of 'Server1' om uniek te identificeren. Vervolgens opent u een Internet-browser op een client voor verbinding met een IPv6-Internet en bladeren om de naam van de host opgegeven voor de dnsNameforIPv6LbIP-parameter van de verdeling van de belasting te bevestigen, end-to-end IPv6-connectiviteit voor elke VM. Als u alleen de webpagina van slechts één server, moet u mogelijk uw browsercache wissen. Meerdere particuliere browsersessies openen. Hier ziet u een reactie van de server.
3. Starten vanaf een client IPv4-Internet-verbinding een verbinding met het openbare IPv4-adres van de taakverdeling. Als u wilt bevestigen dat de taakverdeling load balancing-de twee VMs kan u testen met behulp van IIS, zoals wordt beschreven in stap 2.
4. Start elke VM, een uitgaande verbinding met een apparaat dat IPv6 of IPv4 verbinding Internet. In beide gevallen wordt het bron-IP gezien door het doelapparaat het openbare IPv4- of IPv6-adres van de taakverdeling.

>[AZURE.NOTE]
ICMP voor zowel IPv4 als IPv6 wordt geblokkeerd in het netwerk Azure. Als gevolg hiervan mislukt ICMP-middelen zoals altijd de opdracht ping. Gebruik een alternatieve TCP zoals TCPing of de Test-NetConnection van PowerShell-cmdlet connectiviteit testen. Houd er rekening mee dat de IP-adressen die worden weergegeven in het diagram zijn voorbeelden van de waarden die u kunt zien. Aangezien de IPv6-adressen dynamisch toegewezen worden, worden de adressen die u ontvangt verschillen en is afhankelijk van de regio. Het is ook vaak voor dat de openbare IPv6-adres op de verdeling van de belasting te starten met een andere dan de particuliere IPv6-adressen in de groep back-end-voorvoegsel.

## <a name="template-parameters-and-variables"></a>Sjabloonparameters en variabelen

Een sjabloon Azure Resource Manager bevat meerdere variabelen en parameters die u aan uw behoeften aanpassen kunt. Variabelen worden gebruikt voor vaste waarden die u niet dat een gebruiker wilt te wijzigen. Parameters worden gebruikt voor waarden die u bieden wilt bij het implementeren van de sjabloon gebruiker. De voorbeeldsjabloon is geconfigureerd voor het scenario in dit artikel beschreven. U kunt dit aanpassen aan de behoeften van uw omgeving.

Het van de voorbeeldsjabloon die wordt gebruikt in dit artikel bevat de volgende variabelen en parameters:

| Parameter / variabele | Notities |
|-----------|-------|
| adminUsername | Geef de naam van de beheerdersaccount gebruikt om aan te melden bij de virtuele machines met. |
| adminPassword | Geef het wachtwoord voor de beheerdersaccount gebruikt om aan te melden bij de virtuele machines met. |
| dnsNameforIPv4LbIP | Geef de DNS-hostnaam die u wilt toewijzen aan de openbare naam van de taakverdeling. Deze naam wordt de taakverdeling openbaar IPv4-adres omgezet. De naam moet in kleine letters zijn en overeenkomen met de regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Geef de DNS-hostnaam die u wilt toewijzen aan de openbare naam van de taakverdeling. Deze naam wordt de taakverdeling openbare IPv6-adres omgezet. De naam moet in kleine letters zijn en overeenkomen met de regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Dit is met dezelfde naam als het IPv4-adres. Wanneer een client verzendt een DNS-query voor deze naam Azure retourneert de A- en AAAA wordt vastgelegd wanneer de naam wordt gedeeld. |
| vmNamePrefix | Het voorvoegsel van de naam VM opgeven. De sjabloon wordt een nummer toegevoegd (0, 1, enz.) op de naam wanneer het VMs worden gemaakt. |
| nicNamePrefix | Het netwerk interface naamvoorvoegsel opgeven. De sjabloon wordt een nummer toegevoegd (0, 1, enz.) op de naam bij het maken van de netwerkinterfaces. |
| storageAccountName | Voer de naam van een bestaande account voor opslag of geef de naam van een nieuwe door de sjabloon worden gemaakt. |
| availabilitySetName | Voer vervolgens de naam van de beschikbaarheid is ingesteld voor gebruik met het VMs |
| addressPrefix | Het adresprefix gebruikt voor het definiëren van het adresbereik van het virtuele netwerk |
| subnetName | De naam van het subnet in gemaakt voor de VNet |
| subnetPrefix | Het adresprefix gebruikt voor het definiëren van het adresbereik van het subnet |
| vnetName | Geef de naam voor de VNet die wordt gebruikt door het VMs. |
| ipv4PrivateIPAddressType | Methode voor de toewijzing die wordt gebruikt voor de particuliere IP-adres (statisch of dynamisch) |
| ipv6PrivateIPAddressType | De methode voor de toewijzing die wordt gebruikt voor het particulier IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| numberOfInstances | Het aantal exemplaren van taakverdeling geïmplementeerd door de sjabloon |
| ipv4PublicIPAddressName | Geef de DNS-naam die u wilt gebruiken om te communiceren met de openbare IPv4-adres van de taakverdeling. |
| ipv4PublicIPAddressType | De toewijzingsmethode voor het openbare IP-adres (statisch of dynamisch) |
| Ipv6PublicIPAddressName | Geef de DNS-naam die u wilt gebruiken om te communiceren met het openbare IPv6-adres van de taakverdeling. |
| ipv6PublicIPAddressType | De methode voor de toewijzing die wordt gebruikt voor het openbare IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| lbName | Geef de naam van de taakverdeling. Deze naam wordt weergegeven in de portal of met betrekking tot deze met een CLI of PowerShell-opdracht gebruikt. |

De overige variabelen in de sjabloon afgeleide waarden bevatten die als Azure de resources maakt zijn toegewezen. Deze variabelen niet wijzigen.
