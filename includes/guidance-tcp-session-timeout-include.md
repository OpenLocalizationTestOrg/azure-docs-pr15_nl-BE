##<a name="tcp-settings-for-azure-vms"></a>TCP-instellingen voor Azure VMs

Azure VMs met het openbare Internet communiceren via [NAT] [ nat] (Network Address Translation). NAT-apparaten toewijzen een openbaar IP-adres en -poort voor een Azure VM, waardoor die VM vast te stellen een socket voor communicatie met andere apparaten. Pakketten die socket stromen na een bepaalde tijd stoppen het NAT-apparaat de toewijzing beëindigd als de socket moet worden gebruikt door andere VMs gratis is.

Dit is een gemeenschappelijk gedrag NAT, waardoor problemen in de communicatie op de TCP-gebaseerde toepassingen die een socket verwachten moeten worden onderhouden dan een time-outperiode. Er zijn twee instellingen voor time-out voor inactiviteit te overwegen voor sessies in een *verbinding tot stand gebracht* staat:

- **inbound** via de [Azure de belasting voor documentconversies][azure-lb-timeout]. Deze time-out wordt standaard ingesteld op 4 minuten en kan worden aangepast om tot 30 minuten.
- **uitgaande** [SNAT] met[ snat] (bron NAT). Deze time-out is ingesteld op 4 minuten en kan niet worden aangepast.

Om verbindingen zijn niet verder dan de time-outlimiet verloren, moet u controleren of uw toepassing blijft de sessie actief of kunt u het onderliggende besturingssysteem hiervoor configureren. De instellingen worden gebruikt zijn verschillend voor Linux en Windows-systemen, zoals hieronder wordt weergegeven.

[Linux][linux], moet u de kernelvariabelen hieronder.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Voor [Windows][windows], moet u de volgende registerwaarden.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


De bovenstaande instellingen garanderen een keepalive-pakket wordt verzonden na 2 minuten (120 seconden) van niet-actieve tijd, en vervolgens elke 30 seconden wordt verzonden. En als 8 van deze pakketten niet, de sessie wordt verbroken.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md