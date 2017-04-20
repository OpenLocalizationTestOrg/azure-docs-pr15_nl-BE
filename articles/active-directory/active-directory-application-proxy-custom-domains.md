<properties
    pageTitle="Werken met aangepaste domeinen in Azure AD toepassingsproxy | Microsoft Azure"
    description="Omvat werken hoe met aangepaste domeinen in Azure AD toepassingsproxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Werken met aangepaste domeinen in Azure AD toepassingsproxy

Met behulp van een standaarddomein kunt u dezelfde URL ingesteld als de interne en externe URL voor toegang tot de toepassing zodat uw gebruikers alleen een URL hebben te onthouden voor toegang tot de app, ongeacht waar zij werkt. Dit kunt u ook een enkele snelkoppeling maken in het deelvenster toegang voor de toepassing. Als u het standaarddomein die door de toepassingsproxy Azure AD, is er geen aanvullende configuratie moet u uw domein inschakelen. In het geval dat u een aangepast domein gebruikt, zijn er enkele zaken die u doen moet om ervoor te zorgen dat de toepassingsproxy uw domein herkent en de certificaten worden gevalideerd.

## <a name="selecting-your-custom-domain"></a>Uw aangepaste domein selecteren

1. Publiceer de toepassing volgens de instructies in [de toepassingen publiceren met de Proxy](active-directory-application-proxy-publish.md).
2. Wanneer de toepassing wordt weergegeven in de lijst met toepassingen, selecteert u deze en klikt u op **configureren**.
3. Geef uw aangepaste domein onder **Externe URL**.
4. Als uw externe URL https is, wordt u gevraagd een certificaat uploaden zodat die Azure de URL van de toepassing kan valideren. U kunt ook een jokerteken certificaat dat overeenkomt met de externe URL van de toepassing uploaden. Dit domein moet in de lijst met uw [Azure domeinen gecontroleerd](https://msdn.microsoft.com/library/azure/jj151788.aspx)worden. Azure moet er een certificaat voor de domein-URL van de toepassing of een jokerteken-certificaat dat overeenkomt met de externe URL voor de toepassing.
5. Zorg ervoor dat een DNS-record waardoor de interne URL naar de toepassing waarmee u dezelfde URL voor interne en externe toegang en één snelkoppeling naar de toepassing in de lijst met toepassingen van de gebruiker toe te voegen.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Veelgestelde vragen over het werken met aangepaste domeinen

V: kan ik een certificaat al geüpload selecteren zonder opnieuw te uploaden?  
A: eerder geüploade certificaten automatisch worden gebonden aan een toepassing en is er precies één certificaat dat overeenkomt met de host-naam van de toepassing.  

V: hoe kan ik een certificaat toevoegen en welke indeling moet het geëxporteerde certificaat worden geüpload?  
A: het certificaat moet worden geüpload van de configuratiepagina van toepassing. Het certificaat moet een PFX-bestand.  

V: kunnen ECC-certificaten worden gebruikt?  
A: Er is geen expliciete beperking op handtekening methoden.  

V: kunnen de SAN-certificaten worden gebruikt?  
A: Ja.  

V: kunnen jokertekens certificaten worden gebruikt?  
A: Ja.  

V: kan een ander certificaat op elke toepassing worden gebruikt?  
A: Ja, tenzij de twee toepassingen dezelfde externe host delen.  

Q: als ik een nieuw domein te registreren, kan ik dat domein gebruiken?  
A: Ja, de lijst met domeinen is afkomstig uit de lijst met gecontroleerde domein van de huurder.  

V: Wat gebeurt er wanneer een certificaat is verlopen?  
A: u krijgt een waarschuwing in het gedeelte van het certificaat in de configuratiepagina van toepassing. Wanneer een gebruiker probeert toegang te krijgen tot de toepassing, wordt een beveiligingswaarschuwing weergegeven.  

V: wat moet ik doen als ik wil een certificaat voor een bepaalde toepassing vervangen?  
A: upload een nieuw certificaat uit de configuratiepagina van toepassing.  

V: kan ik een certificaat verwijderen en vervangen?  
A: wanneer u een nieuw certificaat uploaden als het oude certificaat niet gebruikt door een andere toepassing wordt, worden deze automatisch verwijderd.  

V: Wat gebeurt er wanneer een certificaat wordt ingetrokken?  
A: intrekkingscontroles worden niet uitgevoerd voor certificaten. Wanneer een gebruiker probeert toegang te krijgen tot de toepassing, afhankelijk van de browser, worden mogelijk een beveiligingswaarschuwing weergegeven.  

V: kan ik een zelfondertekend certificaat gebruiken?  
A: Ja, zelf-ondertekende certificaten zijn toegestaan. Houd er rekening mee dat als u een persoonlijke certificeringsinstantie, de CDP (certificate revocation punt-distributiepunt) voor het certificaat openbaar zijn moet.  

V: is er een plaats voor een overzicht van alle certificaten voor de huurder?  
A: dit wordt niet ondersteund in de huidige versie.  


## <a name="see-also"></a>Zie ook

- [Uitgeven van toepassingen met de toepassingsproxy](active-directory-application-proxy-publish.md)
- [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)
- [Uw aangepaste domeinnaam toevoegen aan AD Azure](active-directory-add-domain.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
