## <a name="prerequisites"></a>Vereisten

Voordat we CDN management code schrijven kunt, moeten we de voorbereiding om onze code voor de interactie met de Azure Resource Manager.  Hiervoor moet u naar:

* Een resourcegroep bevat het CDN-profiel we in deze zelfstudie maken maken
* Azure Active Directory voor de verificatie van de toepassing configureren
* Machtigingen toewijzen aan de resourcegroep, zodat alleen gemachtigde gebruikers van onze Azure AD huurder met onze CDN-profiel werken kunnen

### <a name="creating-the-resource-group"></a>De resourcegroep maken

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

2. Klik op de knop **Nieuw** in de linkerbovenhoek, en vervolgens **Management**en **Resourcegroep**.
    
    ![Een nieuwe resourcegroep maken](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. De resourcegroep *CdnConsoleTutorial*aanroepen.  Selecteer uw abonnement en kies een locatie bij u in de buurt.  Als u wilt, u het selectievakje **pincode aan dashboard** wilt vastmaken aan het dashboard in de portal.  Hierdoor wordt u later gemakkelijk te vinden.  Nadat u uw selecties hebt gemaakt, klikt u op **maken**.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Nadat de resourcegroep hebt gemaakt, niet hebt u vastmaken aan uw dashboard, kunt u deze vinden door te klikken op **Bladeren**en vervolgens **Resourcegroepen**.  Klik op de groep om deze te openen.  Maak een notitie van uw **Abonnement-ID**.  We zullen het later nodig hebt.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>De Azure AD-toepassing maken en machtigingen toe te passen

Er zijn twee mogelijkheden voor verificatie met Active Directory Azure app: individuele gebruikers of een service principal. Een service principal is vergelijkbaar met een service-account in Windows.  In plaats van een bepaalde gebruiker toekennen van machtigingen voor de interactie met de profielen CDN, wij in plaats daarvan de machtigingen toewijzen aan de service principal.  Service beveiligings-principals worden meestal gebruikt voor geautomatiseerde, niet-interactieve processen.  Hoewel deze zelfstudie een interactieve console app schrijven is, zullen we richten ons op de service principal aanpak.

Maken van een service principal bestaat uit verschillende stappen waaronder een Azure Active Directory-toepassing maken.  Hiervoor gaan we [in deze zelfstudie](../articles/resource-group-create-service-principal-portal.md)te volgen.

> [AZURE.IMPORTANT] Zorg ervoor dat alle stappen in de [gekoppelde zelfstudie](../articles/resource-group-create-service-principal-portal.md).  Het is *zeer belangrijk* dat u deze hebt voltooid precies zoals beschreven.  Onthoud de **ID van de huurder**, **huurder domeinnaam** (vaak een *. onmicrosoft.com* domein tenzij u een aangepast domein hebt opgegeven), **de client-ID**en **client verificatiesleutel**, omdat we deze hoger moeten.  Let zeer beschermt uw **client-ID** en uw **verificatiesleutel client**, als deze referenties door iedereen kunnen worden gebruikt voor het uitvoeren van bewerkingen als de service principal. 
>   
> Als u naar de stap die is [geconfigureerd met meerdere huurder toepassing](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application)met de naam, selecteert u **Nee**.
> 
> Wanneer u naar de stap [toepassing toewijst aan een rol](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), gebruiken de resourcegroep die we eerder hebt gemaakt, *CdnConsoleTutorial*, maar in plaats van de rol van **lezer** toewijst de rol van **Inzender voor CDN-profiel** .  Nadat u de toepassing de rol van **Inzender voor CDN profiel** voor de resourcegroep toewijzen, terug naar deze zelfstudie. 

Nadat u uw service principal hebt gemaakt en de rol van **Inzender voor CDN profiel** toegewezen, de blade **gebruikers** voor de resourcegroep moet uitzien.

![Gebruikers blade](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Verificatie van de interactieve gebruiker.

Als, in plaats van een service principal, u liever interactieve gebruikersverificatie, is het proces vergelijkbaar met die voor een service principal.  In feite, moet u dezelfde procedure volgen, maar een paar kleine wijzigingen aanbrengen.

> [AZURE.IMPORTANT] Deze stappen alleen volgende als u afzonderlijke gebruikers om verificatie te gebruiken in plaats van een service principal.

1. Bij het maken van uw toepassing, in plaats van de **Webtoepassing**, kiest u de **oorspronkelijke toepassing**. 
    
    ![Oorspronkelijke toepassing](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Op de volgende pagina wordt gevraagd voor een **redirect URI**.  De URI won't worden gevalideerd, maar vergeet niet wat u hebt ingevoerd.  U zult het later nodig hebt. 

3. Er is niet nodig om een **verificatiesleutel client**te maken.

4. In plaats van een service principal toe te wijzen aan de rol van **Inzender voor CDN-profiel** , gaan we naar afzonderlijke gebruikers of groepen toewijzen.  In dit voorbeeld kunt u zien dat ik *CDN Demo gebruiker* hebt toegewezen aan de rol van **Inzender voor CDN-profiel** .  
    
    ![Toegang van afzonderlijke gebruikers](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

