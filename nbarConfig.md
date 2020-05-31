#Network Based Application Recognition#



___
>Nbar(Network Based Application Recognition ) is een classificatie motor die verschillende protocollen en toepassingen kan herkennen en classificeren. Nbar geeft een overzicht over welke applicaties er precies gebruik worden in jou network. Via Nbar kunnen we zien welke applicatie en hoeveel verkeer in bytes. Zo kunnen we precies zien welke protocollen veel in netwerk voorkopen en zo het netwerk optimaliseren. We zullen Nbar niet alleen gebruiken voor het analyseren van de data, maar in latere fase zullen het ook gebruiken voor het optimaliseren van het netwerk via auto-Qos.
___


##Configure Nbar Protocol Discovery##

####1. Enabling Protocol Discovery####

 Nbar bevat een functie genaamd Protocol Discovery. Protocol Discovery biedt een eenvoudige manier van het ontdekken van de toepassing en protocollen die actief zijn op een interface zodat de meest efficiënte quality of service (QoS) functies kunnen worden toegepast.

>Router>enable
>
>Router# configure terminal
>
>
>Router(config)# interface type number [name-tag]
>
>Router(config-if)# ip nbar protocol-discovery
>
>Router(config-if)# end


Nu is Nbar geactiveerd en protocol-discovery is aangezet op een bepaalde interface, dus zal hij alle het verkeer die door die interface gaat bijhouden voor ons.


#####2. Reporting Protocol Discovery Statistics ####

We kunnen nu het verkeer in de geselecteerd interface bekijken via protocol-discovery. Per protocol bewaard protocol-discovery voor de intrface de volgende gegevens bij : Totaal aantal download pakketten en bytes,Totaal aantal gedownloade pakketten en bytes, input bit ratio(verhouding), output bit ratio


>Router> enable
>
>Router# (show policy-map interface type number)
>
>Router# show ip nbar protocol-discovery [interface type number] [**stats {byte-count | bit-rate | packet-count| max-bit-rate}] [protocol** protocol-name | **top-n** number]
>
>exit
>
>
>![](top5Nbar.png)


