<properties 
    pageTitle="Logboekregistratie voor webservices van Machine Learning | Microsoft Azure" 
    description="Informatie over het inschakelen van logboekregistratie voor webservices van Machine Learning. Registratie vindt u aanvullende informatie voor het oplossen van de API's." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Logboekregistratie van Machine Learning web-services  

Dit document bevat informatie over de mogelijkheid tot het vastleggen van klassieke Web services. De logboekfunctie inschakelen in webservices biedt aanvullende informatie, dan gewoon een foutnummer en een bericht dat u kan helpen bij het oplossen van de aanroepen van de API's van Machine Learning.  

Met logboekregistratie inschakelen in Web Services in de klassieke Azure portal:   

1.  Aanmelden bij [Azure klassieke portal](https://manage.windowsazure.com/)
2.  Klik in de kolom naar links voorzieningen **MACHINE LEARNING**.
3.  Klik op uw werkruimte, vervolgens **WEB SERVICES**.
4.  Klik op de naam van de webservice in de lijst Web services.
5.  Klik op de naam van het in de lijst van eindpunten.
6.  Klik op **configureren**.
7.  **Diagnostische gegevens van het TRACERINGSNIVEAU** ingesteld op *fout* of *alle*en klik vervolgens op **Opslaan**.

Met logboekregistratie inschakelen in de portal Azure Machine Learning Web-Services.

1. Inloggen op de portal [Azure Machine Learning Web-Services](https://services.azureml.net) .
2. Klik op klassieke webservices.
3.  Klik op de naam van de webservice in de lijst Web services.
4.  Klik op de naam van het in de lijst van eindpunten.
5.  Klik op **configureren**.
6.  **Logboekregistratie** instellen voor *fout* of *alle*en klik vervolgens op **Opslaan**.

## <a name="the-effects-of-enabling-logging"></a>De gevolgen van het inschakelen van logboekregistratie

Als logboekregistratie is ingeschakeld, worden alle de diagnostische gegevens en fouten van het geselecteerde eindpunt aan de Azure opslag Account bent aangemeld met de werkruimte van de gebruiker gekoppeld. U kunt deze account opslag in de klassieke Azure portal dashboardweergave (onder aan het gedeelte snel bekijken) van de werkruimte zien.  

De logboeken kunnen worden bekeken met behulp van de verschillende hulpprogramma's die beschikbaar zijn voor een Account Azure opslag verkennen. De eenvoudigste manier mogelijk gewoon navigeren naar de rekening voor opslag in de klassieke Azure portal en klik vervolgens op **CONTAINERS**. Vervolgens ziet u een Container met de naam **ml diagnostics**. Deze container bevat de diagnostische gegevens voor alle de eindpunten van een webservice voor alle werkruimten die zijn gekoppeld aan deze account opslag. 
 
## <a name="log-blob-detail-information"></a>Logboekgegevens blob

Elke blob in de container bevat de gegevens van de diagnostische gegevens voor één van de volgende opties:

-   De uitvoering van de methode Batch-uitvoering  
-   De uitvoering van de verzoek-antwoord-methode  
-   De initialisatie van een verzoek-antwoord-container
  
De naam van elke blob heeft een voorvoegsel van de volgende vorm: 

{Werkruimte-Id}-{webservice Id}-{Endpoint-Id} / {type Log}  

Type logboek waar is een van de volgende waarden:  

- batch  
- score/aanvragen  
- score/init  