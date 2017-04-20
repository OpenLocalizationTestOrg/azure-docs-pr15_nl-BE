## <a name="what-is-reverse-dns"></a>Wat is reverse DNS?

Conventionele DNS-records kunnen een toewijzing van een DNS-naam (bijvoorbeeld 'www.contoso.com') een IP-adres (bijvoorbeeld 64.4.6.100).  Reverse DNS kunt u de vertaling van een IP-adres (64.4.6.100) terug naar een naam (www.contoso.com).

Reverse DNS-records worden gebruikt in allerlei situaties. Reverse DNS-records worden bijvoorbeeld veel gebruikt in de strijd tegen ongewenste e-mail door te controleren of de afzender van een e-mailbericht.  De ontvangende e-mailserver ophalen van IP-adres van de verzendende server een omgekeerde DNS-record en controleren of die host is gemachtigd om e-mail te verzenden vanuit het domein van oorsprong. (Let wel dat [berekenen Azure services niet verzenden van e-mails voor externe domeinen ondersteunen](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Hoe reverse DNS werkt

Reverse DNS-records worden gehost in een speciale DNS-zones, bekend als 'ARPA' zones.  Deze zones vormen een afzonderlijke DNS-hiërarchie in samenspraak met de normale hiërarchie van domeinen zoals 'contoso.com' hosting.

Zo wordt de DNS-record 'www.contoso.com' geïmplementeerd met behulp van een DNS-'A'-record met de naam 'www' in de zone contoso.com.  Deze A-record verwijst naar het bijbehorende IP-adres in dit geval 64.4.6.100.  De reverse lookup wordt afzonderlijk geïmplementeerd met behulp van een "PTR-record met de naam '100' in de zone '6.4.64.in-addr.arpa' (Let op dat IP-adressen worden omgekeerd in de zones van ARPA.)  Deze PTR-record, verwijst als het goed is geconfigureerd naar de naam www.contoso.com.

Wanneer een organisatie wordt een blok IP-adres worden toegewezen, aanschaffen zij ook het recht om de bijbehorende ARPA zone beheren. De ARPA zones die overeenkomt met het IP-adresblokken gebruikt door Azure worden gehost en beheerd door Microsoft. Uw Internetprovider kan host voor de zone ARPA voor uw eigen IP-adressen voor u of u mogelijk host voor de zone ARPA in een DNS-service van uw keuze, zoals DNS Azure.

>[AZURE.NOTE] Forward DNS-lookups en reverse DNS-lookups worden geïmplementeerd in afzonderlijke, parallelle DNS-hiërarchieën. De reverse lookup voor 'www.contoso.com' wordt **niet** uitgevoerd in de zone 'contoso.com', maar deze is opgenomen in de zone volgens het bijbehorende IP-adres blok.

Zie voor meer informatie over reverse DNS [Reverse DNS-Lookup](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure ondersteuning voor reverse DNS

Azure ondersteunt twee verschillende scenario's met betrekking tot reverse DNS:

1. De ARPA zone die overeenkomt met uw IP-adres blokkeren die als host fungeert.
2. Zodat u kunt reverse DNS-record voor het IP-adres dat is toegewezen aan uw Azure service configureren.

Ter ondersteuning van de voormalige, Azure DNS-server kunnen worden gebruikt voor host van de zones volgens en PTR-records voor elke reverse DNS-lookup beheren.  Het proces van het maken van de zone ARPA, het instellen van de delegatie en PTR-records configureren is hetzelfde als voor gewone DNS-zones.  De enige verschillen zijn dat de overdracht moet worden ingesteld via uw Internet-Aanbieder in plaats van uw DNS registratieservice en alleen de PTR-record type moet worden gebruikt.

Ter ondersteuning van deze laatste, kunt Azure u de reverse lookup voor IP-adressen die zijn toegewezen aan de service configureren.  Deze reverse lookup is geconfigureerd door Azure als een PTR-record in de overeenkomstige ARPA zone.  Deze zones volgens, overeenkomt met alle IP-adresbereiken door Azure, gebruikt worden door Microsoft gehost. **De rest van dit artikel wordt dit scenario in detail beschreven.**
