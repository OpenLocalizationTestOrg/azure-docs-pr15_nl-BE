<properties
  pageTitle="Azure IoT Suite en Azure Active Directory | Microsoft Azure"
  description="Hierin wordt beschreven hoe Azure IoT Suite Azure Active Directory gebruikt om machtigingen te beheren."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Machtigingen voor de site azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

Als je eerst je bij [azureiotsuite.com][lnk-azureiotsuite], de site bepaalt de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde Azure Active Directory (AAD) huurder en Azure abonnement.

1.  De site eerst ontdekt van Azure welke AAD huurders waartoe u behoort te vullen van de lijst van huurders weergegeven naast uw gebruikersnaam is aangemeld. Op dit moment is verkrijgen de site alleen gebruikerstokens voor één huurder op een tijdstip. Als gevolg hiervan wanneer u naar een andere huurder gebruik van de vervolgkeuzelijst in de rechterbovenhoek overschakelt, de site opnieuw meldt zich in om die huurder voor de tokens voor de huurder die.

2.  Vervolgens wordt ontdekt de site uit welke abonnementen u hebt gekoppeld aan de geselecteerde huurder Azure. Bij het maken van een nieuwe vooraf geconfigureerde oplossing ziet u de beschikbare abonnementen.

3.  Tot slot haalt de site alle bronnen in de abonnementen en resourcegroepen gelabeld als vooraf geconfigureerde oplossingen en vult de tegels op de introductiepagina.

De volgende secties worden de rollen die de toegang tot de vooraf geconfigureerde oplossingen.

## <a name="aad-roles"></a>AAD rollen

De rollen van AAD de mogelijkheid dient vooraf geconfigureerde oplossingen bepalen en beheren van gebruikers in een vooraf geconfigureerde oplossing.

U vindt meer informatie over de beheerdersrollen in AAD in [rollen van beheerder toewijzen in Azure AD][lnk-aad-admin], maar dit artikel gaat hoofdzakelijk over de **Globale beheerder** en de rollen die **Domeinlid gebruiker** zoals gebruikt door de vooraf geconfigureerde oplossingen.

**Globale beheerder:** Er zijn veel algemene beheerders per huurder AAD. Bij het maken van een huurder AAD bent u standaard de globale beheerder van deze huurder. De beheerder van de globale gegevens leveren voor een vooraf geconfigureerde oplossing en **een rol voor de toepassing in de huurder AAD** is toegewezen aan. Als een andere gebruiker in de dezelfde AAD huurder een toepassing maakt, is de standaardrol die globale beheerder wordt verleend echter **Impliciete READ ONLY**. Globale beheerders kunnen toewijzen van rollen voor toepassingen met behulp van de [portal voor Azure klassieke][lnk-classic-portal].

**Domeinlid gebruiker:** Er zijn veel gebruikers/leden van een domein per huurder AAD. Een domeingebruiker kan gegevens leveren voor een vooraf geconfigureerde oplossing via de [azureiotsuite.com] [ lnk-azureiotsuite] site. De standaardrol die zij worden verleend voor het inrichten van deze toepassing is de **beheerder**. Zij kunnen een toepassing maken met het script build.cmd in de [azure-iot-afstandsbediening-controle op] [ lnk-rm-github-repo] of [azure-iot-voorspellende-onderhoud] [ lnk-pm-github-repo] opslagplaats, maar de standaardrol zij zijn toegekend is **Impliciet alleen-lezen**, als ze niet zijn gemachtigd voor het toewijzen van rollen. Als een andere gebruiker in de huurder AAD een toepassing maakt, worden ze standaard voor de toepassing de **Impliciete READONLY** rol toegewezen. Ze hebben niet de mogelijkheid voor het toewijzen van rollen voor toepassingen. Daarom toevoegen niet gebruikers of rollen voor een toepassing ook als ze het ingericht.

**Gebruiker Gast/Gast:** Er zijn veel Gast gebruikers/gasten per huurder AAD. Gastgebruikers hebben een beperkte set rechten in de huurder AAD. Gastgebruikers kunnen niet als gevolg hiervan een vooraf geconfigureerde oplossing in de huurder AAD inrichten.

Zie de volgende bronnen voor meer informatie:

