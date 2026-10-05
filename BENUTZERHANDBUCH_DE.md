# KP184control v2.3.0 — Benutzerhandbuch

Dieses Handbuch beschreibt die Funktionen von **KP184control v2.3.0**.

> **Unterstützte Lastmodi:** CC, CP/CW, CR und CV  
> **Validiertes Geräteprofil:** KUNKIN KP184, Modell-ID `0x0730`  
> **Windows-Release:** x64, self-contained

## 1. Zweck

KP184control steuert eine KUNKIN-KP184-Elektroniklast für Batterie-Entladetests. Die Software zeigt Spannung, Strom und Leistung live an, berechnet Ah und Wh, speichert Diagramme und CSV-Protokolle und kann Tests automatisch beenden.

## 2. Batteriedaten und sichere Abschaltspannung

Wählen Sie die Batteriechemie und geben Sie die korrekte maximale Spannung des vollständig geladenen Akkupacks ein. KP184control verwendet diese Daten zur Schätzung der Serienkonfiguration und einer sicheren Standard-Abschaltspannung.

> [!WARNING]
> Geben Sie immer die korrekte maximale Batteriespannung ein. Ein falscher Wert kann zu einer falschen Zellkonfiguration und damit zu einer falschen automatischen Abschaltspannung führen.

Unterstützt werden Li-ion (NMC/NCA), LiPo, LiFePO4 und NiMH. Die Nennkapazität in Ah wird für Fortschritt, Kapazitätsvergleich und Battery Health verwendet.

## 3. Verbindung mit dem KP184

Wählen Sie den richtigen COM-Port. KP184control erkennt unterstützte Kommunikationseinstellungen automatisch und überprüft das Geräteprofil. Hardwareseitig validiert ist der KUNKIN KP184 mit Modell-ID `0x0730`.

Für dieses Profil gelten die bekannten Grenzen:

- maximal 150 V;
- maximal 40 A;
- maximal 400 W.

Ein unbekanntes Profil kann nach Möglichkeit gelesen werden, LOAD wird jedoch erst aktiviert, wenn ein sicheres Schreibprofil bestätigt wurde.

## 4. Lastmodi

### CC — Constant Current

Der KP184 zieht einen eingestellten konstanten Strom. Der Software-Softstart ist standardmäßig aktiviert und erhöht den Strom schrittweise.

### CP/CW — Constant Power

Der KP184 versucht eine konstante Leistung zu ziehen. Da `I = P / V` gilt, kann der Strom bei sinkender Batteriespannung steigen. KP184control prüft deshalb auch den erwarteten Strom bei der gewählten Abschaltspannung.

### CR — Constant Resistance

Der KP184 verhält sich wie ein eingestellter Widerstand. Für das validierte Profil wurde die CR-Skalierung hardwareseitig mit 0,1 Ω pro Registerschritt bestätigt.

Bei einem **erwarteten CR-Strom unter etwa 0,15 A** zeigt v2.3.0 eine Warnung an, da die praktische Regelgenauigkeit bei sehr niedrigen Strömen abnehmen kann.

### CV — Constant Voltage

CV ist in **v2.3.0 aktiviert** für das validierte KP184-Profil.

Native CV besitzt keine separat einstellbare hardwareseitige Strombegrenzung. KP184control führt daher zusätzliche Prüfungen durch und überwacht die Startphase schneller. Wird ein zu hoher Startstrom gemessen, wird LOAD ausgeschaltet.

> [!WARNING]
> Die Softwareüberwachung ist eine zusätzliche Sicherheitsebene und ersetzt keine externe oder hardwareseitige Strombegrenzung, wenn diese für den Test erforderlich ist.

## 5. START und Sicherheitsprüfungen

Vor LOAD ON prüft KP184control unter anderem:

- ob eine gültige Live-Spannung vorliegt;
- ob das erkannte KP184-Profil sichere Schreibzugriffe erlaubt;
- Spannungs-, Strom- und Leistungsgrenzen;
- die erwartete Belastung im gewählten Modus;
- eingegebene Batterie- und Cutoff-Daten.

