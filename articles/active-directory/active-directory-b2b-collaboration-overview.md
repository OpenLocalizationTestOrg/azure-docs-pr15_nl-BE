<properties
   pageTitle="Samenwerking met Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B samenwerking kan zakelijke partners toegang tot uw zakelijke toepassingen, met elk van de gebruikers die wordt vertegenwoordigd door een enkele Azure AD-account"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B-samenwerking

Azure Active Directory (AD Azure) B2B samenwerking kunt u toegang tot uw bedrijfstoepassingen uit partner-beheerde identiteiten inschakelen. Uitnodiging en autorisatie van gebruikers van partnerbedrijven toegang krijgen tot de bronnen kunt intern relaties maken. Complexiteit wordt verminderd, omdat elk bedrijf een keer federates met Azure Active Directory en elke gebruiker wordt vertegenwoordigd door een enkel Azure AD-account. Beveiliging wordt verhoogd als de zakelijke partners hun accounts in Azure AD beheren omdat access wordt ingetrokken wanneer gebruikers partner zijn ontslagen uit hun organisaties en onbedoelde toegang via het lidmaatschap van de interne mappen wordt voorkomen. B2B-samenwerking heeft voor zakelijke partners die nog geen AD Azure, een gestroomlijnde aanmeldingsprocedure te Azure AD accounts bieden uw zakelijke partners.

-   De zakelijke partners hun eigen referenties, die u maakt van het beheren van een externe partner directory en van de noodzaak om toegang te verwijderen wanneer gebruikers de partnerorganisatie laat gebruiken.

-   U beheren toegang tot uw apps onafhankelijk van de levenscyclus van uw business-partner account. Dit betekent bijvoorbeeld dat u access intrekken kunt zonder te vragen de IT-afdeling van uw zakelijke partner te doen.

## <a name="capabilities"></a>Mogelijkheden

B2B samenwerking vereenvoudigt het beheer en verbetert de beveiliging van de partner toegang tot bedrijfsnetwerken, met inbegrip van SaaS-toepassingen, zoals Office 365, televergaderingen, Azure Services en elke mobile, cloud en claimbewuste toepassing op ruimten. B2B samenwerking kunnen partners hun eigen accounts beheren en ondernemingen beveiligingsbeleid partner toegang kunnen toepassen.

Azure Active Directory B2B samenwerking eenvoudig is te configureren met vereenvoudigde aanmelding voor partners van elke omvang zelfs als ze niet beschikken over hun eigen Azure Active Directory via een e-mailadres gecontroleerd proces. Het is ook gemakkelijk te onderhouden met geen externe mappen of per partner federation-configuraties.

## <a name="b2b-collaboration-process"></a>B2B-samenwerkingsproces

1. Azure AD B2B samenwerking kan de bedrijfsbeheerder van een uitnodigen en een aantal externe gebruikers toestaan door het uploaden van een bestand met door komma's gescheiden waarden (CSV) van niet meer dan 2000 regels naar de B2B-portal voor samenwerking.

  ![Het dialoogvenster CSV-bestand uploaden](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. De portal wordt e-mail uitnodigingen verzenden naar deze externe gebruikers.

3. De uitgenodigde gebruiker zal ondertekenen met een bestaande account voor werk met Microsoft (beheerd in Azure AD) of een nieuwe account voor werk in Azure AD ophalen.

4. Eenmaal aangemeld, wordt de gebruiker omgeleid naar de app die was gedeeld met hen.

Uitnodigingen voor de consument e-mailadressen (bijvoorbeeld Gmail of [*comcast.net*](http://comcast.net/)) worden momenteel niet ondersteund.

Bekijk [deze video](http://aka.ms/aadshowb2b)voor meer informatie over de werking van B2B-samenwerking.

## <a name="next-steps"></a>Volgende stappen
Blader in onze andere artikelen over Azure AD B2B samenwerking.

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
