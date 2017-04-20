<properties
   pageTitle="Service principal maken met Azure CLI | Microsoft Azure"
   description="Beschrijving van het Azure CLI gebruiken voor het maken van een Active Directory-toepassing en service principal en toegang tot bronnen via op rollen gebaseerde toegangscontrole. Het laat zien hoe de toepassing met een wachtwoord of certificaat verifiëren."
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
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure CLI gebruiken voor het maken van een service principal voor toegang tot bronnen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wanneer u een toepassing of het script dat moet toegang hebben tot bronnen hebt, wilt hebt u waarschijnlijk niet uitvoeren in dit proces met uw eigen referenties. Mogelijk hebt u verschillende machtigingen die u wilt gebruiken voor de toepassing en u niet wilt dat de toepassing doorgaan met uw referenties als uw verantwoordelijkheden. In plaats daarvan maakt u een identiteit voor de toepassing met verificatiereferenties en toewijzing van functies. Telkens wanneer de toepassing wordt uitgevoerd, verifieert zichzelf met deze referenties. In dit onderwerp wordt beschreven hoe u met [CLI voor Mac, Linux en Windows Azure](xplat-cli-install.md) kunt u een toepassing wilt uitvoeren onder een eigen referenties en identiteit instellen.

Met Azure CLI hebt u twee opties voor het verifiëren van uw AD-toepassing:

 - wachtwoord
 - certificaat

