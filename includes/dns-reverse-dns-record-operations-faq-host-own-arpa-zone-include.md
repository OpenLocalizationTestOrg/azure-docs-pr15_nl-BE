<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Veelgestelde vragen - de ARPA zone in Azure DNS-Hosting

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Ik host van ARPA zones voor mijn Internet-provider zijn toegewezen IP-blokken op Azure DNS?
Ja. De zones volgens uw eigen IP-bereiken in Azure DNS-host wordt volledig ondersteund.

Gewoon [de Azure DNS-zone maken](dns-getstarted-create-dnszone.md)en werken met uw Internet-provider voor het overdragen van [de zone](dns-domain-delegation.md).  Vervolgens kunt u de PTR-records voor elke reverse lookup beheren op dezelfde manier als andere recordtypen.

U kunt ook [een bestaande zone voor reverse lookup met behulp van de CLI Azure importeren](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Wat kost mijn kosten volgens zone hosting?
De zone ARPA hosting voor uw Internet-provider toegewezen IP-blok in Azure DNS in rekening wordt gebracht op [standaardtarieven Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kan ik volgens zones voor zowel IPv4 als IPv6-adressen in Azure DNS-host?
Ja.