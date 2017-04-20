<properties 
    pageTitle="De integratie van identiteiten in ruimten met Azure Active Directory."
    description="Dit is de Azure AD verbinden die beschrijft wat het is en waarom u deze wilt gebruiken."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Meerledige verificatie bouwstenen in aangepaste toepassingen (SDK)

> [AZURE.IMPORTANT]  Als u downloaden van de SDK wilt, moet u een Provider Azure meerledige verificatie maken, zelfs als u de MVR gesloten Azure, AAD Premium of EMS-licenties hebt.  Als u een Provider Azure meerledige verificatie voor dit doel maken en al licenties hebt, is het vereist om de voorziening te maken met het model **Per gebruiker** en de Provider een koppeling naar de map waarin de licenties van de MVR gesloten Azure, Azure AD Premium of EMS.  Hierdoor weet u zeker dat u zijn niet gefactureerd tenzij u meer unieke gebruikers met behulp van de SDK is dan het aantal licenties dat u bezit.

Azure meerledige verificatie Software Development Kit (SDK) kunt u bellen en tekst bericht controle rechtstreeks in de-in of transactie processen van toepassingen in uw huurder Azure AD bouwen.

De SDK meerledige verificatie is beschikbaar voor C#, Visual Basic (.NET), Java, Perl, PHP en Ruby. De SDK biedt een eenvoudige wrapper rond meerledige verificatie. Het bevat alles wat die u nodig hebt voor het schrijven van de code, inclusief commentaar broncodebestanden, van de voorbeeldbestanden en een gedetailleerde Leesmij-bestand. De SDK bevat ook een certificaat en persoonlijke sleutel voor het coderen van transacties die uniek is voor uw Provider meerledige verificatie. Als u een provider hebt, kunt u de SDK in zoveel talen en indelingen downloaden als u nodig hebt.

De structuur van de API's in de SDK meerledige verificatie is heel eenvoudig. U één functie aanroepen van een API met de optie meerledige parameters, zoals de controle op modus en gebruikersgegevens, zoals het telefoonnummer of het PIN-nummer te valideren. De API's vertalen de functieaanroep in web services aanvragen naar de cloud-gebaseerde Azure meerledige verificatie-Service. Alle gesprekken moeten bevatten een verwijzing naar het persoonlijke certificaat dat is opgenomen in elke SDK.

Omdat de API's geen toegang tot de gebruikers geregistreerd in Azure Active Directory, moet u de gebruikersgegevens, zoals telefoonnummers en PIN-codes in een bestand of database opgeven. Ook bieden de API's geen inschrijvings- of beheerfuncties, dus moet u deze processen in uw toepassing.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Azure meerledige verificatie SDK downloaden

Downloaden van de SDK Azure meerledige is een [Provider van Azure meerledige verificatie](multi-factor-authentication-get-started-auth-provider.md)vereist.  Hiervoor moet een volledige Azure abonnement, zelfs als de MVR gesloten Azure, Azure AD Premium of Enterprise Mobility Suite-licenties zijn eigendom.  Downloaden van de SDK, moet u de meerledige Management Portal navigeren door het beheer van de Provider meerledige verificatie rechtstreeks of door te klikken op de koppeling **'Ga naar de portal'** op de instellingenpagina voor MVR gesloten.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Downloaden van de SDK Azure meerledige verificatie van de Azure portal


1. De Azure Portal als beheerder aanmelden.
2. Selecteer Active Directory aan de linkerkant.
3. Klik op de pagina Active Directory boven **Meerledige verificatie Providers**
4. Klik onder **beheren**
5. Hiermee opent u een nieuwe pagina.  Klik op de links onderaan de SDK.
<center>![Downloaden](./media/multi-factor-authentication-sdk/download.png)</center>
6. Selecteer de gewenste taal en klik op een van de bijbehorende downloadkoppelingen.
7. Sla de download.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Downloaden van de SDK Azure meerledige verificatie via de service-instellingen


1. De Azure Portal als beheerder aanmelden.
2. Selecteer Active Directory aan de linkerkant.
3. Dubbelklik vervolgens op uw exemplaar van AD Azure.
4. Klik op boven **configureren**
5. Selecteer onder meerledige verificatie **service-instellingen beheren**
![downloaden](./media/multi-factor-authentication-sdk/download2.png)
6. Op de instellingenpagina van services onder aan het scherm klikt u op **de portal**.
![Downloaden](./media/multi-factor-authentication-sdk/download3a.png)
7. Hiermee opent u een nieuwe pagina.  Klik op de links onderaan de SDK.
8. Selecteer de gewenste taal en klik op een van de bijbehorende downloadkoppelingen.
9. Sla de download.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Inhoud van de Azure meerledige verificatie SDK
In de SDK vindt u de volgende items:

