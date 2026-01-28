# linux-2-Cours

## ip en binaire 

    10.33.70.220

    00001010.00100001.01000110.11011100



# I. Exploration locale en solo


## Interface WiFi 

*info école*

- Nom : **Wifi@ynov**
- mac adresse : **6a:b9:52:fd:21:1f**
- addresse ip : **10.33.70.220**
- addresse réseaux : **10.33.64.0**
- addresse broadcast : **10.33.79.255**

## Interface Ethernet 

*pas de port ethernet donc pas de résultat*


**commande macos :**

    route -n get default


résultat :

    gateway : 192.168.1.254


**commande macos :**

    ifconfig

résultat : 

    <adresse ip, mac, dns, etc.. équivalent de ipconfig/all sur windows>

## En graphique (GUI : Graphical User Interface)

sur macos il faut :


aller sur l'icon wifi en haut a droit de vtre pc clicker sur le logo wifi ensuite clicker sur (réglage wifi) puis sur (details) de votre réseaux wifi pour ensuite avoir tout les details de votre réseaux gateway, dns, ip, réseaux etc...


**gateway :**
la gateway / passerelle c'est un port que l'on utilise sur notre router pour sortire de notre réseaux par exemple pour ping 8.8.8.8 on doit sortire de notre réseaux pour aller chercher cette addresse ip.


## Modifier c'est information :

Notre masque est (école):

    255.255.240.0

donc on est dans une classe C
donc le calcule est de 12^2-2=4094 adresse disponible.
la Premier est: 

    10.33.64.1

la dernier est :

    10.33.79.254

Addresse modifier (perso):

    10.33.70.200

## Nmap

pour scanner le réseaux avec nmpa on peut faire 

    nmap -sn -PE 192.168.1.255

    nmap -sL 1992.168.1.255



Addresse modifier (perso):

    10.33.70.200


pour cela on doit desactiver le protocole DHCP qui automatiquement asigne a tout appareille connecter au réseaux une addresse ip il faut ce passer en manuel pour pouvoir ce donner nous même une addresse ip.

si on modifie la passerelle, on ne peut plus acceder a internet car on ne peut plus sortire du réseaux LAN on est bloquer a l'interrieur de requete ne peuvent pas aller plus loin que notre router plus de ping 8.8.8.8 (google)

II. Exploration locale en duo

*pas de port ethernet*

## III. Manipulations d'autres outils/protocoles côté client

### DHCP


*info école*

- Nom : **Wifi@ynov**
- mac adresse : **6a:b9:52:fd:21:1f**
- addresse ip : **10.33.70.220**
- addresse réseaux : **10.33.64.0**
- addresse broadcast : **10.33.79.255**

sur macos on na pas vraiment le debut et la fi du bail ip on a ce résultat 

    lease_time (uint32): 0x15180

généralement les bails ip sur des box internet classique sans modification dure 24h 

la protocole DHCP come explique au dessue est un protocole qui addresse automatique une ip a chaque appareille qui ce connecte a un réseaux avec donc un bail ip, tout debant la classe de notre réseaux ou le CIDR (classe less) on peut avoir un nombre d'hotes différent donc un nombre d'addresse disponible différent, moins l'addresse du réseaux, moin l'addresse du Broadcast on peut aussi indique une plage d'addresse disponible, il peut y avoir aussi des ip statique pour des apparaille spéciaux comme des caméra, alarme ou autre.

commande macos pour changer d'addresse ip: 

    sudo ipconfig set en0 DHCP

### DNS

le protocol DNS est un protocle qui permete de traduire une addresse ip en nom de domaine et inversement elle peut même traduire et assimiler plusieurs addresse ip a une seul ou plusieur nom de domaine comme pour google.com par exemple grace a ce protocole on peut faire la commande (ping google.com) qui sera fonctionnel pusique que ce protcole vas lui même traduire ca en addresse ip.

router (box bouyge telecom) = 192.168.1.255


    Server:		2001:861:2c20:2940:ba8c:2bff:fe0d:6d14
    Address:	2001:861:2c20:2940:ba8c:2bff:fe0d:6d14#53
    
    nslookup Ynov.com 
    
    =

    Non-authoritative answer:
    Name:	ynov.com
    Address: 104.26.11.233
    Name:	ynov.com
    Address: 104.26.10.233
    Name:	ynov.com
    Address: 172.67.74.226
    nslookup google.com
    
    =

    Non-authoritative answer:
    Name:	google.com
    Address: 142.251.209.174

**on vas traduire tout cela:**

- deja au debut on a notre addresse réseaux et notre addresse ip en IPV6
- le nombre avec le # signifie le nombre du port utilise dans notre cas #53 donc la DNS
- le "Non-authoritative answer" signifie que c'est pas une réponse du service officiel / principale de google ou ynov (question de sécurité)
- ensuite on a le nom de domaine et l'addresse ip traduit de ce nom de domaine

le reverse lookup c'est le procéder inverse c'est-a dire utiliser non pas le DNS (nom de domain) mais bien une addresse ip 

    nslookup 78.78.21.21

    =
    
    Server:		2001:861:2c20:2940:ba8c:2bff:fe0d:6d14
    Address:	2001:861:2c20:2940:ba8c:2bff:fe0d:6d14#53
    
    Non-authoritative answer:
    21.21.78.78.in-addr.arpa	name = host-78-78-21-21.mobileonline.telia.com.

    nslookup 92.16.54.88

    =
    
    Server:		2001:861:2c20:2940:ba8c:2bff:fe0d:6d14
    Address:	2001:861:2c20:2940:ba8c:2bff:fe0d:6d14#53
    
    Non-authoritative answer:
    88.54.16.92.in-addr.arpa	name = host-92-16-54-88.as13285.net.
    
    Authoritative answers can be found from:

**on vas traduire tout cela:**

- deja au debut on a notre addresse réseaux et notre addresse ip en IPV6
- le nombre avec le # signifie le nombre du port utilise dans notre cas #53 donc la DNS
- le "Non-authoritative answer" signifie que c'est pas une réponse du service officiel / principale de google ou ynov (question de sécurité)
- ensuite on a le nom de domaine et l'addresse ip traduit de ce nom de domaine

Format inversé pour chercher le PTR.
Nom de domaine associé (souvent fourni par le FAI).

***BONUS***
____________

la table arp ou (Address Resolution Protocol) est une table utiliser par un switch ou tout autre appareille pour cartographier le réseaux au debut si on utilise un swith pour l'explication ce dernier vas recevoire des paquet de données il vas les recevoir de quelqu'un ce dernier vas alors données sont ip et surtout ca mac addresse (sont addresse physique) eensuite il vas transmettre ceux paquet de données et demande qui est <mac addresse de destination du paquet> (en broadcast) il vas faire ca sur tout les communication jusqu'a avoir la mac addresse de tout les apparaille qu'il vas remplir au fur est a mesure dans ca table arp pour ensuite au lieux de a chaque fois demander qui est le destinataire si il a deja enregistré le destinataire dans ca table arp avant il vas juste transmettre directement les données en les associent a leurs ports. 

mac addresse = addresse physique 
