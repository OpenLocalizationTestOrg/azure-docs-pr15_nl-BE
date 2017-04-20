Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters met alle parameterwaarden.
Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd. 

Bij het definiëren van parameters, gebruikt u het veld **allowedValues** op te geven welke waarden die een gebruiker tijdens de implementatie kan bieden. Gebruik het veld **standaardwaarde** een waarde toewijst aan de parameter, als er geen waarde wordt opgegeven tijdens de implementatie.

Elke parameter in de sjabloon wordt beschreven.

### <a name="sitename"></a>Sitenaam

De naam van de web app die u wilt maken.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

De naam van de App serviceplan te gebruiken voor het hosten van de web app.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

De prijzen laag voor de hosting plan.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

De sjabloon bepaalt de waarden die zijn toegestaan voor deze parameter en een standaardwaarde (S1) wordt toegewezen als er geen waarde is opgegeven.

### <a name="workersize"></a>workerSize

De grootte van het exemplaar van het hosting plan (kleine, middelgrote of grote).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
De sjabloon bepaalt de waarden die zijn toegestaan voor deze parameter (0, 1 of 2) en een standaardwaarde (0) wordt toegewezen als er geen waarde is opgegeven. De waarden komen overeen met de kleine, middelgrote en grote.
