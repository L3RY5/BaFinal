#Virtual Switch en  Static IP#


---

>Een "Virtual switch" is een programma die ons toelaat om verschillende virtuele machines met elkaar te laten communiceren. Met een virtual switch kunnen de VM zowel verbinding maken met een virtuele network als de fysieke netwerk. Momenteel krijgt de VM een Ip adres van de DHCP maar dit betekent dat we elke keer een andere IP zullen krijgen wat inhoud dat het moeilijk zijn zijn om een verbinding te maken via SSH omdat het adres elke keer veranderd bij het opstarten van de pc.

---

##Maak een Virtual Switch##
**Zorgt ervoor dat je VM uitgeschakeld is voordat de onderstaande handelingen toegepast wordt**
#####Maak Switch aan#####
>In de optie gedeelte kies je *Create New Virtual Switch*. Daarna kiezen we voor *Extern*, dit is heel belangrijk. Druk vervolgens *virtuele switch maken*
>
>![](virtualSwitch11.png)
>


#####Naam en verbindingstype#####
>Geef het een naam, beste werkwijze is eerste het type virtuele switch(External), dan de naam. Druk op *Toepassen* om het proces te vervolledigen.
>
>![](virtualSwitch21.png)
>
>

#####MAC Address#####
> Ga naar de instellingen van de VM en kies *Netwerkadapter*. Daarna kiest u voor de *Geavanceerde functies*. Zorg ervoor dat *Statisch* is aangeduid onder de optie *MAC-adres*
>
>![](virtualSwitch31.png)
>
>



##Configur Static IP##

#####Start Virtual Machine#####
>We gaan dit keer onze VM niet via SSH verbinden omdat hij een dynamische IP van de DHCP krijgt. Momenteel weten we iet elke adres hij heeft. Start de VM op.
>
![](graylogAan1.png)


#####Login#####
>Log in de VM met de gemaakte gebruiker en wachtwoord.
>
>![](loginserver1.png)
>
>
>
>
>![](loginserver21.png)
>

#####Controleer IP#####
>Om het adres van onze VM te krijgen doen we het commando *ip -a*. Onze Ip is in het onderdeel *eth0* onder de naam *inet*. Het is een adres die door de DHCP gegeven is. Als we onze VM uit en aan doen, zal het een ander adres zijn.
>
>
>![](ipAforIp.png)

#####Zet Static Ip aan#####
>Om zelf een IP aan onze machine te kunnen geven, moeten we eerste static IP inschakelen. Dit moet gebeuren in de network configuratie document.
>
>`root@graylogDebian:vi /etc/network/interfaces`
> 
> Controleer dat het netwerkdocument eruit ziet als het voorbeeld beneden.
> 
> ![](virtualSwitch4.png)


#####Ip instellen#####
>Om het netjes en geordend te houden gaan we de echt IP configuratie in een aparte document steken.
>
> `root@graylogDebian: vi /etc/network/interfaces.d/eth0`
> 
> In deze document stellen we zowel de IP adres, subnet mask en de default gateway in. We kiezen een IP in de DHCP exclude pool, de adressen in de exclude zullen nooit toegekend worden door de DHCP.
> 
> ![](virtualSwitch5.png)


#####Restart network daemon#####
>`root@graylogDebian: service networking restart`
>
>![](virtualSwitch6.png)


#####Controleer netwerk verandring#####
>Nu alles opgeslagen is en de network daemon opnieuw opgestart is, controleren we of de verandering goed door gevoerd is. Onder het deel *eth0* kunnen we onze verandering terug vinden.
>
>`root@graylogDebian : ip a`
>
>![](virtualSwitch7.png)



##SSH login##
>Nu we een vaste IP hebben kunnen we gemakkelijk verbinding maken met die IP.Om dit te doen kun u gebruik maken van elke terminal emulatie, zolang het SSH-connectie ondersteund. Ik gebruik Putty.
>
>![](puttySSH.png)
>
>U kunt inloggen met uw gebruikersnaam en wachtwoord.
>
>![](loginSSH.png)
>
>Dit bevestigt dat onze configuraties goed zijn en connectie met SSH mogelijk is.
>
>![](loginSshSucses.png)
>


##Login met UI##
>Nu de Graylog configuratie in orde zijn en we kunnen inloggen via onze VM en via een andere toestel met gebruik van SSH, gaan we inloggen via de Grafische UI.
>
> **Zorg ervoor dat de VM aan is**
> 
> Surf naar de VM via een web Browser. Hiervoor moeten we naar de IP-adres surfen die we aan de machine gegeven hebben met het poortnummer. **http://10.20.120.9:9000**
> 
> ![](loginserverGraylog.png)
> 
> Bij gebruiker gebruik je **admin** en bij wachtwoord jou **userpassword** van de VM.








