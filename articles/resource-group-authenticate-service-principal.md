<properties
   pageTitle="Azure service principal maken met PowerShell | Microsoft Azure"
   description="Hierin wordt beschreven hoe Azure PowerShell gebruiken voor het maken van een Active Directory-toepassing en service principal en toegang tot bronnen via op rollen gebaseerde toegangscontrole. Het laat zien hoe de toepassing met een wachtwoord of certificaat verifiëren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Azure PowerShell gebruiken voor het maken van een service principal voor toegang tot bronnen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wanneer u een toepassing of het script dat moet toegang hebben tot bronnen hebt, wilt hebt u waarschijnlijk niet uitvoeren in dit proces met uw eigen referenties. Mogelijk hebt u verschillende machtigingen die u wilt gebruiken voor de toepassing en u niet wilt dat de toepassing doorgaan met uw referenties als uw verantwoordelijkheden. In plaats daarvan maakt u een identiteit voor de toepassing met verificatiereferenties en toewijzing van functies. Telkens wanneer de toepassing wordt uitgevoerd, verifieert zichzelf met deze referenties. In dit onderwerp wordt beschreven hoe u met [Azure PowerShell](powershell-install-configure.md) kunt u alles wat die u nodig hebt voor een toepassing uit te voeren onder eigen referenties en identiteit instellen.

Met PowerShell hebt u twee opties voor het verifiëren van uw AD-toepassing:

 - wachtwoord
 - certificaat

