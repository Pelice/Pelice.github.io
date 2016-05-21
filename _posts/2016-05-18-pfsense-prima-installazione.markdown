---
published: true
title: pfSense - Configurazione base Wifi
layout: post
---
Alix3 e pfSense
===============

Un collega mi ha consigliato al posto del solito router wireless casalingo più o
meno economico di fare il vero salto di qualità e passare a un router-wirewall
come pfSense. Per la parte hardware una delle soluzione entry-level / casalinga
migliore sono gli Alix. Ho quindi recuperato un Alix3 con 3 porte lan e wifi
integrato.  
Dopo essermi procurato l'hardware mi sono messo a configurare il software
pfSense. Sono un programmatore ma di rete ne capisco abbastanza e quindi con
pazienza e google mi sono messo all'opera.  
La configurazione di eolo e della rete lan cablata è stata abbastanza facile
anche perchè la configurazione base di pfSense viene molto in aiuto; per la
parte wifi ho penato abbastanza e per questo ho deciso di scrivere questo
articolo.

Configurazione WIFI
-------------------

Una volta configurato tutto le mi interfacce sono queste:

![interfaces](http://Pelice.github.io/public/images/pfSense/interfaces.PNG)

Si può notare subito che la rete wifi ha una subnet diversa a quella della lan.
Prima di arrivare a questa soluzione mi sono bloccato sull'idea di voler
replicare una configurazione simile a un router wireless standard. Con il senno
di poi, fare invece così è molto più semplice e da molta più libertà di
configurazione. Andiamo passo-passo.

**Passo 1 - Creare l'interfaccia OPT1**  
Andare in menu Intefaces -\> (Assign)  
Qui aggiungere una interfaccia sulla scheda ath0 (in genere la scheda wifi)

![assign\_interfaces](http://Pelice.github.io/public/images/pfSense/assign_int_opt.PNG)

**Passo 2 - Configurare l'interfaccia OPT1**

Per comodità inserisco le immagini che sono autoesplicative.

 

Impostazioni:

-   Abilitare l’interfaccia

-   Impostare IPv4 su Static IP

-   Impostare l’ip statico che rappresenta il gateway dell’interfaccia - Fare
    attenzione a impostare la subnet con **/24**

-   Impostare il wifi:

    -   Modalità Access Point

    -   SSID - nome della rete

    -   Enable WME (anche se è opzionale ma obbligatorio per Wifi-N)

![OPT1 general](http://Pelice.github.io/public/images/pfSense/OPT1_general.PNG)

![OPT1 config](http://Pelice.github.io/public/images/pfSense/OPT1_static_ip.PNG)

![OPT1 wifi](http://Pelice.github.io/public/images/pfSense/OPT1_network_config.PNG)

 

**Passo 3 - DHCP server per rete OPT1**

Ora bisogna configurare il dhcp server per la rete wifi.

Services -\> DHCP Server

![](http://Pelice.github.io/public/images/pfSense/DHCPserver.PNG)

Scegliere la rete OPT1 e abilitare il server DHCP.

Impostare il range che si vuole utilizzare e salvare.

 

**Passo 4 - Regole del firewall**

Andare nel menu Firewall -\> Rules e inserire una regola base: permettere tutto
da qualsiasi sorgente a qualsiasi destinazione.

![](http://Pelice.github.io/public/images/pfSense/Firewall_rules.PNG)

Questa è la base, se poi si vogliono abilitare IPv6 o applicare altre regole,
basta inserireprima di questa le altre regole.

A questo punto, la rete wifi dovrebbe comunicare con la WAN, ma non si riesce a
raggiungere la rete LAN. Per farlo, va impostato un bridge tra LAN e OPT1

 

**Passo 5 - Bridge tra LAN e WIFI**

Ultima fase è configurare il ponte tra le due reti.

E’ abbastanza facile:

-   Andare nel menu Interfaces -\> Assign

-   Scegliere la voce Bridges

-   Fare ADD

-   Con il tasto CTRL seleziona le due reti da mettere in Bridge

-   impostare una descrizione

-   SALVARE

 

A questo punto le due reti possono parlare e “vedersi”.

![](http://Pelice.github.io/public/images/pfSense/Bridge2.PNG)

![](http://Pelice.github.io/public/images/pfSense/Bridge_1.PNG)

**TEST**

Dopo i test sulla connessione verso internet, ho fatto un semplice ping al
cellulare e tutto funziona correttamente.

![Test ping](http://Pelice.github.io/public/images/pfSense/ping_wifi.PNG)

 

Ora conviene fare un backup della configurazione e da qui si possono
implementare tutte le regole evolute.
