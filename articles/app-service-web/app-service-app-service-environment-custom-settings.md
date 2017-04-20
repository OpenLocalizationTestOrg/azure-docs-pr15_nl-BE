<properties
    pageTitle="Aangepaste instellingen voor App-Service-omgevingen"
    description="Aangepaste configuratie-instellingen voor App-Service-omgevingen"
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
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Aangepaste configuratie-instellingen voor App-Service-omgevingen

## <a name="overview"></a>Overzicht ##
Omdat de App serviceomgevingen zijn beperkt tot één klant, zijn er bepaalde configuratie-instellingen die uitsluitend op App Service-omgevingen kunnen worden toegepast. In dit artikel worden de verschillende specifieke aanpassingen die beschikbaar voor omgevingen met App-Service zijn.

Als er niet een App-omgeving, Zie [het maken van een App-omgeving](app-service-web-how-to-create-an-app-service-environment.md).

U kunt App-omgeving aanpassingen opslaan met behulp van een matrix in het nieuwe **clusterSettings** -kenmerk. Dit kenmerk wordt gevonden in de woordenlijst 'Eigenschappen' van de *hostingEnvironments* entiteit Azure Resource Manager.

De volgende afgekorte Resource Manager sjabloon fragment wordt het kenmerk **clusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Het kenmerk **clusterSettings** kan worden opgenomen in een sjabloon Resource Manager voor het bijwerken van de App-omgeving.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Azure Resource Explorer gebruiken voor het bijwerken van een App-omgeving
Ook kunt u de App-omgeving met [Azure Resource Explorer](https://resources.azure.com)bijwerken.  

1. Resource Explorer gaat u naar het knooppunt voor de App-omgeving (**abonnementen** > **resourceGroups** > **providers** > **Micrososft.Web** > **hostingEnvironments**). Klik vervolgens op de specifieke App-omgeving die u wilt bijwerken.

2. Klik in het rechterdeelvenster, **Lezen/schrijven** in de bovenste werkbalk zodat interactieve bewerken in de Resource Explorer.  

3. Klik op de blauwe **bewerken** knop de sjabloon Resource Manager om bewerkbaar te maken.

4. Ga naar de onderkant van het rechter deelvenster. Het kenmerk **clusterSettings** is helemaal onder, kunt u opgeven of de waarde bijwerken.

5. Typ (of kopieer en plak) de array van configuratiewaarden die in het kenmerk **clusterSettings** .  

6. Klik op de groene **plaatsen** die knop boven in het rechterdeelvenster op de wijziging doorvoeren in de App-omgeving heeft gevonden.

Hoewel u de wijziging verzendt, duurt het ongeveer 30 minuten, vermenigvuldigd met het aantal front-ends in App-omgeving om de wijziging van kracht te laten worden.
Bijvoorbeeld, als een App-omgeving vier front-ends heeft, duurt het ongeveer twee uur voor het bijwerken van de configuratie te voltooien. Tijdens het wijzigen van de configuratie uitgerold, kunnen geen andere schaal bewerkingen of bewerkingen van configuratie wijzigen plaatsvinden in de omgeving van App-Service.

## <a name="disable-tls-10"></a>TLS 1.0 uitschakelen ##
Een terugkerende vraag van klanten, met name de klanten die werkt met een PCI-naleving audits, wordt TLS 1.0 expliciet voor hun apps uitschakelen.

TLS 1.0 kan worden uitgeschakeld door de volgende vermelding in de **clusterSettings** :

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Wijzigingsopdracht TLS cipher suite ##
Een andere vraag van klanten is als ze de lijst van ciphers onderhandeld door de server wijzigen en dit kan worden bereikt kunnen door aanpassing van de **clusterSettings** zoals hieronder wordt weergegeven. De lijst met beschikbare coderings-suites uit [dit MSDN-artikel] kan worden opgehaald (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Als u onjuiste waarden worden ingesteld voor de codesuite dat SChannel niet verstaat, kan alle TLS-communicatie met de server functioneren niet meer. In dat geval moet u de vermelding *FrontEndSSLCipherSuiteOrder* verwijderen uit **clusterSettings** en de bijgewerkte sjabloon Resource Manager als u wilt terugkeren naar de standaardinstellingen voor cipher suite indienen.  Gebruik deze functionaliteit voorzichtig.

## <a name="get-started"></a>Aan de slag
De sjabloonsite Azure Quickstart Resource Manager bevat een sjabloon met de definitie van de basis voor het [maken van een App-omgeving](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
