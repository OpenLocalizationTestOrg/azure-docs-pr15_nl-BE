#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Een aangepaste domeinnaam voor een Azure website configureren

Wanneer u een website maakt, Azure een beschrijvende subdomein biedt op het domein azurewebsites.net zodat uw gebruikers toegang tot uw website met behulp van een URL als http://&lt;mijnsite >. azurewebsites.net. Als u uw websites voor gedeelde of standaardmodus configureren, kunt u uw website toewijzen aan uw eigen domeinnaam.

Desgewenst kunt u Azure verkeer Manager laden saldo binnenkomend verkeer naar uw website. Zie voor meer informatie over de werking van beheer van netwerkverkeer met Websites [Beheren Azure websites verkeer met Azure verkeer Manager][trafficmanager].

> [AZURE.NOTE] De procedures in deze taak van toepassing op Websites van Azure; Zie <a href="/develop/net/common-tasks/custom-dns/">een aangepaste domeinnaam in Azure configureren</a>voor Cloud-Services.

> [AZURE.NOTE] De stappen in deze taak moet u uw websites configureren voor gedeelde of de modus standaard gehelen hoeveel u voor uw abonnement gefactureerd. Zie <a href="/pricing/details/web-sites/">Websites prijzen Details</a> voor meer informatie.

In dit artikel:

