#Netflow#


---

>Netflow is een netwerk protocol die informatie verzameld en een toezicht houdt over het verkeersstroom van het netwerk . Netflow is ontwerpen door Cisco en werd in de 1996 beschikbaar gesteld op de Cisco routers. Netflow kan geconfigureerd worden om het verkeer van een interface in gaten te houden. Er is een mogelijkheid om alleen de binnenkomende en/of uitgaande verkeer op die interface te monitoren. In het document *nbarConfig.pdf* hebben het over Nbar gesproken, meer uitleg over de verschillen tussen Nbar en Netflow gaan we in een andere document bespreken. Met Netflow krijgen we een weergave van de volume, de afkomst en bestemming en hoeveel verkeer in het netwerk gegeneerd word door die pakketten.

---

##Configure Netflow and verify config##

####1. Configure Netflow and Data Export ####
>Hier gaan we Netflow configureren en ook data export. Met data export kunnen we de opgenomen gegevens exporteren naar een externe analyzer.
>
>`root@graylogDebian: enable `
>
>`root@graylogDebian: configure terminal`
>
>Nu gaan we netflow configureren en ook aangeven naar welke adres de export gedaan moet worden en door welke port. We hebben een Grylog geconfigureerd om te gebruiken als de analyzer voor onze export. Dus zal de data geëxporteerd worden naar het adres van de Graylog(10.20.120.9).
>
>`root@graylogDebian: ip flow-export destnation*{ip-address|hostname}* udp-port`
>
>![](dataexport.png)
>
>**Herhaal de vorige commando als je data export naar verschillende analyzers en/of via andere porten wild sturen.**
>
>`root@graylogDebian: ip flow-export vesion 9`
>
>Nu moeten we de interface ingeven waar de monitoring gebeurt moet worden.
>
>`root@graylogDebian: interface g0/1`
>
>![](netflowInterface.png)
>
>Het is nu tijd om in te geven of we het inkomende verkeer(ingress) of uitgaande verkeer(egress) op de interface zullen monitoren.  We kunnen ook ervoor kiezen om beide te doen.
>
>`root@graylogDebian: ip flow {ingress|egress}`
>
>![](flowIgEgg.png)
>
>`root@graylogDebian: exit`
>
>**Herhaal de commmandos vanaf** *root@graylogDebian: interface g0/1* **tot en met** *`root@graylogDebian: exit`* **om netflow te configuren op andere interfaces**
>
>`root@graylogDebian: end`

####2. Verify Netflow ####
>We gaan nu controleren of Netflow goed geconfigureerd is en of data export in orde is.


#####1. Is Netflow enabled#####

######Interface check######
>Eerste manier om dit te controleren is de interface, we controleren of de interface waarop we Netflow geconfigureerd hebben in orde is.

>`root@graylogDebian: show ip interface`
>
>![](netfloShowInterface.png)
>
>** Dit klopt, de configuraties gebeurde op g0/1 en het was zowel ingress en egress**

######Cach flow######
>Nu we zeker weten dat de interface in orde is gaan we kijken of Netflow zelf operationeel is en vragen we een samenvatting van de Netflow statistieken. De ontleden van netflow samenvatting gaan we in de documenten ** verder ontleden

>`root@graylogDebian: show ip cach flow`
>
>![](showCachFlowDeel1.png)
>![](showCachFlowDeel2.png)


###### gedataileerde Cach flow######
>Met deze commando krijg je een meer gedataileerde sammenvatting van de netflow statestieken. We krijgen bijvoorbeeld ook poortnr , nexthop etc.

>`root@graylogDebian: show ip cach verbose flow`
>
>![](showCachVerboseFlowDeel1.png)
>![](showCachVerboseFlowDeel2.png)


#####2. Data Export Controle#####
>Nu moet er een controle gebeuren om te controleren of data Export goed geconfigureerd is. Met de volgende commando krijgen we een samenvatting over de data export. We kunnen dan zien naar waar het verstuurd word welke poortnr, versie, hoeveel pakketten verstuurd of gedropped etc.

>`root@graylogDebian: show ip flow export`
>
>![](dataExportCheck.png)


**Deze Document is om voor het configureren en controleren van de netflow. De betekenis van de weergaven zal in het document** *netflowAnalyze.pdf*  **uitgelegd worden in met meer details**


	


