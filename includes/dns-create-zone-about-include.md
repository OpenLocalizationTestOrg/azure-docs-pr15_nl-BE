Een DNS-zone wordt gebruikt voor het hosten van de DNS-records voor een bepaald domein. U moet die als host fungeert voor uw domein wordt gestart, maakt u een DNS-zone. DNS-records voor een bepaald domein gemaakt worden in een DNS-zone voor het domein. 

Het domein 'contoso.com' kan bijvoorbeeld een aantal van de DNS-records, zoals "mail.contoso.com' (voor een e-mailserver) en"www.contoso.com"(voor een website) bevatten. 


## <a name="names"></a>Over namen van DNS-zone
 
- De naam van de zone moet uniek zijn binnen de resourcegroep en de zone mag niet reeds bestaan. Anders mislukt de bewerking.

- De naam van dezelfde zone kan opnieuw worden gebruikt in een resourcegroep of een ander abonnement Azure. 

- Wanneer meerdere zones dezelfde naam hebben, elk exemplaar verschillende adressen van naamservers worden toegewezen en slechts één exemplaar van het bovenliggende domein kan worden overgedragen. Zie [een Azure-DNS-domein delegeren](../articles/dns/dns-domain-delegation.md)voor meer informatie.