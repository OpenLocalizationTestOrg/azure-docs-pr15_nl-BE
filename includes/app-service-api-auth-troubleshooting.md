De meeste van de tijd verificatiefouten gevolg zijn van onjuist of inconsistent configuratie-instellingen. Hier volgen enkele specifieke suggesties voor controle.

* Zorg ervoor dat u de knop **Opslaan** ergens niet mist. Dit is vaak gemakkelijk om te doen en het resultaat is dat u u naar de juiste waarden op een portal-pagina kijkt zult, maar ze eigenlijk nog niet zijn opgeslagen in de Azure-omgeving of Azure AD-toepassing.
* Voor instellingen die zijn geconfigureerd in de **Instellingen van de toepassing** blade van de Azure portal, zorg ervoor dat de juiste API app of web app is geselecteerd wanneer de instellingen zijn ingevoerd.  Ook voor zorgen dat de instellingen als de **App instellingen** en niet **-verbindingsreeksen**zijn ingevoerd als de indeling van de twee gedeelten vergelijkbaar is.
* Download het manifest opnieuw om te controleren voor een JavaScript-front-end-verificatie, `oauth2AllowImplicitFlow` is gewijzigd naar `true`.
* Controleer of u HTTPS gebruikt waar u URL's geconfigureerd:

    * In projectcode
    * In CORS
    * In Azure omgevingsinstellingen App voor elke API app en web app
    * In Azure AD toepassingsinstellingen.
    
    Houd er rekening mee dat als u een API-app de URL vanaf de portal kopiëren, vaak is `http://` en u hebt handmatig te wijzigen naar `https://`.

* Zorg ervoor dat de codewijzigingen zijn geïmplementeerd. In een oplossing met meerdere projecten is het bijvoorbeeld mogelijk de code van een project wijzigen en kiest u een van de andere per ongeluk wanneer u van plan bent om de wijziging te implementeren.
* Zorg ervoor dat u naar een HTTPS-URL's in de browser geen HTTP-URL's gaat. Visual Studio maakt standaard profielen met HTTP-URL's te publiceren, en dat is wat in de browser wordt geopend nadat u een project hebt geïmplementeerd.
* Zorg dat CORS correct is geconfigureerd op de API-app die de JavaScript-code roept voor verificatie van een JavaScript-front-end. Probeer bij twijfel over de vraag of het probleem betrekking heeft op CORS ' * ' als de oorsprong toegestane URL. 
* JavaScript-front-end Developer Tools Console-tabblad van uw browser als u meer informatie over fouten en HTTP-aanvragen op het netwerk te onderzoeken. Foutberichten van de Console mogelijk misleidend. Als u een bericht met een CORS fout krijgt, is het echte probleem mogelijk verificatie. U kunt controleren of dit het geval is met de app en verificatie tijdelijk tijdelijk uitgeschakeld.
* Zorg dat u wel zo veel mogelijk informatie in foutberichten mogelijk door [customErrors-modus op Off](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview)is ingesteld voor een app .NET API.
* Een [sessie voor externe foutopsporing](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)starten voor een toepassing .NET API en onderzoeken van de waarden van de variabelen die worden doorgegeven aan de code die gebruikmaakt van ADAL aan te schaffen van een token aan toonder of programmacode waarin wordt gecontroleerd van de vorderingen op de verwachte service principal-ID. Houd er rekening mee dat uw code configuratiewaarden uit verschillende bronnen, ophalen kan dus het is mogelijk om te zoeken naar verrassingen op deze manier. Bijvoorbeeld, als u een typefout maken in `ida:ClientId` als `ida:ClientID` bij het configureren van omgevingsinstellingen Azure App-Service, de code haalt de `ida:ClientId` -waarde die wordt gezocht in het bestand Web.config, wordt de Azure App-instelling genegeerd. 
* Als dingen niet in een normaal venster van Internet Explorer werken, kan een bestaande aanmelden storen; InPrivate en probeer Chrome of Firefox.
