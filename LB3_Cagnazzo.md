# M300-Services

* Erklärung vom Code
  * docker.sh
  * dockerstart.sh
  * dockerstop.sh
* Shell Script starten
* Shell Script anpassen
* Sicherheit
* Testing
  * Testfälle
  * Testergebnisse
* Extras
  * WebGUI für Docker
  * Minecraft DockerGUI
* Fazit
* Reflexion
* Wissensgewinn
---

## Erklärung vom Code

### docker.sh

Das Script erstellt einen Docker-Umgebung auf der eine Mysql Datenbank läuft.

Danach wird ein zweite Docker-Umgebung aufgebaut, auf der OSTicket läuft.

Beide Umgebungen werden nach der Erstellung gestartet


```
#!/bin/bash
```

* Mit dem Befehl `#!/bin/bash` wird dem System gesagt das die Umgebung mit der Bash aufegbaut werden soll. Das System weiss das es für dieses Shellscript die Bash verwenden muss. 

```
docker run --name osticket_mysql -d -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=osticket -e MYSQL_PASSWORD=secret -e MYSQL_DATABASE=osticket mysql:5
```

* Mit dieser Zeile wird ein Docker-Container erstellt mit dem Namen `osticket_mysql`. Dieser Docker-Container der erzeugt wird hat schon MySQL instaliert. Angaben wie Datenbank Passwort, MySQL User sowie Datenbank name werden angegeben.  

```
echo "MySQL VM erstellt"
```

* Dem User wird mitgeteilt das die MySQL aufgebaut wurde.

```
docker run --name osticket -d --link osticket_mysql:mysql -p 8080:80 campbellsoftwaresolutions/osticket
```

* Ladet den OSTicket Docker-Container von `campbellsoftwaresolutions` herunter und erstellt diesen Conatiner mit dem Namen `osticket`. Der Port 8080 wird auf den internen Port 80 umgeleitet. Es wird eine Verknüpfung der MySQL Datenbank mit der Docker-Umgebung `osticket_mysql` gemacht. Die `osticket` Umgebung benutzt die MySQL Datenbank der `osticket_mysql` Umgebung. 

---

### dockerstart.sh

Mit diesem Script können die beiden Erstellten Dockerumgebungen gestartet werden, falls diese mal gestoppt sind.

```
#!/bin/bash
```

* Mit dem Befehl `#!/bin/bash` wird dem System gesagt das die Umgebung mit der Bash aufegbaut werden soll. Das System weiss das es für dieses Shellscript die Bash verwenden muss. 

```
docker start osticket_mysql
```

* startet den Docker-Container mit dem Namen `osticket_mysql`.

```
echo "Mysql VM gestartet"
```

* Dem User wird mitgeteilt das der Docker-Container `osticket_mysql` gestartet wurde.

```
docker start osticket
```

* startet den Docker-Conatiner mit dem Namen `osticket`.

```
echo "OSticket VM gestartet"
```

* Dem User wird mitgeteilt das der Docker-Container `osticket` gestartet wurde.

---

### dockerstop.sh

Mit diesem Script können die beiden Dockerumgebungen gestoppt werden. 

```
#!/bin/bash
```

* Mit dem Befehl `#!/bin/bash` wird dem System gesagt das die Umgebung mit der Bash aufegbaut werden soll. Das System weiss das es für dieses Shellscript die Bash verwenden muss.

```
docker stop osticket_mysql
```

* stoppt den Docker-Container mit dem Namen `osticket_mysql`.

```
echo "MySQL VM gestoppt"
```

* Dem User wird mitgeteilt das der Docker-Container `osticket_mysql` gestoppt wurde.

```
docker stop osticket
```

* stoppt den Docker-Container mit dem Namen `osticket`.

```
echo "OSTicket VM gestoppt"
```

* Dem User wird mitgeteilt das der Docker-Container `osticket` gestoppt wurde.

---


## Shell Script starten

* Das Script sollte im Verzeichnis sich befinden und gestartet werden in der man auch die VM möchte, da das Script die VM einfach in dem sich befindende Verzeichnis erstellt und startet.

* Das Shell-Script kann mit 3 Arten gestarte werden:
  
  * Im Verzeichnis in dem sich das Script befindet.
     ```
     ./docker.sh
     ```
  * Absolute Pfad
    ```
    /data/docker/docker.sh
    ```  

  * Pfadvariable
    
    Das Script befindet sich in einem Verzeichnis das in der Pfadvariabel angegeben ist.
    ```
    echo $PATH
    ```
    * Pfadvariable anzeigen

---

## Shell Script anpassen

* Das Script kann für weitere Zwecke angepasst werden. 

* Die Parameter können verändert und auch ergänzt werden. 

---

## Sicherheit



[1]: https://de.wikipedia.org/wiki/H%C3%A4rten_(Computer) "härten"

* Systeme und VM's die im Internet (DMZ) stehe sollten immer [gehärtet][1] sein.

* Der Zugriff auf Datenbanken sollte auf die nötigsten Netze beschränkt werden und die Administration nur von einem Bestimmten Netz erlaubt werden.

* Dateinübertragungen sollten immer über eine Verschlüsselte Verbindung gemacht werden. 

