## <a name="peering-across-subscriptions"></a>Peering over abonnementen

In dit scenario maakt u een peering tussen twee VNets die behoren tot verschillende abonnementen.

![cross-sub-scenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Peering VNet is gebaseerd op rollen gebaseerde toegangscontrole (RBAC) voor een vergunning. Cross-abonnementen scenario moet u eerst voldoende machtigingen verlenen aan gebruikers die de peering verbinding maakt:

> [AZURE.NOTE] Als de gebruiker de bevoegdheid voor beide abonnementen heeft, kunt u onderstaande stap 1-4 overslaan.
