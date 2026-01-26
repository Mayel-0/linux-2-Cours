# linux-2-Cours

## ip en binaire 

    10.33.70.220

    00001010.00100001.01000110.11011100



# I. Exploration locale en solo


## Interface WiFi 

*info perso*

Nom : **box bouygues telecom**
mac adresse : **d6:21:78:d3:cd:c2**
addresse ip : **192.168.1.147**
addresse réseaux : **192.168.1.254**
addresse broadcast : **192.168.1.255**

*info école*

Nom : ****
mac adresse : ****
addresse ip : ****
addresse réseaux : ****
addresse broadcast : ****

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

Notre masque est (perso):

    255.255.255.0

donc on est dans une classe C
donc le calcule est de 8^2-2=254 adresse disponible.
la Premier est: 

    192.168.1.1

la dernier est :

    192.168.1.254

Addresse modifier (perso):

    192.168.1.200

## Nmap

pour scanner le réseaux avec nmpa on peut faire 

    nmap -sn -PE 192.168.1.255

    nmap -sL 1992.168.1.255



Addresse modifier (perso):

    192.168.1.200


pour cela on doit desactiver le protocole DHCP qui automatiquement asigne a tout appareille connecter au réseaux une addresse ip il faut ce passer en manuel pour pouvoir ce donner nous même une addresse ip.

si on modifie la passerelle, on ne peut plus acceder a internet car on ne peut plus sortire du réseaux LAN on est bloquer a l'interrieur de requete ne peuvent pas aller plus loin que notre router plus de ping 8.8.8.8 (google)