In dit onderwerp ziet u hoe u beide opties in PowerShell. Als u zich wilt aanmelden bij Azure uit een programming framework (dergelijke Python, Ruby, of Node.js), zijn wachtwoordverificatie de beste optie. Voordat u beslist of u een wachtwoord of certificaat, Zie de sectie [voorbeeldtoepassingen](#sample-applications) voor voorbeelden in de verschillende kaders te verifiëren.

## <a name="active-directory-concepts"></a>Active Directory-begrippen

In dit artikel maakt u twee objecten - de toepassing van Active Directory (AD) en de belangrijkste service. De toepassing AD is een algemene voorstelling van uw toepassing. Het bevat de referenties (een toepassings-id en een wachtwoord of certificaat). De belangrijkste service is de plaatselijke vertegenwoordiging van de toepassing in een Active Directory. Het bevat de roltoewijzing. Dit onderwerp ligt de nadruk op een enkele huurder-toepassing waarin de toepassing is bedoeld om uit te voeren binnen één organisatie. Meestal gebruikt u één huurder toepassingen voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie. In een toepassing één huurder hebt u één AD app en een service principal.

U vraagt zich misschien af - waarom moet ik beide objecten? Deze aanpak wordt bij het kiezen van een huurder met meerdere toepassingen. Meestal gebruikt u huurder met meerdere toepassingen voor software as a service (SaaS)-toepassingen waarin uw toepassing wordt uitgevoerd in een groot aantal verschillende abonnementen. Voor toepassingen met meerdere huurder hebt u één AD app en meerdere service beveiligings-principals (één in elke Active Directory die toegang tot de app verleent). Als u een toepassing met meerdere huurder instellen, Zie [handleiding voor ontwikkelaars op toelating met de API Azure Resource Manager](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Vereiste machtigingen

Als u dit onderwerp wilt moet u over de machtigingen in uw abonnement Azure en Azure Active Directory. Met name moet u kunnen maken van een app in de Active Directory en de principaal service toewijzen aan een rol. 

In Active Directory moet uw account beheerder (bijvoorbeeld **Algemene Admin** of **User Admin**). Als uw account is toegewezen aan de rol **van de gebruiker** , moet u een Administrator om uw machtigingen verhogen.

Uw abonnement, uw account moet beschikken over `Microsoft.Authorization/*/Write` toegang tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de rol [Beheerder toegang](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) wordt verleend. Als uw account is toegewezen aan de rol van **Inzender** , foutbericht er een wanneer u probeert de service principal toewijzen aan een rol. Nogmaals, de beheerder van het abonnement moet u voldoende toegang verlenen.

Nu gaat u verder met een sectie voor verificatie [wachtwoord](#create-service-principal-with-password) of [certificaat](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Service principal maken met wachtwoord

In deze sectie kunt uitvoeren u de stappen:

- de AD-toepassing maken met een wachtwoord
- de service principal maken
- de rol van lezer toewijst aan de service principal

Zie de volgende drie cmdlets om snel uitvoeren van deze stappen. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

We gaan door deze stappen meer zorgvuldig om ervoor te zorgen dat u begrijpt het proces.

1. Aanmelden bij uw account.

        Add-AzureRmAccount

1. Maak een nieuwe Active Directory-toepassing door middel van een naam, de beschrijving van uw toepassing URI, de URI's die uw toepassing en het wachtwoord voor de toepassingsidentiteit van uw bepalen.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     De URI's worden niet gevalideerd voor één huurder-toepassingen.
     
     Als uw account niet beschikt over de [vereiste machtigingen](#required-permissions) in Active Directory, ziet u een foutbericht 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.

1. Bekijk het nieuwe application-object. 

        $app
        
     Let in het bijzonder de **ApplicationId** eigenschap, die nodig is voor het maken van de service beveiligings-principals, toewijzing van functies en het toegangstoken verkrijgen.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Een service principal voor uw toepassing doorgeven in de toepassings-id van de Active Directory-toepassing maken.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. De machtigingen de service principal op uw abonnement. In dit voorbeeld kunt u de service principal toevoegen aan de rol van **lezer** , waarmee hij leesrechten voor alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde functies](./active-directory/role-based-access-built-in-roles.md). Geef voor de parameter **ServicePrincipalName** **ApplicationId** die u gebruikt bij het maken van de toepassing. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht. Het bericht geeft aan uw account **heeft geen machtiging voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid}**. 

Dat is alles. Uw AD toepassing en service principal zijn ingesteld. De volgende sectie wordt beschreven hoe u aan te melden met de referenties via PowerShell. Als u wilt dat de referentie in uw toepassing code gebruikt, kunt u de [voorbeeldtoepassingen](#sample-applications)springen. 

### <a name="provide-credentials-through-powershell"></a>Referenties via PowerShell

Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren.

1. Maak een **PSCredential** -object dat de referenties bevat met de opdracht **Get-Credential** . U moet de **ApplicationId** voordat u deze opdracht dus zorg ervoor dat u hebt dat beschikbaar is om te plakken.

        $creds = Get-Credential

2. U wordt gevraagd of u kunt uw referenties invoeren. Gebruik de **ApplicationId** die u gebruikt bij het maken van de toepassing voor de gebruikersnaam. Gebruik voor het wachtwoord, die u hebt opgegeven bij het maken van de account.

     ![referenties opgeven](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Wanneer u zich als een service principal aanmeldt, moet u de huurder-id van de map voor uw AD app. Een huurder is een exemplaar van Active Directory. Als u alleen een abonnement hebt, kunt u gebruiken:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Als u meer dan één abonnement, het abonnement opgeven waarop uw Active Directory bevindt. Zie [hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Meld u aan als de service principal door te geven dat deze account een service principal is en door middel van het object referenties. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     U bent nu geverifieerd als de belangrijkste service voor de Active Directory-toepassing die u hebt gemaakt.

### <a name="save-access-token-to-simplify-log-in"></a>Toegangstoken ter vereenvoudiging van het logboek opslaan

Als u wilt voorkomen dat de service principal referenties verstrekken telkens wanneer deze zich aanmeldt, kun je het toegangstoken.

1. Als u het huidige toegangstoken in een latere sessie, moet u het profiel opslaan.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Open het profiel en de inhoud ervan te bekijken. U ziet dat deze een toegangstoken bevat. 
        
2. In plaats van handmatig opnieuw in te loggen, het profiel te laden.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Het toegangstoken verloopt met een opgeslagen profiel werkt alleen voor als het token geldig is.
        
## <a name="create-service-principal-with-certificate"></a>Service principal maken met certificaat

In deze sectie kunt uitvoeren u de stappen:

- een zelfondertekend certificaat maken
- de AD-toepassing maken met het certificaat
- de service principal maken
- de rol van lezer toewijst aan de service principal

Zie de volgende cmdlets om snel uitvoeren van deze stappen met Azure PowerShell 2.0 op Windows 10 of Windows Server 2016 technische Preview. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

We gaan door deze stappen meer zorgvuldig om ervoor te zorgen dat u begrijpt het proces. Dit artikel ziet ook hoe u taken uitvoeren als in eerdere versies van besturingssystemen of Azure PowerShell.

### <a name="create-the-self-signed-certificate"></a>Zelf-ondertekend certificaat maken

De PowerShell in Windows 10 en Windows Server 2016 technische Preview-versie heeft een bijgewerkte cmdlet **New-SelfSignedCertificate** voor het genereren van een zelf-ondertekend certificaat. Eerdere besturingssystemen hebben de cmdlet New-SelfSignedCertificate, maar biedt niet de parameters die nodig zijn voor dit onderwerp. In plaats daarvan moet u een module voor het genereren van het certificaat te importeren. Dit onderwerp bevat beide benaderingen voor het genereren van het certificaat dat is gebaseerd op het besturingssysteem u hebt. 

- Als u **Windows 10 of Windows Server 2016 technische Preview**, de volgende opdracht uit te maken van een zelfondertekend certificaat: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Als u **geen Windows 10 of Windows Server 2016 technische Preview**, moet u het [zelf-ondertekend certificaat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) downloaden vanaf Microsoft Script Center. Pak de inhoud en de cmdlet u moet importeren.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Vervolgens genereert u het certificaat.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

U hebt uw certificaat en verder kunt gaan met het maken van uw AD app.

### <a name="create-the-active-directory-app-and-service-principal"></a>De Active Directory-app en service principal maken

1. De waarde van de sleutel uit het certificaat ophalen.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Aanmelden bij uw account Azure.

        Add-AzureRmAccount

3. Maak een nieuwe Active Directory-toepassing door middel van een naam, de beschrijving van uw toepassing URI, de URI's die uw toepassing en het wachtwoord voor de toepassingsidentiteit van uw bepalen.

     Als u beschikt over Azure PowerShell 2.0 (augustus 2016 of hoger), gebruik de volgende cmdlets:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Als u Azure PowerShell 1.0 hebt, gebruikt u de volgende cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    De URI's worden niet gevalideerd voor één huurder-toepassingen.
    
    Als uw account niet beschikt over de [vereiste machtigingen](#required-permissions) in Active Directory, ziet u een foutbericht 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
        
    Bekijk het nieuwe application-object. 

        $app

    Let op de eigenschap **ApplicationId** die nodig is voor het maken van service beveiligings-principals, toewijzing van functies en access tokens ophalen.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Een service principal voor uw toepassing doorgeven in de toepassings-id van de Active Directory-toepassing maken.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. De machtigingen de service principal op uw abonnement. In dit voorbeeld kunt u de service principal toevoegen aan de rol van **lezer** , waarmee hij leesrechten voor alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde functies](./active-directory/role-based-access-built-in-roles.md). Geef voor de parameter **ServicePrincipalName** **ApplicationId** die u gebruikt bij het maken van de toepassing.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht. Het bericht geeft aan uw account **heeft geen machtiging voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid}**.

Dat is alles. Uw AD toepassing en service principal zijn ingesteld. De volgende sectie wordt beschreven hoe u aan te melden met een certificaat via PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Certificaat via geautomatiseerde PowerShell script bevatten

Wanneer u zich als een service principal aanmeldt, moet u de huurder-id van de map voor uw AD app. Een huurder is een exemplaar van Active Directory. Als u alleen een abonnement hebt, kunt u gebruiken:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Als u meer dan één abonnement, het abonnement opgeven waarop uw Active Directory bevindt. Zie voor meer informatie [de Azure AD-map beheren](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Als u wilt verifiëren in het script, geef de account is een service principal en bieden de vingerafdruk van het certificaat, de toepassings-id en id van de huurder. Automatiseren van uw script, kunt u deze waarden opslaan als omgevingsvariabelen en halen ze tijdens de uitvoering of kunt u deze opnemen in uw script.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

U bent nu geverifieerd als de belangrijkste service voor de Active Directory-toepassing die u hebt gemaakt.

## <a name="sample-applications"></a>Voorbeeldtoepassingen

De volgende voorbeeldtoepassingen hoe aan te melden als de service principal.

**.NET**

- [Implementeren van een SSH VM met een sjabloon met .NET ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resources en resourcegroepen met .NET beheren](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Aan de slag met - implementeren met behulp van Azure Resource Manager sjabloon - bronnen in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Aan de slag met Resources - groep beheren - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Implementeren van een SSH VM met een sjabloon in Python ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure resources en resourcegroepen met Python beheren](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Implementeren van een SSH VM met een sjabloon in Node.js ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resources en resourcegroepen met Node.js beheren](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Implementeren van een SSH VM met een sjabloon in Ruby ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure resources en resourcegroepen met Ruby beheren](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Volgende stappen
  
- Zie [handleiding voor ontwikkelaars op toelating met de API Azure Resource Manager](resource-manager-api-authentication.md)voor gedetailleerde stappen over het integreren van een toepassing in Azure voor het beheren van bronnen.
- Zie voor een meer gedetailleerde uitleg van toepassingen en service-principals [toepassing en Service Principal-objecten](./active-directory/active-directory-application-objects.md). 
- Voor meer informatie over Active Directory-verificatie Zie [Verificatie scenario's voor Azure AD](./active-directory/active-directory-authentication-scenarios.md).