- **Leesmij-bestand**. Hoe het meerledige verificatie API's kunt gebruiken in een nieuwe of bestaande toepassing.
- **Bronbestand(en)** voor meerledige verificatie
- U kunt communiceren met de service meerledige verificatie **clientcertificaat**
- De **persoonlijke sleutel** voor het certificaat
- **Bel resultaten.** Een lijst met oproep resultaatcodes. Gebruik dit om bestand te openen, een toepassing met tekstopmaak, zoals WordPad. De oproep resultaatcodes gebruiken voor het testen en oplossen van problemen met de uitvoering van een meerledige verificatie in uw toepassing. Ze zijn niet statuscodes voor verificatie.
- **Voorbeelden.** Voorbeeldcode voor een eenvoudige implementatie van de werken van meerledige verificatie.


>[AZURE.WARNING]Het clientcertificaat is een unieke persoonlijke certificaat die speciaal voor u is gegenereerd. Niet delen of dit bestand verloren. Het is de sleutel tot de beveiliging van uw communicatie met de service meerledige verificatie.

## <a name="code-sample-standard-mode-phone-verification"></a>Codevoorbeeld: Verificatie via telefoon standaardmodus

Dit codevoorbeeld ziet u hoe u de API's in de SDK van Azure meerledige verificatie standaard modus voice call controle toevoegen aan uw toepassing. Standaard modus is een telefoongesprek dat de gebruiker reageert op door de #-toets te drukken.

In dit voorbeeld wordt de C# .NET 2.0 meerledige verificatie SDK in een ASP.NET-toepassing met C# serverlogica basic, maar het proces is vergelijkbaar voor eenvoudige implementaties in andere talen. Omdat de SDK de bronbestanden, geen uitvoerbare bestanden bevat, kunt u maken van de bestanden en ze verwijzen naar of ze rechtstreeks in uw toepassing opnemen.

>[AZURE.NOTE]Bij het implementeren van meerledige verificatie, gebruiken als aanvulling op uw primaire verificatiemethode aanvullende factoren als secundaire of tertiaire controle. Deze methoden zijn niet ontworpen om te worden gebruikt als primaire verificatiemethoden.

### <a name="code-sample-overview"></a>Code monster-overzicht
Deze voorbeeldcode voor een zeer eenvoudige webtoepassing demo maakt gebruik van een telefoongesprek met een reactie # voor het voltooien van de verificatie van de gebruiker. Deze factor telefoongesprek wordt genoemd in een meerledige verificatie standaard modus.

De client-side code een meerledige verificatie elementen niet opgenomen. Omdat de aanvullende factoren onafhankelijk van de primaire verificatie zijn, kunt u deze toevoegen zonder de interface voor bestaande wijzigen. De API's in de SDK van meerdere factoren kunt u aanpassen aan de gebruikerservaring, maar hoeft u wellicht geen helemaal te wijzigen.

De server-side code wordt standaard in de verificatiemodus toegevoegd in stap 2. Een PfAuthParams-object gemaakt met de parameters die vereist voor de controle van de standaard-modus zijn: gebruikersnaam, telefoon nummer, en modus en het pad naar het certificaat (CertFilePath), die is vereist voor elke aanroep. Zie het voorbeeld van een bestand in de SDK voor een demonstratie van alle parameters in de PfAuthParams.

De code wordt vervolgens het PfAuthParams-object doorgegeven aan de functie pf_authenticate(). De retourwaarde geeft aan dat het slagen of mislukken van de verificatie. CallStatus en errorID, de uitvoerparameters bevatten gegevens die extra oproep resultaat. De oproep resultaatcodes worden beschreven in het bestand met resultaten gesprek in de SDK.

Deze minimale implementatie kan worden geschreven in een een paar regels. In de productiecode, zou u bevatten echter meer verfijnde foutafhandeling, extra databasecode en een betere gebruikerservaring.

### <a name="web-client-code"></a>Web Client-Code

Het volgende is web client-code voor een demo pagina.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Server-Side Code

Meerledige verificatie is geconfigureerd en uitgevoerd in stap 2 in de volgende code aan de serverzijde. Standaardmodus (MODE_STANDARD) is een telefoongesprek waaraan de gebruiker reageert door de #-toets te drukken.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
