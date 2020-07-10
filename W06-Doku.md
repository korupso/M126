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
> ## RAID 0 – Data-Striping
> Eigentlich ist »RAID 0« kein RAID-Level, weil das Wichtigste > fehlt: die Redundanz. Mindestens zwei Laufwerke werden zu einem logischen Verbund (Stripeset) zusammengefasst. Der Vorteil von RAID 0 liegt in der Belegung der
gesamten Kapazität mit produktiven Daten und im durch den parallelen Zugriff über mehrere Kanäle multiplizierten Datendurchsatz. Dieser lässt sich nur erzielen, wenn seriell gelesen oder geschrieben wird. Die zu schreibenden Daten erfahren eine Aufteilung in Blöcke, die dann paritätisch auf die einzelnen Laufwerke verteilt werden.
Die Gefahr des Datenverlustes verdoppelt sich mit dem Einsatz zweier Laufwerke für ein Stripeset und erhöht sich mit dem Hinzufügen jeder weiteren Festplatte. Sollte eines der beteiligten Laufwerke ausfallen, so sind auch die auf den anderen gespeicherten Daten nutzlos.

> ## RAID 1 – Mirroring und Duplexing
> »RAID 1« ist die Spiegelung (»Mirroring«, »Duplexing«) von zwei oder mehreren Festplatten. Damit wird gleichzeitig die höchste Schutzstufe mit dem geringsten Aufwand erreicht. Durch Schreiben der Daten auf zwei getrennte, physikalisch gleiche Laufwerke (oder Laufwerks-Verbunde) ist kein Rechenaufwand notwendig, weder beim Erzeugen noch bei der eventuell notwendigen Rekonstruktion. Diese erfolgt durch einfaches Kopieren der dann noch einfach vorhandenen Daten. Die meisten am Markt vorhandenen Controller unterstützen den Austausch fehlerhafter Festplatten im laufenden Betrieb (Hot-Swap) und damit eine unterbrechungsfreie Reparatur.
Ein RAID-1-Verbund lässt sich auf zwei Arten erzeugen. Beim Mirroring werden zwei Festplatten über denselben I/O-Kanal beschrieben, beim Duplexing geschieht dies über zwei getrennte Kanäle. Das heißt, dass der komplette Inhalt einer Harddisk auf ein anderes Laufwerk überspielt wird. Vom Standpunkt der Sicherheit ist diese Methode optimal. Allerdings auch sehr teuer, denn die Redundanz verbraucht 50 Prozent der vorhandenen Kapazität.

> ## RAID 1E, RAID 1E0
> Auf einer »RAID 1E« konfigurierten Plattengruppe werden Daten in kleinere Abschnitte zerlegt, geschrieben und dann auf den jeweils nächsten Datenträger gespiegelt. Ist die letzte Platte beschrieben, beginnt der Schreibvorgang wieder bei der ersten. Wie bei RAID  1 kann auf RAID 1E nur der halbe physikalische Speicherplatz genutzt werden. Datenverlust tritt hier auf, wenn zwei logisch aufeinanderfolgende oder die erste und letzte Platte zum selben Zeitpunkt ausfallen.
»RAID 1E0« verteilt die zu schreibenden Daten auf zwei oder mehr RAID-1E-Gruppen. Auch hier wird der nutzbare Speicherplatz auf die Hälfte der Physik reduziert, zwei nebeneinanderliegende oder die erste und letzte Platte dürfen nicht zusammen ausfallen, um Datenverlust zu vermeiden.

> ## RAID 2 - Hamming Code ECC
> »RAID 2« wird derzeit von keinem Hersteller am Markt unterstützt und bietet vor allem im Vergleich zu »RAID 5« ein unakzeptables Preis-/Leistungsverhältnis.
Ein RAID-2-Verbund besteht aus zwei oder mehr Datenplatten und einer oder mehr ECC-Platten (»Error Correction Code«). Jedes Bit eines Wortes wird verteilt auf die Datenplatten geschrieben, ein ECC nach dem Hamming-Algorithmus erzeugt und dieser auf die entsprechenden Platten abgelegt. Dadurch wird es möglich, »on the fly«, also bei jedem Lesezugriff, die Daten auf Integrität zu überprüfen und nötigenfalls sofort zu korrigieren. Da das Schreiben der Daten und des ECC über separate Kanäle auf getrennte Festplatten geschieht, sind dadurch extrem hohe Transferraten möglich. Allerdings lassen sich diese nur erreichen, wenn die einzelnen Platten synchronisiert werden und dies wiederum ist ein nicht zu rechtfertigender Aufwand.

