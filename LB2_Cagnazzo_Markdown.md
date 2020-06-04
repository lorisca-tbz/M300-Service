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
        config.vm.provision "shell", inline: <<-SHELL 
          sudo apt-get update
          sudo apt-get -y install apache2
        SHELL
        end
%EOF%
$ ls -l

```

Code kann auch `so hervor gehoben` werden

> Backquote


[1]: http://sbb.ch "sbb"
[2]: http://tbz.ch "Technischen Berufsschule Zürich"

Die meisten Lernenden der [TBZ][2] kommen mit der [Bahn][2] zur Schule
