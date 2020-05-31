#Install Debian in Hyper-V#


---
>De eerste stap voordat we kunnen beginnen met alle andere installaties voor onze Graylog, moeten we eerst een Debian Systeem hebben. In de documentatie gaan we een virtuele pc opzetten met onze via vanuit onze Hyper-V systeem. Volg de stappen voor de specifieke stappen voor de installatie en configuratie van de Debian systeem. Deze Documentatie is opgesteld op 07/01/2019.

---



Hyper-V(Veridian)is een hypervisor, dit kan gebruiken worden voor het creëren van virtuele machines op een Windows systeem. Het is een software die gebruikt wordt voor virtualisatie van besturingssystemen en hardware zoals hard drives en netwerk switches. In tegenstelling tot Virtualbox en Fusion is hyper-V niet beperkt tot user devices maar kan ook gebruik worden voor de virtualisatie van servers.


##Maak een Virtuele  PC aan##

###1. download de iso bestand ###
Eerste moeten we de ISO file downloaden. We gaan gebruik maken van een netinstall(Network Intall), deze ISO-file heeft alleen de basic installaties. Het is dus een heel licht versie die niet te groot is, extra installatie kan gebeuren via internet. Download de ISO file op [https://www.debian.org/CD/netinst/](https://www.debian.org/CD/netinst/ "ISO"). Ik gebruik de netinst CD image(amd64). Nu kunnen we beginnen met aanmaken van onze virtuele systeem, volg de stappen hieronder.


### 2. Maak Virtuele PC.###
In Hyper-V onder de "Acties" menu rechtsboven, kiezen we de optie "Nieuw" en daarna "Virtuele Machine". 


![](vertual1.png)

#####Naam en locatie#####

![](vertual2.png)


#####Generatie #####
![](vertual3.png)


#####Gebeugen(RAM) #####
>We moeten nu meer dan 4G kiezen anders krijgen we problemen met de JAVA en Elasticsearch 
>
>![](vertual4.png)


#####Netwerk Configuratie #####
>voor de network optie kiezen we momenteel de optie *Standaarnd schakeloptie*. na de volledige installatie kunnen we en virtuele switch creeren en een statishe ip instellen
>
>![](vertual5.png)

#####Harde schijf #####
>Kies de groote van van je schijf
>
>![](vertual6.png)


#####Kies  besturingssysteem#####
> Ga naar de gedownloade document en kies de ISO bestand die we daarnet gedownload hebben.
>
>![](vertual7.png)



#####Secure boot#####
> Nu is Vertuele pc aangemaakt, voordat we hem kunnen opstarten moeten er nog een belangerijke ding doen. rechter muisklik op de virtuele machine en klik op *instellingen*. onder de optie *hardware* selecteert u *Beveiliging*. De optie *Secure Boot inschakelen* mag niet geselecteerd zijn. 
>
>![](oplossingsecureboot.png)
>
>**Dit is belangerijk anders zal VM niet opstarten**
>![](bootErrorSecureBoot.png)
>


##2. Install Debian##
![](install.png)


#####Kies Taal #####
![](install2.png)


#####Kies locatie #####
![](install3.png)

#####Kies toetsenboord layout #####
![](install4.png)

>In de vijfde moet de gebruikers naam en wachtwoord ingesteld. In sommige gevallen is het niet nodig om een aparte gebruiker aan te maken, daarom heb ik alleen een root gebruiker. Als dit niet het geval is, maak dan een gebruiker aan.

#####Schijven indelen #####
![](install7.png)

>Het is mogelijk de schijf op verschillende manieren in te delen. Ik kies voor de eerste optie.
![](install8.png)

#####Schijven indelen opslaan #####
>![](install9.png)
>
>![](install10.png)

#####Extra installatie pakket #####
> In de stap word er gevraagd of u nog een andere installatie bron heeft voor extra installatie. Kies *nee*. Omdat we network-based installatie doen hallen we alle extra installatie van het internet, niet van een installatie-cd etc.
![](install11.png)

#####Pakketbeheer #####
>Kies  **ja**
![](install12.png)

#####package usage survey #####
>kies nee voor de enquête
![](install13.png)

#####Software selectie #####
>
>![](install14.png)
>
>Hierna krijgen we een scherm met verchillende *software installatie* opties.
>we zorgen er voor dat alleen **SSH Server** en **Standaard Systeem Utilities** is aangeduid.

#####Installatie afronden #####
![](install15.png)

#####Herstart de machine #####
>Nu is de installatie voltooid is kunnen we de pc herstarten.
>![](graylogAan.png)


**Voor we een verbinding kunnen make met de Machine via SSH moeten we een twee belangrijke taken verrichten. Ten eerste moesten we een virtuele switch aan maken ten tweede moeten we een static IP aan de machine geven. Die taken zullen verricht worden in het document "VswitchStaticIP.pdf".**






























