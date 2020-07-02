# LB2 Loris Cagnazzo
## Inhalstverzeichnis
* [Wissensgewinn](#Wissensgewinn)
* [Shellscript Erklärung](#Shellscript-Erklärung)
* [Shellscript bearbeiten](#Shellscript-bearbeiten)
* [Shellscript starten](#Shellscript-starten)
* [Security](#Security)
* [Ergebnisse](#Ergebnisse)
* [Fazit](#Fazit)


## Wissensgewinn

* Github
  * Noch nie mit Github gearbeitet.
  * Eigenes Repository noch nie erstellt.
  * Auch Git-Befehle habe ich gelernt, auch wenn es auf Visual Studio Code einfacher ist
* Vagrant
  * Auch noch nie mit Vagrant gearbeitet.
    * Vagrant up
    * Vagrant ssh
    * Vagrant destroy
* Visual Studio Code
  * Schon damit gearbeitet
* Markdown File
  * Am Anfang dachte ich es wäre Mühsam, es macht ziemlich Spass.


## Shellscript Erklärung
---
* Das Script startet mit einer for Schleife, die zwei Webserver VMs erstellt. Dabei werden jeweilige Ordner erstellt.
```
for vm in web01 web02
do

    mkdir ${vm} 
    cd ${vm} 
```
* Mit `vagrant.vm.box` wird aus der Cloud eine Vagrant-Box benutzt, namens `ubuntu/xenial64`. 
```
    # Vagrantfile 
    cat <<%EOF% >Vagrantfile
        Vagrant.configure(2) do |config|
          config.vm.box = "ubuntu/xenial64"
```
* Mit dem `config.vm.network` wird ein Portforwarding von Port 80 auf 8080 erstellt.
* Mit `config.vm.synced-folder` werden die Ordner `/var/www/html` und vagrant VM Ordner synchronisiert. Damit kann man anpassungen am index.html File machen ohne auf die VM zu gehen.
* Mit `vb.memory` sagt man wie viel RAM die VM bekommen soll.
```
          config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
          config.vm.synced_folder ".", "/var/www/html"  
        config.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
```
* Hier werden alle Einstellungen der VMs gemacht. Zuerst wird ein Update gemacht.
* Mit `install apache2` wird der Webserver installiert.
* Mit `install ufw` und `ufw enable` wird die Firewall installiert und aktiviert.
* `ufw allow 0.0.0.0/0 to any port 80` erlaubt port 80 von überall aus. 
```
        config.vm.provision "shell", inline: <<-SHELL 
          sudo apt-get update
          sudo apt-get -y install apache2
          sudo apt-get -y install ufw
          sudo ufw enable
          sudo ufw allow from 0.0.0.0/0 to any port 80
          sudo 
        SHELL
        end
```
* Hier wird im `Index.html` File, einen Teil bearbeitet, mit einem Titel(`h1`).
```
# index.html 
    cat <<%EOF% >index.html
    <html>
        <body>
            <h1>Loris Cagnazzo's LB2! ${vm}</h1>
        </body>
    <html>   
```
* Bei den Datenbank VMs ist alles gleich konfiguriert ausser einzelne Dinge.
* `synced.folder` ist auf `var/lib/mysql` gesetzt und der Port ist natürlich auch anderst und auf `3306` gesetzt.
```
    cat <<%EOF% >Vagrantfile
        Vagrant.configure(2) do |config|
          config.vm.box = "ubuntu/xenial64"
          config.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: true
          config.vm.synced_folder ".", "/var/lib/mysql"  
        config.vm.provider "virtualbox" do |vb|
          vb.memory = "256"  
        end
```
* Mit `install mysql-server` wird der Datenbank server installiert.
* Mit `useradd mysql-user` und  wird ein User erstellt.
* Die Firewall wird auch hier installiert und konfiguriert.
* Mit einer Regel erlaubt von überall auf den Port 3306.
```
 config.vm.provision "shell", inline: <<-SHELL 
          sudo apt-get update
          sudo apt-get -y install mysql-server
          sudo useradd -m mysql-user
          sudo apt-get -y install ufw
          sudo ufw enable
          sudo ufw allow from 0.0.0.0/0 to any port 3306
        SHELL
        end
```
## Shellscript bearbeiten
In Visual Studio Code den Ordner öffnen mit dem Script drin. In diesem Fall `C:\Users\Loris Cagnazzo\M300-Service`.
Dann bearbeiten und sichern. Als nächstes wird ein Commit ausgeführt, wenn amn will noch ein Kommentar zur Änderung hinzufügen. Zuletzt noch auf das Git pushen.
## Shellscript starten
>cd M300-Service <br>
>./mm.sh

## Security
* Alle VMs die im Internet stehen sollten gehärtet sein.
* Der Zugriff auf Datenbanken sollte auf die nötigsten Netze beschränkt werden und die Administration nur von einem Bestimmten Netz erlaubt werden.
 
* Die Sicherheit kann im Script ausgebaut werden damit es den eigenen Standards entspricht.
* Dateinübertragungen sollten immer über eine Verschlüsselte Verbindung gemacht werden. 
* Firewall ist so konfiguriert, dass `Port 80` offen ist für alle Webserver und `Port 3306` offen ist für alle Datenbankserver.
* Mit SSH (Secure Shell) wird eine Verschlüsselte Verbindung zu einem entferneten Rechner oder System aufgebaut. Mit dieser verschlüsselten Verbindung kann auf die Commandline zugegriffen werden und Sicher auf anderen Systemen gearbeiet werden.

## Ergebnisse
Die VMs wurden alle erstellt. Die befinden sich im M300-Services. `web01,web02, db01 & db02` sind alle hier.<br>
![Ordner](ordner.png)
[Zum Bild](https://ibb.co/92r6rLV)

* Hier sieht man den den `synced Folder` von `/var/www/html/`.
![Synced-Folder](synced.png)
[Zum Bild](https://ibb.co/pLFyrvz)

So sieht das Vagrantfile einer Datenbank VM aus.
![Synced-Folder](vgfiledb.png)
[Zum Bild](https://ibb.co/6mk05y2)

So sieht das Vagrantfile des Webservers aus.
![Synced-Folder](vgfilews.png)
[Zum Bild](https://ibb.co/M9TWdTG)

## Fazit
Dieses Projekt half mir weiter, da ich bis jetzt noch nie mit Github oder Vagrant gearbeitet hatte. Am Anfang kam ich schlecht vorwärts, da ich noch Mühe hatte mit dem Verstehen von vercshiedenen Dinge, zum Beispiel wie man push ausführt oder was der Unterschied ist zwischen Push und Commit. Mit der Zeit probierte ich immer weiter und konnte immer mehr verstehen. 