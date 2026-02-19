# TP – Administration SSH et Serveur Web Nginx

## Partie 1 – Mise en place de l’environnement virtualisé

pour tester si on recoit bien une ip et si on est connetcer a internet on fait

    ip a
    ping 8.8.8.8

<img src="../TP-4/image/test connection.png" height="60%" widgth="60%"/>

l'ip que ma VM a est 192.168.11.2/24

<img src="../TP-4/image/ping mac vers vm.png" height="60%" widgth="60%"/>

on peut ping la vm

ca fonctionne on peut commencer le tp.

## Partie 2 – Serveur SSH

on install ssh

    sudo apt install openssh

puis on starts / verifie starts

    rc-service sshd start
    rc-service sshd status

<img src="../TP-4/image/start ssh.png" height="60%" widgth="60%"/>

## Partie 3 – Sécurisation SSH

on doit modifier le fichier config de ssh pour modifier le port le mots de pas pour pouvoire crée une clef

    vi /etc/ssh/sshd_config

<img src="../TP-4/image/sshconfig.png" height="60%" widgth="60%"/>

voici les modfification apporter a la config ssh

    Port 2222
    PermitRootLogin no
    PasswordAuthentication no
    PubkeyAuthentication yes

pour genere clef sur macOs

    ssh-keygen -t rsa -b 4096 -C "tp-alpine"

pour copier sur le vm

    ssh-copy-id -p 2222 etudiant@192.168.11.2

puis on ajoute un alias pour une connection plus simple

    nano ~/.ssh/config

ensuite on peut tester et ca fonctionne

<img src="../TP-4/image/alias.png" height="60%" widgth="60%"/>
<img src="../TP-4/image/coco.png" height="60%" widgth="60%"/>

## Partie 4 – Transfert de fichiers

### SCP

pour ce faire on doit utiliser la commande

    scp -P 2222 fichier.txt etudiant@192.168.11.2:/home/etudiant/

(-P pour le port )

<img src="../TP-4/image/envoiefichier.png" height="60%" widgth="60%"/>

et si on affiche le contenue du fichier on le retrouve bien dans notre vm

    cat /home/etudiant/fichier.txt

<img src="../TP-4/image/testsshfichier.png" height="60%" widgth="60%"/>

### SFTP

on doit donc ce connecter en SFTP a notre VM

    sftp -P 2222 etudiant@192.168.11.2

    put fichier2.txt          # Upload fichier local → distant
    get fichier.txt           # Download fichier distant → local
    ls                       # Vérifier distant
    lls                      # Vérifier local

pour recupere un fichier ou autre j'ai donc utiliser la commande

    get fichier.txt <rename>

<img src="../TP-4/image/sftp.png" height="60%" widgth="60%"/>

<img src="../TP-4/image/fichiervm.png" height="60%" widgth="60%"/>

j'obtient mon fichier

### RSYNC

## Partie 5 – Analyse des logs et sécurité

    tail -f /var/log/messages | grep sshd

cette commande affiche les logs ssh

ensuite on doit passer en super users (su root) pour modifier encore une fois le fichier sshd_config et mettre ca

<img src="../TP-4/image/logsshconfig.png" height="60%" widgth="60%"/>

on ajout

    LogLevel VERBOSE
    SyslogFacility AUTH

<img src="../TP-4/image/testerreurssh.png" height="60%" widgth="60%"/>

on configure

    cat > /etc/fail2ban/jail.d/alpine-ssh.conf << 'EOF'
    [sshd]
    enabled = true
    filter = alpine-sshd
    port = 2222
    logpath = /var/log/messages
    maxretry = 3
    bantime = 3600

    [sshd-ddos]
    enabled = true
    filter = alpine-sshd-ddos
    port = 2222
    logpath = /var/log/messages
    maxretry = 2
    EOF

j'affiche les bannissement

<img src="../TP-4/image/testban.png" height="60%" widgth="60%"/>

### Partie 6 – Tunnel SSH

on ouvre le tunnel local avec cete commande

    ssh -p 2222 -L 8080:localhost:8080 etudiant@192.168.11.2

<img src="../TP-4/image/8080test.png" height="60%" widgth="60%"/>

### Partie 7 – Nginx et HTTPS

voici les commande pour installer nginx

    apk add nginx openssl
    mkdir -p /var/www/site-tp

crée un site teste

    cat > /var/www/site-tp/index.html << 'EOF'
    <!DOCTYPE html>
    <html>
    <head><title>TP SysAdmin - Site Test</title></head>
    <body>
    <h1>🎉 TP Réussi ! Nginx + HTTPS fonctionnel</h1>
    <p>Parties 6-9 validées ✅</p>
    </body>
    </html>
    EOF

on doit genere maintenant une certification auto asigné

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -subj "/C=FR/ST=Nouvelle-Aquitaine/L=LeBouscat/O=TP/CN=192.168.11.2"

configuration de nginx

    cat > /etc/nginx/http.d/site-tp.conf << 'EOF'
      server {
        listen 80;
        server_name 192.168.11.2;
        root /var/www/site-tp;
        index index.html;

        # Redirection HTTP → HTTPS
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl;
        server_name 192.168.11.2;
        root /var/www/site-tp;
        index index.html;

        ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
        ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
        ssl_protocols TLSv1.2 TLSv1.3;
    }
    EOF

ensuite apres tout cette configuration ou lance nginx

    rc-update add nginx default
    rc-service nginx start
    nginx -t && echo "✅ Config OK"

ensuite on peut tester depuis notre ordinateur si on a bien la page.

<img src="../TP-4/image/testsite.png" height="60%" widgth="60%"/>


## 
