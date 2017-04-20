Met Azure Resource Manager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters met alle parameterwaarden.
Een parameter voor de waarden die verschillen op basis van het project dat u wilt implementeren of op basis van de omgeving die u levert, moet u definiëren. Definieert parameters voor de waarden die altijd hetzelfde zal blijven. Elke parameterwaarde wordt gebruikt in de sjabloon voor het definiëren van de bronnen die zijn geïmplementeerd. 

Bij het definiëren van parameters, gebruikt u het veld **allowedValues** op te geven welke waarden die een gebruiker tijdens de implementatie kan bieden. Gebruik het veld **standaardwaarde** een waarde toewijst aan de parameter, als er geen waarde wordt opgegeven tijdens de implementatie.

Elke parameter in de sjabloon wordt beschreven.

### <a name="logicappname"></a>logicAppName

De naam van de app logica te maken.

    "logicAppName": {
        "type": "string"
    }