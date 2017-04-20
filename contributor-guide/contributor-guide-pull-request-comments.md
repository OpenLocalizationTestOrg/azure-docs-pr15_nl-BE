# <a name="pull-request-comment-automation"></a>Pull-aanvraag Opmerking automatisering

We Opmerking automatisering in pull-aanvraag opmerkingen dat inzenders en auteurs voor het toewijzen van labels die de pull-aanvraag proces bekijken.

| Opmerking | Wat het doet | Beschikbaarheid|
| -------- |-------------|-------------|
|#afmelden | Wanneer de auteur van een artikel de opmerking **#sign-off** in de stroom van de opmerking, is de **ready-samenvoegen -** label toegewezen. | Openbare en particuliere|
|#afmelden | Als een medewerker die niet de vermelde auteur pogingen op Afmelden op een openbare pull-aanvraag met de opmerking **#sign-off** en wordt een bericht naar de pull-aanvraag die aangeeft dat het etiket kan worden toegewezen door de auteur geschreven. | Publiek |
|#wachtstand uitschakelen | Als u in een pull-aanvraag Opmerking **#hold-off** typt, wordt het label **klaar voor samenvoegen** - verwijderd als u van gedachten verandert of een fout maakt. In de particuliere repo, wijst u toe het label **kan niet samenvoegen** . | Openbare en particuliere |
| #u kunt sluiten | Auteurs kunnen de opmerking **#please sluiten** typt in de stroom van de opmerking van een pull-aanvraag te sluiten als u niet wilt dat die wijzigingen samengevoegd. | Publiek |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Problemen met de teken-en nadelen in de openbare repo

De openbare repo afmelden automatisering is kan alleen de auteur afmelden. Sommige uitzondering voor handmatige verwerking nodig zijn:

- **De auteurs van het artikel**: voor het gebruik van de openbare bibliotheek Opmerking automatisering, uw eigen GitHub account moet exact overeenkomen met de GitHub-account die wordt vermeld in de metagegevens van het artikel. Het hoofdlettergebruik van de aangelegenheden van uw account. Als u van een handtekening af vanwege dit probleem zijn geblokkeerd, kunt u e-mail verzenden naar de alias van azdocprs.

- **Andere werknemers**: als u een werknemer zich aanmeldt uit ten behoeve van de auteur bent en u bent geblokkeerd door de automatisering, neem dan contact op met de azdocprs met de koppeling pull-aanvraag. Aangeven aan wie u bent--PMs op het hetzelfde productteam, collega's op het team het schrijven en het schrijven van teammanagers worden beschouwd als betrouwbare bronnen.



## <a name="related"></a>Gerelateerd

- [Pull-aanvraag etiquette en aanbevolen procedures voor Microsoft-medewerkers](contributor-guide-pull-request-etiquette.md)

- [Kwaliteitscriteria voor pull-aanvraag controleren](contributor-guide-pr-criteria.md)
