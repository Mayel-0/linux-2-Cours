# linux-2-Cours

## ip en binaire

    10.33.70.220

    00001010.00100001.01000110.11011100

# I. Exploration locale en solo

## Interface WiFi

_info école_

- Nom : **Wifi@ynov**
- mac adresse : **6a:b9:52:fd:21:1f**
- addresse ip : **10.33.70.220**
- addresse réseaux : **10.33.64.0**
- addresse broadcast : **10.33.79.255**

## Interface Ethernet

_pas de port ethernet donc pas de résultat_

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

_pas de port ethernet_

## III. Manipulations d'autres outils/protocoles côté client

### DHCP

_info école_

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

**_BONUS_**

---

la table arp ou (Address Resolution Protocol) est une table utiliser par un switch ou tout autre appareille pour cartographier le réseaux au debut si on utilise un swith pour l'explication ce dernier vas recevoire des paquet de données il vas les recevoir de quelqu'un ce dernier vas alors données sont ip et surtout ca mac addresse (sont addresse physique) eensuite il vas transmettre ceux paquet de données et demande qui est <mac addresse de destination du paquet> (en broadcast) il vas faire ca sur tout les communication jusqu'a avoir la mac addresse de tout les apparaille qu'il vas remplir au fur est a mesure dans ca table arp pour ensuite au lieux de a chaque fois demander qui est le destinataire si il a deja enregistré le destinataire dans ca table arp avant il vas juste transmettre directement les données en les associent a leurs ports.

mac addresse = addresse physique

# TP DUO

### configurattion réseaux

voici les information du PC1 et PC2

**_PC-1_**

<img src="../TP-2/tp duo/config réseau mael.png" height="60%" widgth="60%"/>

**_PC-2_**

<img src="../TP-2/tp duo/config réseau mathis.png" height="60%" widgth="60%"/>

### communication et initialisation router(passerelle)

        Intrnet           Internet
          X                   |
          X                 WiFi
          |                   |
        PC 1 ---Ethernet--- PC 2

le principe c'est que le PC1 doit mettre le PC2 en passerelle

pourquoi ?

car c'est part lui qu'il doit passer pour obtenire internet

pour ce faire on doit le mettre dans le reglage du réseaux puis le PC-1 (sous macOS) doit activer le partage de connecter par ethernet en magie le PC-1 passe par PC-2 pour obtenire internet

<img src="../TP-2/tp duo/ping mathis vers mayel.png" height="60%" widgth="60%"/>

<img src="../TP-2/tp duo/ping mathis vers google.png" height="60%" widgth="60%"/>

voici les deux ping montrant que la communication sur le pc deux fonctionne.

### Chat privée

maintenant on doit utiliset netcat (nc) pour pouvoire communiquer entre pc pour cela un des deux pc doit ouvrire et ecouter sont port :8888 ou autre

        nc -l 8888

et l'autre pc doit mettre sont addresse ip et envoyer la communication vers le port choisi ici 8888

        nc 192.168.2.1 8888

la PC-1 avec la premier commande lance une écoute sur le port 8888 et le PC-2 avec la deuxieme commande ouvre une communication avec l'ip choisi ici 192.168.2.1 et le port choisi ici 8888 avec ca on peut communiquer comme un vrai chat privé

<img src="../TP-2/tp duo/chat privée.png" height="80%" widgth="80%"/>

### Wireshark

voici les tests et l'affichage des communication

**_ping classique_**

<img src="../TP-2/tp duo/W ping.png" height="80%" widgth="80%"/>

on voit (echo request reply) du protocole Ping entre les deux pc

**_chat pivée_**

<img src="../TP-2/tp duo/w chat privée.png" height="80%" widgth="80%"/>

on peut même voir le message recu dans la parti data du paquet intercepter par Wirshark "bonjour je suis mathis"

### Firewall

ici on doit activer le firewall allouer un port pour nous 12345 pour la communication entre pc via nc

ensuite avec des ligne de commande autoriser certaine communications

et ensuite faire le teste de communcation

on peut aussi faire les commande de verification pour voie si tout est bien rentre dans les parametre firewall et voir si ca fonctionne 

<img src="../TP-2/tp duo/fire wall.png" height="80%" widgth="80%"/>
