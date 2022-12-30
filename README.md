RO en attente de la fin de migration






































# Qt-how-to
Rassemble toutes mes connaissances autour de Qt

# Gratuité
En gros, gratuit si on ne produit pas de projets qui nous rapporte de l'argent (=> colle parfaitement à l'Open Source), sinon payant

# Exécuter application avec un autre Framwork Qt
Partons d'un exemple : Beebeep 5.8.2 nécessite Qt5, or sous mon Ubuntu Xenial 16.04 la version installé était Qt4 et impossible d'installer la version Qt5 (je ne sais pas vraiment pourquoi)

Solution : 
1. installer Qt5 dans /data/noinstall (voir procédure pour installer manuellement un framework)
2. préciser à l'exécution quelle version utiliser avec la variable d'environnement LD_LIBRARY ([détails ici](https://forum.qt.io/topic/55100/linux-run-program-if-install-two-version-qt/3) et [ici](http://www.linuxcertif.com/doc/keyword/LD_LIBRARY_PATH/)): 

```sh
LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib beebeep &
```
Nota 1 : depuis un lanceur Desktop on ne peut utiliser la ligne telle quelle ("Valider" est grisé), il faut [précéder par env](https://askubuntu.com/questions/144968/set-variable-in-desktop-file/144971#144971) pour définir l'environnement

```sh
cat /usr/share/applications/beebeep.desktop
...
Exec=env LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib /usr/bin/beebeep
...
```
Nota 2 : à la date de rédaction de cette procédure il existe un installeur de Qt5 plus récent "qt-opensource-linux-x64-5.12.10.run" mais il provoque une erreur qui semble minime et qui n'est pas produit par qt-opensource-linux-x64-5.9.1.run

# Compiler un projet en précisant la version de Qt
Partons d'un exemple : QPXSee 7.31 nécessite Qt5 (5.10.1 pour toutes les fonctionnalités, or sous mon Ubuntu Xenial 16.04 la version installé était Qt4 et impossible d'installer la version Qt5 (je ne sais pas vraiment pourquoi). De plus, il n'existe pas de version opérationnelle juste à décompresser, mais uniquement un paquet DEB (dpkg -i DEB) qui indique des problèmes de dépendances non résolues (et que je ne parviens pas à résoudre). Donc ma seule solution c'est de compiler.

Avertissement : cette procédure n'est certainement pas la meilleure, ni celle recommandée, mais comme c'est la seule qui fonctionne pour moi, je la conserve.

Pour satisfaire aux conditions, j'ai installé Qt v5.12.10 dans /data/noinstall (voir procédure pour installer manuellement un framework)

Voici ce que j'ai fait pour permettre la compilation (basé sur [ceci](https://unix.stackexchange.com/questions/116254/how-do-i-change-which-version-of-qt-is-used-for-qmake/427366#427366)):
1. Renommer la version par défaut : 
```sh
root@host:~# mv /usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf /usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf_orig
```
2. Créer sa version par défaut : 
```sh
root@host:~# cat /usr/share/qtchooser/my_Qt5.12.10_Desktop_gcc_x64.conf
/data/noinstall/Qt5.12.10/5.12.10/gcc_64/bin
/data/noinstall/Qt5.12.10/5.12.10/gcc_64/lib
```
3. Rendre sa version comme étant celle par défaut
```sh
root@host:~# ln -s /usr/share/qtchooser/my_Qt5.12.10_Desktop_gcc_x64.conf /usr/lib/x86_64-linux-gnu/qt-default/default.conf
```

Pour compiler à partir du framework voulu (basé sur [procédure officielle](https://github.com/tumic0/GPXSee)) :
```sh
user@host:~$ /data/noinstall/Qt5.12.10/5.12.10/gcc_64/bin/lrelease gpxsee.pro
user@host:~$ /data/noinstall/Qt5.12.10/5.12.10/gcc_64/bin/qmake gpxsee.pro
user@host:~$ make
```
Nota : l'application est bien compilée, mais à l'exécution j'ai des erreurs et les cartes ne sont pas ajoutées (entre autres)
```ssh
user@host:/data/noinstall/GPXSee-7.31$ LD_LIBRARY_PATH=/data/noinstall/Qt5.12.10/5.12.10/gcc_64/lib ./gpxsee 
No ellipsoids file found.
No GCS file found.
Maps based on a datum different from WGS84 won't work.
No PCS file found.
qt.network.ssl: QSslSocket: cannot resolve OPENSSL_init_ssl
...
qt.network.ssl: Incompatible version of OpenSSL
```
DONC : semi-réussite/echec (50/50)

# Installer manuellement un Framework Qt pour faire cohabiter plusieurs versions de Qt
Source des téléchargements : https://www.qt.io/offline-installers

1. Télécharger l'installeur voulu (ici qt-opensource-linux-x64-5.9.1.run)
2. Rendre l'installeur exécutable : chmod +x qt-opensource-linux-x64-5.9.1.run
3. Exécuter l'installeur (nécessite un compte Qt, qu'on pourra créer au besoin) par la commande "./qt-opensource-linux-x64-5.9.1.run" et installer dans /data/noinstall

# Installer trebleshot Bullseye
Source : https://github.com/trebleshot/desktop
Permet depuis le LAN de transférer des fichiers entre un smartphone et un autre hôte.

```sh
mkdir -p /data/noinstall
cd /data/noinstall
apt update
apt install -y vim htop locate less aptitude wget gawk man sshfs rsync tree curl net-tools gnupg2 rfkill util-linux nmap tcpdump binutils git build-essential
apt install -y cmake qtbase5-dev libkf5dnssd-dev
git clone https://github.com/trebleshot/desktop.git
mv desktop trebleshot
cd trebleshot/
git submodule update --init
./build_trebleshot.sh
file cmake-build-release/trebleshot
chown -R USER: /data
```
Dépuis session utilisateur USER :
```sh
/data/noinstall/trebleshot/cmake-build-release/trebleshot
```

