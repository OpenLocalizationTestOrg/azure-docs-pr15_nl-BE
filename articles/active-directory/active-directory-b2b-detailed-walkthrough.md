<properties
   pageTitle="Gedetailleerd overzicht van het gebruik van het voorbeeld van de samenwerking Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B samenwerking ondersteunt uw relaties tussen bedrijven met zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen inschakelen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure AD B2B samenwerking voorbeeld: gedetailleerde scenario

In dit scenario geeft een overzicht van het gebruik van Azure AD B2B samenwerking. Als de IT-beheerder van Contoso willen we toepassingen delen met medewerkers uit drie partnerbedrijven. Geen van de partnerbedrijven moet AD Azure.

- Alice van eenvoudige Partner Org
- Bob van gemiddeld Partner Org, moet toegang tot een set van apps
- Carol van complexe organigrammen Partner, moet toegang tot een set van apps en het lidmaatschap van groepen bij Contoso

Nadat uitnodigingen worden verzonden naar gebruikers van partners, kunnen we ze in Azure AD verlenen toegang tot apps en het lidmaatschap van groepen via de portal Azure configureren. Laten we beginnen door Alice toe te voegen.

## <a name="adding-alice-to-the-contoso-directory"></a>Lisa toe te voegen aan de map Contoso
1. Een CSV-bestand maken met de kopteksten zoals, vullen met alleen Alice's **e-mailadres**, **naam**en **InviteContactUsUrl**. **Weergavenaam** is de naam die wordt weergegeven in de uitnodiging en de naam die wordt weergegeven in de map Contoso Azure AD. **InviteContactUsUrl** is een manier voor Alice contact opnemen met Contoso. In het volgende voorbeeld geeft de InviteContactUsUrl aan het LinkedIn-profiel van Contoso. Het is belangrijk dat de spelling van de labels in de eerste rij van het CSV-bestand precies zoals opgegeven in de [indeling verwijzing naar een CSV-bestand](active-directory-b2b-references-csv-file-format.md).  
![Voorbeeld van een CSV-bestand voor Alice](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. In de Azure portal kunt u een gebruiker toevoegen aan de map Contoso (Active Directory > Contoso > Gebruikers > gebruiker toevoegen). In het 'Type gebruiker' vervolgkeuzelijst, selecteer 'Gebruikers in partnerbedrijven'. Het CSV-bestand uploaden. Zorg ervoor dat het CSV-bestand wordt gesloten voordat u uploadt.  
![CSV-bestand uploaden voor Alice](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice wordt vertegenwoordigd als een externe gebruiker in Active directory AD Azure Contoso.  
![Alice staat in Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice ontvangt het volgende e-mailbericht.  
![Uitnodiging via e-mail voor Alice](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice op de koppeling klikt en zij wordt gevraagd om de uitnodiging te accepteren en aan te melden met de referenties van haar werk. Als Alice niet in Active directory AD Azure, Alice gevraagd aan te melden.  
![Aanmelden na de uitnodiging voor Alice](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice wordt omgeleid naar de App toegang tot Configuratiescherm leeg totdat zij wordt toegang verleend tot toepassingen.  
![Deelvenster toegang voor Alice](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Met deze procedure kunt de eenvoudigste vorm van B2B-samenwerking. Als een gebruiker in Active directory AD Azure Contoso kan Alice toegang worden verleend tot toepassingen en groepen via het portal Azure. Nu we Bob die toegang tot de toepassingen Moodle en televergaderingen moet toevoegen.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Bob toe te voegen aan de map Contoso en het verlenen van toegang tot apps
1. Gebruik Windows PowerShell met de Azure AD Module geïnstalleerd om de aanvraag-id's van Moodle en televergaderingen te vinden. De id's kunnen worden opgehaald met de cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` verschijnt een lijst met alle beschikbare toepassingen in Contoso en hun AppPrincialIds.  
![Id's ophalen voor Bob](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Maak een CSV-bestand met Bob's e-mailadres en DisplayName, **InviteAppID**, **InviteAppResources**en InviteContactUsUrl. Vul **InviteAppResources** met de AppPrincipalIds van Moodle en televergaderingen gevonden in PowerShell, gescheiden door een spatie. Vul de **InviteAppId** met de dezelfde AppPrincipalId van Moodle te voorzien van het e-mailadres en log in pagina's met een Moodle logo.  
![Voorbeeld van een CSV-bestand voor Bob](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Upload het CSV-bestand via de Portal Azure net zoals deze voor Alice is gedaan. Bob is nu een externe gebruiker in Active directory AD Azure Contoso.

4. Bob krijgt de volgende e-mailadres.  
![Uitnodiging via e-mail voor Bob](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob wordt op de koppeling klikt en gevraagd om de uitnodiging te accepteren. Nadat hij is aangemeld, kan hij is gericht op het Access-venster en al Moodle en televergaderingen.  
![Deelvenster toegang voor Bob](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

We zullen toevoegen Carol vervolgens die toegang tot toepassingen en het lidmaatschap van groepen in de directory Contoso nodig heeft.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Carol toe te voegen aan de map Contoso, toegang verlenen tot apps en groepslidmaatschap geven

1. Windows PowerShell gebruiken met de Azure AD Module geïnstalleerd om te zoeken naar de toepassing-id en groeps-id's binnen Contoso.
 - Ophalen met de cmdlet AppPrincipalId `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, hetzelfde geldt voor de Bob
 - Object-id ophalen voor groepen met cmdlet `Get-MsolGroup | fl DisplayName, ObjectId`. Hiermee wordt een lijst met alle groepen in Contoso en hun extensies. Groep-id's kunnen ook worden opgehaald als de Object-ID op het tabblad Eigenschappen van de groep in de portal Azure.  
![Id's en groepen voor Carol ophalen](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. CSV-bestand, vullen met Carol van e-mailadres, naam, InviteAppID, InviteAppResources, **InviteGroupResources**en InviteContactUsUrl maken. **InviteGroupResources** wordt gevuld met de extensies van de groepen MyGroup1 en externe sleutels, gescheiden door een spatie.  
![Voorbeeld van een CSV-bestand voor Carol](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Upload het CSV-bestand via de portal Azure.

4. Carol is een gebruiker in de map Contoso en is ook een lid van de MyGroup1 en externe sleutels, zoals weergegeven in de portal Azure.  
![Carol wordt in een groep weergegeven in Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol ontvangt een e-mail met een link om de uitnodiging te accepteren. Nadat ze zich aanmeldt, wordt zij omgeleid naar het deelvenster App toegang toegang hebben tot de Moodle en televergaderingen.  

Dat is alles met het toevoegen van gebruikers van bedrijven in samenwerking met Azure AD B2B partner is. In dit scenario blijkt hoe gebruikers Alice en Bob Carol toevoegen aan de Contoso-map met drie afzonderlijke CSV-bestanden. Dit proces kan worden vergemakkelijkt door condensatie van de afzonderlijke CSV-bestanden in één bestand.  
![Voorbeeld van CSV-bestand voor Alice en Bob Carol](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Verwante artikelen
Bezoek onze andere artikelen over Azure AD B2B samenwerking:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
