<properties pageTitle="Opdrachten voor een nieuw artikel maken of bijwerken van een bestaand artikel GIT" description="Stappen voor het werken met de technische Azure inhoud GitHub opslagplaatsen." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Opdrachten voor een nieuw artikel maken of bijwerken van een bestaand artikel GIT


## <a name="standard-process-working-from-master"></a>Standaard CMYK (werken van model)
Volg de stappen in dit artikel voor het maken van een lokale vestiging van werken op uw computer zodat u kunt een nieuw artikel voor de sectie technische documentatie van de azure.microsoft.com maken of bijwerken van een bestaand artikel.

1. Start Git Bash (of het opdrachtregelhulpprogramma dat voor Git u).

 **Opmerking:** Als u in de openbare bibliotheek werkt, omzetten in azure-inhoud-pr azure-inhoud in alle opdrachten.

2. Ga naar azure-inhoud-pr:

        cd azure-content-pr
3. De master branch uitchecken:

        git checkout master

4. Maakt u een nieuwe lokale werken vertakking is afgeleid van de master branch:

        git pull upstream master:<working branch>


5. Verplaatsen naar de nieuwe tak van de werken:

        git checkout <working branch>

6. Laat uw vork weet dat u hebt gemaakt met de lokale vestiging van werken:

        git push origin <working branch>

7. Het nieuwe artikel maken of wijzigingen aanbrengen in een bestaand artikel. Gebruik Windows Verkenner te openen en nieuwe prijsverlaging bestanden maken en Atom (http://atom.io) gebruiken als editor prijsverlaging. Nadat u hebt gemaakt of gewijzigd voor uw artikel en afbeeldingen, gaat u naar de volgende stap.

8. Toevoegen en het doorvoeren van de wijzigingen:

        git add .
        git commit –m "<comment>"
        
   Of bestanden toevoegen van alleen de specifieke u gewijzigd:

        git add <file path>
        git commit –m "<comment>"

   Als u bestanden hebt verwijderd, hebt u dit gebruiken:
   
        git add --all
        git commit -m "<comment>"

9. Uw lokale vestiging van de werkmap bijwerken met wijzigingen van upstream:

        git pull upstream master

10. De wijzigingen aan de fork push op GitHub:

        git push origin <working branch>

12. Als u uw inhoud verzenden naar de upstream master vertakking voor staging, validatie, en/of publiceren, in de UI GitHub maken een pull-aanvraag van de vork op de master-vertakking.

13. Als u een werknemer in de persoonlijke bibliotheek werken de wijzigingen die u verzendt automatisch worden klaargezet en een staging-koppeling naar de pull-aanvraag wordt geschreven. Bekijk de inhoud van uw gefaseerde en afmelden in de opmerkingen pull-aanvraag door de **sign uit #** opmerking toe te voegen.  Hier worden dat de wijzigingen zijn nu live worden geduwd.  Als u niet dat de pull-aanvraag te aanvaarden - wilt als u alleen de wijzigingen - zijn tijdelijke toevoegen de **#hold-off** aan de pull-aanvraag.

14. De acceptor pull-aanvraag beoordeelt uw aanvraag pull, geeft feedback en/of de pull-aanvraag heeft geaccepteerd. 

15. Eventueel ook controleren of uw gepubliceerde artikel of wijzigingen op

 *name-of-your-article-without-the-MD-extension* http://Azure.Microsoft.com/Documentation/articles/

## <a name="publishing"></a>Publiceren

- Artikelen worden gepubliceerd op ongeveer 10:00 en 15:00 uur Pacific Time, maandag tot vrijdag. Het kan maximaal 30 minuten duren voordat artikelen online na de publicatie wordt weergegeven. Vergeet niet dat uw pull-aanvraag is door een revisor pull-aanvraag wordt samengevoegd voordat de wijzigingen kunnen worden opgenomen in de volgende geplande publicatie uitvoeren. U moet werken met de pull revisor van tevoren om ervoor te zorgen dat een pull-aanvraag voor een bepaalde publicatie uitgevoerd wordt samengevoegd. Anders worden PRs gecontroleerd in de volgorde waarin die ze zijn ingediend.

- Als u een werknemer die werkt in de persoonlijke bibliotheek, worden alle pull aanvragen validatieregels die worden aangepakt moeten voordat de pull-aanvraag kan worden samengevoegd. 

## <a name="working-with-release-branches"></a>Werken met release bijkantoren

Als u met een vertakking release werkt, is de beste manier om een lokale vestiging van werken van het bijkantoor release maken gebruik van de syntaxis van deze opdracht:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Dit maakt de lokale vestiging rechtstreeks vanuit de upstream filialen, lokale samen voorkomen.

