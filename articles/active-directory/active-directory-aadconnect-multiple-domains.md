<properties
    pageTitle="Azure AD verbinding maken met meerdere domeinen"
    description="Dit document beschrijft het instellen en configureren van meerdere domeinen van het hoogste niveau met O365 en Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Ondersteuning voor meerdere domein voor federeren met Azure Active Directory
De volgende documentatie biedt richtlijnen voor het gebruik van meerdere domeinen en subdomeinen als federeren van Office 365 of Azure AD domeinen.

## <a name="multiple-top-level-domain-support"></a>Ondersteuning voor meerdere domein op het hoogste niveau
Federeren van meerdere domeinen op hoogste niveau met Azure AD sommige extra configuratie is niet vereist voor federeren met één domein van het hoogste niveau vereist.

Wanneer een domein is federatieve met Azure Active Directory, worden verschillende eigenschappen ingesteld op het domein in Azure.  Een is belangrijk IssuerUri.  Dit is een URI die door AD Azure wordt gebruikt ter identificatie van het domein dat het token is gekoppeld.  De URI hoeft niet te herleiden tot allesbehalve moet een geldige URI.  Standaard Azure AD Hiermee wordt de waarde ingesteld van de federation-service-id in uw locatie op AD FS configureren.

>[AZURE.NOTE]De federation-service-id is een URI die de unieke aanduiding van een federation-service.  De federation-service is een exemplaar van AD FS die werkt als de beveiligingstokenservice. 

U kunt vew IssuerUri met de PowerShell-opdracht `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Een probleem ontstaat wanneer we meer dan één domein van het hoogste niveau.  Bijvoorbeeld: Stel dat u hebt setup-federatie tussen AD Azure en uw omgeving op gebouwen.  Ik gebruik bmcontoso.com voor dit document.  Nu heb ik een tweede, op het hoogste niveau domein, bmfabrikam.com.

![Domeinen](./media/active-directory-multiple-domains/domains.png)

Wanneer we onze bmfabrikam.com domein federatieve worden geconverteerd, foutbericht er een weergegeven.  De reden hiervoor is Azure AD is een beperking die door de eigenschap IssuerUri op dezelfde waarde hebben voor meer dan één domein is niet toegestaan.  
  

![Federation-fout](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain Parameter

Oplossen, moeten we het toevoegen van een andere IssuerUri die kan worden uitgevoerd met behulp van de `-SupportMultipleDomain` parameter.  Deze parameter wordt gebruikt met de volgende cmdlets:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Deze parameter is Azure AD de IssuerUri zodanig configureren dat deze is gebaseerd op de naam van het domein.  Dit is uniek in mappen in Azure AD.  Met de parameter kunt de PowerShell-opdracht te voltooien.

![Federation-fout](./media/active-directory-multiple-domains/convert.png)

De instellingen van onze nieuwe bmfabrikam.com domein ziet u het volgende:

![Federation-fout](./media/active-directory-multiple-domains/settings.png)

Houd rekening met het `-SupportMultipleDomain` de eindpunten die nog steeds worden geconfigureerd om te verwijzen naar onze adfs.bmcontoso.com federation-service wordt niet gewijzigd.

Een ander ding die `-SupportMultipleDomain` heeft is dat waarborgt dat de AD FS-systeem de juiste waarde van de uitgevende instelling in tokens die worden uitgegeven voor Azure advertentie bevat. Dit gebeurt door het domeingedeelte van de UPN-gebruikers nemen en deze instellen als het domein in de IssuerUri, d.w.z. https://{upn achtervoegsel} / services-AD FS/vertrouwen. 

Dus tijdens de verificatie van AD Azure of Office 365, het IssuerUri-element in het token van de gebruiker wordt gebruikt, kunt u het domein in Azure AD.  Als een overeenkomst wordt gevonden, de verificatie mislukt. 

Als een gebruiker van UPN is bijvoorbeeld bsimon@bmcontoso.com, het IssuerUri-element in de problemen token AD FS wordt ingesteld op http://bmcontoso.com/adfs/services/trust. Dit komt overeen met de configuratie van AD Azure en verificatie worden uitgevoerd.

Hieronder ziet u de aangepaste claim regel waarin deze logica geïmplementeerd:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]U moet de schakeloptie - SupportMultipleDomain gebruikt wanneer u probeert in te zetten al toevoegen nieuw toegevoegde domeinen, setup uw federatieve vertrouwensrelatie ter ondersteuning van deze oorspronkelijk hebt.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Het bijwerken van de vertrouwensrelatie tussen AD FS en Azure AD
Als u hebt niet de federatieve vertrouwensrelatie tussen AD FS en uw exemplaar van AD Azure, moet u mogelijk deze vertrouwensrelatie opnieuw te maken.  Dit komt doordat, wanneer deze is oorspronkelijk ingesteld zonder de `-SupportMultipleDomain` parameter, de IssuerUri met de standaardwaarde is ingesteld.  In het screenshot hieronder u ziet dat de IssuerUri is ingesteld op https://adfs.bmcontoso.com/adfs/services/trust.

Zo nu, als we een nieuw domein in de portal Azure AD hebt toegevoegd en vervolgens probeert te converteren met behulp van `Convert-MsolDomaintoFederated -DomainName <your domain>`, krijgen we de volgende fout.

![Federation-fout](./media/active-directory-multiple-domains/trust1.png)

Als u probeert toe te voegen de `-SupportMultipleDomain` switch wij ontvangt de volgende fout:

![Federation-fout](./media/active-directory-multiple-domains/trust2.png)

Gewoon probeert uit te voeren `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` op het oorspronkelijke domein ook leidt tot een fout.

![Federation-fout](./media/active-directory-multiple-domains/trust3.png)

Gebruik de volgende stappen uit om toe te voegen een extra domein op het hoogste niveau.  Als u een domein al heeft toegevoegd en niet de `-SupportMultipleDomain` parameter start met de stappen voor het verwijderen en bijwerken van uw oorspronkelijke domein.  Als u een domein op het hoogste niveau hebt toegevoegd kunt nog u starten met de stappen voor het toevoegen van een domein met PowerShell Azure AD verbinden.

Gebruik de volgende stappen uit om de Microsoft Online-vertrouwensrelatie verwijderen en bijwerken van uw oorspronkelijke domein.

2.  Op de federatieserver van AD FS open **beheer van AD FS.** 
2.  Vouw aan de linkerzijde **Vertrouwensrelaties** en **Partij vertrouwt vertrouwen**
3.  Aan de rechterkant, de **Microsoft Office 365 identiteit Platform** -vermelding te verwijderen.
![Verwijder Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
1.  Uitvoeren op een machine met [Azure Active Directory Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) is geïnstalleerd het volgende: `$cred=Get-Credential`.  
2.  Voer de gebruikersnaam en het wachtwoord van een beheerder van een globale voor u zijn federeren met Azure AD-domein
2.  Voer in PowerShell`Connect-MsolService -Credential $cred`
4.  Voer in PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Dit is voor het oorspronkelijke domein.  Met behulp van de bovenstaande domeinen die zou zijn:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Gebruik de volgende stappen voor het toevoegen van het nieuwe topleveldomein met PowerShell

1.  Uitvoeren op een machine met [Azure Active Directory Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) is geïnstalleerd het volgende: `$cred=Get-Credential`.  
2.  Voer de gebruikersnaam en het wachtwoord van een beheerder van een globale voor u zijn federeren met Azure AD-domein
2.  Voer in PowerShell`Connect-MsolService -Credential $cred`
3.  Voer in PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gebruik de volgende stappen uit om toe te voegen van het nieuwe topleveldomein Azure AD verbinden met.

1.  Azure AD verbinding maken vanaf het bureaublad start of het menu start
2.  Kies 'Een extra Azure AD-domein toevoegen' ![toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add1.png)
3.  Geef uw advertentie Azure en Active Directory-referenties
4.  Selecteer het tweede domein dat u wilt configureren voor de federation.
![Toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add2.png)
5.  Klik op installeren


### <a name="verify-the-new-top-level-domain"></a>Controleer of het nieuwe domein op het hoogste niveau
Met de opdracht PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`vindt u de bijgewerkte IssuerUri.  Het screenshot hieronder toont de federatie instellingen zijn bijgewerkt op onze oorspronkelijke domein http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