> ## RAID 3 – Data-Striping plus Parity
> In »RAID 3«-Stripesets werden Blöcke über mindestens zwei Laufwerke verteilt, während Kontrollinformationen (»Parity«) auf einer separaten Festplatte gespeichert werden.
Bei »RAID 3« ist der Controller für die Berechnung der Kontrollinformationen zuständig. Diese werden bei jedem Schreibvorgang erzeugt und beim Lesen überprüft. Der dabei erstellte zusätzliche Index kommt ebenfalls auf das Parity-Laufwerk. Mit einem Rechenalgorithmus (XOR-Verknüpfung) ist es möglich, beim Ausfall einer Disk die fehlenden Daten zusammen mit der Prüfsumme zu rekonstruieren.
Die Daten bleiben in diesem Falle zwar 100-prozentig verfügbar, allerdings steht während der Rekonstruktionsphase nur eine sehr geringe Bandbreite zur Verfügung. Für das Rückrechnen und -schreiben arbeiten aktuelle Laufwerke derzeit mit einer Datenübertragung von circa fünf bis sieben GByte/h.
Die nutzbare Kapazität hängt bei RAID 3 von der Zahl der Laufwerke ab. Für die Kontrollinformationen ist insgesamt die Kapazität eines Einzellaufwerkes erforderlich. In einem RAID-3-System mit drei 146-GByte-Festplatten stehen demnach 292 GByte also 66,7 Prozent der Bruttokapazität zur Verfügung, bei vier Harddisks 438 GByte (entspricht 75 Prozent), bei fünf Drives 584 GByte (entspricht 80 Prozent).
Die meisten Controller lassen nur die Nutzung gleichartiger und gleich großer Festplatten zu. Manche Controller tolerieren auch unterschiedliche Disk-Typen, jedoch beschränkt sich die nutzbare Kapazität dann auf das kleinste am Kanal vorhandene Laufwerk. Beim Einsatz von drei 181-GByte- und zwei 146-GByte-Festplatten beträgt die verfügbare Kapazität insgesamt nur 584 GByte, da auf den großen Laufwerken auch nur 146 GByte genutzt werden.
RAID 3 findet heute kaum noch Verwendung, da durch die Erzeugung der Kontrollinformationen auf dieser Festplatte ein »Hot Spot« mit sehr hoher I/O-Rate entstehen kann. Darüber hinaus ist es nicht möglich, die Daten gleichmäßig über alle vorhandenen Laufwerke zu verteilen.

> ## RAID 4 – Data plus Parity
> Prinzipiell funktioniert »RAID 4« wie »RAID 3«. Die Blöcke werden hier allerdings nicht verteilt auf die Festplatten geschrieben, sondern in Gänze auf einem Laufwerk abgelegt. Auch verzichtet man auf das Synchronisieren der Festplattenköpfe, um die Nachteile von RAID 3 bei der Verarbeitung kleiner Dateien zu umgehen. Die Prüfsumme wird auf einem extra Laufwerk gespeichert. Speziell beim Schreiben kleiner File-Größen entpuppt sich das Parity-Drive als Flaschenhals. Bei jeder Schreiboperation muss zunächst die Checksumme errechnet, die Parity-Informationen auf dem Laufwerk gefunden und angesteuert werden. Vorteile bietet RAID 4 in Umgebungen, in denen vor allem Lesezugriffe anfallen. In der Praxis verwendete fast nur NetApp dieses Verfahren.

> ## RAID 5 – Data- and Parity-Striping
> Auch in »RAID 5«-Stripesets werden Daten und Kontrollinformationen (Parity) über mindestens drei Laufwerke verteilt. Diese Schutzklasse stellt den derzeit besten Kompromiss zwischen nutzbarer Kapazität, Leistung und Rekonstruktion der Daten dar.
Der Controller ist für die Berechnung der Kontrollinformationen zuständig, die bei jedem Schreibvorgang erzeugt und beim Lesen überprüft werden. Der dabei entstehende zusätzliche Index wird ebenfalls über alle Laufwerke verteilt. Die Parität wird durch eine Exklusiv-oder-Verknüpfung realisiert. Ein Laufwerk innerhalb des RAIDs darf vollständig ausfallen, ohne dass Daten verloren gehen. Das System kann weiterhin auf das Array und die Daten der fehlerhaften Platte zugreifen, weil ihre Daten aufgrund der Exklusiv-oder-Operation unter Zuhilfenahme der Parität aus den verbliebenen Platten errechnet werden können. Ansonsten verhält sich RAID 5 wie RAID 3, was die Rekonstruktion von Daten eventuell ausgefallener Festplatten und die dafür benötigte Zeit angeht.
RAID 5 stellt den besten Kompromiss zwischen Datenschutz, Verfügbarkeit und Leistung dar, da es keine Hot Spots durch die Verteilung der Kontrolldaten gibt und der Zugriff auf alle Festplatten gleichzeitig erfolgen kann. Für die Parität muss auch in größeren Umgebungen maximal die Kapazität eines Laufwerks »geopfert« werden.

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