<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Veelgestelde vragen - Reverse DNS voor uw Azure toegewezen IP-adres

### <a name="how-much-do-reverse-dns-records-cost"></a>Hoeveel reverse DNS-records kosten?
Ze zijn gratis!  Er is geen extra kosten voor reverse DNS-records of query's.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Mijn reverse DNS-records de oplossing van het internet?
Ja. Als u de omgekeerde DNS-eigenschap ingesteld voor uw Service Cloud, beheert Azure de delegaties van DNS en DNS-zones die zijn vereist om ervoor te zorgen dat reverse DNS-record worden opgelost voor alle internet-gebruikers.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Een standaard reverse DNS-record gemaakt voor mijn Cloud Services?
Nr. Reverse DNS is een optionele functie. Geen standaard reverse DNS-records worden gemaakt als u niet wilt configureren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wat is de indeling voor de volledig gekwalificeerde domeinnaam (FQDN)?
FQDN-namen worden opgegeven in oplopende volgorde en moeten worden afgesloten met een punt (bijvoorbeeld "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Wat gebeurt er als de validatie wordt gecontroleerd voor de reverse DNS-ik opgegeven mislukt heb?
Wanneer de validatie voor reverse DNS controleert mislukken, mislukt de bewerking van service management. Corrigeer de reverse DNS-waarde vereist, en probeer het opnieuw.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kan ik voor mijn Website Azure reverse DNS beheren?
Reverse DNS wordt niet ondersteund voor Azure Websites. Reverse DNS wordt ondersteund voor rollen Azure PaaS en IaaS virtuele machines.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Kan ik meerdere reverse DNS-records configureren voor mijn Cloud-Service?
Nr. Azure ondersteunt één reverse DNS-record voor elke Azure Cloud-Service. Elke Azure Cloud-Service kan wel hun eigen reverse DNS-record.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kan ik e-mails verzenden naar externe domeinen van mijn diensten Azure berekenen?
Nr. [Berekenen van azure services bieden geen ondersteuning voor verzenden van e-mails voor externe domeinen](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
