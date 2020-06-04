# LB2 Loris Cagnazzo

## Wissensgewinn

das ist **fett**, das ist *kursiv* das ist ***fett und kursiv** und jetzt wider normal.

* erstens
* zweitens
  * Einrücken

## Shellscript Erklärung
---

```
for vm in web01 web02
do

    mkdir ${vm} --> Erstellt ein Verzeichnis für die neuen Webserver-VMs.
    cd ${vm} --> Wechselt in das neu erstellte Verzeichnis.
    
    # Vagrantfile 
    cat <<%EOF% >Vagrantfile
        Vagrant.configure(2) do |config|
          config.vm.box = "ubuntu/xenial64"
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
