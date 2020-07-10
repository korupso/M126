# Kontrollfragen
## 00 Einstieg IOT Fragen

**Peripheriegeräte im Netzwerkbetrieb einsetzen, was verstehen Sie darunter?**  

Sind Geräte mit Controller, welche ausserhalb eines  zentralen  Computers Daten sammeln (Sensoren) und diese via Netzwerk an den  zentralen  Computer  senden,  oder  von diesem  empfangen  und verarbeiten (Aktoren) wie zum Beispiel ausdrucken.  

**Was könnte Ihrer Meinung nach unter „Things“ alles verbunden werden? Zählen Sie auf!**  

Angeschlossene Sicherheitssysteme, Thermostate, Autos, elektronische Geräte, Leuchten in Haushalts- und Geschäftsumgebungen, Wecker, Lautsprechersysteme, Verkaufsautomaten und mehr.  

**Futuristische Beispiele?**  

Kühlschrank, welcher dich informiert wenn bald ein Lebensmittel darin schlecht wird oder eine Wasch und Trockenmaschine welches beides kann.  

**IOT Vorteile?**  

Sie könnten online schauen, ob der Herd zuhause an oder abgeschaltet ist. Ob zu Hause alle Fenster geschlossen sind oder welche Lebensmittel fehlen.

**Wo sehen Sie selbst die Gefahren?**

Wir werden im Alltag dann viel zu bequemer habe ich das Gefühl und viele Leute welche mit Technik nichts am Hut haben werden sich sehr überfordert fühlen.

---
## 01 Einführung Raspberry PI

**Informieren Sie sich über NOOBS. Was beinhaltet es? Welche möglichen Betriebssysteme lassen sich damit installieren?**

NOOBS ist ein einfaches Betriebssystem-Installationsprogramm, das Raspberry Pi OS und LibreELEC enthält. Es bietet auch eine Auswahl alternativer Betriebssysteme, die dann aus dem Internet heruntergeladen und installiert werden. 

**Wie viele Zeichen darf das Volume Label bei FAT 32 maximal enthalten?**

Die Adressierung arbeitet mit 32 Bit, wovon 4 Bit reserviert sind, so dass 228 = 268.435.456 Cluster adressiert werden können.

**Welche Zeichen dürfen verwendet werden?**

Alle Zahlen, Buchstaben, einige Sonderzeichen aber nicht alle.

**Wo wird die Information des Volume Labels gespeichert?
Direkt in der Partitionstabelle.
Unser Debian Linux Portierung Raspian (Version 10, V10) hat den Namen Buster. Welche Namen hatten die Versionen V9, V8 und V7?**  

V7 = Wheezy
V8 = Jessie
V9 = Strech

**Schreiben Sie in Ihrem ePortfolio einen kurzen Artikel über die Geschichte des RasPi. Wie ist 
er entstand, welche HW Versionen gibt es, usw.** 

Obwohl sie für eine der unerwarteten Erfolgsgeschichten im Computing des Jahres 2012 verantwortlich sind, hatten Eben Upton und seine Kollegen ursprünglich gar nicht die Absicht, einen Computer zu entwickeln. Die Raspberry Pi Foundation wurde eigentlich mit dem Ziel gegründet, die nächste Generation für das Programmieren zu begeistern. Es stellte sich jedoch heraus, dass es am besten zu erreichen war durch einen Computer, der billig genug für Jugendliche war und zugleich einfach genug, um ihn „hacken“ zu können. 

**Mit welchem Linux Befehl lassen sich die Partitionen im Raspian anzeigen?**

Mit dem Befehl lsblk.

**Welche Schreib und Lesegeschwindigkeiten weisst ihre SD-Karte auf?**

Etwas mehr als 20Mbyte/s.

---  
## 03 Datenschutz und Sicherheit

**Da  wir  mit  IoT Geräten  aber  ebenso  Daten  lokal  erheben  und  ins  Netz  stellen,  kommt  ein 
weiterer Aspekt dazu: Welche Daten dürfen im Netz zugänglich gemacht werden?**

Daten aus welchem man nicht Schlussfolgern kann wer das ist sind grundsätzlich ok. Zum Bespiel Lieblingsfarbe, Hobby etc.

**Zählen sie auf, welche Daten ihrer Meinung nach „heikel“ sind:**

Passwörter, Adresse, Name

**Studieren Sie Art. 8.
Welche Art von Auskunft kann eine Person also verlangen?**

Jede Person kann vom Inhaber einer Datensammlung Auskunft darüber verlangen, 
ob Daten über sie bearbeitet werden. 
Der Inhaber der Datensammlung muss der betroffenen Person mitteilen.

**Welche Cloud Hoster bieten ihre Dienste im Europäischen Raum an? 
Welche Alternative gibt es für eine kleine StartUp Firma?**

Dropbox, Mediafile, Google Drive und Public Cloud von OVH etc.

---
```bash
sudo apt-get update
``` 	

Lädt die Paketlisten aus den Repositories herunter und "aktualisiert" sie, um Informationen über die neuesten Versionen von Paketen und deren Abhängigkeiten zu erhalten. Dies geschieht für alle Repositories und PPAs.

```bash
sudo apt-get upgrade
```

Holt neue Versionen auf dem Rechner von vorhandenen Paketen, wenn APT über apt-get update von diesen neuen Versionen weiß.

```bash
sudo apt-get dist-upgrade
```

Wird die gleiche Arbeit erledigen, die apt-get upgrade erledigt, zusätzlich wird es auch die Abhängigkeiten intelligent handhaben, so dass es veraltete Pakete entfernen oder neue hinzufügen kann.

```bash
sudo rpi-update
```

Wird die neueste Vorabversion des Linux-Kernels, die entsprechenden Module, Gerätedateien sowie die neuesten Versionen der VideoCore-Firmware herunterladen. Diese Dateien werden dann an den entsprechenden Stellen auf der SD-Karte installiert, wobei alle früheren Versionen überschrieben werden.

---
**Datenschutz**

Schutz  des  Bürgers  vor  Beeinträchtigungen  seiner  Privatsphäre  durch  unbefugte  Erhebung, Speicherung und Weitergabe von Daten, die seine Person betreffen. 

Zum einen betrifft es die „innere“ Sicherheit des Systems, d.h. dass ein unbefugter Zugriff auf die gespeicherten Daten verwehrt wird.

Die Erhebung, Verarbeitung und Nutzung personenbezogener Daten ist grundsätzlich verboten, es sei denn, ein gesetzlicher Erlaubnisgrund oder die freiwillige Einwilligung rechtfertigen dies. Beim Datenschutz geht es also um die zentrale Frage, ob personenbezogene Daten überhaupt erhoben und verarbeitet werden dürfen oder nicht.


**Datensicherheit**

Im Unterschied zum Datenschutz betrifft die Datensicherheit alle Daten, unabhängig davon, ob diese einen Personenbezug haben oder nicht. Unter diesen Begriff fallen daher auch solche Daten, die keinen Personenbezug haben, ganz egal ob in analoger oder digitaler Form. 
Datensicherheit hat das technische Ziel, Daten jeglicher Art in ausreichendem Maße gegen Manipulation, Verlust, unberechtigte Kenntnisnahme durch Dritte oder andere Bedrohungen zu sichern.


