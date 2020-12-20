# Qt-how-to
Rassemble toutes mes connaissances autour de Qt

# Gratuité
En gros, gratuit si on ne produit pas de projets qui nous rapporte de l'argent (=> colle parfaitement à l'Open Source), sinon payant

# Exécuter application avec un autre Framwork Qt
Partons d'un exemple : Beebeep 5.8.2 nécessite Qt5, or sous mon Ubuntu Xenial 16.04 la version installé était Qt4 et impossible d'installer la version qt5 (je ne sais pas vraiment pourquoi)

Solution : 
1. installer Qt5 dans /data/noinstall (voir procédure pour installer manuellement un framework)
2. préciser à l'exécution quelle version utiliser avec la variable d'environnement LD_LIBRARY ([détails ici](https://forum.qt.io/topic/55100/linux-run-program-if-install-two-version-qt/3) et [ici](http://www.linuxcertif.com/doc/keyword/LD_LIBRARY_PATH/)): 

```sh
LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib beebeep &
```
Nota : depuis un lanceur Desktop on ne peut utiliser la ligne telle quelle ("Valider" est grisé), il faut [précéder par env](https://askubuntu.com/questions/144968/set-variable-in-desktop-file/144971#144971) pour définir l'environnement

```sh
cat /usr/share/applications/beebeep.desktop
...
Exec=env LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib /usr/bin/beebeep
...
```

# Installer manuellement un Framework Qt pour faire cohabiter plusieurs versions de Qt

Source des téléchargements : https://www.qt.io/offline-installers

1. Télécharger l'installeur voulu (ici qt-opensource-linux-x64-5.9.1.run)
2. Rendre l'installeur exécutable : chmod +x qt-opensource-linux-x64-5.9.1.run
3. Exécuter l'installeur (nécessite un compte Qt, qu'on pourra créer au besoin) par la commande "./qt-opensource-linux-x64-5.9.1.run" et installer dans /data/noinstall

Nota : à la date de rédaction de cette procédure il existe un installeur de Qt5 plus récent "qt-opensource-linux-x64-5.12.10.run" mais il provoque une erreur qui semble minime et qui n'est pas produit par qt-opensource-linux-x64-5.9.1.run
