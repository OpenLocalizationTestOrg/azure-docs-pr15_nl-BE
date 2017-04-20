<properties
    pageTitle="Real-Time waarschuwingen Azure CDN | Microsoft Azure"
    description="Real-time waarschuwingen in Microsoft Azure CDN. Real-time waarschuwingen kunnen meldingen over de prestaties van de eindpunten in uw profiel CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Real-time waarschuwingen in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Overzicht

In dit document worden real-time waarschuwingen in Microsoft Azure CDN. Deze functionaliteit biedt real-time meldingen over de prestaties van de eindpunten in uw profiel CDN.  U kunt per e-mail of op basis van HTTP-berichten instellen:

* Bandbreedte
* Statuscodes
* Cache status
* Verbindingen

## <a name="creating-a-real-time-alert"></a>Een real-time signaal maken

1. Ga naar uw profiel CDN in [Azure Portal](https://portal.azure.com).

    ![CDN profiel blade](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

3. Zweven boven de tab **Analytics** en vervolgens zweven over het doel van de **Real-Time statistieken** .  Klik op **Real-Time waarschuwingen**.

    ![CDN management portal](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    De lijst met bestaande waarschuwing configuraties (indien aanwezig) wordt weergegeven.

4. Klik op de knop **Waarschuwing toevoegen** .

    ![Knop Waarschuwing toevoegen](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Een formulier voor het maken van een nieuwe waarschuwing wordt weergegeven.

    ![Nieuwe Meldingsformulier](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Als u deze waarschuwing kan pas actief wanneer u op **Opslaan**klikt, schakel het selectievakje **Waarschuwing is ingeschakeld** in.

6. Voer een beschrijvende naam voor de waarschuwing in het veld **naam** .

7. Selecteer in de vervolgkeuzelijst **Type medium** **Groot Object HTTP**.

    ![Type medium met HTTP-grote geselecteerd Object](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Als het **Type Media**moet u **Http-groot Object** .  De andere opties worden niet gebruikt door **Azure CDN van Verizon**.  Fout in **Http-groot Object** te selecteren, zal de waarschuwing niet worden geactiveerd.

8. Maak een **expressie** om te controleren door een **Metric**, **Operator**en **Trigger-waarde**te selecteren.

    - Selecteer het gewenste gecontroleerde voorwaarde voor **Metric**.  **Mbps bandbreedte** is de hoeveelheid bandbreedte in megabits per seconde.  **Totaal aantal verbindingen** is het aantal gelijktijdige HTTP-verbindingen met onze servers van de rand.  Zie voor definities van de verschillende statussen van de cache en statuscodes [Azure CDN Cache-statuscodes](https://msdn.microsoft.com/library/mt759237.aspx) en [Azure CDN HTTP-statuscodes](https://msdn.microsoft.com/library/mt759238.aspx)
    - **De operator** is de rekenkundige operator waarmee de relatie tussen de metric en de trigger-waarde.
    - **Trigger-waarde** is de drempelwaarde moet worden voldaan voordat een melding wordt verzonden.

    In het onderstaande voorbeeld wordt de expressie die ik heb gemaakt geeft aan dat ik wil graag op de hoogte gesteld wanneer het nummer van de statuscodes 404 groter dan 25 is.

    ![Real-time waarschuwingen voorbeeldexpressie](./media/cdn-real-time-alerts/cdn-expression.png)

9. Voer voor **Interval**, hoe vaak u wilt dat de expressie die wordt geëvalueerd.

10. Selecteer in de vervolgkeuzelijst **Waarschuwen op** wanneer u worden gewaarschuwd wilt als de expressie waar is.
    
    - **Voorwaarde Start** geeft aan dat een melding wordt verzonden wanneer de opgegeven voorwaarde wordt ontdekt.
    - **Toestand einde** geeft aan dat een melding wordt verzonden wanneer de opgegeven voorwaarde wordt niet meer gedetecteerd. Deze melding kan alleen worden geactiveerd nadat ons netwerk monitoring systeem gevonden dat de opgegeven voorwaarde.
    - **Doorlopend** geeft aan dat een bericht wordt verzonden telkens wanneer dat het netwerk monitoring systeem de opgegeven voorwaarde detecteert. Houd er rekening mee dat het netwerk monitoring systeem slechts eenmaal per interval voor de opgegeven voorwaarde controleert.
    - Geeft aan dat een melding wordt verzonden wanneer de eerste keer dat de opgegeven voorwaarde wordt gedetecteerd en opnieuw wanneer de voorwaarde wordt niet meer gedetecteerd **voorwaarde Start en End** .

11. Als u meldingen ontvangen per e-mail wilt, schakel het selectievakje **Waarschuwen per E-mail** in.  

    ![E-mailformulier sturen](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    Voer in **het veld** het e-mailadres dat u waar u dat meldingen worden wilt verzonden. Standaard kunnen blijven voor het **onderwerp** en de **hoofdtekst**of kunt u aanpassen op het bericht met behulp van de lijst van **beschikbare trefwoorden** waarschuwingsgegevens dynamisch invoegen wanneer het bericht is verzonden.

    > [AZURE.NOTE] U kunt het e-mailbericht testen door te klikken op de knop **Test melding** , maar alleen nadat de configuratie waarschuwing is opgeslagen.

12. Als u wilt dat meldingen worden geboekt naar een webserver, schakel het selectievakje **Waarschuwen door HTTP Post** in.

    ![Formulier HTTP Post sturen](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Voer in het veld **Url** de URL waar u de HTTP-bericht wilt boeken. Voer in het tekstvak voor **kop** , de HTTP-headers worden verzonden in de aanvraag.  Voor **tekst** kunt u het bericht waarschuwingsgegevens dynamisch invoegen wanneer het bericht is verzonden met behulp van de lijst van **beschikbare trefwoorden** aanpassen.  **Koppen** en **hoofdtekst** standaard naar een XML-nettolading vergelijkbaar met het onderstaande voorbeeld.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] U kunt de melding HTTP Post testen door te klikken op de knop **Test melding** , maar alleen nadat de configuratie waarschuwing is opgeslagen.

13. Klik op de knop **Opslaan** om het opslaan van de configuratie van de waarschuwing.  Als u in stap 5 **Waarschuwing is ingeschakeld** hebt ingeschakeld, is uw melding nu actief.

## <a name="next-steps"></a>Volgende stappen

- [Real-time statistieken in Azure CDN](cdn-real-time-stats.md) analyseren
- De afdruk met [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
- Analyseren van [gebruikspatronen](cdn-analyze-usage-patterns.md)

