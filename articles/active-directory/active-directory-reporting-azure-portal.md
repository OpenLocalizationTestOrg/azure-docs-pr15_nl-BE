<properties
   pageTitle="Azure Active Directory reporting - preview | Microsoft Azure"
   description="Overzicht van de verschillende beschikbare rapporten Azure Active Directory bekijken"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory reporting - voorbeeld

> [AZURE.SELECTOR]
- [Azure portal](active-directory-reporting-azure-portal.md)
- [Azure klassieke portal](active-directory-reporting-guide.md)

*Deze documentatie is onderdeel van de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Met rapporten in het afdrukvoorbeeld Azure Active Directory, krijgt u alle informatie die u nodig hebt om te bepalen hoe uw omgeving doet. [Wat is in het voorbeeld?](active-directory-preview-explainer.md)

Er zijn twee hoofdgebieden van melden:

- **Aanmelden activiteiten** – informatie over het gebruik van beheerde toepassingen en gebruiker aanmelden activiteiten

- **Controlelogboeken** - activiteit van systeeminformatie over gebruikers en groepsbeheer, uw beheerde toepassingen en directory-activiteiten

Afhankelijk van het bereik van de gegevens die u zoekt, u hebt toegang tot deze rapporten door te klikken op **gebruikers en groepen** of **zakelijke toepassingen** in de servicelijst in [Azure portal](https://portal.azure.com).

## <a name="sign-in-activities"></a>Aanmelden activiteiten

### <a name="user-sign-in-activities"></a>Gebruiker aanmelden activiteiten

Met de informatie die door de gebruiker-in lijst, vinden u antwoorden op vragen zoals:

- Wat is het patroon-in van een gebruiker?
- Hoeveel gebruikers zijn gebruikers ingelogd gedurende een week?
- Wat is de status van deze aanmeldingen?

Uw startpunt voor deze gegevens is de gebruiker-in de grafiek in het gedeelte **Overzicht** onder **gebruikers en groepen**.

 ![Rapportage] (./media/active-directory-reporting-azure-portal/05.png "Rapportage")

De gebruiker-in de grafiek ziet u wekelijks aggregaties teken modules voor alle gebruikers in een bepaalde periode. De standaardinstelling voor de periode is 30 dagen.

![Rapportage] (./media/active-directory-reporting-azure-portal/02.png "Rapportage")

Wanneer u klikt op een dag in de grafiek-in, krijgt u een gedetailleerd overzicht van de activiteiten-in.

![Rapportage] (./media/active-directory-reporting-azure-portal/03.png "Rapportage")

Elke rij in de lijst aanmelden activiteiten kunt u gedetailleerde informatie over de geselecteerde aanmelden, zoals:

- Wie is aangemeld?

- Wat was de verwante UPN?

- Welke toepassing is het doel van het aanmelden?

- Wat is het IP-adres van het aanmelden?

- Wat is de status van het aanmelden?

### <a name="usage-of-managed-applications"></a>Gebruik van beheerde toepassingen

Met een toepassingsgerichte weergave van uw gegevens-in kan u vragen beantwoorden, zoals:

- Wie is mijn toepassingen gebruikt?

- Wat zijn de top 3-toepassingen in uw organisatie?

- Ik heb hebt onlangs een toepassing uitgerold. Hoe doet het?


Uw startpunt voor deze gegevens is de top 3-toepassingen in uw organisatie binnen de laatste 30 dagen rapport in de sectie **Overzicht** onder **zakelijke toepassingen**.

 ![Rapportage] (./media/active-directory-reporting-azure-portal/06.png "Rapportage")


App gebruik grafiek wekelijkse aggregaties van log ins voor uw top 3 toepassingen in een bepaalde periode. De standaardinstelling voor de periode is 30 dagen.

![Rapportage] (./media/active-directory-reporting-azure-portal/78.png "Rapportage")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.

![Rapportage] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Rapportage")


Als u op een dag in de grafiek voor het gebruik van app klikt, krijgt u een gedetailleerd overzicht van de activiteiten-in.


![Rapportage] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Rapportage")



De optie **aanmeldingen** geeft een volledig overzicht van alle evenementen aanmelden voor uw toepassingen.

![Rapportage] (./media/active-directory-reporting-azure-portal/85.png "Rapportage")

Met behulp van de kolomkiezer, kunt u de gegevensvelden die u wilt weergeven.

![Rapportage] (./media/active-directory-reporting-azure-portal/column_chooser.png "Rapportage")



### <a name="filtering-sign-ins"></a>Aanmeldingen filteren

Aanmeldingen kunt u filteren op een tijdsinterval voor de weergegeven gegevens beperken.

![Rapportage] (./media/active-directory-reporting-azure-portal/927.png "Rapportage")


Er is een andere methode voor het filteren van de posten van de activiteiten-in om te zoeken naar specifieke vermeldingen.
De methode search kunt u het bereik van de aanmeldingen rond specifieke **gebruikers**, **groepen** of **toepassingen**.


![Rapportage] (./media/active-directory-reporting-azure-portal/84.png "Rapportage")

## <a name="audit-logs"></a>Controlelogboeken

De controle Logboeken in Azure Active Directory kunt u records van de systeemactiviteiten voldoen.

Er zijn drie hoofdcategorieën voor het controleren van verwante activiteiten in Azure portal:

- Gebruikers en groepen   

- Toepassingen

- Directory   


Zie de [lijst met controlegebeurtenissen rapport](active-directory-reporting-audit-events.md#list-of-audit-report-events)voor een volledige lijst van audit rapport activiteiten.


Uw toegangspoort tot alle gegevens van de controle is **controlelogboeken** in de sectie **activiteit** van **Azure Active Directory**.


![Controle] (./media/active-directory-reporting-azure-portal/61.png "Controle")


Een controlelogboek heeft een lijstweergave in waarin de actoren (die) de activiteiten (wat) en de doelen.


![Controle] (./media/active-directory-reporting-azure-portal/345.png "Controle")


Door te klikken op een item in de lijstweergave, kunt u meer informatie over het krijgen.

![Controle] (./media/active-directory-reporting-azure-portal/873.png "Controle")




### <a name="users-and-groups-audit-logs"></a>Gebruikers en groepen van controlelogboekbestanden


Met de gebruiker en groep gebaseerde controlerapporten, vindt u antwoorden op vragen zoals:

- Welke typen updates zijn toegepast van de gebruikers?

- Hoeveel gebruikers zijn gewijzigd?

- Hoeveel wachtwoorden zijn gewijzigd?

- Wat is een beheerder uitgevoerd in een map?

- Wat zijn de groepen die zijn toegevoegd?

- Zijn er groepen met wijzigingen in groepslidmaatschappen?

- De eigenaars van de groep zijn gewijzigd?

- Welke licenties zijn toegewezen aan een gebruiker of een groep?


Als u alleen controleren van de gegevens die zijn gerelateerd aan gebruikers en groepen bekijken wilt, vindt u een gefilterde weergave onder **controlelogboeken** in het gedeelte van de **activiteiten** van **gebruikers en groepen**.


![Controle] (./media/active-directory-reporting-azure-portal/93.png "Controle")


### <a name="application-audit-logs"></a>Controlelogboeken voor de toepassing

Audit rapporten met op basis van een toepassing, kunt u antwoorden op vragen zoals:

- Wat zijn de toepassingen die u hebt toegevoegd of bijgewerkt?

- Wat zijn de toepassingen die u hebt verwijderd?

- Is een service-principe voor een toepassing gewijzigd?

- De namen van de toepassingen zijn gewijzigd?

- Die toestemming hebt gegeven voor een toepassing?


Als u alleen gegevens bekijken controleren met betrekking tot toepassingen wilt, vindt u een gefilterde weergave onder **controlelogboeken** in de sectie **activiteit** van **zakelijke toepassingen**.


![Controle] (./media/active-directory-reporting-azure-portal/134.png "Controle")


### <a name="filtering-audit-logs"></a>Controlelogboeken filteren

U kunt een controlerapport filteren op een tijdsinterval voor de weergegeven gegevens beperken.

![Controle] (./media/active-directory-reporting-azure-portal/324.png "Controle")

Een andere methode voor het filteren van de posten van een controlelogboek moet worden gezocht naar specifieke vermeldingen.

![Controle] (./media/active-directory-reporting-azure-portal/237.png "Controle")

## <a name="next-steps"></a>Volgende stappen

Zie de [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).
