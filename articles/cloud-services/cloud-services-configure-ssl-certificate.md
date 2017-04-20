<properties 
    pageTitle="SSL configureren voor een cloud-service (klassiek) | Microsoft Azure" 
    description="Informatie over het opgeven van een HTTPS-eindpunt voor de rol van een website en het uploaden van een SSL-certificaat voor het beveiligen van uw toepassing." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL configureren voor een toepassing in Azure

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-configure-ssl-certificate-portal.md)
- [Azure klassieke portal](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL) versleuteling is de meest gebruikte methode van het beveiligen van gegevens die worden verzonden via het internet. Deze veel voorkomende taak wordt beschreven hoe u een HTTPS-eindpunt voor een Webrol opgeeft en het uploaden van een SSL-certificaat voor het beveiligen van uw toepassing.

> [AZURE.NOTE] De procedures in deze taak van toepassing op Azure Cloud diensten; Zie [Dit](../app-service-web/web-sites-configure-ssl-certificate.md) artikel voor App-Services.

Deze taak wordt een productie-implementatie. Aan het einde van dit onderwerp vindt u informatie over het gebruik van een staging-implementatie.

Lees [Dit](cloud-services-how-to-create-deploy.md) artikel eerst hebt u niet nog een cloud-service gemaakt.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Stap 1: Een SSL-certificaat ophalen

SSL configureren voor een toepassing, moet u eerst een SSL-certificaat dat is ondertekend door een certificeringsinstantie (CA), een vertrouwde derde partij die certificaten voor dit doel uitgeeft te halen. Als u nog geen een, moet u aanvragen bij een bedrijf dat SSL-certificaten verkoopt.

Het certificaat moet aan de volgende vereisten voldoen voor SSL-certificaten in Azure:

-   Het certificaat bevat een persoonlijke sleutel.
-   Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
-   De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de service cloud. U kunt geen een SSL-certificaat van een certificeringsinstantie (CA) voor het domein cloudapp.net. U dient aan te schaffen een aangepaste domeinnaam moet worden gebruikt wanneer toegang tot uw service. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de naam van het aangepaste domein gebruikt voor toegang tot uw toepassing overeenkomen met de certificaatnaam. Bijvoorbeeld als de naam van uw aangepaste domein **contoso.com** u zou een certificaat aanvragen bij uw Certificeringsinstantie voor * **. contoso.com** of * *www.contoso.com**.
-   Het certificaat moet een minimum van 2048-bits codering te gebruiken.

Voor testdoeleinden kunt u [maken](cloud-services-certs-create.md) en gebruiken van een zelf-ondertekend certificaat. Een zelfondertekend certificaat niet is geverifieerd door een Certificeringsinstantie en het cloudapp.net domein als de URL van de website kunt gebruiken. De volgende taak wordt bijvoorbeeld een zelf-ondertekend certificaat waarin de algemene naam (CN) die wordt gebruikt in het certificaat **sslexample.cloudapp.net is**.

Vervolgens moet u informatie over het certificaat opnemen in de definitie van service en service-configuratiebestanden.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Stap 2: De definitie en de configuratie-bestanden wijzigen

Uw toepassing moet worden geconfigureerd voor het gebruik van het certificaat en een eindpunt HTTPS moet worden toegevoegd. Als gevolg hiervan, de definitie van service en service-configuratiebestanden moeten worden bijgewerkt.

1.  Open bestand (CSDEF) van de definitie, toevoegen van een sectie van de **certificaten** in de sectie **WebRole** in uw ontwikkelomgeving en bevatten de volgende informatie over het certificaat (en de tussenliggende certificaten):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    **Certificaten** definieert de naam van onze certificaat, de locatie en de naam van de winkel waar het zich bevindt.
    
    Machtigingen (`permisionLevel` kenmerk) kan worden ingesteld op een van de volgende waarden:

  	| Waarde van machtiging  | Beschrijving |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Standaard)** Alle processen van de rol van toegang tot de persoonlijke sleutel. |
  	| verhoogde          | Alleen verhoogde processen toegang tot de persoonlijke sleutel.|