* Die Sicherheit kann im Script ausgebaut werden damit es den eigenen Standards entspricht.

* Mit SSH (Secure Shell) wird eine Verschlüsselte Verbindung zu einem entferneten Rechner oder System aufgebaut. Mit dieser verschlüsselten Verbindung kann auf die Commandline zugegriffen werden und Sicher auf anderen Systemen gearbeiet werden. 


---

## Testing

### Testfälle

* Die Container wurden erzeugt mit den richtigen Namen?

* Die VM's wurden gestartet und die Dienste laufen?

* Die OSTicket Seite ins verfügbar und das Tool kann verwendet werden? 

### Testergebnisse

* mit dem Befehl `docker ps` können alle laufenden Docker Container angezeigt werden. 
  * b3013259433f        campbellsoftwaresolutions/osticket   "docker-php-entrypoi…"   5 days ago          Up About a minute   9000/tcp, 0.0.0.0:8                           080->80/tcp   osticket
  
  * bd222b9b4092        mysql:5                              "docker-entrypoint.s…"   5 days ago          Up 2 minutes        3306/tcp, 33060/tcp

* beide Container sind erfolgrecih erstellt worden und laufen. 

* Mit der Adresse des Servers `192.168.122.13:8080/scp/` wird die Login Seite von OSTicket angezeigt. 
---


## Extras

### WebGUI für Docker

* Es ist möglich ein Docker-Container herunterzuladen der ein WebGUI für die Administration von Docker-Container erzeugt. 

```
docker run -d -p 9000:9000 --privileged -v /var/run/docker.sock:/var/run/docker.sock uifd/ui-for-docker
```

* das WebGUI das von diesem Conatiner erzeugt wird kann mit der Server Adresse auf einem Browser angeschauit werden. `http://192.168.122.15:9000/` Der Port kann vor der installation nach Wunsch geändert werden.  


### Minecraft DockerGUI

* Mit Dockercraft kann die Docker-Umgebung mit Minecraft Visualisiert werden und auch gesteuert werden. Dockercraft erzeugt einen Java Minecraft Server der mit der Dockerumgebung verbunden ist. 

* Dies ist zwar für die meisten unütz zeigt aber was mit Docker und Container alles möglich ist und das auf jeder Umgebung. Es ist nicht Hardware abhängig. Die Container laufen bei jedem User der Docker installiert hat.

* Dies wurde auf dem PC von Robin gemacht da ich keine Vollversion von Minecraft habe und nicht extra Geld ausgeben wollte.
---


## Fazit

* Mit Docker können wie mit Vagrant schnell Umgebungen Aufegabut werden. Diese Umgebung können selbst oder von anderen erstellt worden sein. 

* Mit Docker sind die möglichekiten fast schon grenzenlos.

* Bei Docker-Container die von anderen erstellt wurden kann es zu Problemen kommne um etwas im Imnage zu verstehen oder zu ändern.

* Mit Container können Umgebung in Sekunden schnelle auf jedem System repliziert werden und jeder kann dies Nutzen der auch Docker besitzt. Dies ist auch bei Forschungen sehr nützlich, da so jemand von China die Umgebung von einem Schweizer Forschungsteam in sekunden schnelle 1zu1 replizieren kann. 
 

---

## Relflexion

Ich habe viel über Git gelernt und mich mit dem Thema ausanander gesetzt. Mit Visual Studio Code habe ich das ganze Prozedere schon ziemlich gut im Griff um Sachen zu pushen und stagen. Ich finde Git macht in vielen Anwendungsbereichen viel sinn und werde versuchen dies in Zukunft mehr zu gebrauchen und es auch im Geschäft zu Pushen. Das Projekt mit der Vagrantbox hat mir auch viel gelernt. Docker ist sehr änhlich wie vagrant, die Befehle und der Aufabue der Docker und Vagrantfiles sind anderst. Durch den eher knappen Zeitraum haben wir nicht einene Riesen Umgebung aufbauen können. Der sinn von Docker ist auch das nicht jeder Informatiker eine Umgebung selber aufbauen muss, sondern das man auf der ganzen Welt vom Wissen von anderen Profitiert und diese Umgebungen selber nutzen kann und dies haben wir gemacht. Wir haben uns versucht so schnell eine OSTicket Umgeung aufzubaune, welche auchs ehr gut funktioniert hat. Zuerst hatten wir schwierigkeiten die Umgebung zum laufen zu bringen, da Docker auf Windows nicht so gut funktioniert. Auch die Umstellung von Vagrant auf Docker hat seine Zeit gebraucht. Die Tutorials über Docker die im Github angelegt sind haben mir aber sehr geholfen dies zu verstehen. Mit der TBZ Linux umgebung hat alles sehr gut funktioniert und ich kann dies nur empfehlen.

---

## Wissensgewinn

Docker sowie Vagrant Umgebungen aufzubauen und diese selbst anpassen. Ich habe viel über Container gelernt und selbst gesehen welche Vorteile dies gegen den normalen VM hat. Ich nehme aus diesem Modul viel mit in meine Zukunft und versuchte das gelernte die angeschnittenen Themen in der Praxis und im Arbeitsleben einzufliessen und anzuwenden. 

---