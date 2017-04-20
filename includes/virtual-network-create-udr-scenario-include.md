## <a name="scenario"></a>Scenario

Dit document om beter illustreren hoe u UDRs maakt, gebruikt het volgende scenario.

![BESCHRIJVING VAN AFBEELDING](./media/virtual-network-create-udr-scenario-include/figure1.png)

In dit scenario maakt u één UDR voor de *Front end subnet* en een andere UDR voor de *Back-end-subnet* zoals hieronder beschreven: 

- **UDR FrontEnd**. De front-end UDR wordt toegepast op het subnet *FrontEnd* en één route bevatten:  
    - **RouteToBackend**. Deze route wordt al het verkeer verzenden naar het subnet van de back-end **FW1** virtual machine.
- **UDR BackEnd**. De back-end UDR wordt toegepast op het subnet van de *back-end* en één route bevatten: 
    - **RouteToFrontend**. Deze route wordt alle verkeer worden verzonden naar het subnet-front-end **FW1** virtual machine.

De combinatie van deze routes zorgt ervoor dat alle verkeer van een subnet naar de andere worden doorgestuurd naar de **FW1** virtuele machine die wordt gebruikt als een virtueel toestel. Ook moet u doorsturen via IP voor die VM, zodat het verkeer dat bestemd is voor andere VMs kan ontvangen inschakelen.
