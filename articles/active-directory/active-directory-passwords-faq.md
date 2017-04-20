<properties
    pageTitle="Veelgestelde vragen over: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Veelgestelde vragen (FAQ) over wachtwoordbeheer in Azure AD, inclusief wachtwoord opnieuw instellen, registratie, rapporten en Write-back naar Active Directory voor het bedrijf."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Veelgestelde vragen over wachtwoordbeheer

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Hier volgen enkele veelgestelde vragen voor alle zaken die betrekking hebben op wachtwoordbeheer.

Als u merkt dat u met een vraag die u niet het antwoord op weet of met bepaalde problemen die u kampen zoekt, kunt u lezen op hieronder om te zien als we hebben besproken dat al.  Als we nog niet gedaan hebt, geen probleem! Altijd vragen u hebt waarop geen hier op de [Forums van Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) en wij geven je antwoord aan u, zodra we kunnen.

Deze Veelgestelde vraag is opgedeeld in de volgende secties:

- [**Vragen over het registreren van wachtwoord opnieuw instellen**](#password-reset-registration)
- [**Vragen over het opnieuw instellen van wachtwoorden**](#password-reset)
- [**Vragen over het wachtwoord Management rapporten**](#password-management-reports)
- [**Vragen over het wachtwoord Write-back**](#password-writeback)

## <a name="password-reset-registration"></a>Wachtwoordhersteldiskette-registratie
 - **V: kunnen mijn gebruikers hun eigen wachtwoord opnieuw instellen van gegevens registreren?**

 > **A:** Ja, als opnieuw instellen van wachtwoorden is ingeschakeld en het gebruiksrecht, ze gaat naar de portal Registratie van wachtwoord opnieuw instellen op http://aka.ms/ssprsetup registreren hun verificatie-informatie kan worden gebruikt bij het opnieuw instellen van wachtwoorden. Gebruikers kunnen ook registreren door te gaan naar het deelvenster toegang op http://myapps.microsoft.com, klikt u op het profieltabblad en het Register voor de optie wachtwoord opnieuw instellen te klikken. Meer informatie over het verkrijgen van uw gebruikers is geconfigureerd voor het wachtwoord opnieuw instellen door te lezen hoe u gebruikers die zijn geconfigureerd voor het opnieuw instellen van wachtwoorden.

 - **V: kan ik voor Mijn gebruikers wachtwoord opnieuw instellen van gegevens definiëren?**

 > **A:** Ja, u kunt dit doen met DirSync of PowerShell of via de portal [Azure Management Portal](https://manage.windowsazure.com) of Office-beheerder. Meer weten over deze functie op het weblogbericht verbeterde Privacy voor Azure AD MVR gesloten en wachtwoord opnieuw instellen telefoonnummers en door te lezen leren hoe gegevens worden gebruikt door het wachtwoord opnieuw instellen.

 - **V: kan ik synchroniseren met gegevens voor vragen over beveiliging van in gebouwen?**

 > **A:** Nee, dit is vandaag niet mogelijk, maar we overwegen.

 - **V: kunnen mijn gebruikers gegevens registreren op zodanige wijze dat andere gebruikers deze gegevens niet zien?**

 > **A:** Ja, wanneer gebruikers registreren met behulp van het wachtwoord opnieuw instellen-Portal registratie van gegevens het wordt opgeslagen in persoonlijke verificatie-velden die alleen zichtbaar door algemene beheerders en de gebruiker zelf. Meer weten over deze functie op het weblogbericht verbeterde Privacy voor Azure AD MVR gesloten en wachtwoord opnieuw instellen telefoonnummers en door te lezen leren hoe gegevens worden gebruikt door het wachtwoord opnieuw instellen.

 - **V: Mijn gebruikers hoeft te worden geregistreerd voordat u ze opnieuw instellen van wachtwoorden kunnen gebruiken?**

 > **A:** Nee, als u onvoldoende verificatie-informatie namens hun definiëren, gebruikers geen te registreren. Opnieuw instellen van wachtwoorden werken prima, zolang u hebt gegevens die zijn opgeslagen in de juiste velden in de map correct opgemaakt. Meer die informatie over door te lezen leren hoe gegevens worden gebruikt door het wachtwoord opnieuw instellen.

 - **V: kan ik synchroniseren of de verificatie-e-mailadres of een alternatieve verificatie telefoon velden, telefoon-verificatie instellen voor Mijn gebruikers?**

 > **A:** Op dit moment niet, maar we overwegen deze functionaliteit inschakelen.

 - **V: hoe weet de portal Registratie van welke opties mijn gebruikers weergeven**

 > **A:** Het wachtwoord opnieuw instellen-portal Registratie bevat alleen de opties die u hebt ingeschakeld voor uw gebruikers onder het wachtwoord opnieuw instellen voor gebruikersbeleid gedeelte van de tabblad configureren van de map. Dit betekent dat als u niet, zeggen, vragen over beveiliging inschakelt, vervolgens gebruikers niet zal kunnen registreren voor die optie.

 - **V: wanneer de gebruiker wordt geacht ingeschreven?**

 > **A:** Een gebruiker wordt beschouwd als geregistreerd wanneer hij of zij ten minste heeft N stukjes info verificatie is gedefinieerd, waarbij N staat voor het nummer van verificatie methoden is vereist dat u hebt ingesteld in de [Azure Management Portal](https://manage.windowsazure.com). Voor meer informatie, Zie aanpassen van wachtwoord opnieuw instellen voor gebruikersbeleid.


## <a name="password-reset"></a>Wachtwoord opnieuw instellen

 - **V: hoe lang moet ik wachten voor het ontvangen van een e-mail, SMS of telefoon gesprek van het wachtwoord opnieuw instellen?**

 > **A:** E-mail, SMS-berichten en telefoongesprekken moet binnenkomen onder 1 minuut met normale geval 5-20 seconden. Als u de melding in die periode niet ontvangt, Controleer uw map Ongewenste e-mail, die het nummer / e-mailadres waarmee contact wordt gemaakt, is degene die u verwacht en dat de verificatiegegevens in Active directory juist is opgemaakt. Adressen voor gebruik met het opnieuw instellen van wachtwoorden Zie voor meer informatie over het opmaken van telefoonnummers en e-leren hoe gegevens worden gebruikt door het wachtwoord opnieuw instellen.

 - **V: welke talen worden ondersteund door het wachtwoord opnieuw instellen?**

 > **A:** Het wachtwoord opnieuw instellen van UI, SMS-berichten en telefoongesprekken worden gelokaliseerd in de dezelfde 40 talen die worden ondersteund in Office 365. Dit zijn: Arabisch, Bulgaars, vereenvoudigd Chinees, traditioneel Chinees, Kroatisch, Tsjechisch, Deens, Nederlands, Engels, Estisch, Fins, Frans, Duits, Grieks, Hebreeuws, Hindi, Hongaars, Indonesisch, Italiaans, Japans, Kazachs, Koreaans, Lets, Litouws, Maleisisch (Maleisië), Noors (Bokmål), Pools, Portugees (Brazilië), Portugees (Portugal), Roemeens, Russisch, Servisch (Latijn), Slowaakse, Sloveens, Spaans, Zweeds, Thais, Turks, Oekraïens, en Vietnamees.

 - **V: welke delen van het wachtwoord opnieuw instellen ervaring krijgen merk organisatie-huisstijl in mijn map wanneer ik het tabblad configureren?**

 > **A:** De portal wachtwoord opnieuw instellen wordt het logo van uw organisatie weergeven en kunt u voor het configureren van het Contact de beheerder link om te verwijzen naar een aangepast e-mailadres of URL. E-mail die wordt verzonden door een reset van het wachtwoord van uw organisatie logo, kleuren (in dit geval rood) naam in de hoofdtekst van het e-mailbericht, worden opgenomen en van de naam van aangepast. Zie een voorbeeld waarbij alle onderstaande huisstijl elementen. Lees voor meer informatie, uiterlijk aanpassen wachtwoord opnieuw instellen.

  ![][001]

 - **V: hoe kan ik mijn gebruikers opleiden waar u om hun wachtwoorden opnieuw instellen?**

 > **A:** Kunt u uw gebruikers rechtstreeks verzenden naar https://passwordreset.microsoftonline.com of u kunt opgeven dat ze op de heeft geen toegang tot uw account link gevonden op School of werk-ID tekenen in het scherm. U kunt gerust publiceren deze koppelingen (of URL-omleidingen voor hen maken) op een plaats die gemakkelijk toegankelijk is voor uw gebruikers.

 - **V: kan ik deze pagina vanaf een mobiel apparaat gebruiken?**

 > **A:** Ja, deze pagina werkt op mobiele apparaten.

 - **V: ondersteund in de lokale active directory-accounts ontgrendelen wanneer gebruikers hun wachtwoorden opnieuw instellen?**

 > **A:** Ja, wanneer een gebruiker zijn of haar wachtwoord en wachtwoord Write-back wordt hersteld is geïmplementeerd met alle versies van Azure AD verbinden of versies van Azure AD Sync 1.0.0485.0222 of hoger, en vervolgens de account van de gebruiker automatisch ontgrendeld worden wordt wanneer die gebruiker zijn of haar wachtwoord opnieuw instelt.

 - **V: hoe kan ik de integratie van wachtwoord opnieuw instellen in mijn bureaublad aanmelden gebruikerservaring?**

 > **A:** Dit is niet mogelijk vandaag. Echter als u absoluut deze mogelijkheid moet en een Azure AD Premium klant, kunt u installeren Microsoft Identity Manager zonder extra kosten en het wachtwoord op ruimten oplossing gevonden daarin op te lossen deze eis implementeren.

 - **V: kan ik verschillende beveiliging vragen voor verschillende landinstellingen instellen?**

 > **A:** Nee, dit is vandaag niet mogelijk, maar we overwegen.

 - **V: hoeveel vragen kunnen wij configureren voor de verificatieoptie vragen over beveiliging**

 > **A:** U kunt maximaal 20 aangepaste beveiligingsinstellingen vragen in [Azure Management Portal](https://manage.windowsazure.com).

 - **V: hoe lang mogelijk vragen over beveiliging?**

 > **A:** Vragen over beveiliging mogelijk tussen 3 en 200 tekens lang zijn.

 - **V: hoe lang mogelijk antwoorden op vragen over beveiliging?**

 > **A:** Antwoorden kunnen 3 tot 40 tekens lang zijn.

 - **V: zijn dubbele antwoorden op vragen over beveiliging afgewezen?**

 > **A:** Ja, we weigeren dubbele antwoorden op vragen over beveiliging.

 - **V: mei een gebruiker registreren meer dan één van de beveiligingsvraag met dezelfde?**

 > **A:** Nee, zodra een gebruiker een bepaalde vraag wordt geregistreerd, hij of zij kan niet registreren voor die vraag een tweede keer.

 - **V: is het mogelijk dat de ondergrens van vragen over beveiliging voor registratie instellen en resetten?**

 > **A:** Ja, een limiet kan worden ingesteld voor registratie en een andere voor opnieuw instellen. vragen over 3-5 beveiliging nodig kunnen zijn voor registratie en 3-5 nodig kunnen zijn voor opnieuw instellen.

 - **V: als een gebruiker meer dan het maximum aantal vragen die nodig zijn om te herstellen is geregistreerd, hoe vragen over beveiliging geselecteerd tijdens opnieuw instellen?**

 > **A:** N beveiliging vragen zijn willekeurig geselecteerd uit het totale aantal vragen dat een gebruiker is geregistreerd, waarbij N staat voor het minimum aantal vragen die nodig is voor het opnieuw instellen van wachtwoorden. Bijvoorbeeld, als een gebruiker 5 vragen over beveiliging geregistreerd heeft, maar slechts 3 nodig zijn om te herstellen, wordt 3 van de 5 willekeurig gekozen en aan de gebruiker gepresenteerd op het moment van opnieuw instellen. De gebruiker ontvangt de antwoorden op de vragen onjuist, het selectieproces opnieuw treedt op als om te voorkomen dat de vraag hammering.

 - **V: heb u voorkomen dat gebruikers proberen vaak in een korte periode voor het opnieuw instellen?**

 > **A:** Ja, er zijn diverse beveiligingsvoorzieningen over het opnieuw instellen van wachtwoorden. Gebruikers kunnen alleen proberen 5 wachtwoord reset pogingen binnen een uur voordat het wordt vergrendeld voor 24 uur. Gebruikers kunnen alleen proberen te valideren van een nummer 5 keer binnen een uur voordat het wordt vergrendeld voor 24 uur. Gebruikers kunnen alleen een één verificatiemethode proberen 5 keer binnen een uur voordat het wordt vergrendeld voor 24 uur.

 - **V: hoe lang zijn voor e-mail en SMS, eenmalige code geldig?**

 > **A:** De levensduur van de sessie voor het opnieuw instellen van wachtwoorden is 105 minuten. Dit betekent dat vanaf het begin van het wachtwoord opnieuw instellen van bewerking, de gebruiker heeft 105 minuten zijn of haar wachtwoord opnieuw instellen. E-mail en SMS, eenmalige code zijn ongeldig na het verstrijken van deze periode.


## <a name="password-management-reports"></a>Wachtwoord Management rapporten

 - **V: hoe lang duurt het voordat gegevens weergeven op de wachtwoord management rapporten?**

 > **A:** Gegevens moet worden weergegeven op de wachtwoord management rapporten binnen 5-10 minuten. Deze bepaalde gevallen is het een uur duren kan om te worden weergegeven.

 - **V: hoe kan ik het wachtwoord management rapporten filteren?**

 > **A:** U kunt het wachtwoord management rapporten filteren door te klikken op het Vergrootglas kleine uiterst rechts van de kolomlabels naar de bovenzijde van het rapport (Zie screenshot). Als u filteren op rijkere wilt, kunt u het rapport naar excel en maakt u een draaitabel kunt downloaden.

  ![][002]

 - **V: Wat is het maximale aantal gebeurtenissen worden opgeslagen in de rapporten wachtwoord beheer?**

 > **A:** Tot 1.000 wachtwoord opnieuw instellen of wachtwoord opnieuw instellen registratie van gebeurtenissen opgeslagen in het wachtwoord management rapporten.  Wij werken om dit nummer om meer gebeurtenissen weer te geven.

 - **V: hoe ver terug het wachtwoordbeheer rapporten gaan doen?**

 > **A:** Het wachtwoord management rapporten met bewerkingen die in de laatste 30 dagen. Wij onderzoeken momenteel hoe u dit een langere periode. Nu als u nodig hebt voor het archiveren van gegevens, kunt u regelmatig de rapporten te downloaden en deze op een andere locatie opslaan.

 - **V: is er een maximum aantal rijen dat kan worden weergegeven in de rapporten wachtwoord beheer?**

 > **A:** Ja, een maximum van 1000 rijen kan worden weergegeven op een van de rapporten wachtwoordbeheer of ze worden weergegeven in de gebruikersinterface of worden gedownload. Wij onderzoeken momenteel hoe deze limiet verhogen.

 - **V: is er een API voor het opnieuw instellen van wachtwoorden of registratie gegevens melden?**

 > **A:** Ja, raadpleegt u de volgende documentatie voor meer informatie over hoe u toegang hebben tot het opnieuw instellen van wachtwoorden gegevensstroom melden.  [Wachtwoord toegang tot gebeurtenissen rapportage via programmacode opnieuw leren](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Wachtwoord Write-back
 - **V: hoe werkt Writeback wachtwoord achter de schermen?**

 > **A:** Zie [Hoe wachtwoord terugschrijven](active-directory-passwords-learn-more.md#how-password-writeback-works) voor een gedetailleerde uitleg van wat er gebeurt wanneer u wachtwoord Write-back, en hoe gegevens worden uitgewisseld via het systeem weer in uw omgeving op gebouwen. Zie [beveiligingsmodel wachtwoord Write-back](active-directory-passwords-learn-more.md#password-writeback-security-model) in hoe wachtwoord Writeback werkt als u wilt weten hoe we wachtwoord Write-back is een zeer veilige service garanderen.

 - **V: hoe lang duurt Writeback wachtwoord voordat werken?  Is er een vertraging van de synchronisatie als met wachtwoord hash-synchronisatie?**

 > **A:** Wachtwoord Write-back is direct. Het is een synchrone pijpleiding die fundamenteel anders dan de Wachtwoordsynchronisatie-hash werkt. Writeback wachtwoord kan gebruikers krijgen real-time feedback over het succes van hun wachtwoord opnieuw instellen of wijzigen van de bewerking. De gemiddelde tijd voor een geslaagde write-back van een wachtwoord is onder 500 ms.

 - **V: welke typen accounts werkt Writeback wachtwoord voor?**

 > **A:** Wachtwoord Writeback werkt voor federatieve en hash-Password-Sync'd gebruikers.

 - **V: is Writeback wachtwoord van mijn domein wachtwoordbeleid afdwingen?**

 > **A:** Ja, terugschrijven wachtwoord afgedwongen wachtwoordduur, geschiedenis, complexiteit, filters en eventuele andere beperkingen die u in uw lokale domein in plaats van wachtwoorden kan plaatsen.

 - **V: Writeback wachtwoord beveiligd is?  Hoe kan ik controleren of dat ik won't get hacked zijn?**

 > **A:** Ja, het wachtwoord Write-back is zeer veilig. Meer informatie over de 4 lagen van beveiliging door het wachtwoord Write-back-service geïmplementeerd, bekijk het [beveiligingsmodel wachtwoord Write-back](active-directory-passwords-learn-more.md#password-writeback-security-model) in works hoe wachtwoord Write-back.




## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD-wachtwoord opnieuw instellen:

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe het werkt**](active-directory-passwords-how-it-works.md) - informatie over de zes verschillende onderdelen van de service en wat elk wordt
* [**Aan de slag**](active-directory-passwords-getting-started.md) - leren hoe u toestaan dat gebruikers hun wachtwoord cloud of op locatie wijzigen en instellen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van de look en feel en gedrag van de service aan de behoeften van uw organisatie
* [**Best practices**](active-directory-passwords-best-practices.md) - informatie over het snel en effectief beheren van wachtwoorden in uw organisatie
* [**Krijg inzicht**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden.
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - informatie over het oplossen van problemen met de service snel
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep in de technische details van de werking van de service


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
