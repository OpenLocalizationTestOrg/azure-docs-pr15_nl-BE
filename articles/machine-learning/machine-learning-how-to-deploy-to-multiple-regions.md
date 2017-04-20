<properties
    pageTitle="Het implementeren van een webservice aan meerdere landen | Microsoft Azure"
    description="Stappen voor het implementeren (kopie) een nieuwe webservice naar andere regio's."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Het implementeren van een webservice aan meerdere landen

De nieuwe Azure-webservices kunt u eenvoudig een webservice aan meerdere landen implementeren zonder dat meerdere abonnementen of werkruimten. 

Prijzen is regio bepaald, dat dus moet u een factureringsaccount plan voor elke regio waarin u de web-service implementeert.

## <a name="to-create-a-plan-in-another-region"></a>Een plan maken in een andere regio

1. Aanmelden bij [Microsoft Azure Machine Learning Web-Services](https://services.azureml.net/).
2. Klik op de optie **plannen** .
3. Klik op **Nieuw**op de plannen via de pagina weergeven.
4. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement waarin het nieuwe plan wilt maken.
5. Selecteer in de vervolgkeuzelijst **regio** een regio voor het nieuwe plan. De plannen van opties voor het geselecteerde gebied wordt weergegeven in de sectie **Opties voor het plannen** van de pagina.
6. Selecteer een groep voor het plan in de vervolgkeuzelijst **Resourcegroep** . Zie voor meer informatie over resourcegroepen, [resources Azure beheren via de portal](../azure-portal/resource-group-portal.md).
7. Typ in **Naam** de naam van het plan.
8. Klik onder **Opties voor**het factuuradres op voor het nieuwe plan.
9. Klik op **maken**.


## <a name="deploying-the-web-service-to-another-region"></a>Implementatie van de web-service naar een andere regio

1. Klik op de optie **Web Services** .
2. Selecteer de webservice die u op een nieuw gebied implementeert.
3. Klik op **kopiëren**.
4. Typ een nieuwe naam voor de webservice in het vak **Naam van Web Service**.
5. Typ een beschrijving voor de webservice in het vak **Beschrijving van de webservice**.
6. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement waar de nieuwe webservice zich bevindt.
7. Selecteer in de vervolgkeuzelijst **Groep** een resourcegroep voor de webservice. Zie voor meer informatie over resourcegroepen, [resources Azure beheren via de portal](../azure-portal/resource-group-portal.md).
8. Selecteer het gebied waarin de webservice te implementeren in de vervolgkeuzelijst **regio** .
9. Selecteer in de vervolgkeuzelijst **opslag account** een opslag voor het opslaan van de webservice.
10. Selecteer in de vervolgkeuzelijst **Prijs Plan** een plan in de regio die u hebt geselecteerd in stap 8.
11. Klik op **kopiëren**.

