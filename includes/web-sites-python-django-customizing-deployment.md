Azure bepaalt dat de toepassing gebruikmaakt van Python **Als aan deze voorwaarden voldaan wordt**:

- Requirements.txt bestand in de hoofdmap
- .py-bestanden in de hoofdmap of een runtime.txt waarmee u python

Als dat het geval is, wordt een script voor het specifieke implementatie van Python, waarmee de standaard synchronisatie van bestanden als extra Python bewerkingen zoals gebruikt:

- Automatisch beheer van virtuele omgeving
- Installatie van pakketten in de lijst in requirements.txt met pip
- Het maken van de juiste web.config op basis van de geselecteerde versie van Python.
- Verzamelen van statische bestanden voor Django toepassingen

U kunt bepaalde aspecten van de implementatie van standaard stappen bepalen zonder dat u het script aanpassen.

Als u wilt dat alle Python implementatie specifieke stappen overslaat, kunt u dit leeg bestand:

    \.skipPythonDeployment

Als u verzamelen van statische bestanden voor de toepassing van Django overslaan wilt:

    \.skipDjango 

Voor meer controle over de implementatie kunt u een script voor de implementatie van de standaard overschrijven door het maken van de volgende bestanden:

    \.deployment
    \deploy.cmd

U kunt de [opdrachtregelinterface Azure][] om de bestanden te maken.  Gebruik deze opdracht uit vanuit de projectmap:

    azure site deploymentscript --python

Wanneer deze bestanden niet aanwezig zijn, worden de Azure maakt een tijdelijke installatiescript en uitvoeren.  Het is identiek aan die u met de bovenstaande opdracht maakt.

[Azure interface met opdrachtregel]: http://azure.microsoft.com/downloads/
