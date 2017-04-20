<properties 
    pageTitle="Service Bus verificatie en autorisatie | Microsoft Azure"
    description="Overzicht van de verificatie Shared Access handtekening (SAS)."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Bus service verificatie en autorisatie

Toepassingen kunnen worden geverifieerd naar Azure Service Bus met beide gedeelde toegang handtekening (SAS)-verificatie of via Azure Active Directory-toegangsbeheer (ook bekend als Access Control-Service of ACS). Gedeelde toegang handtekening verificatie kunnen toepassingen worden geverifieerd bij een Service Bus met een toegangstoets geconfigureerd op de naamruimte of de entiteit waaraan specifieke rechten zijn gekoppeld. Vervolgens kunt u deze sleutel voor het genereren van een token voor gedeelde toegang handtekening die clients gebruiken kunnen om Service Bus te verifiëren.

>AZURE. BELANGRIJKE SAS wordt aangeraden via ACS, aangezien deze een eenvoudige, flexibele en gemakkelijk te gebruiken verificatiemethode voor Service Bus biedt. Toepassingen kunnen SAS gebruiken in scenario's waarin ze niet hoeven te beheren de notie van een geautoriseerde "gebruiker".

## <a name="shared-access-signature-authentication"></a>Verificatie met een gedeelde Access-handtekening

[SAS-verificatie](service-bus-sas-overview.md) kunt u een gebruikerstoegang verlenen tot bronnen Service Bus met specifieke rechten. SAS-verificatie in de Bus-Service omvat de configuratie van een cryptografische sleutel met de bijbehorende rechten op de resource voor een Service Bus. Clients kunnen vervolgens toegang tot die bron krijgen door overlegging van een SAS-token die bestaat uit de bron-URI wordt geopend en een afloop ondertekend met de geconfigureerde sleutel.

U kunt sleutels voor SA's voor een Service Bus-naamruimte. De sleutel is van toepassing op alle messaging-entiteiten in die naamruimte. U kunt ook toetsen van Service Bus wachtrijen en onderwerpen. SAS wordt ook ondersteund door de Service Bus doorgeeft.

Voor het gebruik van SAS, kunt u een [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) -object in een naamruimte, wachtrij of onderwerp dat uit de volgende opties bestaat:

- *KeyName* die de regel aangeeft.

- *PrimaryKey* is een cryptografiesleutel gebruikt teken/SAS om tokens te valideren.

- *Secundaire sleutel* is een cryptografiesleutel gebruikt teken/SAS om tokens te valideren.

- *Rechten* die vertegenwoordigt de collectie van luisteren, verzenden of beheren van de verleende rechten.

Verificatieregels zijn geconfigureerd op het niveau van de naamruimte kunnen toegang verlenen tot alle entiteiten in een naamruimte voor clients met tokens die zijn ondertekend met de bijbehorende sleutel. Tot en met 12 van deze vergunning kunnen regels worden geconfigureerd op een naamruimte Service Bus, wachtrij of onderwerp. Een [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) met alle rechten is standaard geconfigureerd voor elke naamruimte als deze eerst is ingericht.

Als u een entiteit, moet de client een SAS-tokens gegenereerd met behulp van een specifieke [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). De SAS-token wordt gegenereerd op basis van de HMAC-SHA256 van resource string die bestaat uit de URI van de resource waaraan toegang wordt aangevraagd en een verloop met een cryptografiesleutel die is gekoppeld aan de verificatieregel.

Ondersteuning voor SAS-verificatie voor Service Bus is opgenomen in de SDK voor .NET Azure versies 2.0 en hoger. SAS biedt ondersteuning voor een [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Alle API's die een tekenreeks als parameter aanvaarden bieden ondersteuning voor SAS-verbindingsreeksen.

## <a name="acs-authentication"></a>ACS-verificatie

Verificatie via ACS service Bus wordt beheerd via een companion "-sb" ACS-naamruimte. Als u een naamruimte companion ACS moet worden gemaakt voor een Service Bus-naamruimte, kunt u uw Service Bus naamruimte met de klassieke Azure portal; geen maken moet u de naamruimte de PowerShell-cmdlet [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) gebruiken. Bijvoorbeeld:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Om te voorkomen dat een ACS-naamruimte te maken, geeft u de volgende opdracht:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Bijvoorbeeld, als u een Service Bus naamruimte **contoso.servicebus.windows.net**maakt, is een companion ACS-naamruimte **contoso sb.accesscontrol.windows.net** naam ingericht automatisch. Voor alle naamruimten die zijn gemaakt vóór augustus 2014 is ook een bijbehorende ACS-naamruimte gemaakt.

Een standaard service-identiteit "eigenaar", met alle rechten, is standaard in deze naamruimte companion ACS ingericht. Door het configureren van de juiste vertrouwensrelaties kunt u nauwkeurig bepalen naar een Service Bus entiteit via de ACS. U kunt extra service identiteiten voor het beheren van toegang tot de Service Bus entiteiten.

Als u een entiteit, vraagt de client een SWT-token uit ACS met de juiste claims door overlegging van de referenties. Het token SWT moet vervolgens worden verzonden als onderdeel van de aanvraag aan Service Bus waarmee de verificatie van de client voor toegang tot de dienst.

Ondersteuning voor ACS-verificatie voor Service Bus is opgenomen in de SDK voor .NET Azure versies 2.0 en hoger. Deze verificatie biedt ondersteuning voor een [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Alle API's die een tekenreeks als parameter aanvaarden bieden ondersteuning voor ACS-verbindingsreeksen.

## <a name="next-steps"></a>Volgende stappen

Gaan met [gedeelde toegang handtekening verificatie met Bus Service](service-bus-shared-access-signature-authentication.md) voor meer informatie over SA's.

Zie voor een overzicht van SA's in de Bus Service, [Gedeelde toegang handtekeningen](service-bus-sas-overview.md).

U vindt meer informatie over het ACS-tokens in [hoe: een Token aanvragen bij ACS via het Protocol OAuth WRAP](https://msdn.microsoft.com/library/hh674475.aspx).



