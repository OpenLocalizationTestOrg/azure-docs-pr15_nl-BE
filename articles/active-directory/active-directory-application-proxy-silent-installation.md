<properties
    pageTitle="Zonder de Azure AD Application Proxy Connector installeren | Microsoft Azure"
    description="Het uitvoeren van een stille installatie van Azure AD Application Proxy Connector veilige externe toegang tot uw toepassingen in gebouwen bedekt."
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

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Zonder de Azure AD Application Proxy-Connector installeren

Wilt u een installatiescript versturen naar meerdere servers van Windows of Windows-Servers die geen gebruikersinterface is ingeschakeld. In dit onderwerp wordt uitgelegd hoe u een Windows PowerShell-script waarmee u via installatie zonder toezicht te installeren en registreren van uw Azure AD Application Proxy-Connector te maken.

## <a name="enabling-access"></a>Toegang inschakelen
De Proxy werkt door een dunne Windows Server-service genoemd de Connector in het netwerk te installeren. Heeft geregistreerd zijn bij de Azure AD-map met een globale beheerder en het wachtwoord voor de toepassing Proxy-Connector aan het werk. Dit is normaal gesproken tijdens de installatie van de verbindingslijn in het dialoogvenster pop ingevoerd. Ook kunt u Windows PowerShell gebruiken voor het maken van een referentie-object om uw registratie-informatie invoeren of u kunt uw eigen sleutel maken en gebruiken voor het invoeren van uw registratie-informatie.

## <a name="step-1--install-the-connector-without-registration"></a>Stap 1: Installeer de Connector zonder registratie


De MSI-bestanden voor de Connector installeren zonder het registreren van de Connector als volgt:


1. Open een opdrachtprompt.
2. Voer de volgende opdracht, waarbij de /q stille installatie - betekent wordt de installatie niet gevraagd om de gebruiksrechtovereenkomst te accepteren.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Stap 2: De Connector met Azure Active Directory registreren
Dit kunt u doen met behulp van een van de volgende methoden:


- Registreren van de Connector met een Windows PowerShell-referentie-object
- De Connector met een token gemaakt off line registreren

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registreren van de Connector met een Windows PowerShell-referentie-object


1. De referenties van Windows PowerShell-object maken door het uitvoeren van de volgende, waar "<username>"en"<password>" moet worden vervangen door de gebruikersnaam en het wachtwoord voor de map:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Ga naar **C:\Program Files\Microsoft AAD App Proxy Connector** en voer het script uit met behulp van de PowerShell referenties object dat u hebt gemaakt, waarbij $cred is de naam van de PowerShell referenties object dat u hebt gemaakt:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>De Connector met een token gemaakt off line registreren

1. Maak een off line token met behulp van de AuthenticationContext-klasse met de waarden in het volgende codefragment:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Zodra u het maken van een SecureString met behulp van het token-token: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Voer de volgende Windows PowerShell-opdracht, waarbij SecureToken de naam is van het token dat u hiervoor hebt gemaakt en tenantID van de huurder GUID: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Zie ook

- [Toepassingsproxy voor Azure Active Directory inschakelen](active-directory-application-proxy-enable.md)
- [Uitgeven van toepassingen met behulp van uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Oplossen van problemen met de toepassingsproxy](active-directory-application-proxy-troubleshoot.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
