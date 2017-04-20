<properties
   pageTitle="CSV-indeling voor Azure Active Directory B2B samenwerking preview | Microsoft Azure"
   description="Azure Active Directory B2B ondersteunt uw relaties tussen bedrijven met zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen inschakelen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD B2B samenwerking voorbeeld: CSV-indeling

De voorbeeldversie van Azure AD B2B samenwerking vereist een CSV-bestand partner gebruikersgegevens te uploaden via de portal Azure AD opgeven. Het CSV-bestand moet de vereiste labels hieronder, en optionele velden bevatten. Het CSV-voorbeeldbestand (onder) wijzigen zonder de spelling van de labels in de eerste rij.

>[AZURE.NOTE] De eerste rij van labels (zoals E-mail, naam, enzovoort) is nodig voor het CSV-bestand is verdeeld. De spelling moet overeenkomen met de velden hieronder. Deze labels geven de inhoud zichtbaar worden. Voor optionele velden die niet nodig zijn, kunnen de labels uit het CSV-bestand worden verwijderd. De hele kolom kan leeg zijn.

## <a name="required-fields-br"></a>Verplichte velden: <br/>
**E-mail:** E-mailadres van de uitgenodigde gebruiker. <br/>
**Naam:** De weergegeven naam voor de uitgenodigde gebruiker (meestal de eerste en laatste naam).<br/>


## <a name="optional-fields-br"></a>Optionele velden: <br/>

**InvitationText:** E-mailuitnodiging aanpassen na app huisstijl en voor de koppeling van de terugbetaling.<br/>
**InvitedToApplications:** Bewerkingswaarde tot bedrijfstoepassingen, gebruikers toe te wijzen. Bewerkingswaarde worden opgehaald in PowerShell worden door te bellen`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Extensies voor groepen gebruikers toevoegen. Extensies worden opgehaald in PowerShell worden door te bellen`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL naar een uitgenodigde gebruiker direct na de aanvaarding van de uitnodiging. Dit moet een bedrijfsspecifieke URL (bijvoorbeeld [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Als u dit optionele veld niet opgeeft, worden de uitgenodigde gebruiker is gericht op het deelvenster App toegang waar ze tot uw gekozen bedrijfstoepassingen kunnen navigeren. De App toegang deelvenster URL van het formulier is `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: e-mailadres waarnaar een kopie van de uitnodiging via e-mail verzonden. Als u het veld CcEmailAddress wordt gebruikt, kan deze uitnodiging kan niet worden gebruikt voor de gebruiker e-mail wordt gecontroleerd of de huurder maken.<br/>
**Taal:** Taal voor uitnodiging e-mail en aflossing ervaring met 'nl' (Engels) als standaard wanneer niet is opgegeven. De 10 andere ondersteunde taal codes zijn:<br/>
1. de: Duits<br/>
2. ES: Spaans<br/>
3. FR: Frans<br/>
4. Deze: Italiaans<br/>
5. Ja: Japans<br/>
6. Ko: Koreaans<br/>
7. pt-BR: Portugees (Brazilië)<br/>
8. RU: Russisch<br/>
9. zh-HANS: vereenvoudigd Chinees<br/>
10. zh-HANT: traditioneel Chinees<br/>

## <a name="sample-csv-file"></a>Voorbeeld van een CSV-bestand
Hier is een voorbeeld van een CSV-u kunt wijzigen.

>[AZURE.NOTE] Kopieer en plak deze in Kladblok en opslaan met de extensie 'CSV'. Deze vervolgens bewerken in Excel. Deze worden gestructureerd in een tabel met labels in de eerste rij.

> Optionele velden toevoegen in Excel door te geven het etiket en het vullen van de kolom eronder.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen over Azure AD B2B samenwerking bladeren

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
- [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
