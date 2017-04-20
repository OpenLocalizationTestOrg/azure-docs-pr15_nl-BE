<properties
    pageTitle="Azure Stack sleutel kluis Inleiding | Microsoft Azure"
    description="Meer informatie over het Azure Stack sleutel kluis beheer van sleutels en geheimen"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Inleiding tot de sleutel kluis in Azure stapel #

## <a name="before-you-start"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan:

- De lezer heeft toegang tot een abonnement met Azure sleutel kluis is ingeschakeld.
- De Azure PowerShell SDK is geconfigureerd en beschikbaar.
- Voor de TP2, kunnen alle huurder gerichte bewerkingen alleen worden uitgevoerd vanaf PowerShell.

## <a name="key-vault-basics"></a>Grondbeginselen van sleutel kluis

Sleutel kluis in Azure stapel helpt bij het beschermen van cryptografische sleutels en geheimen die cloud toepassingen en services gebruiken. U kunt sleutels en geheimen (zoals verificatiesleutels, opslag account sleutels sleutels voor gegevenscodering, .pfx-bestanden en wachtwoorden) coderen met behulp van de sleutel kluis.

Sleutel kluis kunt u beheer van sleutels die toegang en uw gegevens te coderen en vergemakkelijkt het beheer van sleutels. Ontwikkelaars kunnen toetsen voor het ontwikkelen en testen in minuten maken en vervolgens naadloos migreren naar productie-sleutels. Beveiligingsbeheerders kunnen verlenen (en intrekken) machtigingen voor de sleutels, indien nodig.

Iedereen met een stapel Azure abonnement kunt maken en gebruiken van sleutels kluizen. Hoewel de sleutel kluis nuttig voor ontwikkelaars en security administrators, kan deze worden geïmplementeerd en beheerd door een beheerder die de andere stapel Azure services voor een organisatie beheert. Deze beheerder kunt aanmelden met een abonnement Azure Stack maakt bijvoorbeeld een kluis voor de organisatie voor het opslaan van sleutels, en vervolgens verantwoordelijk voor deze operationele taken:

- Maak of importeer een sleutel of een geheim
- Intrekken of verwijderen van een sleutel of een geheim
- Toestaan dat gebruikers en toepassingen toegang krijgen tot de sleutel kluis, zodat u ze kunnen beheren of gebruik de toetsen en geheimen
- Sleutelgebruik configureren (bijvoorbeeld ondertekenen of coderen)

Deze beheerder kan vervolgens ontwikkelaars URI's aan te roepen vanuit hun toepassingen, en bieden een beveiligingsbeheerder met logboekgegevens voor sleutelgebruik.

Ontwikkelaars kunnen ook beheren de sleutels rechtstreeks met behulp van API's. Zie de sleutel kluis developer's guide voor meer informatie.

## <a name="scenarios"></a>Scenario 's

De volgende tabel ziet u de scenario's waarin sleutel kluis kan helpen bij het voldoen aan de behoeften van ontwikkelaars en security administrators:


### <a name="developer-for-an-azure-stack-application"></a>Ontwikkelaar voor een toepassing Azure Stack

**Probleem**: ik wil een toepassing schrijven voor Azure-Stack die sleutels voor het ondertekenen en coderen gebruikt, maar ik wil deze externe van mijn toepassing worden zodat de oplossing geschikt is voor een toepassing die is geografisch verspreid.

**Instructie**: sleutels worden opgeslagen in een kluis en aangeroepen door URI als dat nodig is.


### <a name="developer-for-software-as-a-service-saas"></a>Ontwikkelaar voor software als service (SaaS)

**Probleem:** Ik wil geen verantwoordelijkheid of aansprakelijkheid voor mogelijke voor mijn klanten huurder sleutels en geheimen.

**Overzicht:** Klanten kunnen hun eigen sleutels importeren in Azure Stack en ze beheren. Ik wil klanten hebben en hun sleutels beheren zodat ik zich concentreren kunnen op wat ik doen best, die de kern biedt software-functies uitvoeren.


### <a name="chief-security-officer-cso"></a>Chief Security Officer (BBF)

**Probleem:** Ik wil controleren of mijn organisatie in de controle van de levensduur van sleutels en sleutelgebruik kunt controleren.

**Instructie** Sleutel kluis is zodanig ontworpen dat Microsoft niet zien of uw sleutels ophalen.  Wanneer een toepassing moet de cryptografische bewerkingen uitvoeren met behulp van toetsen voor klanten, wordt sleutel kluis voor de toepassing. Toetsen voor de klanten niet wordt weergegeven in de toepassing.  Maar we verschillende Stack Azure services en resources gebruiken, wil ik de sleutels beheren vanaf één locatie in Azure stapel. De kluis biedt één interface, ongeacht hoeveel kluizen er in Azure Stack, welke gebieden zij ondersteuning en welke toepassingen gebruiken.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met sleutel kluis](azure-stack-kv-getting-started.md)
