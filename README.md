# Qt-how-to
Rassemble toutes mes connaissances autour de Qt

# Gratuité
En gros, gratuit si on ne produit pas de projets qui nous rapporte de l'argent (=> colle parfaitement à l'Open Source), sinon payant

# Exécuter application avec un autre Framwork Qt
Partons d'un exemple : Beebeep 5.8.2 nécessite Qt5, or sous mon Ubuntu Xenial 16.04 la version installé était Qt4 et impossible d'installer la version qt5 (je ne sais pas vraiment pourquoi)

Solution : 
1. installer Qt5 dans /data/noinstall (voir procédure pour installer manuellement un framework)
2. préciser à l'exécution quelle version utiliser avec la variable d'environnement LD_LIBRARY ([détails ici]() : 

```sh
LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib beebeep &
```
Nota : depuis un lanceur Desktop on ne peut utiliser la ligne telle quelle ("Valider" est grisé)

exec=env LD_LIBRARY_PATH=/data/noinstall/Qt5.9.1/5.9.1/gcc_64/lib /usr/bin/beebeep

# Installer manuellement un Framework Qt pour faire cohabiter plusieurs versions de Qt

Source des téléchargements : https://www.qt.io/offline-installers

1. Télécharger l'installeur voulu (ici qt-opensource-linux-x64-5.9.1.run)
2. Rendre l'installeur exécutable : chmod +x qt-opensource-linux-x64-5.9.1.run
3. Exécuter l'installeur (nécessite un compte Qt, qu'on pourra créer au besoin) par la commande "./qt-opensource-linux-x64-5.9.1.run" et installer dans /data/noinstall

Nota : à la date de rédaction de cette procédure il existe un installeur plus récent "qt-opensource-linux-x64-5.12.10.run" mais il provoque une erreur qui semble minime que qt-opensource-linux-x64-5.9.1.run ne produit pas