Während eines aktiven Tests werden die bekannten Strom- und Leistungsgrenzen überwacht. Warnungen erscheinen, bevor eine bestätigte Grenzüberschreitung zu LOAD OFF führt.

## 6. Automatisches Beenden

Die automatische Cutoff-Funktion ist standardmäßig aktiviert. Ein Test kann außerdem beendet werden bei:

- maximaler Testdauer;
- maximalen Ah;
- maximalen Wh.

Der Stoppgrund wird in den Ergebnissen und im CSV-Protokoll gespeichert.

## 7. USB-Verbindungsverlust und Fortsetzen

Nach mehreren aufeinanderfolgenden Kommunikationsfehlern wird ein aktiver Test pausiert und KP184control versucht automatisch, die Verbindung wiederherzustellen. Ein Test wird **niemals automatisch fortgesetzt**.

Nach erfolgreicher Wiederverbindung bestätigt die Software zunächst einen sicheren LOAD-OFF-Zustand. Danach kann der Benutzer den Test manuell fortsetzen.

## 8. Presets und Historie

v2.3.0 unterstützt lokale Test-Presets für wiederverwendbare Testeinstellungen. Außerdem wird eine kompakte lokale Historie abgeschlossener Tests geführt. Die vollständigen Messpunkte verbleiben in den CSV-Dateien.

## 9. CSV, Diagramme und Berichte

Neue CSV-Protokolle enthalten unter anderem:

- `APP;Naam;KP184control`
- `APP;Versie;v2.3.0`
- Geräte- und Kommunikationsdaten;
- Sicherheitsinformationen;
- Testeinstellungen;
- Messwerte und Ereignisse;
- Endergebnisse und Stoppgrund.

Die Anwendung zeigt Diagramme der Messdaten und kann einen PDF-Testbericht erzeugen.

## 10. Mobile/Web-Oberfläche

KP184control v2.3.0 enthält eine lokale Weboberfläche, die von einem Smartphone oder anderen Gerät im selben Netzwerk verwendet werden kann.

Verfügbare Zugriffsmodi:

- **Aus** — Webzugriff deaktiviert;
- **Nur lesen** — Überwachung ohne Steuerbefehle;
- **Volle Steuerung** — unterstützte Einstellungen und START/STOP aus der Ferne.

Steueraktionen verwenden ein lokales Aktionstoken. Dieses Token kann unter Windows erneuert werden; bereits geöffnete mobile Seiten müssen danach neu geladen werden.

Die aktuelle Version ist für **lokalen Netzwerkzugriff** vorgesehen und nicht als öffentlich erreichbarer Internetdienst.

## 11. Installation von v2.3.0

Download über GitHub Releases:

- `KP184control-v2.3.0-windows-x64.zip`
- `KP184control-v2.3.0-windows-x64-SHA256.txt`

ZIP entpacken und `KP184control.exe` starten.

Die Windows-x64-Version ist **self-contained**. Eine separate Installation der Microsoft .NET Desktop Runtime ist für dieses Paket nicht erforderlich.

Mit der SHA256-Datei kann die Integrität der heruntergeladenen ZIP-Datei geprüft werden.

## 12. Sicherheit

Tests mit Batterien und elektronischen Lasten können hohe Ströme, Wärme, Lichtbögen, BMS-Abschaltungen und Brandgefahr verursachen. Verwenden Sie ausreichend dimensionierte Kabel, Steckverbinder und Sicherungen, prüfen Sie Polarität und Grenzwerte vor START und lassen Sie einen Test nicht unbeaufsichtigt.

KP184control ist ein Hilfsmittel und ersetzt keine korrekte elektrische Absicherung, Aufsicht oder Beurteilung des Batteriezustands.

## 13. Lizenz und Quellcode

KP184control ist proprietäre Software und kein Open-Source-Projekt. Das öffentliche Repository enthält absichtlich keinen C#-Quellcode. Siehe `LICENSE.txt` für die Nutzungsbedingungen.
