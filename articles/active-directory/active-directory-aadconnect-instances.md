<properties
    pageTitle="Azure AD verbinden: Synchronisatie-exemplaren | Microsoft Azure"
    description="Deze pagina worden speciale overwegingen voor Azure AD exemplaren."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Verbinden van Azure AD: Speciale overwegingen bij het instanties
Azure AD verbinding wordt meestal gebruikt met het wereldwijde exemplaar van AD Azure en Office 365. Maar er zijn ook andere varianten en deze hebben verschillende vereisten voor URL's en andere zaken.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud-Duitsland
De [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) is een soevereine cloud beheerd door een beheerder Duitse gegevens.

Deze wolk is momenteel in de voorvertoning. Veel van de scenario's die normaal gesproken u door uzelf, zoals doen kunt domeinen te controleren, door de exploitant moet doen. Neem contact op met uw plaatselijke Microsoft-vertegenwoordiger voor meer informatie over hoe u kunt deelnemen aan het voorbeeld.

URL's openen in de proxy-server |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Lijsten met ingetrokken certificaten |

Wanneer u bij uw directory Azure AD inlogt moet u een account in het domein onmicrosoft.de.

Functies die momenteel niet aanwezig is in het Microsoft Cloud-Duitsland:

- Gezondheid Azure AD verbinding is niet beschikbaar.
- Automatische updates is niet beschikbaar.
- Wachtwoord Write-back is niet beschikbaar.

## <a name="microsoft-azure-government-cloud"></a>Regering van Microsoft Azure cloud
De [regering van Microsoft Azure cloud](https://azure.microsoft.com/features/gov/) is een wolk voor de Amerikaanse overheid.

Deze wolk werd geschraagd door oudere versies van DirSync. Vanaf versie 1.1.180 van Azure AD verbinding maken met de volgende wordt generatie van de cloud ondersteund. Deze generatie gebruikt alleen US-gebaseerde eindpunten en hebben een andere lijst met URL's in uw proxy-server te openen.

URL's openen in de proxy-server |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Lijsten met ingetrokken certificaten |

Azure AD verbinden worden niet automatisch kunnen detecteren dat de Azure AD-map bevindt zich in de cloud regering. In plaats daarvan moet u de volgende acties uitvoeren tijdens de installatie van Azure AD verbinden.

1. Start de installatie Azure AD verbinden.
2. Als u ziet dat de eerste pagina waar u moet de gebruiksrechtovereenkomst accepteren, niet door maar laat u de installatiewizard uitvoeren.
3. Start regedit en wijzigt u de registersleutel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` met de waarde `2`.
4. Ga terug naar de installatiewizard Azure AD verbinding maken met de gebruiksrechtovereenkomst accepteren en doorgaan. Tijdens de installatie moet het installatiepad van de **aangepaste configuratie** gebruikt (en geen snelle installatie). Volg de installatie zoals gebruikelijk.

Functies die momenteel niet aanwezig is in de cloud met Microsoft Azure overheid:

- Gezondheid Azure AD verbinding is niet beschikbaar.
- Automatische updates is niet beschikbaar.
- Wachtwoord Write-back is niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
