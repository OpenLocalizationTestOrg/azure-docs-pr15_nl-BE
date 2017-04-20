
De code voor alle functies in een bepaalde functie app woont in een hoofdmap met een hostconfiguratiebestand en een of meer submappen, waarin de code voor een aparte functie, zoals in het volgende voorbeeld bevat

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Het bestand *host.json* bevat een runtime-specifieke configuratie en bevindt zich in de hoofdmap van de toepassing van de functie. Zie voor informatie over instellingen die beschikbaar zijn, [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in de bibliotheek WebJobs.Script wiki.

Elke functie heeft een map met een of meer codebestanden, de configuratie van de function.json en andere afhankelijkheden.