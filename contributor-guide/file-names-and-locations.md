<properties title="" pageTitle="Bestandsnamen en locaties voor Azure technische artikelen" description="De structuur voor artikelen en de naamgevingsregels die u volgen moet bij het maken van een nieuw artikel uitgelegd." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Bestandsnamen en locaties voor Azure technische artikelen

We gebruiken een afzonderlijke map (de **artikelen** ) voor alle artikelen in onze opslagplaats voor technische inhoud. Er is geen maphiërarchie - alle artikelen in de structuur van het flat wonen. Als u mappen met de artikelen in deze maakt, kunnen niet uw artikelen worden gepubliceerd.

In plaats van de bestandsstructuur van een als een beginsel organiseren, gebruiken we een strikte naamgevingsregel die onderwerpen duidelijk wordt aangegeven en dat bijdraagt tot het ontdekken van op het web.

Dit is wat u moet weten:

+ [Regels]
+ [Patroon]
+ [Voorbeelden van standaard]
+ [Marketplace-inhoud]
+ [Bestand naam goedkeuring]

##<a name="rules"></a>Regels

- Bestandsnamen mogen alleen kleine letters, cijfers en streepjes. 
- Geen spaties of leestekens. De afbreekstreepjes gebruiken voor het scheiden van woorden en getallen in de bestandsnaam.
- Niet meer dan 80 tekens - dit is een publicatie limiet
- Gebruik actie woorden die specifiek, zoals zijn ontwikkelen, kopen, bouwen, problemen oplossen. Geen woorden - ing.
- Geen woorden, kleine bevatten geen een, en, de, in of, enz.
- Alle bestanden moeten in een prijsverlaging en gebruiken de extensie .md.
- De spelling van de woorden voorkomen dat niet-goedgekeurde of onnodige acroniemen in bestandsnamen

Acroniemen en initialisms in namen - specifieke richtlijnen:

- Gebruik geen afkortingen van namen van Azure service - de eerste woorden van het bestand moet de standaard Azure service of technologie naam voluit. 
-   Niet toestaan rm of arm als acroniemen in de bestandsnaam van een
- Andere standaard-afkortingen zijn aanvaardbaar indien nodig in bestandsnamen. 

##<a name="pattern"></a>Patroon

Dit is het algemene patroon:

 **Service-platform-Language-Content-product-Version.MD**

Gebruik de delen van het patroon en bekijk de lijst met artikelen in de bibliotheek om te leren van bestaande namen. Namen die niet met een dev-platform beginnen of de naam van een service zijn waarschijnlijk verdacht en uitgestelde via.

##<a name="standard-examples"></a>Voorbeelden van standaard

Hier volgen enkele voorbeelden van geldige namen die het patroon volgen. :

- cloud-services-dotnet-continu-delivery.md
- Mobile-services-ios-get-started.md
- documentdb-beheren account.md
- Mobile-services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-eclipse.MD
- Virtual-machines-Install-Windows-Server-2008r2.MD
- cache-aspnet-session-status-provider
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-site-Recovery

##<a name="marketplace-content"></a>Marketplace-inhoud

Start om inhoud die is gericht op de partner bijdragen aan de Azure markt onderscheiden, bestandsnamen met 'marktplaats'. Deze inhoud mag niet worden te vaak, zoals de meeste partner inhoud op de websites partners moet worden gemaakt.

- Marketplace-mongodb-Virtual-machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Bestand naam goedkeuring

Het is de taak van onze groep pull-aanvraag revisoren bestandsnamen bekijken wanneer een nieuw bestand aan de opslagplaats voor het eerst wordt ingediend. Pull-aanvraag revisoren Controleer de bestandsnaam en feedback geven via de pull-aanvraag Opmerking stream als er wijzigingen nodig zijn. De bestandsnaam moet worden gecorrigeerd voordat de pull-aanvraag wordt geaccepteerd. Medewerkers kunnen eenvoudig push-de update op de aanvraag die in behandeling zijn pull.

##<a name="folder-names-in-the-repo"></a>De mapnamen in de repo

Mappen moeten alleen worden gemaakt voor services en de naam moet overeenkomen met de service witruimte rond pagina's. Gebruik alleen letters en streepjes, en kleine letters. Goedkeuring krijgen van de beheerder van de opslagplaats, voordat u een nieuwe map maken die is niet bedoeld voor een officiële dienst.

##<a name="changing-case-in-file-names"></a>Hoofdlettergebruik in bestandsnamen wijzigen

Windows-besturingssystemen zijn niet hoofdlettergevoelig, dus als u wijzigen van een naam op te lossen, behuizing wilt, is het beter een inhoudelijke wijziging aan te brengen tenzij kunt u de wijziging doorvoert op een Linux of Mac. Bijvoorbeeld:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Gebruik de volgende opdracht om een bestand te wijzigen:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Medewerkers handleiding voor koppelingen

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)


<!--Anchors-->
[Regels]: #rules
[Patroon]: #pattern
[Voorbeelden van standaard]: #standard-examples
[Marketplace-inhoud]: #marketplace-content
[Bestand naam goedkeuring]: #file-name-approval
