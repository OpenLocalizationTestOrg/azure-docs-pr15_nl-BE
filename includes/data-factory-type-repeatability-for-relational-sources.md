## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens kopiëren

Bij het kopiëren van gegevens van en naar de relationele winkels, moet u de herhaalbaarheid moet u rekening houden om te voorkomen dat ongewenste resultaten. 

Een segment kan worden automatisch opnieuw uitvoeren in Azure Data Factory aan de hand van het opgegeven beleid voor opnieuw proberen. We raden u een beleid opnieuw te beschermen tegen storingen van voorbijgaande aard. Herhaalbaarheid is daarom een belangrijk aspect om te zorgen voor tijdens de verplaatsing van gegevens. 

**Als een bron:**

> [AZURE.NOTE] De volgende voorbeelden zijn van Azure SQL maar van toepassing zijn op een gegevensarchief waarin de rechthoekige datasets. Misschien moet u het **type** van de bron en de eigenschap **query** aanpassen (bijvoorbeeld: query in plaats van sqlReaderQuery) voor de gegevens worden opgeslagen.   

Meestal bij het lezen van relationele winkels, wilt u lezen, alleen de gegevens die overeenkomen met dat segment. Een manier om dit te doen zou zijn met behulp van de variabelen WindowStart en WindowEnd beschikbaar in Azure Data Factory. Meer informatie over de variabelen en functies in Azure Data Factory hier in de [planning en uitvoering van](../articles/data-factory/data-factory-scheduling-and-execution.md) het artikel. Voorbeeld: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Deze query ophaalt gegevens from MijnTabel' die in het segment duur bereik valt. Opnieuw uitvoeren van dit segment zou dit probleem ook altijd garanderen. 

In andere gevallen kunt u de hele tabel te lezen (Stel dat voor één keer alleen verplaatsen) en de sqlReaderQuery kunnen als volgt definiëren:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
