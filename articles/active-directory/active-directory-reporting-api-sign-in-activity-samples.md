<properties
    pageTitle="Azure Active Directory-in activiteit rapport API monsters | Microsoft Azure"
    description="Om te beginnen met de Azure Active Directory Reporting-API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory-in activiteit rapport API monsters

In dit onderwerp maakt deel uit van een verzameling onderwerpen over Active Directory reporting API Azure.  
Azure AD reporting biedt een API waarmee u toegang tot gegevens over de activiteit aanmelden met behulp van code of verwante hulpprogramma's.  
Het bereik van dit onderwerp is te voorzien u voorbeeldcode voor de **activiteit - in API**.

Zie:

- [Controlelogboeken](active-directory-reporting-azure-portal.md#audit-logs) voor meer algemene informatie
- [Aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) voor meer informatie over de API van de rapportage.

Voor vragen, problemen of feedback, neem contact op met [AAD te melden](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Vereisten
Voordat u de voorbeelden in dit onderwerp gebruiken kunt, moet u de [voorwaarden voor toegang tot de Azure advertentie melden API](active-directory-reporting-api-prerequisites.md)te voltooien.  


##<a name="powershell-script"></a>PowerShell script

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Het script
Zodra u klaar bent met het script te bewerken, uit te voeren en controleer dat de verwachte gegevens uit de Audit rapport vastgelegd als resultaat gegeven.

Het script wordt de uitvoer van het rapport-in JSON-indeling. Ook is er een `SigninActivities.json` bestand met hetzelfde resultaat. U kunt experimenteren met het aanpassen van het script om gegevens uit andere lijsten en commentaar van de output-indelingen die u niet nodig hebt.



## <a name="next-steps"></a>Volgende stappen

- Wilt u de voorbeelden in dit onderwerp aanpassen? Bekijk de [Azure Active Directory-in activiteit API reference](active-directory-reporting-api-sign-in-activity-reference.md). 

- Als u zien van een volledig overzicht wilt van het gebruik van Active Directory reporting API Azure, Zie [aan de slag met Azure Active Directory API melden](active-directory-reporting-api-getting-started.md).

- Als u meer weten wilt over Azure Active Directory reporting Zie [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  