- [Maken of gebruikers in Azure advertentie bewerken][lnk-create-edit-users]
- [App rollen in AAD toewijzen][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen Azure abonnement

De Azure admin rollen bepalen de mogelijkheid een abonnement op Azure toewijzen aan een AD-huurder.

U vindt meer informatie over de rollen Azure CO-beheerder, beheerder en de Administrator-Account in het artikel [toevoegen of wijzigen van Azure CO-beheerder, beheerder en beheerder van de Account][lnk-admin-roles].

## <a name="application-roles"></a>Toepassingsrollen

De rollen van de toepassing toegang tot apparaten in uw vooraf geconfigureerde oplossing.

Er zijn twee gedefinieerd en een impliciete functie gedefinieerd in de toepassing die wordt gemaakt wanneer u een vooraf geconfigureerde oplossing inrichten.

-   **ADMINISTRATOR:** Volledig beheer voor het toevoegen, beheren en verwijderen van apparaten

-   **Alleen-lezen:** De mogelijkheid apparaten weergeven

-   **Alleen voor impliciete lezen:** Dit is hetzelfde als alleen-lezen, maar wordt verleend aan alle gebruikers van uw huurder AAD. Dit is gedaan voor het gemak tijdens de ontwikkeling. U kunt deze rol verwijderen door aanpassing van de [RolePermissions.cs] [ lnk-resource-cs] bronbestand.

### <a name="changing-application-roles-for-a-user"></a>De rollen van de toepassing voor een gebruiker wijzigen

De volgende procedure kunt u een gebruiker in Active Directory een beheerder van de vooraf geconfigureerde oplossing maken.

U moet een globale beheerder AAD rollen voor een gebruiker wijzigen:

1. Ga naar de [Azure klassieke portal][lnk-classic-portal].

2. Selecteer **Active Directory**.

3. Klik op de naam van de huurder AAD is (dit is de map die u hebt gekozen in de azureiotsuite.com bij het inrichten van uw oplossing).

4. Klik op **toepassingen**.

5. Klik op de naam van de toepassing die overeenkomt met de naam van de vooraf geconfigureerde oplossing. Als uw toepassing in de lijst niet wordt weergegeven, schakelen naar **toepassingen die mijn bedrijf eigenaar is van** de daling van de **weergeven** en klikt u op het vinkje.

7. Klik op **gebruikers**.

8. Selecteer de gebruiker die u wilt overschakelen van rollen.

9. Klik op **toewijzen** en selecteer de functie (bijvoorbeeld **Admin**) u wilt toewijzen aan de gebruiker, klikt u op het vinkje.

## <a name="faq"></a>FAQ

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ik ben een servicebeheerder en ik wil graag de toewijzing van de directory tussen mijn abonnement en een specifieke AAD huurder wijzigen. Hoe moet ik dit doen?

1. Ga naar de [Azure klassieke portal][lnk-classic-portal], klikt u op **Instellingen** in de lijst met services aan de linkerkant.

2. Het abonnement dat u wilt wijzigen van de directory-toewijzing te selecteren.

3. Klik op **map bewerken**.

4. Selecteer de **map** die u wilt gebruiken in de vervolgkeuzelijst. Klik op de pijl naar rechts.

5. De map Toewijzing bevestigen en betrokken collega beheerders. Houd er rekening mee dat als u van een andere map verplaatst, collega beheerders uit de oorspronkelijke map verwijderd.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ik ben gebruiker/lid van een domein op de huurder AAD en ik heb een vooraf geconfigureerde oplossing gemaakt. Hoe ik krijg een bepaalde rol voor mijn toepassing?

Een globale beheerder toegewezen aan als beheerder op de huurder AAD voor machtigingen die aan rollen toewijzen aan gebruikers zelf van een algemene vraag of vraag een globale beheerder u een rol toekennen. Als u de vooraf geconfigureerde oplossing voor de pachter AAD wijzigen is als u wilt, Zie de volgende vraag geïmplementeerd.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hoe schakel ik de AAD pachter die vooraf geconfigureerde oplossing voor externe controle en de toepassing worden toegewezen aan

U kunt het uitvoeren van de implementatie van een wolk van <https://github.com/Azure/azure-iot-remote-monitoring> en opnieuw met een nieuwe huurder van AAD. Aangezien u standaard een globale beheerder als u een nieuwe huurder van AAD maakt, hebt u toegang voor het toevoegen van gebruikers en rollen toewijzen aan gebruikers.

1. Een nieuwe AAD-map maken in de [portal Azure Management][lnk-classic-portal].

2. Ga naar <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Uitvoeren van `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (bijvoorbeeld, `build.cmd cloud debug myRMSolution`)

4. Wanneer dat wordt gevraagd, stelt de **tenantid** voor de nieuwe huurder in plaats van de vorige huurder.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ik wil een beheerder of een collega Administrator wanneer aangemeld met een account met organisatorische wijzigen

Zie het ondersteuningsartikel [wijzigen van de beheerder en CO-beheerder als aangemeld met een account met organisatorische][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom deze fout? 'Uw account beschikt niet over de juiste machtigingen om een oplossing te maken. Neem contact op met de accountbeheerder van uw of probeer met een andere account."

Bekijk het onderstaande diagram:

![][img-flowchart]

> [AZURE.NOTE] Als u nog steeds zien de fout na de validatie u een globale beheerder op de huurder AAD en een CO-beheerder van het abonnement zijn, de accountbeheerder de gebruiker te verwijderen en opnieuw toewijzen vereiste machtigingen in deze volgorde: de gebruiker toevoegen als beheerder van een globale en voeg gebruiker toe als beheerder van een collega op de Azure-abonnement. Als er problemen blijven optreden, neem u contact op met [Help en ondersteuning van][lnk-help-support].

**Waarom deze fout wanneer ik een Azure-abonnement?** *Een Azure-abonnement is vereist voor het maken van vooraf geconfigureerde oplossingen. U kunt een gratis evaluatieversie account maken binnen een paar minuten.*

Als u zeker weet dat u een Azure-abonnement hebt, de huurder toewijzen voor uw abonnement controleren en controleer of dat de juiste huurder is geselecteerd in de vervolgkeuzelijst. Als u hebt gevalideerd de huurder gewenste correct is, volgt u de bovenstaande diagram en valideren van de toewijzing van het abonnement en deze huurder AAD.

## <a name="next-steps"></a>Volgende stappen

Zie hoe u kunt [een vooraf geconfigureerde oplossing aanpassen]om te blijven leren over IoT Suite,[lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
