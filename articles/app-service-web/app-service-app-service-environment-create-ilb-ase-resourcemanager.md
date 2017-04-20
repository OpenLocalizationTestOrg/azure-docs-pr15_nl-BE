<properties 
    pageTitle="Het maken van een ILB ASE met Azure Resource Manager sjablonen | Microsoft Azure" 
    description="Informatie over het maken van een interne load balancer ASE Azure Resource Manager-sjablonen gebruiken." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Het maken van een ILB ASE Azure Resource Manager-sjablonen gebruiken

## <a name="overview"></a>Overzicht ##
App-Service-omgevingen kunnen worden gemaakt met een virtueel netwerk intern adres in plaats van een openbare VIP.  Deze interne adres wordt geleverd door een Azure-onderdeel in de interne taakverdeling (ILB) genoemd.  Een ASE ILB kunnen worden gemaakt met behulp van de portal Azure.  Ook kunnen worden gemaakt met behulp van automatisering in Azure Resource Manager-sjablonen.  Dit artikel helpt bij het werk en de syntaxis die nodig zijn voor het maken van een ASE ILB met Azure Resource Manager.

Er zijn drie stappen die betrokken zijn bij het maken van een ASE ILB automatiseren:
1. Eerst de basis ASE in een virtueel netwerk met een interne load balancer-adres in plaats van een VIP openbaar gemaakt.  Een hoofddomein is als onderdeel van deze stap is toegewezen aan het ILB ASE.
2. Zodra het ILB ASE is gemaakt, wordt een SSL-certificaat is geüpload.  
3. Het geüploade SSL-certificaat is expliciet toegewezen aan het ILB ASE als de 'standaard' SSL-certificaat.  Deze SSL-certificaat wordt gebruikt voor SSL-verkeer apps op de ASE ILB wanneer de apps worden behandeld met het gemeenschappelijk hoofddomein toegewezen aan de ASE (bv. https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>De Base ILB ASE maken ##
Een voorbeeldsjabloon Azure Resource Manager en de bijbehorende parameters-bestand beschikbaar zijn op GitHub [hier][quickstartilbasecreate].

De meeste parameters in het bestand *azuredeploy.parameters.json* gelden voor zowel ILB ASEs, als gebonden aan een openbare VIP ASEs maken.  De onderstaande oproepen van speciale Opmerking uitvoerparameters of die zijn uniek, bij het maken van een ASE ILB:


- *interalLoadBalancingMode*: set met deze 3, wat betekent HTTP/HTTPS-verkeer op poort 80/443 en de besturingsgegevens channel-poorten naar geluisterd door de FTP-service op de ASE, In de meeste gevallen wordt gebonden aan een virtueel netwerk intern adres toegewezen ILB.  Als deze eigenschap is in plaats daarvan ingesteld op 2, gerelateerde alleen de FTP-service poorten (controle- en gegevens-kanalen) afhankelijk zijn van een adres ILB terwijl het HTTP/HTTPS-verkeer op de openbare VIP blijft.
-  *dnsSuffix*: met deze parameter bepaalt het standaard-hoofddomein wordt toegewezen aan de ASE.  In de openbare variant van Azure App Service is het hoofddomein standaard voor alle web-apps *azurewebsites.net*.  Maar aangezien een ASE ILB interne virtuele netwerk van de klant, niet dat u gebruik van de openbare dienst standaard hoofddomein.  In plaats daarvan moet een ASE ILB een hoofddomein standaard geschikt voor gebruik binnen de interne virtuele netwerk van het bedrijf.  Bijvoorbeeld, een hypothetische Contoso Corporation een hoofddomein standaard *interne* contoso.com gebruiken voor toepassingen die bedoeld zijn voor alleen worden omgezet en in een virtueel netwerk van Contoso toegankelijk. 
-  *ipSslAddressCount*: met deze parameter wordt automatisch standaard ingesteld op de waarde 0 in het bestand *azuredeploy.json* omdat ILB ASEs slechts één adres ILB hebben.  Er zijn geen expliciete IP-SSL-adressen voor een ASE ILB en vandaar de adresgroep IP-SSL voor een ASE ILB moet worden ingesteld op nul, anders een provisioning fout zal optreden. 

Zodra het bestand *azuredeploy.parameters.json* is ingevuld voor een ASE ILB kunnen het ILB ASE vervolgens worden gemaakt met het volgende codefragment van Powershell.  Het bestand paden overeen waar de sjabloonbestanden Azure Resource Manager zich bevinden op de computer te wijzigen.  Ook moet u uw eigen waarden voor de Azure Resource Manager deployment en resource groepsnaam opgeeft.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Na de bronnenbeheerder Azure sjabloon is ingediend dat het duurt een paar uur voor de ASE ILB moet worden gemaakt.  Nadat het maken is voltooid, wordt de ASE ILB weergegeven in de portal UX in de lijst met App serviceomgevingen voor het abonnement waarvoor de implementatie.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Uploaden en configureren van de SSL-certificaat 'Default' ##

Zodra het ILB ASE is gemaakt, moet een SSL-certificaat worden gekoppeld aan de ASE als 'standaard' SSL-certificaat voor SSL-verbindingen om apps te gebruiken.  Doorgaan met het hypothetische voorbeeld Contoso Corporation, als dat van de ASE standaard DNS-achtervoegsel is *interne contoso.com*en vervolgens een verbinding met *https://some-random-app.internal-contoso.com* vereist een SSL-certificaat dat geldig is voor **.internal contoso.com*. 

Er zijn tal van manieren een geldig SSL-certificaat met inbegrip van interne certificeringsinstanties, aanschaf van een certificaat van een externe uitgever en met behulp van een zelfondertekend certificaat te verkrijgen.  Ongeacht de bron van het SSL-certificaat moeten de volgende kenmerken van certificaat correct is geconfigureerd:

- *Onderwerp*: dit kenmerk moet worden ingesteld op **.your root-domein here.com*
- *Alternatieve naam voor onderwerp*: dit kenmerk moet bevatten beide * *.your root-domein here.com*, en * *.scm.your-root-domein-here.com*.  De reden voor het tweede item is dat de SCM/Kudu-site die is gekoppeld aan elke app SSL-verbindingen worden uitgevoerd met behulp van het adres van het formulier *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in de hand, zijn twee extra voorbereidende stappen nodig.  Het SSL-certificaat moet worden geconverteerd en opgeslagen als een .pfx-bestand.  Vergeet niet dat het .pfx-bestand moet omvatten alle tussenliggende en hoofd-certificaten, en ook moet worden beveiligd met een wachtwoord.

De resulterende pfx-bestand dat moet worden geconverteerd naar een base64-tekenreeks omdat het SSL-certificaat uploaden met een sjabloon van Azure Resource Manager.  Aangezien de bronnenbeheerder Azure sjablonen zijn tekstbestanden, moet het .pfx-bestand worden geconverteerd naar een base64-tekenreeks, zodat deze opgenomen als een parameter van de sjabloon worden kan.

Het codefragment Powershell toont een voorbeeld van een zelfondertekend certificaat wordt gegenereerd, het certificaat exporteert als een .pfx-bestand het .pfx-bestand converteren naar een met base64 gecodeerde tekenreeks en vervolgens op te slaan de base64 gecodeerde tekenreeks naar een afzonderlijk bestand.  De Powershell-code voor het base64-codering is gebaseerd op de [Blog van Powershell Scripts][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Als het SSL-certificaat is is gegenereerd en geconverteerd naar een met base64 gecodeerde tekenreeks, de bronnenbeheerder Azure voorbeeldsjabloon voor het [configureren van het standaard-SSL-certificaat] op GitHub[ configuringDefaultSSLCertificate] kan worden gebruikt.

Hieronder staan de parameters in het bestand *azuredeploy.parameters.json* :

- *appServiceEnvironmentName*: de naam van het ILB ASE wordt geconfigureerd.
- *existingAseLocation*: tekenreeks met de Azure regio waar het ILB ASE is geïmplementeerd.  Bijvoorbeeld: 'South Central ons'.
- *pfxBlobString*: de based64 tekenreeksweergave van het pfx-bestand gecodeerd.  Met behulp van het bovenstaande codefragment u de tekenreeks 'exportedcert.pfx.b64' Kopieer en plak deze in als de waarde van het kenmerk *pfxBlobString* .
- *wachtwoord*: het wachtwoord voor de beveiliging van het pfx-bestand gebruikt.
- *certificateThumbprint*: vingerafdruk van het certificaat.  Als u deze waarde van Powershell ophalen (bv. *$certificate. Vingerafdruk* uit het vorige codefragment), kunt u de waarde-is.  Maar als u de waarde in het dialoogvenster certificaat voor Windows kopieert, moet u het verwijderen van overbodige spaties.  De *certificateThumbprint* moet er ongeveer zo uitzien: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: een beschrijvende tekenreeks-id van uw eigen keuze die identiteit van het certificaat wordt gebruikt.  De naam wordt gebruikt als onderdeel van de unieke id van Azure Resource Manager voor de *Microsoft.Web/certificates* -entiteit die het SSL-certificaat.  De naam **moet** eindigen met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE.  Dit achtervoegsel wordt door de portal gebruikt als een indicator dat het certificaat wordt gebruikt voor het beveiligen van een ASE ILB ingeschakeld.


Hieronder vindt u een verkorte voorbeeld van *azuredeploy.parameters.json* :


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Zodra het bestand *azuredeploy.parameters.json* is ingevuld, kan het standaard-SSL-certificaat worden geconfigureerd met behulp van het volgende codefragment van Powershell.  Het bestand paden overeen waar de sjabloonbestanden Azure Resource Manager zich bevinden op de computer te wijzigen.  Ook moet u uw eigen waarden voor de Azure Resource Manager deployment en resource groepsnaam opgeeft.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de sjabloon Azure Resource Manager is ingediend dat het duurt ongeveer 40 minuten minuten per ASE-front-de wijziging.  Bijvoorbeeld met een standaard formaat ASE met twee front-ends, zal de sjabloon duren ongeveer een uur en twintig minuten.  Terwijl de sjabloon wordt uitgevoerd kunnen de ASE worden niet aangepast.  

Nadat de sjabloon is voltooid, apps op het ILB ASE is toegankelijk via HTTPS en de verbindingen worden beveiligd met behulp van het standaard-SSL-certificaat.  Het standaard-SSL-certificaat gebruikt als apps op de ASE ILB zijn behandeld met een combinatie van de hostnaam standaard, plus de naam van de toepassing.  *Https://mycustomapp.internal-contoso.com* gebruikt bijvoorbeeld het standaard-SSL-certificaat voor **.internal contoso.com*.

Echter net als apps op de openbare meerdere huurder service uitgevoerd, kunnen ontwikkelaars ook aangepaste hostnamen voor afzonderlijke toepassingen configureren en configureert u unieke SNI SSL certificaatbindingen voor afzonderlijke toepassingen.  


## <a name="getting-started"></a>Aan de slag

Zie [Inleiding tot de App-omgeving](app-service-app-service-environment-intro.md) aan de slag met App Service-omgevingen

Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
