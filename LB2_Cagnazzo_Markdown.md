# LB2 Loris Cagnazzo
## Inhalstverzeichnis
* Wissensgewinn (#Wissensgewinn)
* Shellscrip Erklärung 
* Shellscript bearbeiten
* []Shellscript starten


## Wissensgewinn

Alle was in diesem Kapitel fest steht ***fett und kursiv** und jetzt wider normal.

* Github
  * Noch nie mit Github gearbeitet.
  * Eigenes Repository noch nie erstellt.
  * 
* Vagrant
  * Einrücken

## Shellscript Erklärung
---
* Das Script startet mit einer for Schleife, die zwei Webserver VMs erstellt. Dabei werden jeweilige Ordner erstellt.
```
for vm in web01 web02
do

    mkdir ${vm} --> Erstellt ein Verzeichnis für die neuen Webserver-VMs.
    cd ${vm} --> Wechselt in das neu erstellte Verzeichnis.
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
```

```

Code kann auch `so hervor gehoben` werden

> Backquote


[1]: http://sbb.ch "sbb"
[2]: http://tbz.ch "Technischen Berufsschule Zürich"

Die meisten Lernenden der [TBZ][2] kommen mit der [Bahn][2] zur Schule
