<properties
    pageTitle="Azure AD verbinden sync: wat declaratieve inrichten expressies | Microsoft Azure"
    description="Verklaart de declaratieve provisioning expressies."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD verbinden sync: wat declaratieve inrichten van expressies
Azure AD verbinden sync is gebaseerd op het declaratieve ingericht voor het eerst geïntroduceerd in 2010 Forefront Identiteitenbeheer. U kunt uw volledige identiteit integratie bedrijfslogica zonder te hoeven schrijven gecompileerde code implementeren.

Een essentieel onderdeel van het inrichten van declaratieve is de taal voor sjabloonexpressies gebruikt in het kenmerk stromen. De taal is een subset van Microsoft® Visual Basic® for Applications (VBA). Deze taal wordt gebruikt in Microsoft Office en ook door gebruikers met de ervaring van VBScript wordt herkend. De declaratieve inrichten taal voor sjabloonexpressies is alleen met behulp van functies en is niet een gestructureerde taal. Er zijn geen methoden of instructies. Functies worden in plaats daarvan naar express programmaverloop genest.

Voor meer informatie Zie [Welkom bij de Visual Basic for Applications-Naslaggids voor Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

De attributen worden sterk getypeerd. Een functie accepteert alleen kenmerken van het juiste type. Ook is hoofdlettergevoelig. Zowel de functienamen van de en kenmerk moet juiste hoofdlettergebruik of wordt een fout gegenereerd.

## <a name="language-definitions-and-identifiers"></a>Definities van taal en id 's

- Functies hebben een naam gevolgd door de argumenten tussen haakjes: de functienaam (1 argument, argument N).
- Kenmerken worden aangeduid met vierkante haken: [attributeName]
- Parameters worden aangeduid door procenttekens: % ParameterName %
- Tekenreeksconstanten staan tussen aanhalingstekens: bijvoorbeeld, 'Contoso' (Opmerking: rechte aanhalingstekens "" en niet gekrulde aanhalingstekens "")
- Numerieke waarden worden uitgedrukt zonder aanhalingstekens en decimaal naar verwachting. Hexadecimale waarden worden voorafgegaan door & H. Bijvoorbeeld 98052 & HFF
- Boole-waarden worden uitgedrukt met behulp van constanten: True, False.
- Ingebouwde constanten en letterlijke waarden worden uitgedrukt met alleen hun naam: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Functies
Declaratieve inrichten wordt met veel functies de mogelijkheid om te zetten van de waarden van het kenmerk. Deze functies kunnen worden genest, zodat het resultaat van een functie wordt doorgegeven aan een andere functie.

`Function1(Function2(Function3()))`

De volledige lijst met functies vindt u in de [verwijzing naar een functie](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameters
Een parameter is gedefinieerd door een Connector of door een beheerder met PowerShell. Gewoonlijk bevatten waarden die verschillen van systeem naar systeem parameters, bijvoorbeeld de naam van het domein van de gebruiker bevindt zich in. Deze parameters kunnen worden gebruikt in het kenmerk stromen.

De volgende parameters beschikbaar de Active Directory Connector voor binnenkomende regels voor synchronisatie:

| Naam van de parameter | Opmerking |
| --- | --- |
| Domain.Netbios | NetBIOS-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMSALES |
| Domain.FQDN | Indeling van de FQDN-naam van het domein dat momenteel wordt geïmporteerd, zoals sales.fabrikam.com |
| Domain.LDAP | LDAP-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld DC = sales, DC = fabrikam, DC = com |
| Forest.Netbios | NetBIOS-notatie van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMCORP |
| Forest.FQDN | FQDN-notatie van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld fabrikam.com |
| Forest.LDAP | LDAP-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld DC = fabrikam, DC = com |

Het systeem biedt de volgende parameter die wordt gebruikt voor de identificatie van de verbindingslijn die momenteel worden uitgevoerd:  
`Connector.ID`

Hier volgt een voorbeeld met de metaverse kenmerk domein met de NetBIOS-naam van het domein waarin de gebruiker zich bevindt:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operators
De volgende operatoren u kunt gebruiken:

- **Vergelijking**: <, < =, <>, =, > > =
- **Wiskunde**: +, -, \*, -
- **String**: & (samenvoegen)
- **Logische**: & & (en) || (of)
- **Evaluatievolgorde van**:)

Operatoren zijn links naar rechts geëvalueerd en dezelfde prioriteit voor evaluatie. Dat wil zeggen, de \* (multiplier) wordt niet geëvalueerd voordat - (aftrekken). 2\*(5 + 3) is niet hetzelfde als 2\*5 + 3. De haakjes () worden gebruikt om de evaluatievolgorde als van links naar rechts evaluatievolgorde niet juist.

## <a name="multi-valued-attributes"></a>Kenmerken met meerdere waarden
De functies kunnen worden uitgevoerd op kenmerken met één waarde zowel met meerdere waarden. Voor kenmerken met meerdere waarden, wordt de functie werkt via elke waarde en dezelfde functie is van toepassing op elke waarde.

Bijvoorbeeld:  
`Trim([proxyAddresses])`Een bekleding van elke waarde in het kenmerk proxyAddress doen.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Voor elke waarde met een @-sign, vervangen door het domein met @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Het SIP-adres zoeken en verwijderen uit de waarden.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in [Wat declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Zie hoe declaratieve inrichting gebruikte out-of-box inzicht te verwerven in [de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md)wordt.
- Zie praktische wijzigen met behulp van declaratieve ingericht in [het maken van een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)

**Naslagonderwerpen**

- [Azure AD verbinden sync: functies, naslag](active-directory-aadconnectsync-functions-reference.md)