2.  In het definitiebestand service toevoegen een **InputEndpoint** element in de sectie **eindpunten** HTTPS inschakelen:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Voeg een **bindende** element in de sectie **Sites** in het definitiebestand service. In deze sectie wordt een HTTPS-binding het eindpunt toewijzen aan de site toegevoegd:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    De vereiste wijzigingen aan het bestand van de definitie zijn voltooid, maar nog steeds moet u informatie over het certificaat toevoegen aan het bestand van de configuratie.

4.  Toevoegen in uw service configuratiebestand (CSCFG), ServiceConfiguration.Cloud.cscfg, een sectie **certificaten** binnen de sectie **rol** , vervangen de vingerafdruk Voorbeeldwaarde met die van uw certificaat hieronder weergegeven:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Dit voorbeeld maakt gebruik van **sha1** voor de vingerafdruk-algoritme. Geeft de juiste waarde voor Vingerafdruk-algoritme van uw certificaat.)

Nu dat de definitie van service en service-configuratiebestanden zijn bijgewerkt pakket de implementatie voor het uploaden naar Azure. Als u **cspack**gebruikt, niet de vlag **/generateConfigurationFile** gebruiken zoals die wordt overschreven door de gegevens van het certificaat dat u hebt ingevoegd.

## <a name="step-3-upload-a-certificate"></a>Stap 3: Een certificaat uploaden

Het installatiepakket is bijgewerkt om het certificaat te gebruiken en een eindpunt HTTPS is toegevoegd. U kunt nu het pakket en het certificaat uploaden naar Azure met Azure klassieke portal.

1. Log in op de [Azure klassieke portal][]. 
2. **Cloud Services** Klik in het navigatievenster links.
3. Klik op de gewenste cloud-service.
4. Klik op het tabblad **certificaten** .

    ![Klik op het tabblad certificaten](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klik op de knop **uploaden** .

    ![Uploaden](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Geef het **bestand**, **wachtwoord**, klikt u op **Voltooien** (vinkje).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Stap 4: Verbinding maken met het exemplaar rol via HTTPS

De implementatie is actief in Azure, kunt u met behulp van HTTPS.

1.  Selecteer uw implementatie in de klassieke Azure portal, vervolgens klikt u op de koppeling onder de **URL van de Site**.

    ![URL van de site bepalen][2]

2.  Wijzigen van de koppeling voor het gebruik van **https** in plaats van **http**in uw webbrowser en Ga naar de pagina.

    >[AZURE.NOTE] Als u een zelf-ondertekend certificaat wanneer u naar een HTTPS-eindpunt dat is gekoppeld aan de zelf-ondertekend certificaat bladert, dat wordt er een certificaatfout is opgetreden in de browser. Met een certificaat dat is ondertekend door een vertrouwde certificeringsinstantie lost dit probleem; in de tussentijd kunt u de fout negeren. (Een andere mogelijkheid is het zelfondertekende certificaat aan het certificaatarchief van de gebruiker vertrouwd certificaat toevoegen.)

    ![Voorbeeld van website SSL][3]

Als u SSL gebruiken voor de implementatie van een gefaseerde installatie in plaats van een productie-implementatie wilt, moet u eerst de URL die wordt gebruikt voor de implementatie voor gefaseerde installatie. Uw cloud-service op de staging-omgeving implementeren zonder een certificaat of het certificaatinformatie. Eenmaal geïnstalleerd, kunt u de URL op basis van een GUID die is opgenomen in de Azure klassieke portal **Website URL** veld bepalen. Een certificaat maken met de algemene naam (CN) gelijk is aan de GUID gebaseerde URL (bijvoorbeeld **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). De Azure klassieke portal gebruiken om het certificaat toevoegen aan de gefaseerde cloud-service. Informatie over het certificaat vervolgens toevoegen aan de bestanden CSDEF en CSCFG en de gefaseerde implementatie voor het gebruik van het nieuwe pakket bijwerken opnieuw inpakken van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van de service cloud](cloud-services-how-to-configure.md).
* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Uw cloud-service beheren](cloud-services-how-to-manage.md).


  [Azure klassieke portal]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
