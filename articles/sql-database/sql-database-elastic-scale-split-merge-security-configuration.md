<properties 
    pageTitle="Gesplitste samenvoegen Beveiligingsconfiguratie | Microsoft Azure" 
    description="Instellen van x409 certificaten voor codering" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Gesplitste samenvoegen Beveiligingsconfiguratie  

Het splitsen/samenvoegen om service te gebruiken, moet u beveiliging correct te configureren. De service is onderdeel van de elastische schaal-functie van Microsoft Azure SQL-Database. Zie [elastische schaal splitsen en samenvoegen-zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md)voor meer informatie.

## <a name="configuring-certificates"></a>Certificaten configureren

Certificaten worden op twee manieren geconfigureerd. 

1. [Voor het configureren van de SSL-certificaat](#To-Configure-the-SSL#Certificate)
2. [Clientcertificaten configureren](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Certificaten verkrijgen

Certificaten kunnen worden verkregen van openbare certificeringsinstanties (CA's) of de [Certificaat-Service van Windows](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Dit zijn de beste methoden om certificaten te verkrijgen.

Als deze opties niet beschikbaar zijn, kunt u een **zelfondertekende certificaten**genereren.
 
## <a name="tools-to-generate-certificates"></a>Hulpprogramma's voor het genereren van certificaten

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>De hulpprogramma's uitvoeren

* Van ontwikkelaar vanaf de opdrachtprompt voor visuele Studios Zie [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx) 

    Als u hebt geïnstalleerd, gaat u naar:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Krijgen de WDK van [Windows 8.1: Download kits en extra](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Voor het configureren van de SSL-certificaat
Een SSL-certificaat is vereist voor het versleutelen van de communicatie en verificatie van de server. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="create-a-new-self-signed-certificate"></a>Een nieuw, zelfondertekend certificaat maken

1.    [Een zelfondertekend certificaat maken](#Create-a-Self-Signed-Certificate)
2.    [PFX-bestand maken voor zelfondertekende SSL-certificaat](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [SSL-certificaat voor de Service Cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [SSL-certificaat in het configuratiebestand Service bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [SSL Certificate Authority importeren](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Gebruik van een bestaand certificaat uit het certificaatarchief
1. [SSL-certificaat uit een certificaatarchief exporteren](#Export-SSL-Certificate-From-Certificate-Store)
2. [SSL-certificaat voor de Service Cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
3. [SSL-certificaat in het configuratiebestand Service bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Een bestaand certificaat in een PFX-bestand gebruiken

1. [SSL-certificaat voor de Service Cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
2. [SSL-certificaat in het configuratiebestand Service bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Clientcertificaten configureren
Clientcertificaten nodig zijn om te verifiëren, aanvragen voor de service. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="turn-off-client-certificates"></a>Clientcertificaten uitschakelen
1.    [Op certificaten gebaseerde verificatie uitschakelen](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Nieuwe zelfondertekende certificaten uitgeven
1.    [Een zelfondertekend certificeringsinstantie maken](#Create-a-Self-Signed-Certification-Authority)
2.    [CA-certificaat voor de Service Cloud uploaden](#Upload-CA-Certificate-to-Cloud-Service)
3.    [CA-certificaat in het configuratiebestand Service bijwerken](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Clientcertificaten uitgeven](#Issue-Client-Certificates)
5.    [PFX-bestanden maken voor clientcertificaten](#Create-PFX-files-for-Client-Certificates)
6.    [Client-certificaat importeren](#Import-Client-Certificate)
7.    [Client-certificaat vingerafdrukken kopiëren](#Copy-Client-Certificate-Thumbprints)
8.    [Toegestane Clients in het configuratiebestand voor de Service configureren](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Bestaande certificaten gebruiken
1.    [Openbare sleutel CA zoeken](#Find-CA-Public Key)
2.    [CA-certificaat voor de Service Cloud uploaden](#Upload-CA-certificate-to-cloud-service)
3.    [CA-certificaat in het configuratiebestand Service bijwerken](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Client-certificaat vingerafdrukken kopiëren](#Copy-Client-Certificate-Thumbprints)
5.    [Toegestane Clients in het configuratiebestand voor de Service configureren](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Controle op intrekking certificaat Client configureren](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Toegestane IP-adressen

Toegang tot de eindpunten van de service kan worden beperkt tot specifieke bereiken van IP-adressen.

## <a name="to-configure-encryption-for-the-store"></a>Codering voor de winkel configureren

Een certificaat is vereist voor het coderen van de referenties die zijn opgeslagen in het metagegevensarchief met. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="use-a-new-self-signed-certificate"></a>Een nieuw, zelfondertekend certificaat gebruiken

1.     [Een zelfondertekend certificaat maken](#Create-a-Self-Signed-Certificate)
2.     [PFX-bestand voor zelfondertekende versleutelingscertificaat maken](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Coderingscertificaat om Service Cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Coderingscertificaat update in Service-configuratiebestand](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Gebruik een bestaand certificaat uit het certificaatarchief

1.     [Coderingscertificaat uit een certificaatarchief exporteren](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Coderingscertificaat om Service Cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Coderingscertificaat update in Service-configuratiebestand](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Een bestaand certificaat in een PFX-bestand gebruiken

1.     [Coderingscertificaat om Service Cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Coderingscertificaat update in Service-configuratiebestand](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>De standaardconfiguratie

De standaardconfiguratie weigert alle toegang tot de HTTP-eindpunt. Dit is de aanbevolen instelling, omdat de aanvragen voor deze eindpunten kunnen gevoelige informatie zoals databasereferenties uitvoeren.
De standaardconfiguratie kunt alle toegang tot het eindpunt HTTPS. Deze instelling kan verder worden beperkt.

### <a name="changing-the-configuration"></a>De configuratie wijzigen

De groep toegangsbeheerregels die betrekking hebben op en het eindpunt is geconfigureerd de **<EndpointAcls>** met de **service-configuratiebestand**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

De regels in een besturingselement toegangsgroep zijn geconfigureerd in een <AccessControl name=""> sectie van het bestand van de configuratie. 

De indeling wordt uitgelegd in de documentatie van Network Access Control Lists.
Bijvoorbeeld als u alleen IP-adressen in het bereik van 100.100.0.0-100.100.255.255 voor toegang tot het eindpunt HTTPS, eruit de regels als volgt:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Denial of service voorkomen

Er zijn twee verschillende mechanismen worden gedetecteerd en Denial of Service-aanvallen te voorkomen:

*    Beperk het aantal gelijktijdige aanvragen per externe host (standaard uit)
*    Beperken van de snelheid van de toegang per externe host (op standaard)

Deze zijn gebaseerd op de functies die verder beschreven in dynamische IP-beveiliging in IIS. Bij deze configuratie wijzigen pas op de volgende factoren:

* De werking van de proxy- en NAT-apparaten via de externe hostinformatie
* Elke aanvraag alle bronnen in de Webrol wordt geacht (b.v. laden scripts, afbeeldingen, enzovoort)

## <a name="restricting-number-of-concurrent-accesses"></a>Aantal gelijktijdige toegang beperken

De instellingen voor het configureren van dit probleem zijn:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Wijzig DynamicIpRestrictionDenyByConcurrentRequests in waar deze bescherming inschakelen.

## <a name="restricting-rate-of-access"></a>Snelheid van de toegang beperken

De instellingen voor het configureren van dit probleem zijn:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Het antwoord op een aanvraag geweigerd configureren

De volgende instelling configureert u het antwoord op een aanvraag geweigerd:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Raadpleeg de documentatie voor dynamische IP-beveiliging in IIS voor andere ondersteunde waarden.

## <a name="operations-for-configuring-service-certificates"></a>Bewerkingen voor het configureren van certificaten service
Dit onderwerp is alleen ter referentie. Volg de configuratiestappen in:

* Het SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Uitvoeren:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Aanpassen:

*    -n met de URL. Jokertekens ("CN = * .cloudapp .net ') en alternatieve namen ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") worden ondersteund.
*    e - met de vervaldatum van het certificaat een sterk wachtwoord maken en opgeven wanneer daarom wordt gevraagd.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>PFX-bestand maken voor het zelfondertekend certificaat voor SSL

Uitvoeren:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Voer wachtwoord in en vervolgens exporteren certificaat met deze opties:
* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-certificaat uit een certificaatarchief exporteren

* Certificaat zoeken
* Klik op Acties -> alle taken -> exporteren...
* Exporteer het certificaat naar een. Met deze opties PFX-bestand:
    * Ja, de persoonlijke sleutel exporteren
    * Indien mogelijk exporteren met alle certificaten in het certificeringspad * alle uitgebreide eigenschappen exporteren

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-certificaat uploaden naar cloud-service

Upload het certificaat met de bestaande of gegenereerd. Met het SSL-sleutelpaar PFX-bestand:

* Voer de gegevens voor de persoonlijke sleutels te beveiligen met een wachtwoord

## <a name="update-ssl-certificate-in-service-configuration-file"></a>SSL-certificaat in het configuratiebestand service bijwerken

De waarde van de volgende instelling in het configuratie-bestand bijwerken met de blauwdruk van het certificaat dat is geüpload naar de cloud-service:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL Certificate authority importeren

Volg deze stappen in alle account/machine die moeten communiceren met de service:

* Dubbelklik op het. CER-bestand in Windows Verkenner
* Klik op certificaat installeren in het dialoogvenster certificaat...
* Certificaten importeren in het archief met vertrouwde basiscertificeringsinstanties

## <a name="turn-off-client-certificate-based-authentication"></a>Op certificaten gebaseerde verificatie uitschakelen

Alleen op certificaten gebaseerde verificatie wordt ondersteund en het uitschakelen van deze kunnen worden publieke toegang tot de service-endpoints, tenzij er andere mechanismen bestaan (bijvoorbeeld Microsoft Azure Virtual Network).

Deze instellingen wijzigen op false in het configuratiebestand van de service om de functie uit te schakelen:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopieer de vingerafdruk hetzelfde als het SSL-certificaat in het CA-certificaat:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Een zelfondertekend certificeringsinstantie maken
Voer de volgende stappen uit om een zelfondertekend certificaat als een certificeringsinstantie maken:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Aanpassen

*    met de vervaldatum van de certificatie -e


## <a name="find-ca-public-key"></a>Openbare sleutel CA zoeken

Alle clientcertificaten moeten zijn uitgegeven door een certificeringsinstantie die wordt vertrouwd door de service. De openbare sleutel van de certificeringsinstantie die certificaten de client die voor verificatie worden gebruikt om te uploaden naar de cloud service vinden.

Als het bestand met de openbare sleutel niet beschikbaar is, uit het certificaatarchief exporteren:

* Certificaat zoeken
    * Zoekt u een clientcertificaat dat is uitgegeven door de certificeringsinstantie dezelfde
* Dubbelklik op het certificaat.
* Selecteer het tabblad certificaatpad in het dialoogvenster certificaat.
* Dubbelklik op de vermelding van de Certificeringsinstantie in het pad.
* Notities van de certificaateigenschappen.
* Sluit het dialoogvenster **certificaat** .
* Certificaat zoeken
    * Zoeken naar de Certificeringsinstantie die hierboven zijn vermeld.
* Klik op Acties -> alle taken -> exporteren...
* Exporteer het certificaat naar een. CER met deze opties:
    * **Nee, de persoonlijke sleutel niet exporteren**
    * Indien mogelijk exporteren met alle certificaten in het certificeringspad.
    * Alle uitgebreide eigenschappen exporteren.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA-certificaat uploaden naar cloud-service

Upload het certificaat met de bestaande of gegenereerd. CER-bestand met de openbare sleutel van de Certificeringsinstantie.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Update voor de certificeringsinstantie in het configuratiebestand service

De waarde van de volgende instelling in het configuratie-bestand bijwerken met de blauwdruk van het certificaat dat is geüpload naar de cloud-service:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

De waarde van de volgende instelling met de vingerafdruk van dezelfde update:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Clientcertificaten uitgeven

Elke gebruiker die toegang hebben tot de service moet een certificaat afgegeven voor his/hers exclusief gebruik en his/hers eigen sterke wachtwoord ter bescherming van de persoonlijke sleutel moet kiezen. 

De volgende stappen moeten worden uitgevoerd in dezelfde computer waar het CA-certificaat zelfondertekend is gegenereerd en opgeslagen:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Aanpassen:

* -n met een ID voor naar de client wordt geverifieerd met dit certificaat
* e - met de vervaldatum van het certificaat
* MyID.pvk en MyID.cer met unieke bestandsnamen voor dit certificaat

Met deze opdracht vraagt om een wachtwoord kan worden gemaakt en vervolgens één keer worden gebruikt. Gebruik een sterk wachtwoord.

## <a name="create-pfx-files-for-client-certificates"></a>PFX-bestanden voor client-certificaten maken

Voor elk clientcertificaat gegenereerd uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aanpassen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Voer wachtwoord in en vervolgens exporteren certificaat met deze opties:

* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren
* De persoon aan wie dit certificaat wordt uitgegeven moet het wachtwoord voor de uitvoer kiezen

## <a name="import-client-certificate"></a>Client-certificaat importeren

Iedere persoon voor wie een certificaat is afgegeven, moet het sleutelpaar in de machines die hij gebruikt om te communiceren met de service importeren:

* Dubbelklik op het. PFX-bestand in Windows Verkenner
* Importeren in het persoonlijke certificaatarchief met ten minste deze optie:
    * Alle uitgebreide eigenschappen worden gecontroleerd

## <a name="copy-client-certificate-thumbprints"></a>Client-certificaat vingerafdrukken kopiëren
Iedere persoon voor wie een certificaat is afgegeven moet als volgt te werk om de vingerafdruk van his/hers certificaat dat wordt toegevoegd aan het bestand van de configuratie:
* Certmgr.exe uitvoeren
* Selecteer het tabblad persoonlijke gegevens
* Dubbelklik op het certificaat moet worden gebruikt voor verificatie
* In het dialoogvenster certificaat dat verschijnt, selecteer het tabblad Details
* Controleer of dat weergeven wordt weergegeven op alle
* Selecteer het veld met de naam vingerafdruk in de lijst
* De waarde van de **niet-zichtbare Unicode-tekens voor het eerste cijfer verwijderen** vingerafdruk kopie alle spaties verwijderen

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Toegestane clients in het configuratiebestand voor de service configureren

De waarde van de volgende instelling in het configuratie-bestand bijwerken met een door komma's gescheiden lijst met de vingerafdrukken van clientcertificaten toegang hebben tot de service:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Controle op intrekking certificaat client configureren

De standaardinstelling wordt niet met de certificeringsinstantie voor client intrekkingsstatus gecontroleerd. Als u de controles, als de certificeringsinstantie die certificaten uitgegeven aan de client deze controles ondersteunt, de volgende instelling te wijzigen met een van de waarden die zijn gedefinieerd in de opsomming X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX-bestand voor de van zelfondertekende coderingscertificaten maken

Voor een versleutelingscertificaat uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aanpassen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Voer wachtwoord in en vervolgens exporteren certificaat met deze opties:
*    Ja, de persoonlijke sleutel exporteren
*    Alle uitgebreide eigenschappen exporteren
*    U moet het wachtwoord als het certificaat uploaden naar de cloud-service.

## <a name="export-encryption-certificate-from-certificate-store"></a>Coderingscertificaat uit een certificaatarchief exporteren

*    Certificaat zoeken
*    Klik op Acties -> alle taken -> exporteren...
*    Exporteer het certificaat naar een. Met deze opties PFX-bestand: 
  *    Ja, de persoonlijke sleutel exporteren
  *    Indien mogelijk exporteren met alle certificaten in het certificeringspad 
*    Alle uitgebreide eigenschappen exporteren

## <a name="upload-encryption-certificate-to-cloud-service"></a>Coderingscertificaat uploaden naar cloud-service

Upload het certificaat met de bestaande of gegenereerd. Met het sleutelpaar voor codering PFX-bestand:

* Voer de gegevens voor de persoonlijke sleutels te beveiligen met een wachtwoord

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Coderingscertificaat update in service-configuratiebestand

De waarde van de volgende instellingen in het configuratiebestand voor de service bijwerken met de blauwdruk van het certificaat dat is geüpload naar de cloud-service:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Veelvoorkomende bewerkingen met certificaat

* Het SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="find-certificate"></a>Certificaat zoeken

Ga als volgt te werk:

1. Mmc.exe uitvoeren.
2. Bestand -> module toevoegen/verwijderen...
3. Selecteer **certificaten**.
4. Klik op **toevoegen**.
5. Kies de locatie van het certificaat.
6. Klik op **Voltooien**.
7. Klik op **OK**.
8. Vouw **certificaten**uit.
9. Vouw het knooppunt certificate store.
10. Vouw het knooppunt van de onderliggende certificaat.
11. Selecteer een certificaat in de lijst.

## <a name="export-certificate"></a>Certificaat exporteren
In de **Wizard Certificaat exporteren**

1. Klik op **volgende**.
2. Selecteer **Ja**, **de persoonlijke sleutel exporteren**.
3. Klik op **volgende**.
4. Selecteer de gewenste uitvoerindeling bestand.
5. Controleer de gewenste opties.
6. Controleer het **wachtwoord**.
7. Geef een sterk wachtwoord en Bevestig het.
8. Klik op **volgende**.
9. Typ of Selecteer een bestandsnaam voor het opslaan van het certificaat (gebruik een. PFX-extensie).
10. Klik op **volgende**.
11. Klik op **Voltooien**.
12. Klik op **OK**.

## <a name="import-certificate"></a>Certificaat importeren

In de Wizard Certificaat importeren:

1. Selecteer de winkellocatie.

    * **Huidige gebruiker** selecteren als alleen de processen die worden uitgevoerd onder de huidige gebruiker heeft toegang tot de service
    * Selecteer **Lokale computer** als andere processen in deze computer toegang de service tot heeft
2. Klik op **volgende**.
3. Als u importeert uit een bestand, bevestig het pad naar het bestand.
4. Als het importeren van een. PFX-bestand:
    1.     Voer het wachtwoord bescherming van de persoonlijke sleutel
    2.     Selecteer opties voor importeren
5.     Selecteer "Plaats"-certificaten in het onderstaande archief opslaan
6.     Klik op **Bladeren**.
7.     Selecteer het gewenste archief.
8.     Klik op **Voltooien**.
       
    * Als het archief met vertrouwde basis-CA is geselecteerd, klikt u op **Ja**.
9.     Klik op **OK** in alle vensters van het dialoogvenster.

## <a name="upload-certificate"></a>Certificaat uploaden

In de [Azure Portal](https://portal.azure.com/)

1. **Cloud Services**selecteren.
2. Selecteer de service cloud.
3. Klik op **certificaten**in het bovenste menu.
4. Klik op de onderste balk op **uploaden**.
5. Selecteer het certificaatbestand.
6. Als het is een. PFX-bestand, typ het wachtwoord voor de persoonlijke sleutel.
7. Zodra voltooid, kopieert u de vingerafdruk van het certificaat van de nieuwe vermelding in de lijst.

## <a name="other-security-considerations"></a>Andere beveiligingsmaatregelen
 
De SSL-instellingen beschreven in dit document coderen van communicatie tussen de service en de clients wanneer het eindpunt HTTPS wordt gebruikt. Dit is belangrijk omdat de referenties voor toegang tot de database en mogelijk ook andere vertrouwelijke informatie worden opgenomen in de mededeling. Bedenk wel dat de service blijft bestaan interne status, met inbegrip van referenties, in de interne tabellen van de Microsoft Azure SQL-database die u hebt opgegeven voor de metagegevensopslag van uw abonnement op Microsoft Azure. Deze database is gedefinieerd als onderdeel van de volgende instelling in het bestand met de service (. CSCFG-bestand): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Referenties die zijn opgeslagen in deze database worden gecodeerd. Echter als beste ervoor zorgen dat web- en werknemer rollen van de distributie van service up-to-date blijven en veilig als ze toegang tot de metagegevensdatabase en het certificaat voor het coderen en decoderen van opgeslagen referenties hebben. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
