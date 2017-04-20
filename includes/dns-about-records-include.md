## <a name="about-records"></a>Over records

Elke DNS-record heeft een naam en een type. Records worden ingedeeld in verschillende typen op basis van de gegevens die ze bevatten. Het meest voorkomende type is een A-record, dat wordt een naam aan een IPv4-adres toegewezen. Een ander type is een record "MX", die een naam aan een e-mailserver koppelt.

Azure DNS ondersteunt alle gangbare DNS-recordtypen, waaronder A, AAAA, CNAME, MX, NS, PTR-, SOA, SRV- en TXT. Houd rekening met:
- SOA-record wordt automatisch met elke zone worden gemaakt, kunnen niet afzonderlijk worden gemaakt.
- SPF-records moeten worden gemaakt met behulp van het recordtype TXT. Zie [deze pagina](http://tools.ietf.org/html/rfc7208#section-3.1)voor meer informatie.

In Azure DNS records opgegeven met behulp van relatieve namen. Een "" FQDN-naam (Fully Qualified Domain Name) bevat de zonenaam van de, dat de naam van een "relatieve" niet. De relatieve recordnaam "www" in de zone 'contoso.com' geeft bijvoorbeeld de volledig gekwalificeerde naam van de record www.contoso.com.

## <a name="about-record-sets"></a>Over recordsets

Soms moet u meer dan één DNS-record maken met een opgegeven naam en type. Stel bijvoorbeeld dat de "www.contoso.com" website wordt gehost op twee verschillende IP-adressen. De website van twee verschillende A-records, één voor elk IP-adres is vereist. Dit is een voorbeeld van een verzameling:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS beheert DNS-records met behulp van recordsets. Een verzameling is de verzameling van DNS-records in een zone die dezelfde naam en hetzelfde type. De meeste recordsets bevat één record maar voorbeelden zoals deze, waarin een verzameling meer dan één record bevat zijn niet ongebruikelijk.

SOA- en CNAME-record stelt zijn uitzonderingen. De DNS-standaarden niet toestaan dat meerdere records met dezelfde naam voor deze typen.

De TTL of TTL geeft aan hoe lang elke record in de cache geplaatst door clients voordat de query opnieuw wordt uitgevoerd. In dit voorbeeld wordt is de TTL-waarde 3600 seconden of 1 uur. De TTL-waarde is opgegeven voor de record is ingesteld, niet voor elke record, zodat voor alle records in deze verzameling dezelfde waarde wordt gebruikt.

#### <a name="wildcard-record-sets"></a>Wildcard recordsets

Azure DNS ondersteunt [records met jokertekens](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Deze worden geretourneerd voor een query met een overeenkomende naam (tenzij een geschikter is vanuit een niet-jokerteken verzameling). Joker-recordsets worden ondersteund voor alle typen behalve NS- en SOA-records.  

Maakt u een jokerteken verzameling met de naam van de record "\*". Of gebruik een naam met het label "\*", bijvoorbeeld"\*.foo".

#### <a name="cname-record-sets"></a>CNAME-record instellen

CNAME-record instellen kunnen niet worden gecombineerd met andere records sets met dezelfde naam. U kan bijvoorbeeld een CNAME-record instellen met de relatieve naam 'www' en een A-record maken met de relatieve naam 'www' op hetzelfde moment. Omdat de top van de zone (naam = ‘@’) bevat altijd de NS- en SOA-recordsets die zijn gemaakt als de zone is gemaakt, kunt u een CNAME-record instellen op de top van de zone niet kan maken. Deze beperkingen zullen voortvloeien uit de DNS-standaarden en beperkingen van Azure DNS niet.
