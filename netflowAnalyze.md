#Analyze Netflow Statistics#

---
>In deze document gaan we de verschillende delen van de show commando's die we het document *netflowConfig.pdf*gebruikt hebben doorlopen en uitleggen. Hierdoor horen we een betere zicht te krijgen van onze verkeer en een idee van de oorzaak van de problemen in onze netwerk. Daarna zullen we zien hoe we de show commando's kunnen aanpassen en filteren om meer gefocust informaties kunnen krijgen.

---

##Analyzing the Show commands##

####1. Show ip cach flow ####

#####1.Packet size Distrubution #####
>Dit is de eerste deel van de samenvatting. Hier krijgen we informatie over de Groottes van de verschillende IP-pakketten die door de interface gaan. De bovenste regel vertelt ons hoeveel pakketten we in totaal hebben. Er zijn in totaal 109 miljoen pakketten door onze interface gekomen.
>
>![](showCachAnalyze1.0.png)
>
>Middelste regel geeft ons een overzicht met van de verschillende groottes. De verdeling heeft een toename van 32 Byte. 
>
>![](showCachAnalyze1.0.5.png)
>
>In de laatste regel zien we de verdeling van pakketten in percentage. De eerste pakket die we tegen komen is 64 Byte. Een IP Pakket heeft een minimum grootte van 64 Byte. Er zijn 33% bij 64 Byte en de meeste aantal pakketten zijn 1536 Byte groot met een percentage van 42%.
>
>![](showCachAnalyze1.png)


#####2.Protocols #####
>Helemaal links heb je een overzicht van de protocollen die door die interface gekomen is. Deze lijst is natuurlijk een verdeling van de 109 Miljoenen pakketten
>
>![](showCachAnalyze2.0.png)
>
>*Total Flows* de totaal aantal keer die protocol door de interface gekomen is sinds de laatste statistieken verwijderd werden. *Flow/sec* Hoe vaak die protocol per seconde door de interface komt. *Packets/Flow* Gemiddelde pakketten die er elke keer zijn wanneer deze protocol actief is.
>
>
>![](showCachAnalyze2.1.png)
>
>
>
>![](showCachAnalyze2.2.png)
>
>De twee laatste velden bereken hoeveel sec een bepaalde pakket actief was tot ze inactief werden.
>
>![](showCachAnalyze2.3.png)

#####3.Senders, Receivers and Destinations #####
>*SrcIf* Is de Interface die het pakket ontvangen heeft. *SrcIPaddress* IP-adres van het toestel die het pakket verzonden heeft
>
>![](showCachAnalyze3.1.png)
>
>*DstIf* Interface die het pakket verstuurd heeft. *DstIPaddress* IP-adres voor de ontvanger.
>
>![](showCachAnalyze3.2.png)
>
>*Pr* de IP-protocol poortnummer in de RFC 1340, poortnummer is in hexadecimaal getal. *SrcP* Poortnummer van de verzender van deze protocol. *DstP* de ontvanger poortnummer voor deze protocol.
>
>![](showCachAnalyze3.3.png)
>
>*Pkts* de Hoeveelheid pakketten die door de verkeersstroom gestroomd is.
>
>![](showCachAnalyze3.4.png)
>
>**Samenvatting**
>
>Met de nieuwe informatie die we hebben kunnen we nu de output een beetje beter begrijpen, dus zal ik de eerste regel in normale mensen taal schrijven.
>
>Er is **1** pakket van **HTTPS** verstuurd van de interface **Gi0/1** met IP-adres **13.69.158.96** naar de **01BB**(443) via de door de interface **Gi0/2** met adres **10.20.1.104**.
>
>Met andere woorden iemand gaat op internet door de interface **Gi0/2**.
>![](showCachAnalyze3.5.png)


####2. Show ip cach verbose flow ####
>De output van de show verbose commando is, een beetje hetzelfde als de eerste. De verbose heeft een paar extra informaties die de eerste optie niet heeft. De poort is leesbaarder omdat het in cijfer is **8865**. Er is een veld MSK voor de subnet mask van het netwerk **/24**. Ten laatste is eer en veld *Next hop* voor volgende Hop-adres **10.20.40.95**.
>
>![](showCachAnalyzeVerbose1.png)


##Filtering the Show commands##
>Nu gaan we een paar command doen om meer informatie te krijgen. We gaan de gegevens filteren om de antwoorden te vinden op bepaalde vragen.

####1. Show top 10 ####
>Nu gaan we zien welke protocollen veel verkeer versturen op het netwerk . Om dit te doen moeten eerst een paar configuraties doen.
>
>`root@graylogDebian: config terminal `
>
>`root@graylogDebian: ip flow-top-talkers `
>
>`root@graylogDebian: top 10 `
>
>`root@graylogDebian: sort-by bytes`
>
>![](topTalkers2.png)
>
>`root@graylogDebian: show ip flow top-takers `
>
>![](topTalkersShowSort.png)
>
>Wat ik hieruit kan lezen is dat het meeste pakketten via port **01BB**(443) dus HTTPS wat geen probleem is. De tweede meeste is **0050**(80)HTTP. Meeste pakketten zijn zijn van mensen die op internet gaan dit is geen probleem.


####2. Show top 20 ####
>In de top 10 hebben we niets verdachts of erg gezien dus Nu top 20.
>
>`root@graylogDebian: config terminal `
>
>`root@graylogDebian: ip flow-top-talkers `
>
>`root@graylogDebian: top 20 `
>
>`root@graylogDebian: sort-by bytes`
>
>`root@graylogDebian: show ip flow top-takers `
>
>Nu komen we een poort **0FE6**(4070)tegen. Het protocol die aan deze poort verbonden is heet *tripe* **Trival IP Encription(TrIPE)**, dit is een Amazone service die stremming connecties maak met Spotify. Dit ga we moeten oplossen.
>
>![](topTalkersShowSortTop20.png)
>
>De tweede poort die ik opgemerkt heb, is de poort **0800**(2048). Het is een port die gebruikt wordt door Web cach Control Protocol van Cisco, dit is geen probleem. Deze poort wordt gebruik door Camarades die zorgt voor portfowarding en dls-monitor van Nmap. Deze twee services zijn geen Probleem, ik heb op meerder site gezien dat poort 2048 gevaarlijk kan zijn omdat er een threat genaamd *Shiva/spider* op die poort kan luister naar configuraties van Telnet.
>
>![](topTalkersShowSortTop20.1.png)
>
>Poort **01BB**(443) komt veel voor dus ik wil de top 20 zien zonder de poort 443.  
>
>`root@graylogDebian: show ip flow top-takers | exclude 001BB`
>
>Dit ziet er ook uit. Poort **03E1**(993) word gebruikt door IMAP, en poort **E09C**(57500) wordt gebruikt door *Xsan Filesystem Access* van Apple.
>
>![](topTalkersShowSortTop20Exclude01BB.1.png)




