<properties 
    pageTitle="Hoe u een telefoongesprek van Twilio (.NET) | Microsoft Azure" 
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden van code in .NET geschreven." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Het maken van een telefoongesprek voeren via Twilio in de Webrol van een op Azure

Deze handleiding wordt beschreven hoe Twilio gebruiken om een oproep van een webpagina in Azure. De uiteindelijke toepassing vraagt de gebruiker om waarden, telefoongesprek, zoals in de volgende schermafdruk.

![Azure call-formulier met behulp van Twilio en ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Vereisten

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verwerving van een Twilio account en verificatie token. Om te beginnen met Twilio, meld u aan bij [https://www.twilio.com/try-twilio][try_twilio]. U kunt evalueren prijzen op [http://www.twilio.com/pricing][twilio_pricing]. Zie voor informatie over de API die door Twilio [http://www.twilio.com/voice/api][twilio_api].
2. De bibliotheek Twilio .NET toevoegen aan uw Webrol. Zie ' de Twilio bibliotheken toevoegen aan uw webproject rol' verderop in dit onderwerp.

U moet vertrouwd zijn met het maken van een eenvoudige web-functie op Azure.

## <a name="howtocreateform"></a>Procedure: een webformulier voor een verbinding maken

<a id="use_nuget"></a>De Twilio-bibliotheken toevoegen aan uw webproject rol:

1.  Open uw oplossing in Visual Studio.
2.  Met de rechtermuisknop op **verwijzingen**.
3.  Klik op **NuGet pakketten te beheren**.
4.  Klik op **on line**.
5.  Typ in het zoekvak online *twilio*.
6.  Klik op **installeren** in het pakket Twilio.

De volgende code ziet u hoe een webformulier maken voor het ophalen van gegevens voor een verbinding van de gebruiker. In dit voorbeeld wordt een functie van ASP.NET web met de naam **TwilioCloud** gemaakt.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Procedure: de code als u het gesprek wilt maken
De volgende code wordt aangeroepen wanneer de gebruiker het formulier is voltooid, maakt het bericht van de aanroep en de aanroep genereert. In dit voorbeeld wordt de code uitgevoerd in de gebeurtenis-handler onclick van de knop op het formulier. (Gebruik uw Twilio account en verificatie in plaats van de tijdelijke aanduiding waarden toegewezen aan **accountSID** en **authToken** in de onderstaande code token).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

De oproep is gedaan en het eindpunt van de Twilio API-versie en de oproepstatus weergegeven. De volgende schermafbeelding ziet u uitvoer van een steekproef uitvoeren.

![Azure oproep reactie met Twilio en ASP.NET][twilio_dotnet_basic_form_output]

Meer informatie over TwiML vindt u op [http://www.twilio.com/docs/api/twiml][twiml]. Meer informatie over &lt;zeg&gt; en andere Twilio bewerkingen kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Volgende stappen
Deze code is aangegeven, zodat u met behulp van Twilio in de rol van een ASP.NET-webpagina op Azure basisfunctionaliteit geleverd. Voordat u implementeert voor Azure in productie, kunt u foutafhandeling meer of andere functies toe te voegen. Bijvoorbeeld:

* In plaats van een webformulier, kunt u Azure Blob-opslag of een exemplaar Azure SQL-Database opgeslagen telefoonnummers en tekst. Zie voor meer informatie over het gebruik van BLOB's in Azure, [het gebruik van de Azure Blob storage-service in .NET][howto_blob_storage_dotnet]. Zie voor informatie over het gebruik van SQL-Database [Azure SQL-Database in .NET-toepassingen gebruiken][howto_sql_azure_dotnet].
* Met RoleEnvironment.getConfigurationSettings kunt u de Twilio-ID ophalen en verificatietoken van de implementatie van de configuratie-instellingen, in plaats van harde codering de waarden in het formulier. Zie voor informatie over de RoleEnvironment klasse [Namespace Microsoft.WindowsAzure.ServiceRuntime][azure_runtime_ref_dotnet].
* Lees de richtlijnen op [https://www.twilio.com/docs/security]Twilio[twilio_docs_security].
* Meer informatie over Twilio op [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden van Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