-   [Wat zijn de records CNAME en A](#understanding-records)
-   [Uw websites voor gedeelde of standard-modus configureren](#bkmk_configsharedmode)
-   [Websites toevoegen aan beheer van verkeer](#trafficmanager)
-   [Een CNAME voor uw aangepaste domein toevoegen](#bkmk_configurecname)
-   [Een A-record voor uw aangepaste domein toevoegen](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Begrijpen van de records CNAME en A</h2>

CNAME (aliasrecords of) en een records kunnen u een domeinnaam koppelen aan een website, maar elke verschillend werken.

###<a name="cname-or-alias-record"></a>Record of een CNAME-Alias

Een *specifiek* domein, zoals **contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een canonieke naam. In dit geval de canonieke naam is de de ** &lt;Mijntoep >. azurewebsites.net** de domeinnaam van uw website Azure of de ** &lt;Mijntoep >. trafficmgr.com** -naam van uw serviceprofiel verkeer. Eenmaal gemaakt, de CNAME een alias gemaakt voor de ** &lt;Mijntoep >. azurewebsites.net** of ** &lt;Mijntoep >. trafficmgr.com** domeinnaam. De CNAME-post wordt omgezet in het IP-adres van uw ** &lt;Mijntoep >. azurewebsites.net** of ** &lt;Mijntoep >. trafficmgr.com** domeinnaam automatisch, zodat als het IP-adres van de website wijzigt, hoeft u geen actie te ondernemen.

> [AZURE.NOTE] Sommige registratieservices domein alleen kunnen u subdomeinen toewijzen wanneer u een CNAME-record, bijvoorbeeld www.contoso.com en geen basis-namen, zoals contoso.com. Zie de documentatie bij uw registratieservice, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-post op de CNAME-record</a>of het <a href="http://tools.ietf.org/html/rfc1035">IETF-domeinnamen - implementatie en specificatie</a> document voor meer informatie over CNAME-records.

###<a name="a-record"></a>Een record

Een A-record koppelt een domein, bijvoorbeeld **contoso.com** of **www.contoso.com**, *of in een jokertekendomein* , zoals ** \*. contoso.com**, een IP-adres. In het geval van een Website Azure, het virtuele IP van de service of een bepaald IP-het adres die u hebt aangeschaft voor uw website. Dus het belangrijkste voordeel van een A-record via een CNAME-record is dat u kunt een post met een jokerteken, zoals * **. contoso.com**, die zou staat aanvragen verwerken voor meerdere subdomeinen, zoals * *mail.contoso.com*,*, *, *login.contoso.com**, of * *www.contso.com**.

> [AZURE.NOTE] Aangezien een A-record is toegewezen aan een statisch IP-adres, kan het automatisch omzetten in wijzigingen het IP-adres van uw website. Een IP-adres voor gebruik met een records weergegeven wanneer u aangepaste domein naam instellingen voor uw website configureren; Deze waarde kan echter wijzigen als het verwijderen en opnieuw maken van uw website of de website modus back-vrij te maken.

> [AZURE.NOTE] A-records worden niet gebruikt voor taakverdeling met verkeer Manager. Voor meer informatie, Zie [Beheren van Azure websites verkeer met Azure verkeer Manager][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Uw websites voor gedeelde of standard-modus configureren</h2>

Een aangepaste domeinnaam instellen op een website is alleen beschikbaar voor de gedeelde en standaard modi voor Azure websites. Voordat u een website uit de gratis website naar de gedeelde of standaard website modus overschakelt, moet u eerst bestedingslimiet caps in plaats voor uw abonnement op Website verwijderen. Zie voor meer informatie over gedeelde en standaard modus prijzen, [Prijzen Details][PricingDetails].

1. Open in uw browser, de [Portal Management][portal].
2. Klik op de naam van uw site op het tabblad **Websites** .

    ![][standardmode1]

3. Klik op het tabblad **schaal** .

    ![][standardmode2]


4. Stel de modus website door te klikken op **gedeelde**in de sectie **Algemeen** .

    ![][standardmode3]

    > [AZURE.NOTE] Als u beheer van verkeer met deze website gebruikt, moet u standaard modus selecteren in plaats van gedeeld.

5. Klik op **Opslaan**.
6. Over de stijging van de kosten voor de gedeelde modus (of voor de standaardmodus als u standaard kiest), klik op **Ja** als u akkoord gaat.

    <!--![][standardmode4]-->

    **Opmerking**<br />
   Als een fout 'Schaal configureren voor website website name' mislukt' wordt weergegeven, kunt u de knop details voor meer informatie.

<a name="trafficmanager"></a><h2>(Optioneel) De websites toevoegen aan beheer van verkeer</h2>

Als u uw website met verkeer Manager gebruiken wilt, moet u de volgende stappen uitvoeren.

1. Als u nog geen profiel beheer van verkeer, het gebruik van de informatie in [een verkeer Manager profiel met behulp van snelle invoer maken] [ createprofile] maken. Opmerking de **. trafficmgr.com** -naam die is gekoppeld aan uw serviceprofiel verkeer. Dit wordt gebruikt in een latere stap.

2. Gebruik de informatie [toevoegen of verwijderen van eindpunten] in[ addendpoint] aan uw website toevoegen als een eindpunt in uw serviceprofiel verkeer.

    > [AZURE.NOTE] Als uw website niet wordt weergegeven bij het toevoegen van een eindpunt, controleert u of deze is geconfigureerd voor de modus standaard. Om te kunnen werken met beheer van verkeer moet u de standaardmodus voor uw website.

3. Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**.

4. Nu kunt u selecteren of geef de CNAME-records zoeken. Mogelijk moet naar beneden selecteert u het recordtype dat in een vervolgkeuzelijst of gaat u naar een pagina voor geavanceerde instellingen. U moet zoeken naar woorden **CNAME**, **Alias**of **subdomeinen**.

5. Ook moet u het domein of subdomein alias opgeven voor de CNAME. Bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**.

5. U moet ook een hostnaam die is de canonieke naam van deze CNAME-alias opgeven. Dit is de **. trafficmgr.com** naam voor uw website.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.trafficmgr.com**, de domeinnaam van een website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host-naam/subdomein</strong></td>
<td><strong>Canonieke domein</strong></td>
</tr>
<tr>
<td>www</td>
<td>Contoso.trafficmgr.com</td>
</tr>
</table>

Een bezoeker van **www.contoso.com** zien nooit de waarde true host (contoso.azurewebsite.net), zodat het doorstuurproces onzichtbaar voor de eindgebruiker is.

> [AZURE.NOTE] Als u beheer van verkeer op een website, hoeft u niet de stappen in de volgende gedeelten, '**een CNAME voor uw aangepaste domein toevoegen**' en '**een A-record voor uw aangepaste domein toevoegen**'. De CNAME-record gemaakt in de vorige stappen route binnenkomend verkeer naar verkeer Manager, die stuurt het verkeer vervolgens door naar de website endpoint(s).

<a name="bkmk_configurecname"></a><h2>Een CNAME voor uw aangepaste domein toevoegen</h2>

Om een CNAME-record maakt, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van hulpmiddelen die door uw registratieservice. Elke registrator heeft een soortgelijke, maar enigszins verschillende manier een CNAME-record, maar de concepten zijn hetzelfde.

1. Een van deze methoden gebruiken om te zoeken naar de **. azurewebsite.net** domeinnaam toegewezen aan uw website.

    * Meld u aan bij de [Azure Management Portal][portal]uw website, **Dashboard**selecteren en selecteer vervolgens de vermelding van de **URL van de Site** te vinden in de sectie **Overzicht** .

    * Installeren en configureren van [Azure Powershell](/manage/install-and-configure-windows-powershell/)en gebruikt u de volgende opdracht:

            get-azurewebsite yoursitename | select hostnames

    * Installeer en configureer de [Azure Command Line Interface](/manage/install-and-configure-cli/), en gebruik vervolgens de volgende opdracht:

            azure site domain list yoursitename

    Sla dit **. azurewebsite.net** naam, zoals deze wordt gebruikt in de volgende stappen.

3. Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**.

4. Nu kunt u selecteren of geef de CNAME-records zoeken. Mogelijk moet naar beneden selecteert u het recordtype dat in een vervolgkeuzelijst of gaat u naar een pagina voor geavanceerde instellingen. U moet zoeken naar woorden **CNAME**, **Alias**of **subdomeinen**.

5. Ook moet u het domein of subdomein alias opgeven voor de CNAME. Bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein, kan deze worden vermeld als de "**@**" symbool in een van uw registratieservice DNS-hulpprogramma's.

5. U moet ook een hostnaam die is de canonieke naam van deze CNAME-alias opgeven. Dit is de **. azurewebsite.net** naam voor uw website.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.azurewebsite.net**, de domeinnaam van een website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host-naam/subdomein</strong></td>
<td><strong>Canonieke domein</strong></td>
</tr>
<tr>
<td>www</td>
<td>Contoso.azurewebsite.NET</td>
</tr>
</table>

Een bezoeker van **www.contoso.com** zien nooit de waarde true host (contoso.azurewebsite.net), zodat het doorstuurproces onzichtbaar voor de eindgebruiker is.

> [AZURE.NOTE] Het bovenstaande voorbeeld is alleen van toepassing op het verkeer bij het subdomein __www__ . Aangezien u jokertekens met CNAME-records gebruiken kunt, moet u een CNAME voor elk domein/subdomein maken. Als u wilt, zoals verkeer van subdomeinen, directe *. contoso.com, aan het adres van uw azurewebsite.net, kunt u een post __URL omgeleid__ of __Volgende URL__ in uw DNS-instellingen configureren of een A-record maken.

> [AZURE.NOTE] Het kan even duren voor uw CNAME doorgeven via het DNS-systeem. U kunt de CNAME van de website niet instellen, totdat de CNAME is doorgegeven. Een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> kunt u controleren of de CNAME beschikbaar is.

###<a name="add-the-domain-name-to-your-website"></a>De domeinnaam aan uw website toevoegen

Nadat u de CNAME-record voor de domeinnaam is doorgegeven, moet u deze koppelen aan uw website. U kunt de aangepaste domeinnaam gedefinieerd door de CNAME-record voor uw website met behulp van hetzij de Azure opdrachtregelinterface (CLI Azure) of via de Portal Azure Management toevoegen.

**Een domeinnaam met behulp van opdrachtregelprogramma's toevoegen**

Installeer en configureer de [Interface met opdrachtregel Azure](/manage/install-and-configure-cli/), en gebruikt u de volgende opdracht:

    azure site domain add customdomain yoursitename

Bijvoorbeeld, het volgende toegevoegd **www.contoso.com** aangepaste domeinnaam naar de website van **contoso.azurewebsite.net** :

    azure site domain add www.contoso.com contoso

U kunt controleren of de aangepaste domeinnaam is toegevoegd aan de website met de volgende opdracht:

    azure site domain list yoursitename

De lijst die wordt geretourneerd door deze opdracht moet bevatten de aangepaste domeinnaam als de standaard- **. azurewebsite.net** post.

**Een domeinnaam met de Azure Management Portal toevoegen**

1. Open in uw browser de [Azure Management Portal][portal].

2. Klik op de naam van uw site op het tabblad **Websites** **Dashboard**selecteren en selecteer **Domeinen beheren** vanaf de onderkant van de pagina.

    ![][setcname2]

6. Typ de domeinnaam die u hebt ingesteld in het tekstvak **DOMEINNAMEN** .

    ![][setcname3]

6. Klik op het selectievakje om de domeinnaam te accepteren.

Nadat de configuratie is voltooid, wordt de aangepaste naam worden vermeld in de sectie **domeinnamen** van de pagina **configureren** van uw website.

<a name="bkmk_configurearecord"></a><h2>Een A-Record voor uw aangepaste domein toevoegen</h2>

Een A-record maken, moet u eerst het IP-adres van uw website vinden. Vervolgens een vermelding in de DNS voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's die door uw registratieservice. Elke registrator heeft een soortgelijke, maar enigszins verschillende methode voor het opgeven van een A-record, maar de concepten zijn hetzelfde. Naast het maken van een A-record, moet u een CNAME-record die Azure worden gebruikt om te controleren of de A-record maken.

1. Open in uw browser de [Azure Management Portal][portal].

2. Klik op de naam van uw site op het tabblad **Websites** **Dashboard**selecteren en selecteer **Domeinen beheren** vanaf de onderkant van het scherm.

    ![][setcname2]

5. Zoek **Het IP-adres moet worden gebruikt bij het configureren van A-records**in het dialoogvenster **aangepaste domeinen beheren** . Kopieer het IP-adres. Deze worden gebruikt bij het maken van de A-record.

5. Opmerking in het dialoogvenster **aangepaste domeinen beheren** awverify domeinnaam aan het einde van de tekst boven aan het dialoogvenster. Moet zijn **awverify.mysite.azurewebsites.net** waarbij **mijnsite** de naam is van uw website. Kopiëren, is de domeinnaam die wordt gebruikt voor verificatie van de CNAME-record maken.

6. Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**.

6. Zoeken waar selecteren of invoeren van A- en CNAME-records. Mogelijk moet naar beneden selecteert u het recordtype dat in een vervolgkeuzelijst of gaat u naar een pagina voor geavanceerde instellingen.

7. Voer de volgende stappen uit om de A-record te maken:

    1. Selecteer of typ het domein of subdomein met de A-record. Selecteer bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u een jokerteken vermelding maken voor alle subdomeinen, voer '__*__'. Hieronder vallen alle subdomeinen, zoals **mail.customdomain.com**, **login.customdomain.com**en **www.customdomain.com**.

        Als u maken voor het hoofddomein van een A-record wilt, kan deze worden vermeld als de "**@**" symbool in een van uw registratieservice DNS-hulpprogramma's.

    2. Voer het IP-adres van uw cloud-service in het opgegeven veld. Hiermee wordt de domein-vermelding in de A-record met het IP-adres van uw implementatie van cloud-service gebruikt.

        Bijvoorbeeld, de volgende die record al het verkeer van **contoso.com** naar **137.135.70.239**, het IP-adres van de gedistribueerde toepassing doorstuurt:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host-naam/subdomein</strong></td>
        <td><strong>IP-adres</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        In dit voorbeeld wordt het maken van een A-record voor het hoofddomein. Als u een jokerteken vermelding wilt ter dekking van alle subdomeinen maken, typt u '__*__' als de subdomein.

7. Vervolgens maakt u een CNAME-record is een alias van **awverify**en een canonieke domein van de **awverify.mysite.azurewebsites.net** die u eerder hebt gekregen.

    > [AZURE.NOTE] Terwijl een alias van awverify voor sommige registratieservices werkt kan, kunnen anderen het volledige alias domeinnaam van awverify.www.customdomainname.com of awverify.customdomainname.com nodig.

    Bijvoorbeeld maakt de volgende u een CNAME-record die Azure gebruiken kunt om te controleren of de configuratie van een record.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias/Host-naam/subdomein</strong></td>
    <td><strong>Canonieke domein</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Het kan even duren voor de awverify CNAME doorgeven via het DNS-systeem. U kunt de aangepaste domeinnaam gedefinieerd door de A-record voor de website totdat de CNAME-awverify heeft doorgegeven niet instellen. Een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> kunt u controleren of de CNAME beschikbaar is.

###<a name="add-the-domain-name-to-your-website"></a>De domeinnaam aan uw website toevoegen

Nadat de **awverify** CNAME-record voor de domeinnaam heeft doorgegeven, kunt u vervolgens de aangepaste domein gedefinieerd door de A-record met uw website te koppelen. U kunt de aangepaste domeinnaam gedefinieerd door de A-record voor uw website met behulp van hetzij de Azure CLI of via de Portal Azure Management toevoegen.

**Een domeinnaam met de Azure opdrachtregelinterface (CLI Azure) toevoegen**

Installeer en configureer de [CLI Azure](/manage/install-and-configure-cli/), en gebruik vervolgens de volgende opdracht:

    azure site domain add customdomain yoursitename

Bijvoorbeeld wordt het volgende de naam van een aangepaste domein **contoso.com** aan de **contoso.azurewebsite.net** website toevoegen:

    azure site domain add contoso.com contoso

U kunt controleren of de aangepaste domeinnaam is toegevoegd aan de website met de volgende opdracht:

    azure site domain list yoursitename

De lijst die wordt geretourneerd door deze opdracht moet bevatten de aangepaste domeinnaam als de standaard- **. azurewebsite.net** post.

**Een domeinnaam met de Azure Management Portal toevoegen**

1. Open in uw browser de [Azure Management Portal][portal].

2. Klik op de naam van uw site op het tabblad **Websites** **Dashboard**selecteren en selecteer **Domeinen beheren** vanaf de onderkant van de pagina.

    ![][setcname2]

6. Typ de domeinnaam die u hebt ingesteld in het tekstvak **DOMEINNAMEN** .

    ![][setcname3]

6. Klik op het selectievakje om de domeinnaam te accepteren.

Nadat de configuratie is voltooid, wordt de aangepaste naam worden vermeld in de sectie **domeinnamen** van de pagina **configureren** van uw website.

> [AZURE.NOTE] Nadat u de aangepaste domeinnaam gedefinieerd door de A-record voor uw website hebt toegevoegd, kunt u de awverify CNAME-record met de hulpprogramma's van uw registratieservice. Echter, als u wilt toevoegen een andere record in de toekomst, hebt u de awverify opnieuw te record voordat u de nieuwe domeinnaam kunt koppelen, gedefinieerd door de nieuwe record met de website.

## <a name="next-steps"></a>Volgende stappen

-   [Het beheren van websites](/manage/services/web-sites/how-to-manage-websites/)

-   [Een SSL-certificaat voor websites configureren](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
