<properties
    pageTitle="Active Directory Federation Services Management en aanpassingsmogelijkheden met Azure AD verbinden | Microsoft Azure"
    description="Beheer van AD FS met Azure AD verbinding maken en aanpassen van AD FS-in gebruikerservaring met Azure AD verbinden en PowerShell."
    keywords="AD FS, ADFS, AD FS management, AAD verbinden, verbinding maken, aanmelden, AD FS aanpassen, herstellen van vertrouwen, O365, Federatie, gebruikmakende partij"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services-beheer en aanpassingsmogelijkheden met Azure AD verbinden

In dit artikel details van taken met betrekking tot Active Directory Federation Services (AD FS) die kunnen worden uitgevoerd met behulp van Microsoft Azure Active Directory-verbinding, plus andere algemene taken voor AD FS die nodig zijn voor een volledige configuratie van een AD FS-serverfarm.

| Onderwerp | Wat dekt |
|:------|:-------------|
|**AD FS beheren**|
|[Het vertrouwen herstellen](#repairthetrust)| Herstellen van de federation-vertrouwensrelatie met Office 365 |
|[Een AD FS-server toevoegen](#addadfsserver) | De AD FS-farm met een extra AD FS-server uit te breiden|
|[Een AD FS web application proxy-server toevoegen](#addwapserver) | De AD FS-farm met een extra WAP-server uit te breiden|
|[Een federatieve domein toevoegen](#addfeddomain)| Een federatieve domein toevoegen|
| **AD FS-aanpassing**|
|[Een aangepaste bedrijfslogo of een illustratie toevoegen](#customlogo)| Een AD FS-aanmeldingspagina met een bedrijfslogo en een afbeelding aanpassen |
|[Een omschrijving toevoegen](#addsignindescription) | Een beschrijving van de aanmeldingspagina toevoegen |
|[AD FS-claims regels wijzigen](#modclaims) | AD FS-claims voor verschillende federation-scenario's wijzigen |

## <a name="ad-fs-management"></a>AD FS beheren

Azure AD Connect biedt diverse taken in verband met AD FS die kunnen worden uitgevoerd met behulp van de wizard Azure AD verbinden met minimale gebruikersinteractie. Nadat u klaar bent met het installeren van Azure AD verbinding maken met de wizard kunt u de wizard opnieuw uitvoeren van extra taken uitvoeren.

### Het vertrouwen herstellen<a name=repairthetrust></a>

Azure AD Connect kan controleren of de huidige status van de vertrouwensrelatie voor AD FS en Azure Active Directory en nemen passende maatregelen om de vertrouwensrelatie te herstellen. Volg deze stappen om te herstellen van uw advertentie Azure en AD FS-vertrouwen.

1. **Reparatie AAD en AD FS vertrouwt** selecteren uit de lijst met extra taken.
![Herstellen van AAD en AD FS vertrouwen](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Op de pagina **verbinding maken met Azure AD** uw globale administrator-referenties voor Azure advertentie, en klik op **volgende**.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Voer de referenties voor de beheerder van het domein op de pagina **RAS-referenties** .
![RAS-referenties](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Nadat u op **volgende**klikt, wordt Azure AD verbinden voor de gezondheid van een certificaat controleren en problemen weergeven.

    ![Status van certificaten](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    De **configureren** ziet u de lijst met acties die worden uitgevoerd om de vertrouwensrelatie te herstellen.

    ![Configureren](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klik op **installeren** om de vertrouwensrelatie te herstellen.

>[AZURE.NOTE] Azure AD verbinden kan enige reparatie of handelen in de zelf-ondertekende certificaten. Azure AD verbinding maken met certificaten van derden niet worden hersteld.

### Een AD FS-server toevoegen<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD verbinden moet het PFX-certificaatbestand aan een AD FS-server toevoegen. Daarom kunt u deze bewerking uitvoeren alleen als u de AD FS-serverfarm met Azure AD-verbinding geconfigureerd.

1. Selecteer **een extra Federation-Server implementeren** en klik op **volgende**.
![Extra federation-server](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Op de pagina **verbinding maken met Azure AD** uw globale administrator-referenties voor Azure advertentie en klik op **volgende**.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Beheerdersreferenties voor het domein bevatten.
![Domeinadministrator](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD verbinden wordt gevraagd om het wachtwoord van het PFX-bestand dat u hebt opgegeven tijdens het configureren van de nieuwe AD FS-serverfarm met Azure AD verbinden. Klik op **Wachtwoord invoeren** om het wachtwoord voor het PFX-bestand.
![Wachtwoord voor het certificaat](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Geef het SSL-certificaat](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Voer op de pagina **AD FS-Servers** de servernaam of IP-adres moet worden toegevoegd aan de AD FS-farm.
![AD FS-servers](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Klik op **volgende** en doorloop de uiteindelijke pagina **configureren** . Nadat de Azure AD verbinden is voltooid de servers toe te voegen aan de AD FS-farm, krijgt u de optie om te controleren of de verbinding.
![Configureren](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Installatie voltooid](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Een AD FS web application proxy-server toevoegen<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD verbinden vereist het PFX-certificaatbestand toe te voegen een web application proxyserver. U zal dus kan deze bewerking alleen uitvoeren als u de AD FS-serverfarm met Azure AD-verbinding geconfigureerd.

1. Selecteer **Web Application Proxy implementeren** in de lijst met beschikbare taken.
![Webproxy-toepassing implementeren](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Geef de referenties Azure globale beheerder.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Op de pagina **Geef SSL-certificaat** , moet u het wachtwoord opgeven voor het PFX-bestand dat u hebt opgegeven tijdens het configureren van de AD FS-serverfarm met Azure AD verbinden.
![Wachtwoord voor het certificaat](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Geef het SSL-certificaat](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. De server moet worden toegevoegd als een toepassingsproxy web toevoegen. Omdat de web application proxyserver kan niet worden toegevoegd aan het domein, wordt gevraagd voor beheerdersreferenties op de server wordt toegevoegd.
![Administratieve referenties](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Op de pagina **Proxy vertrouwen referenties** uw beheerdersreferenties opgeven om de proxy-vertrouwensrelatie configureren en toegang tot de primaire server in de farm met AD FS.
![Proxy vertrouwen referenties](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Op de pagina **Gereed voor het configureren van** ziet de wizard u de lijst met acties die worden uitgevoerd.
![Configureren](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klik op **installeren** om de configuratie te voltooien. Nadat de configuratie voltooid is, kunt u de wizard de optie om te controleren of de verbinding met de servers. Klik op **controleren** om te controleren van verbindingen.
![Installatie voltooid](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Een federatieve domein toevoegen<a name=addfeddomain></a>

Het is eenvoudig een domein worden met Azure AD federatieve met Azure AD verbinding toevoegen. Azure AD verbinden het domein voor de federation toegevoegd en wordt de claim regels overeenkomen met de uitgevende instelling wanneer u meerdere federatieve met Azure Active Directory-domeinen hebt gewijzigd.

1. Als u wilt een federatieve domein toevoegen, selecteert u de taak **toevoegen van een extra domein Azure AD**.
![Extra Azure AD-domein](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Op de volgende pagina van de wizard, voorzien in de algemene beheerdersreferenties Azure AD.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Op de pagina **referenties van de RAS-** referenties van het domein beheerder.
![RAS-referenties](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. De wizard biedt een lijst met Azure AD domeinen waarmee u de map op locatie kan communiceren op de volgende pagina. Kies het domein uit de lijst.
![Azure AD-domein](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Nadat u het domein hebt gekozen, krijgt de wizard u de nodige informatie met betrekking tot de verdere acties die door de wizard en de impact van de configuratie. In sommige gevallen, als u een domein dat nog niet is gecontroleerd in Azure AD, selecteert krijgt de wizard u informatie waarmee u kunt controleren of het domein. Raadpleeg [uw aangepaste domeinnaam naar Azure Active Directory toevoegen](active-directory-add-domain.md) voor meer informatie.

5. Klik op **volgende**en op de pagina **Gereed voor het configureren van** de lijst met acties die Azure AD verbinden zal uitvoeren wordt weergegeven. Klik op **installeren** om de configuratie te voltooien.
![Configureren](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS-aanpassing

De volgende secties geven informatie over een aantal algemene taken die u uitvoeren moet mogelijk bij het aanpassen van uw AD FS-aanmeldingspagina.

### Een aangepaste bedrijfslogo of een illustratie toevoegen<a name=customlogo></a>

Met de volgende Windows PowerShell-cmdlet en syntaxis kunt u het logo van het bedrijf dat wordt weergegeven op **de pagina** wijzigen.

> [AZURE.NOTE] De aanbevolen afmetingen voor het logo zijn 260 x 35 @ 96 dpi met een bestandsgrootte die niet groter zijn dan 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] De *doelnaam* parameter is vereist. Het standaardthema die met AD FS wordt uitgebracht met de naam standaard.


### Een omschrijving toevoegen<a name=addsignindescription></a>

Een aanmeldingspagina als beschrijving wilt toevoegen aan de **aanmeldingspagina**, gebruikt u de volgende Windows PowerShell-cmdlet en syntaxis.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### AD FS-claims regels wijzigen<a name=modclaims></a>

AD FS ondersteunt een uitgebreide claim taal die u gebruiken kunt om aangepaste claim regels te maken. Zie [De functie van de Claim regel taal](https://technet.microsoft.com/library/dd807118.aspx)voor meer informatie.

De volgende secties wordt beschreven hoe u aangepaste regels voor bepaalde scenario's die betrekking hebben op AD Azure kunt schrijven en AD FS-federatie.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Onveranderbare afhankelijk zijn van een waarde in het kenmerk-ID

Azure AD verbinding maken kunt u een kenmerk moet worden gebruikt als een anker bron wanneer objecten worden gesynchroniseerd met Azure AD opgeven. Als de waarde in het aangepaste kenmerk niet leeg is, kunt u een claim in onveranderbare ID geven. U kan bijvoorbeeld **ms-ds-consistencyguid** als het kenmerk voor het anker bron selecteren en wilt uitgeven **ImmutableID** als **ms-ds-consistencyguid** voor het geval het kenmerk heeft waarde tegen. Als er geen waarde voor het kenmerk, **objectGuid** uitgeven als het onveranderbaar.  U kunt de set aangepaste claim regels zoals beschreven in de volgende sectie maken.

**Regel 1: Querykenmerken**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

In deze regel, kunt u de waarden voor **ms-ds-consistencyguid** en de **kenmerken objectGuid** voor de gebruiker uit Active Directory wilt zoeken. Wijzig de winkelnaam een passende winkelnaam in uw AD FS-implementatie. Ook het type claims wijzigen naar een juiste claims voor de Federatie, zoals omschreven voor de **kenmerken objectGuid** en **ms-ds-consistencyguid**.

Ook met **toevoegen** en geen **probleem**, u te voorkomen dat het toevoegen van een uitgaande probleem voor de entiteit en de waarden als tussenliggende waarden kunt gebruiken. Verleent u de vordering in een later regel nadat u welke waarde u vaststellen moet gebruiken als de onveranderbaar.

**Regel 2: Controleren of ms-ds-consistencyguid voor de gebruiker bestaat**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Deze regel definieert een tijdelijke vlag met de naam **idflag** die is ingesteld op **useguid** als er geen **ms-ds-concistencyguid** voor de gebruiker wordt gevuld. De logica achter dit is het feit dat AD FS kan geen lege vorderingen. Dus wanneer u claims, http://contoso.com/ws/2016/02/identity/claims/objectguid en http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid in 1 regel toevoegt, u uiteindelijk met een **msdsconsistencyguid** claim alleen als de waarde wordt opgehaald voor de gebruiker. Als deze niet is ingevuld, ziet AD FS deze heeft een lege waarde en deze direct wordt neergezet. Alle objecten heeft de **kenmerken objectGuid**, zodat die vordering blijft bestaan nadat regel 1 wordt uitgevoerd.

**Regel 3: Ms-ds-consistencyguid uitgeven als onveranderbaar-ID als dit aanwezig is**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dit is een impliciete **bestaat** uit. Als de waarde voor dit argument bestaat, verlenen die als de onveranderbaar. In het vorige voorbeeld wordt het argument **nameidentifier** gebruikt. U moet dit wijzigen in de juiste claimtype voor onveranderbare-ID in uw omgeving.

**Regel 4: ObjectGuid onveranderbare id geven als ms-ds-consistencyGuid niet aanwezig is**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In deze regel dat u alleen de tijdelijke vlag **idflag**controleert. Beslist u om de aanvraag op basis van de waarde ervan.

> [AZURE.NOTE] De volgorde van deze regels is belangrijk.

#### <a name="sso-with-a-subdomain-upn"></a>Eenmalige aanmelding met een subdomein UPN

U kunt meer dan één domein worden met Azure AD-verbinding, zoals wordt beschreven in het [toevoegen van een nieuw domein federatieve](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)federatieve toevoegen. U moet de UPN-claim zodanig aanpassen dat de uitgever-ID komt met het hoofddomein en niet het subdomein overeen, omdat het hoofddomein federatieve tevens het kind omvat.

De claim-regel voor de uitgever-ID is standaard ingesteld als:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standaard uitgever-ID claim](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Het UPN-achtervoegsel wordt gewoon de standaardregel en wordt gebruikt in de claim uitgever-ID. Bijvoorbeeld, John is een gebruiker in sub.contoso.com en contoso.com federated is met Azure Active Directory. John voert john@sub.contoso.com als de naam van de gebruiker tijdens het aanmelden bij AD Azure, en de standaard uitgever-ID claimen regel in AD FS deze op de volgende wijze worden verwerkt.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Waarde van claim:** http://sub.contoso.com/adfs/services/trust/

Als u wilt dat het hoofddomein in de claimwaarde uitgevende instelling, de claim regel overeenkomt met het volgende te wijzigen.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [gebruikersopties aanmelden](active-directory-aadconnect-user-signin.md).
