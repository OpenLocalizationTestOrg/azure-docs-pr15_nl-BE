#### <a name="create-an-a-record-set-with-single-record"></a>Een A-record instellen met één record maken

U kunt een verzameling maken `azure network dns record-set create`. Geef de resourcegroep, de naam van de zone, record relatieve naam, het recordtype en tijd live (TTL) ingesteld.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Na het maken van de record set, het IPv4-adres toevoegen aan de record die is ingesteld met `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Maak een AAAA-record met een enkele record instellen

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Een CNAME-record instellen met een enkele record maken

CNAME-records kunnen slechts één enkele tekenreekswaarde.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Een MX-record instellen met een enkele record maken

In dit voorbeeld gebruiken we de naam van de record "@" de MX-record maken op de top van de zone (in dit geval 'contoso.com'). Dit is vaak de MX-records.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Maak een NS-record met een enkele record instellen

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Een PTR-record instellen met een enkele record maken  
In dit geval "Mijn-arpa-zone.com' de ARPA zone waarmee het IP-bereik vertegenwoordigt.  Elke PTR-record instellen in deze zone komt overeen met een IP-adres binnen het bereik van deze IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Een SRV-record instellen met een enkele record maken

Als u een SRV-record in de hoofdmap van de zone maken wilt, kunt u "_service" en "_protocol" in de recordnaam van de. Het is niet nodig om op te nemen "@" in de recordnaam van de.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Maak een TXT-record met één record instellen

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