En de IssuerUri op ons nieuwe domein is ingesteld op https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Ondersteuning voor subdomeinen
Als u een subdomein door de domeinen manier Azure AD verwerkt toevoegt, worden de instellingen van het bovenliggende object overgenomen.  Dit betekent dat de IssuerUri moet overeenkomen met de ouders.

Zo kunt zeggen bijvoorbeeld dat ik bmcontoso.com en corp.bmcontoso.com toevoegen.  Dit betekent dat de IssuerUri voor een gebruiker van de corp.bmcontoso.com moet worden **http://bmcontoso.com/adfs/services/trust.**  Maar de standaard regel boven geïmplementeerd voor Azure AD, genereert een token met een verlener aangewezen als **http://corp.bmcontoso.com/adfs/services/trust.** die niet overeen met het domein van de vereiste waarde en mislukt de verificatie.

### <a name="how-to-enable-support-for-sub-domains"></a>Het inschakelen van ondersteuning voor subdomeinen
Om dit probleem oplossen door de AD FS moet gebruikmakende partij vertrouwen voor Microsoft Online worden bijgewerkt.  Hiervoor moet u een aangepaste claim regel configureren zodat deze uit alle subdomeinen van een UPN-achtervoegsel van de gebruiker stroken bij het maken van de aangepaste waarde van de uitgevende instelling. 

De volgende claim zal dit doen:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Gebruik de volgende stappen voor het toevoegen van een aangepaste claim voor de ondersteuning van subdomeinen.

1.  Open AD FS beheren
2.  De vertrouwde Microsoft Online RP Klik met de rechtermuisknop en kiest u bewerken Claim regels
3.  Selecteer de derde regel van de claim en vervangen ![claim bewerken](./media/active-directory-multiple-domains/sub1.png)
4.  Vervang de huidige aanvraag:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    met
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Claim vervangen](./media/active-directory-multiple-domains/sub2.png)
5.  Klik op Ok.  Klik op toepassen.  Klik op Ok.  Sluit beheer van AD FS.

