<properties
   pageTitle="Connector versie Release-geschiedenis | Microsoft Azure"
   description="In dit onderwerp worden alle releases van de verbindingslijnen voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Connector versie Release-geschiedenis
De aansluitingen voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM) worden regelmatig bijgewerkt.

>[AZURE.NOTE]
Dit onderwerp is alleen van FIM en MIM. Deze verbindingslijnen worden niet ondersteund op Azure AD verbinden.

In dit onderwerp een overzicht van alle versies van de verbindingslijnen die zijn vrijgegeven.

Koppelingen naar verwante informatie:

- [Download de nieuwste verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=717495)
- Documentatie [Algemene LDAP-Connector](active-directory-aadconnectsync-connector-genericldap.md)
- Documentatie [Algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md)
- Documentatie [Web Services-Connector](http://go.microsoft.com/fwlink/?LinkID=226245)
- [PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) documentatie
- Documentatie van [Lotus Domino-Connector](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Uitgebracht: 2016 maart

**Nieuwe verbindingslijn**  
Eerste versie van de [Algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md).

**Nieuwe functies:**

- Algemene LDAP-Connector:
    - Toegevoegde ondersteuning voor het importeren van delta met Isode.
- Web Services-Connector:
    - De csEntryChangeResult en setImportErrorCode activiteit waarmee fouten worden geretourneerd aan de sync-engine weer object bijgewerkt.
    - De sjablonen voor SAP6 en SAP6User voor het gebruik van de nieuwe functionaliteit van object-fout bijgewerkt.
- Connector voor Lotus Domino:
    - Exporteren moet u een certifier per adresboek. U kunt nu voor alle certifiers hetzelfde wachtwoord gebruiken om het beheer te vereenvoudigen.

**Vaste problemen:**

- Algemene LDAP-Connector:
    - Voor IBM Tivoli DS, sommige verwijzing kenmerken zijn niet correct is gedetecteerd.
    - Voor Open LDAP tijdens het importeren van een delta-zijn whitespaces aan het begin en einde van tekenreeksen afgekapt.
    - Voor Novell en NetIQ hernoemd uitvoer die een object tussen organisatie-eenheden/containers en tegelijkertijd verplaatst het object is mislukt.
- Web Services-Connector:
    - Als de webservice had meerdere parameters voor dezelfde binding, is vervolgens de verbindingslijn niet goed ontdekken deze parameters.
- Connector voor Lotus Domino:
    - Uitvoer van het kenmerk volledige naam met een mail-in-database werkt niet.
    - Uitvoer die zowel toegevoegd als lid verwijderd uit een groep alleen de toegevoegde leden geëxporteerd.
    - Als u een Notes-Document is ongeldig (het kenmerk isValid ingesteld op false), wordt de verbindingslijn niet.

## <a name="older-releases"></a>Oudere versies
Vóór maart 2016, worden de verbindingslijnen zijn uitgebracht als onderwerpen over productondersteuning.

**Algemene LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, September 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, maart 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, januari 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, September 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, maart 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, September 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, September 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, September 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, maart 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, augustus 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, februari 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, oktober 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, augustus 2013

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
