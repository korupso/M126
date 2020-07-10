# NAS und SMB  
## NTFS Installation  
Als erstes muss das Windows Dateisystem NTFS funktionieren mit dem RaspberryPI, welches auf Linux läuft.  
Dies kann man mit ntfs-3g gemacht werden, welches einfach installiert werden kann.  
```bash
sudo apt-get install ntfs-3g
```  

---  
## Ordner Mounten  
Als erstes muss man den Ordner erstellen, welcher man an das Speichergerät anbinden will.
```bash
sudo mkdir /home/pi/smb
```  
Mit diesem Befehl kann man alle Speichergeräte anzeigen lassen.  
```bash
fsdik -l
```
Standart heisst das Gerät, das wir brauchen "/dev/sda1".  
Mit dem unteren Befehl kann man den, vorhin erstellten, Ordner anbinden an das Speichergerät.  
```bash
sudo mount -t auto /dev/[STICKNAME] /home/pi/smb
```  

---  
## Samba Installation  
Samba ist ein Tool, mit dem man NAS verwalten kann.  
Installiert ist es ganz einfach mit diesem Befehl:
```bash
sudo apt-get install samba samba-common-bin
```  

---  
## Samba Konfiguration  
Die Konfiguration kann ganz einfach übersprungen werden mit diesem Befehl:
```bash
sudo echo "[Raspberry]
comment = Raspberry Pi
path = /home/pi/smb
valid users = @users
force group = users
create mask = 0660
directory mask = 0771
read only = no" >> /etc/samba/smb.conf
```  

---  
## Samba Starten  
Samba muss für die folgenden Schritte gestartet werden.  
Um allfällige Probleme zu vermeiden, machen wir einen Neustart bei Samba.  
Restart bedeutet bei Linux fast immer, dass das Programm einfach geschlossen wird, falls irgendeine Version noch offen ist.
```bash
sudo /etc/init.d/samba restart
```  

---  
## NAS Benutzer Erstellen  
Um nicht den Hauptbenutzer des RaspberryPIs für das NAS zu benutzen, sollte man einen Benutzer extra für das NAS erstellen.  
Zudem will der Auftrag, dass wir auch einen zweiten Benutzer erstellen.
Dies geht mit dem Befehl unten.
```bash
sudo useradd nas -m -G users
```  
Natürlich muss für diesen Benutzer ein Passwort gesetzt werden.
```bash
sudo passwd nas
```  
Für den zweiten Benutzer kann man diesen Schritt einfach wiederholen, jedoch mit einem anderen Benutzernamen.

---  
## Samba Kennwort Setzen  
Zusätzlich muss auch noch ein Passwort für Samba erstellt werden.  
Zum Glück ist das ein einfacher Befehl.
```bash
sudo smbpasswd -a nas
```  
Diesen Befehl muss man natürlich ebenfalls wiederholen für den zweiten Benutzer.

---  
## Konfiguration für das NAS  
Jetzt muss man nur noch dafür sorgen, dass das Anbinden automatisch passiert, falls der RaspberryPI neugestartet wird.  
Diese Konfiguration ist sehr schnell gemacht, wenn man es mit dieser Abkürzung benutzt.
```bash
sudo echo "/dev/[STICKNAME] /home/pi/smb auto noatime 0 0" >> /etc/fstab
```

---  
## Fremdwörter  
> **NAS**  
> Kurz für *Network-attached storage*.
> Wird benutzt, um Ordner und Dateien mit anderen Rechnern im gleichen Netzwerk zu teilen.  

  
> **NTFS**  
> Ursprünglich *New Technology File System*.  
> Da es aber in 17 Tagen 27 Jahre alt wird, wurde die Bedeutung auf *NT File System* geändert.  
> Ist seit Windows NT 3.1 das Dateisystem von allen Windows Geräten.  

  
> **Samba**  
> Samba bietet Datei- und Druckservices an, welche auf fast allen Betriebssystemen laufen.  
> Hier wurde es verwendet um das NAS richtig zu sichern.