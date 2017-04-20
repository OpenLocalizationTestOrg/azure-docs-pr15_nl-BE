### <a name="app-service-plan"></a>App-serviceplan

Het abonnement voor het hosten van de web app maakt. U opgeven de naam van het plan tot en met de parameter **hostingPlanName** . De locatie van het plan wordt dezelfde locatie gebruikt voor de resourcegroep. De prijzen laag en werknemer grootte zijn opgegeven in de **sku** - en **workerSize**

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

