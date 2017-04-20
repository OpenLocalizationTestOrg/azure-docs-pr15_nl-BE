Organisaties gebruiken meer [Software als Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) toepassingen voor productiviteit omdat cloud-technologie en hulpmiddelen worden steeds vaker beschikbaar. Als het aantal apps SaaS groeit, wordt het moeilijk voor de beheerders voor het beheren van accounts en toegangsrechten en de gebruikers hun verschillende wachtwoorden onthouden. Beheer van deze toepassingen afzonderlijk maakt extra werk en minder veilig is.


- Medewerkers die te veel wachtwoorden hebben meestal minder veilige methoden gebruiken om te onthouden zijn, op wachtwoorden opschrijven of met behulp van dezelfde wachtwoorden in veel rekeningen.

- Wanneer een nieuwe werknemer aankomt of een verlaat, alle hun accounts moeten worden individueel ingericht of buiten gebruik gesteld.

- Werknemers kunnen ook beginnen met SaaS apps voor hun werk zonder tussenkomst van IT, wat betekent dat ze hun eigen accounts maken op systemen die de IT-beheerders niet hebt goedgekeurd en zijn niet controleren.  

Er is een oplossing voor al deze uitdagingen eenmalige aanmelding (SSO). Het is de eenvoudigste manier om meerdere apps beheren en gebruikers een consistente ervaring aanmelding bieden. Azure Active Directory (AD Azure) biedt een robuuste oplossing voor eenmalige aanmelding en heeft veel beschikbare vooraf geïntegreerde toepassingen, met cursussen voor beheerders snel een nieuwe app instellen en beginnen met het inrichten van gebruikers.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Hoe is Azure Active Directory geïntegreerd apps?  

Azure AD kunt u uw apps en ingerichte accounts te integreren. Dit kan worden gedaan via een van de twee benaderingen.

- Als de app vooraf geïntegreerd in de galerie van app is, kunt u die portal apps instellen en configureren van de instellingen voor eenmalige aanmelding doorlopen. Voor elke toepassing galerie u kunt aan de slag door als volgt de eenvoudige stapsgewijze instructies wordt weergegeven in de galerij app in Azure portal met eenmalige aanmelding inschakelen.

- Als de toepassing niet in de galerie, kunt u de meeste apps nog steeds instellen in Azure AD als een aangepaste toepassing. Hiervoor moet een beetje meer technische expertise om te configureren. U kunt elke toepassing die ondersteunt SAML 2.0 als een federatieve toepassing of een toepassing met een op basis van HTML-aanmeldingspagina als een wachtwoord SSO-app toevoegen.

In het geval waar gebruikers hun eigen accounts voor SaaS-toepassingen die niet worden beheerd door hebben gemaakt IT, de [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) tool biedt een oplossing. Dit hulpprogramma controleert het webverkeer om te bepalen welke toepassingen worden gebruikt binnen de organisatie en het aantal mensen dat met elk van hen. IT kan deze informatie gebruiken voor meer informatie over welke apps de gebruikers geven de voorkeur aan om te bepalen welke te integreren in AD Azure voor eenmalige aanmelding.  

Wanneer u een app in Azure AD integreren, kunt u de gebruiker ingestelde toepassing identiteiten toewijzen aan hun respectieve Azure AD identiteiten.  
