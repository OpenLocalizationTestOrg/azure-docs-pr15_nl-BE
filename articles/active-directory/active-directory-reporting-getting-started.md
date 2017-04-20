<properties
   pageTitle="Azure Active Directory Reporting: Aan de slag | Microsoft Azure"
   description="Overzicht van de verschillende beschikbare rapporten in Azure Active Directory reporting"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Aan de slag met Azure Active Directory Reporting

## <a name="what-it-is"></a>Wat is het

Azure Active Directory (AD Azure) beveiliging, activiteit en controlerapporten voor de map bevat. Hier volgt een lijst van de verslagen opgenomen:

### <a name="security-reports"></a>Beveiligingsrapporten

- Aanmeldingen van onbekende bronnen
- Na meerdere mislukte aanmeldingen
- Aanmeldingen vanaf meerdere locaties
- Aanmeldingen van IP-adressen met verdachte activiteiten
- Onregelmatige activiteit aanmelden
- Aanmeldingen van mogelijk besmette apparaten
- Gebruikers met een afwijkende activiteit aanmelden

### <a name="activity-reports"></a>Activiteitenoverzicht

- Gebruik van toepassingen: overzicht
- Gebruik van toepassingen: gedetailleerd
- Toepassing dashboard
- Rekening met het inrichten van fouten
- Afzonderlijke Gebruikersapparaten
- Individuele gebruiker activiteit
- Activiteitenverslag voor groepen
- Wachtwoord opnieuw instellen van registratie activiteitenverslag
- Wachtwoordhersteldiskette-activiteit

### <a name="audit-reports"></a>Auditverslagen

- Controleverslag Directory

> [AZURE.TIP] Voor meer documentatie over Azure AD-rapportage, uitchecken [en het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Hoe het werkt


### <a name="reporting-pipeline"></a>Pijpleiding melden

De rapportage pijplijn bestaat uit drie stappen. Telkens wanneer een gebruiker zich aanmeldt of een verificatie wordt uitgevoerd, gebeurt het volgende:

- Eerst de gebruiker is geverifieerd (geslaagd of mislukt) en het resultaat wordt opgeslagen in de databases van Azure Active Directory-service.
- Met regelmatige tussenpozen, alle recente teken modules worden verwerkt. Op dit moment onze beveiligings- en afwijkende activiteit algoritmen zoekt alle recente teken modules voor verdachte activiteiten.
- Na verwerking, zijn de rapporten geschreven in de cache opgeslagen en geactiveerd in de klassieke Azure portal.

### <a name="report-generation-times"></a>Rapport genereren van tijden

Vanwege het grote aantal verificaties en verwerkt door het AD Azure platform ins ondertekenen, zijn de meest recente aanmeldingen verwerkt, gemiddeld een uur oud. In zeldzame gevallen kan voor het verwerken van de meest recente aanmeldingen maximaal acht uur duren.

U vindt de meest recente verwerkte aanmelden door het onderzoeken van de help-tekst aan de bovenkant van elk rapport.

![Help-tekst aan de bovenkant van elk rapport](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Voor meer documentatie over Azure AD-rapportage, uitchecken [en het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Aan de slag


### <a name="sign-into-the-azure-classic-portal"></a>Inloggen op de klassieke Azure portal

Eerst moet u inloggen op de [Azure klassieke portal](https://manage.windowsazure.com) als een globale of een beheerder van de conformiteit. U moet ook een servicebeheerder Azure abonnement of CO-beheerder, of worden met behulp van de "toegang tot Azure AD" Azure abonnement.

### <a name="navigate-to-reports"></a>Ga naar rapporten

Om rapporten te bekijken, Ga naar het tabblad rapporten boven aan de map.

Als dit de eerste keer dat de rapporten te bekijken, moet u een dialoogvenster accepteren voordat u de rapporten kunt weergeven. Dit is om ervoor te zorgen voor beheerders in uw organisatie deze gegevens, die worden beschouwd als privé-gegevens in sommige landen acceptabel is.

![In het dialoogvenster](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Elk rapport verkennen

Ga in elk rapport voor een overzicht van de gegevens worden verzameld en de aanmeldingen verwerkt. U kunt een [lijst met alle rapporten die hier](active-directory-reporting-guide.md)vinden.

![Alle rapporten](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>De rapporten downloaden als CSV

Elk rapport kan worden gedownload als een CSV-bestand (comma separated value). U kunt deze bestanden in Excel, PowerBI of derden analyse programma's verder te analyseren van uw gegevens.

Ga naar het rapport downloaden van een rapport als een CSV en klik onderaan op 'Downloaden'.

![Knop downloaden](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Voor meer documentatie over Azure AD-rapportage, uitchecken [en het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Volgende stappen

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Waarschuwingen voor afwijkende teken op activiteit aanpassen

Ga naar het tabblad "Configureren" van de map.

Blader naar het gedeelte 'Meldingen'.

In- of uitschakelen van de sectie "E-mail meldingen van afwijkende aanmeldingen".

![De sectie meldingen](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integreren met de Azure advertentie melden API

Zie [aan de slag met de API voor rapportage](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Oefenen meerledige verificatie van gebruikers

Selecteer een gebruiker in een rapport.

Klik op 'MVR gesloten inschakelen' onder aan het scherm.

![Op de onderkant van het scherm de knop meerledige verificatie](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Voor meer documentatie over Azure AD-rapportage, uitchecken [en het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Meer informatie


### <a name="audit-events"></a>Gebeurtenissen controleren

Meer informatie over welke gebeurtenissen in de directory in [Azure Active Directory Reporting controlegebeurtenissen](active-directory-reporting-audit-events.md)worden gecontroleerd.

### <a name="api-integration"></a>API-integratie

Zie [aan de slag met de API voor rapportage](active-directory-reporting-api-getting-started.md) en de [API-documentatie](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Get in touch

E-mail [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) voor feedback, Help-informatie of vragen die u hebt.

> [AZURE.TIP] Voor meer documentatie over Azure AD-rapportage, uitchecken [en het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md).
