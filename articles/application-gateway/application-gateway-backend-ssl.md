<properties
   pageTitle="Inschakelen van SSL-beleid en complete SSL op Application Gateway | Microsoft Azure"
   description="Deze pagina biedt een overzicht van de toepassingsgateway complete SSL ondersteunen."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>SSL-beleid en complete SSL op Application Gateway inschakelen

## <a name="overview"></a>Overzicht

Toepassingsgateway ondersteunt SSL-beëindiging op de gateway, nadat die meestal verkeersstromen ongecodeerd naar de back-end servers. Hierdoor kunt webservers worden unburdened van de overhead van kostbare ontsleutelen. Voor sommige klanten niet-gecodeerde communicatie met de back-end servers is echter niet een aanvaardbare optie. Dit kan het gevolg zijn van beveiliging/conformiteitsvereisten of de toepassing accepteren alleen beveiligde verbinding. Toepassingsgateway ondersteunt nu complete SSL-codering voor dergelijke toepassingen.

Einde tot einde SSL kunt u veilig gevoelige gegevens verzenden naar de back-end gecodeerde nog steeds profiteert van de voordelen van de functies voor taakverdeling Layer 7 welke toepassingsgateway, zoals cookie-affiniteit op URL gebaseerde routering, ondersteuning biedt voor routering op basis van sites of de mogelijkheid om te ' injecteren ' X-doorgestuurde-* headers.

Geconfigureerd met complete SSL-communicatie-modus, toepassingsgateway gebruiker SSL-sessies op de gateway wordt beëindigd als gebruikersverkeer wordt gedecodeerd. Selecteer een instantie van de juiste back-end van toepassingen te routeren van netwerkverkeer naar de geconfigureerde regels vervolgens toepassing. Toepassingsgateway vervolgens een nieuwe SSL-verbinding met de back endserver initieert en opnieuw worden gecodeerd met behulp van de back-endserver van openbare-sleutelcertificaat voordat het verzoek om de back-end verzendt gegevens. Einde tot het einde van SSL is ingeschakeld door in het BackendHTTPSetting naar Https, die vervolgens wordt toegepast op een groep met back-end protocol te stellen. Elke back-endserver in de back-end-toepassingen met complete SSL is ingeschakeld, moet worden geconfigureerd met een certificaat voor de beveiligde communicatie.

![complete ssl-scenario][1]

In dit voorbeeld worden aanvragen via TLS1.2 doorgestuurd naar back-end-servers in de Pool1 van begin tot eind SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-end SSL en whitelisting van certificaten

Toepassingsgateway communiceert alleen met bekende backend exemplaren met whitelisted hun certificaat met de toepassingsgateway. Om whitelisting van certificaten inschakelen, moet u de openbare sleutel van de back-end servercertificaten met de application gateway (niet het basiscertificaat) uploaden. Alleen verbindingen met bekende en whitelisted backends zijn vervolgens toegestaan. De resterende backends resulteert in een fout van de gateway. Zelf-ondertekende certificaten zijn voor testdoeleinden alleen en niet aanbevolen voor de werkbelasting van de productie. Deze certificaten moeten whitelisted met de toepassingsgateway zoals beschreven in de voorgaande stappen voordat ze kunnen worden gebruikt.

## <a name="application-gateway-ssl-policy"></a>Toepassingenbeleid Gateway SSL.

Toepassingsgateway ondersteunt configureerbare SSL-onderhandeling gebruikersbeleid, waardoor klanten meer controle over SSL-verbindingen op de toepassingsgateway.

1. SSL 2.0 en 3.0 standaard uitgeschakeld voor alle Gateways van toepassing. Ze zijn niet geconfigureerd op alle.
2. SSL beleid definitie geeft u de optie uit te schakelen op een van de volgende protocollen voor 3 - TLSv1\_TLSv1 0\_1, TLSv1\_2.
3. Als er geen SSL-beleid is gedefinieerd drie (TLSv1\_TLSv1 0\_1, TLSv1_2) zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

Na het leren over einde aan einde SSL en SSL-beleid, Ga naar het [complete SSL op application gateway inschakelen](application-gateway-end-to-end-ssl-powershell.md) voor het maken van een toepassingsgateway met mogelijkheid om verkeer naar backends in gecodeerde vorm verzenden.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png