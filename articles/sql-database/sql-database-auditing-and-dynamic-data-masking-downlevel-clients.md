<properties
    pageTitle="Ondersteuning voor downlevel clients van SQL-Database en IP-eindpunt voor controlebeleid gewijzigd | Microsoft Azure"
    description="Informatie over ondersteuning voor SQL-Database voor downlevel-clients en IP-eindpunt wijzigingen voor controle."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-Database - ondersteuning voor Downlevel-clients en IP-eindpunt wijzigingen voor controle


[Controle](sql-database-auditing-get-started.md) werkt automatisch met SQL-clients die ondersteuning bieden voor omleiden TDS.


##<a id="subheading-1"></a>Ondersteuning voor downlevel clients

Elke client waarmee TDS 7.4 geïmplementeerd moet ook ondersteuning bieden voor omleiden. Uitzonderingen hierop zijn JDBC 4.0 waarin de functie Mapomleiding niet volledig wordt ondersteund en Tedious voor Node.JS in welke Mapomleiding niet is geïmplementeerd.

Voor 'Downlevel clients', moet dat wil zeggen welke ondersteuning TDS versie 7.3 en onder - de FQDN-naam van de server in verbinding met de tekenreeks worden gewijzigd:

Oorspronkelijke server FQDN-naam in de verbindingsreeks: <*servernaam*>. database.windows.net

Gewijzigde server FQDN-naam in de verbindingsreeks: <*servernaam*> .database. **beveiligde**. windows.net

Een gedeeltelijke lijst van "Downlevel clients" omvat:

- .NET 4.0 en lager,
- ODBC-10.0 en lager.
- JDBC (Hoewel JDBC TDS 7.4 ondersteunt, de TDS-functie Mapomleiding niet volledig wordt ondersteund)
- Vervelende (voor Node.JS)

**Opmerking:** De bovenstaande wijziging FULLY-server mogelijk handig ook voor de toepassing van een beleid voor controle van SQL Server op zonder een noodzaak voor een configuratiestap van elke database (tijdelijke beperking).

##<a id="subheading-2"></a>IP-eindpunt wordt gewijzigd wanneer u controle inschakelt

Houd er rekening mee dat wanneer u controle inschakelt, het IP-eindpunt van de database worden gewijzigd. Als er strikte firewall-instellingen, werk de firewallinstellingen dienovereenkomstig.

De nieuwe database IP-eindpunt is afhankelijk van de regio:

| Databaseregio | Mogelijke IP-eindpunten |
|----------|---------------|
| China-Noord  | 139.217.29.176, 139.217.28.254 |
| East China  | 42.159.245.65, 42.159.246.245 |
| Australië, Oost  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Zuidoost Australië | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazilië-Zuid  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Centrale VS  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Oost-Azië   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Oost-Verenigde Staten 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Oost-Verenigde Staten   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Centraal India  | 104.211.98.219, 104.211.103.71 |
| Zuid-India   | 104.211.227.102, 104.211.225.157 |
| West-India  | 104.211.161.152, 104.211.162.21 |
| East Japan   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| West Japan    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Centrale Noord-Amerikaanse  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Noord-Europa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Zuid-centraal-Verenigde Staten  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Zuidoost-Azië  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| West-Europa  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| West-Verenigde Staten  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Canada Central  | 13.88.248.106 |
| Canada Oost  |  40.86.227.82 |