In dit onderwerp ziet u hoe u beide opties in Azure CLI gebruiken. Als u zich wilt aanmelden bij Azure uit een programming framework (dergelijke Python, Ruby, of Node.js), zijn wachtwoordverificatie de beste optie. Voordat u beslist of u een wachtwoord of certificaat, Zie de sectie [voorbeeldtoepassingen](#sample-applications) voor voorbeelden in de verschillende kaders te verifiëren.

## <a name="active-directory-concepts"></a>Active Directory-begrippen

In dit artikel maakt u twee objecten - de toepassing van Active Directory (AD) en de belangrijkste service. De toepassing AD is een algemene voorstelling van uw toepassing. Het bevat de referenties (een toepassings-id en een wachtwoord of certificaat). De belangrijkste service is de plaatselijke vertegenwoordiging van de toepassing in een Active Directory. Het bevat de roltoewijzing. Dit onderwerp ligt de nadruk op een enkele huurder-toepassing waarin de toepassing is bedoeld om uit te voeren binnen één organisatie. Meestal gebruikt u één huurder toepassingen voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie. In een toepassing één huurder hebt u één AD app en een service principal.

U vraagt zich misschien af - waarom moet ik beide objecten? Deze aanpak wordt bij het kiezen van een huurder met meerdere toepassingen. Meestal gebruikt u huurder met meerdere toepassingen voor software as a service (SaaS)-toepassingen waarin uw toepassing wordt uitgevoerd in een groot aantal verschillende abonnementen. Voor toepassingen met meerdere huurder hebt u één AD app en meerdere service beveiligings-principals (één in elke Active Directory die toegang tot de app verleent). Als u een toepassing met meerdere huurder instellen, Zie [handleiding voor ontwikkelaars op toelating met de API Azure Resource Manager](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Vereiste machtigingen

Als u dit onderwerp wilt moet u over de machtigingen in uw abonnement Azure en Azure Active Directory. Met name moet u kunnen maken van een app in de Active Directory en de principaal service toewijzen aan een rol. 

In Active Directory moet uw account beheerder (bijvoorbeeld **Algemene Admin** of **User Admin**). Als uw account is toegewezen aan de rol **van de gebruiker** , moet u een Administrator om uw machtigingen verhogen.

Uw abonnement, uw account moet beschikken over `Microsoft.Authorization/*/Write` toegang tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de rol [Beheerder toegang](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) wordt verleend. Als uw account is toegewezen aan de rol van **Inzender** , foutbericht er een wanneer u probeert de service principal toewijzen aan een rol. Nogmaals, de beheerder van het abonnement moet u voldoende toegang verlenen.

Nu gaat u verder met een sectie voor verificatie [wachtwoord](#create-service-principal-with-password) of [certificaat](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Service principal maken met wachtwoord

In deze sectie, voeren de stappen voor het maken van de AD-toepassing met een wachtwoord en de rol van lezer toewijst aan de service principal.

We gaan door middel van deze stappen.

1. Aanmelden bij uw account.

        azure login

1. U hebt twee opties voor het maken van de toepassing AD. U kunt de toepassing AD en de service principal in één stap maken of deze apart te maken. Als u niet nodig hebt een introductiepagina en URI's-id voor uw app, moet u deze maken in één stap. Maak deze afzonderlijk als u deze waarden instellen voor een web app. Beide opties worden weergegeven in deze stap.

     - Als u wilt maken het AD toepassing en service principal in één stap, bieden de naam van de app en een wachtwoord, zoals in de volgende opdracht:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - De toepassing AD afzonderlijk maken, bevatten de naam van de app, een introductiepagina URI URI's-id en een wachtwoord, zoals in de volgende opdracht:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Deze opdracht retourneert de waarde van een toepassings-id. Een service principal maken, bevatten die waarde als een parameter met de volgende opdracht:
     
            azure ad sp create -a <AppId>
     
     Als uw account niet beschikt over de [vereiste machtigingen](#required-permissions) in Active Directory, ziet u een foutbericht 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
    
     Voor beide opties voor de nieuwe service principal geretourneerd. De **Object-Id** is vereist bij het verlenen van machtigingen. De guid weergegeven met de **Service Principal Names** is nodig wanneer u zich aanmeldt. Deze guid is dezelfde waarde als de app-id. In de voorbeeldtoepassingen deze waarde de **Client-ID**genoemd. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. De machtigingen de service principal op uw abonnement. In dit voorbeeld kunt u de service principal toevoegen aan de rol van **lezer** , waarmee hij leesrechten voor alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde functies](./active-directory/role-based-access-built-in-roles.md). Voor de parameter **ServicePrincipalName** bevatten de **object-id** die u hebt gebruikt bij het maken van de toepassing. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht. Het bericht geeft aan uw account **heeft geen machtiging voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid}**. 

Dat is alles. Uw AD toepassing en service principal zijn ingesteld. De volgende sectie wordt beschreven hoe u aan te melden met de referenties via CLI Azure. Als u wilt dat de referentie in uw toepassing code gebruiken, hoeft u niet om door te gaan met dit onderwerp. U kunt springen naar de [voorbeeldtoepassingen](#sample-applications) voor voorbeelden van logboekregistratie met de toepassings-id en wachtwoord. 

### <a name="provide-credentials-through-azure-cli"></a>Referenties via Azure CLI

Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich als een service principal aanmeldt, moet u de huurder-id van de map voor uw AD app. Een huurder is een exemplaar van Active Directory. Gebruik voor het ophalen van de huurder-id voor uw abonnement op dit moment geverifieerd:

        azure account show

     Die wordt geretourneerd:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Als u de id van de pachter van een ander abonnement nodig hebt, gebruikt u de volgende opdracht:

        azure account show -s {subscription-id}

2. Als u nodig hebt voor het ophalen van de client-id moet worden gebruikt voor het aanmelden, gebruikt u:

        azure ad sp show -c exampleapp --json

     De waarde moet worden gebruikt voor het aanmelden is de guid die in de service principal-namen.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Meld u aan als de service principal.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    U wordt gevraagd om het wachtwoord. Geef het wachtwoord dat u hebt opgegeven bij het maken van de toepassing AD.

        info:    Executing command login
        Password: ********

U bent nu geverifieerd als de service principal voor de service-principal die u hebt gemaakt.

## <a name="create-service-principal-with-certificate"></a>Service principal maken met certificaat

In deze sectie kunt uitvoeren u de stappen:

- een zelfondertekend certificaat maken
- de AD-toepassing maken met het certificaat en de service principal
- de rol van lezer toewijst aan de service principal

Als u deze stappen hebt uitgevoerd, moet u [OpenSSL](http://www.openssl.org/) geïnstalleerd hebben.

1. Een zelfondertekend certificaat maken.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Combineer de openbare en persoonlijke sleutels.

        cat privkey.pem cert.pem > examplecert.pem

3. Open het bestand **examplecert.pem** en zoek naar de lange reeks tekens tussen **---BEGIN certificaat---** en **---END CERTIFICATE---**. Kopieer de certificaatgegevens. U kunt deze gegevens als parameter doorgeven bij het maken van de service principal.

1. Aanmelden bij uw account.

        azure login

1. U hebt twee opties voor het maken van de toepassing AD. U kunt de toepassing AD en de service principal in één stap maken of deze apart te maken. Als u niet nodig hebt een introductiepagina en URI's-id voor uw app, moet u deze maken in één stap. Maak deze afzonderlijk als u deze waarden instellen voor een web app. Beide opties worden weergegeven in deze stap.

     - Maken de AD-toepassing en service principal in één stap, vindt u de naam van de app en de certificaatgegevens, zoals in de volgende opdracht:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - AD toepassingen afzonderlijk gemaakt, bevatten de naam van de app, een introductiepagina URI id URI's en de certificaatgegevens, zoals in de volgende opdracht:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Deze opdracht retourneert de waarde van een toepassings-id. Een service principal maken, bevatten die waarde als een parameter met de volgende opdracht:
     
            azure ad sp create -a <AppId>
  
     Als uw account niet beschikt over de [vereiste machtigingen](#required-permissions) in Active Directory, ziet u een foutbericht 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
    
     Voor beide opties voor de nieuwe service principal geretourneerd. De Object-Id is vereist bij het verlenen van machtigingen. De guid weergegeven met de **Service Principal Names** is nodig wanneer u zich aanmeldt. Deze guid is dezelfde waarde als de app-id. In de voorbeeldtoepassingen deze waarde de **Client-ID**genoemd. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. De machtigingen de service principal op uw abonnement. In dit voorbeeld kunt u de service principal toevoegen aan de rol van **lezer** , waarmee hij leesrechten voor alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde functies](./active-directory/role-based-access-built-in-roles.md). Voor de parameter **ServicePrincipalName** bevatten de **object-id** die u hebt gebruikt bij het maken van de toepassing. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht. Het bericht geeft aan uw account **heeft geen machtiging voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid}**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Verstrekken certificaat via geautomatiseerde Azure CLI-script

Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich als een service principal aanmeldt, moet u de huurder-id van de map voor uw AD app. Een huurder is een exemplaar van Active Directory. Gebruik voor het ophalen van de huurder-id voor uw abonnement op dit moment geverifieerd:

        azure account show

     Die wordt geretourneerd:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Als u de id van de pachter van een ander abonnement nodig hebt, gebruikt u de volgende opdracht:

        azure account show -s {subscription-id}

1. Aan vingerafdruk van het certificaat ophalen en verwijderen van overbodige tekens gebruiken:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Die als resultaat een waarde vergelijkbaar met:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Als u nodig hebt voor het ophalen van de client-id moet worden gebruikt voor het aanmelden, gebruikt u:

        azure ad sp show -c exampleapp

     De waarde moet worden gebruikt voor het aanmelden is de guid die in de service principal-namen.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Meld u aan als de service principal.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

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
- Zie voor meer informatie over het gebruik van certificaten en Azure CLI, [verificatie met Azure Service beveiligings-Principals van Linux vanaf de opdrachtregel](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
