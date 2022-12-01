# Homeassistant

![image](/images/ha.png)

## Warum HA?
* Smart Home wird nie smart sein, solange jeder Hersteller sein eigenes System baut
* privacy
* früher Bedarf nach Custom-lösungen: 
  * billige 433MHz Schaltsteckdosen, statt teuer WLAN-Steckdosen mit China-FW --> DIY 433MHz Sender
  * IR-LEDStrips statt Philips HUE --> DIY IR Sender 
  * Einschränkung der Hersteller-Apps
* nach kurzem Versuch von openHAB sehr schnell bei Homeassistant gelandet & geblieben: schön, durchdacht, gutes Konzept, sehr aktive community - und: es funktioniert einfach ¯\\_(ツ)_/¯

## Was ist Homeassistant?
* Plattform zum "Zusammenstecken" aller möglichen Smarthome-Systeme
* entwickelt in Python
* lokal, cloud-less --> meine Daten bleiben im Haus (Cloud-Integrationen extra markiert)
* open-source & lebendig(!)
* hochgradig individualisierbar
* unzählige Erweiterungen
* lebt (idealerweise) "neben" proprietären Systemen (Bsp. Hue)

## Funktionsweise

[Homeassistant.local](http://homeassistant.local:8123/config/dashboard)

* Integrationen --> Geräte --> Entitäten --> Helfer
* Automatisierungen
    * Auslöser
    * Bedingung
    * Aktion
* Szenen: "Snapshots" von Geräte-Zuständen
* Skripte: Skripte ;-)
* Vorlagen/Blueprints: "Methoden" für Automatisierungen
* Und:
  * Personen, verknüpfbar mit Entities/Geräten
  * Zonen (GPS-location based, tracking)
  * native Anwendungen für Android & iOS
  * Add-Ons
* Logging / Debugging-Möglichkeiten schauen wir uns später an, wenn es was zu debuggen gibt ;)

[next --> ](/03_Praxis.md)