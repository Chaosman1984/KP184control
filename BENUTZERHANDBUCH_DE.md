# KP184control v2.1.0 — Ausführliches Benutzerhandbuch

Dieses Handbuch beschreibt die Funktionen, die in **KP184control v2.1.0** tatsächlich verfügbar sind.

> **Unterstützte Lastmodi:** CC, CP/CW und CR  
> **CV:** in der Oberfläche sichtbar, in v2.1.0 jedoch bewusst deaktiviert  
> **Getestetes Geräteprofil:** KUNKIN KP184, Modell-ID `0x0730`

---

## 1. Zweck von KP184control

KP184control steuert eine elektronische Last KUNKIN KP184 und ist für die Durchführung, Überwachung und Protokollierung von Batterie-Entladetests vorgesehen.

Die Software kann unter anderem:

- den KP184 automatisch erkennen;
- Spannung, Strom und Leistung live anzeigen;
- in CC-, CP/CW- und CR-Modus entladen;
- Ah und Wh berechnen;
- die Entladekurve speichern und grafisch darstellen;
- bei einer eingestellten Abschaltspannung automatisch stoppen;
- auf Basis der Batteriechemie und der maximalen Batteriespannung eine sichere Standard-Abschaltspannung berechnen;
- Hardwaregrenzen vor dem Start eines Tests prüfen;
- Testergebnisse automatisch als CSV speichern;
- gemessene Kapazität mit der Werkskapazität vergleichen;
- nach einer brauchbaren automatischen Abschaltung die verbleibende Kapazität schätzen.

---

## 2. Batteriedaten

### Batteriechemie

Verfügbare Auswahl:

- Li-ion (NMC/NCA)
- LiPo
- LiFePO4
- NiMH

Die gewählte Chemie wird verwendet für:

1. die Schätzung der Anzahl in Reihe geschalteter Zellen;
2. die automatische Berechnung der sicheren Standard-Abschaltspannung;
3. die spätere Schätzung der Kapazität unterhalb der eingestellten Abschaltspannung.

### Werkskapazität

Hier wird die ursprüngliche Nennkapazität der Batterie in Ah eingetragen, zum Beispiel:

`30.000 Ah`

Dieses Feld ist nicht erforderlich, damit der KP184 als Last arbeitet, wird aber verwendet für:

- Prozent der Werkskapazität;
- Fortschrittsbalken;
- Battery-Health-Anzeige;
- Schätzung der Gesamtkapazität nach automatischer Abschaltung.

### Maximale Batteriespannung

Hier wird die Spannung der vollständig geladenen Batterie eingetragen.

Beispiel:

`67.200 V`

KP184control verwendet diesen Wert zusammen mit der gewählten Chemie, um die Reihenkonfiguration zu schätzen.

---

## 3. Automatische Zellkonfiguration

Die Software teilt die eingegebene maximale Batteriespannung durch die typische vollgeladene Zellspannung und rundet das Ergebnis auf eine ganze Anzahl Zellen.

Verwendete Werte:

| Chemie | Vollgeladen pro Zelle |
|---|---:|
| Li-ion (NMC/NCA) | 4.20 V |
| LiPo | 4.20 V |
| LiFePO4 | 3.65 V |
| NiMH | 1.45 V |

Beispiel:

- Chemie: Li-ion
- maximale Batteriespannung: 67.2 V
- 67.2 / 4.20 = 16

Die Software zeigt dann ungefähr:

`Konfiguration: 16S | 4.200 V/Zelle`

---

## 4. Automatische sichere Abschaltspannung

### Wichtig

In v2.1.0 wird die **sichere Standard-Abschaltspannung nicht aus dem eingestellten Strom berechnet**.

Die automatische Abschaltspannung wird berechnet aus:

- Batteriechemie;
- maximaler Batteriespannung;
- der daraus abgeleiteten Anzahl in Reihe geschalteter Zellen.

Der eingestellte Strom wird separat gegen die Strom- und Leistungsgrenzen des KP184 geprüft.

### Standard-Abschaltung pro Zelle

| Chemie | Automatische Abschaltung pro Zelle |
|---|---:|
| Li-ion (NMC/NCA) | 3.10 V |
| LiPo | 3.20 V |
| LiFePO4 | 3.00 V |
| NiMH | 1.00 V |

Beispiel für eine 16S-Li-ion-Batterie:

`16 × 3.10 V = 49.6 V`

Die Software trägt dann automatisch ungefähr **49.600 V** als Abschaltspannung ein.

Der Benutzer kann die Abschaltspannung anschließend manuell anpassen.

---

## 5. Automatisches Stoppen bei Abschaltspannung

Die Option **Automatisch bei Abschaltspannung stoppen** ist standardmäßig aktiviert.

Während eines aktiven Tests:

1. wartet KP184control 2 Sekunden, bevor der Abschaltschutz aktiv wird;
2. wird die Batteriespannung ungefähr einmal pro Sekunde geprüft;
3. muss die Spannung bei 3 aufeinanderfolgenden Messungen auf oder unter der eingestellten Abschaltspannung liegen;
4. danach wird LOAD OFF gesendet und der Test gestoppt.

Dadurch wird verhindert, dass ein sehr kurzer Spannungseinbruch den Test sofort beendet.

Wenn die automatische Abschaltung manuell deaktiviert wird, zeigt die Software zunächst eine Warnung an.

---

## 6. Verbindung mit dem KP184

### COM-Port und Adresse

Wählen Sie den COM-Port, an dem der KP184 angeschlossen ist. Die Modbus-Adresse ist einstellbar; standardmäßig wird Adresse 1 verwendet.

### Automatische Kommunikationserkennung

KP184control versucht automatisch mehrere Baudraten:

- 9600
- 115200
- 57600
- 38400
- 19200
- 4800
- 2400

Zusätzlich testet die Software beide verwendeten CRC-Byte-Reihenfolgen.

### Modellerkennung

Bei Modell-ID `0x0730` wird das Gerät als KP184 erkannt.

Für dieses bestätigte Profil verwendet die Software folgende Gerätegrenzen:

- maximal 150 V;
- maximal 40 A;
- maximal 400 W.

Wenn ein Gerät zwar gelesen werden kann, das Schreibprofil jedoch nicht bestätigt ist, startet KP184control aus Sicherheitsgründen keinen Lasttest.

---

## 7. Live-Messung

Nach der Verbindung zeigt die Software kontinuierlich:

- Spannung in V;
- Strom in A;
- Leistung in W.

Die Messung wird ungefähr einmal pro Sekunde aktualisiert.

Während eines Tests werden dieselben Werte verwendet für:

- Kapazität in Ah;
- Energie in Wh;
- CSV-Protokollierung;
- Diagramme;
- Abschaltkontrolle.

---

## 8. CC — Konstantstrom

Im CC-Modus versucht der KP184, einen konstanten Entladestrom zu ziehen.

Eingestellt werden:

- Entladestrom in Ampere;
- Abschaltspannung;
- Soft-Start ein/aus.

### Prüfungen vor START

KP184control prüft unter anderem:

- Strom muss größer als 0 A sein;
- eingestellter Strom darf die Modellgrenze nicht überschreiten;
- `Spannung × Strom` darf die maximale KP184-Leistung nicht überschreiten.

Beispiel:

Bei 66 V und 10 A würden ungefähr 660 W angefordert. Da das bestätigte KP184-Profil auf 400 W begrenzt ist, wird der Test nicht gestartet.

In diesem Fall berechnet die Software außerdem ungefähr, welcher Strom bei der aktuellen Spannung noch innerhalb von 400 W liegt.

---

## 9. CC Soft-Start

Soft-Start ist in v2.1.0 standardmäßig aktiviert.

Der Soft-Start:

1. stellt zunächst maximal 0.100 A ein;
2. schaltet LOAD ON;
3. erhöht danach den CC-Strom um jeweils 0.100 A;
4. wartet 100 ms zwischen jedem Schritt;
5. beendet die Erhöhung, sobald der eingestellte Endstrom erreicht ist.

Beispiel bei einem Zielwert von 1.000 A:

`0.1 → 0.2 → 0.3 → ... → 1.0 A`

Dadurch wird der plötzliche Lastsprung beim Start reduziert.

Soft-Start kann manuell deaktiviert werden. In diesem Fall wird der gewählte CC-Strom direkt eingestellt, bevor LOAD ON aktiviert wird.

---

## 10. CP/CW — Konstantleistung

Im CP/CW-Modus versucht der KP184, eine konstante Leistung zu ziehen.

Die gewünschte Leistung wird in Watt eingestellt.

### Prüfungen vor START

KP184control prüft:

- Leistung muss größer als 0 W sein;
- Leistung darf die Modellgrenze des KP184 nicht überschreiten;
- der erwartete Strom bei der eingestellten Abschaltspannung darf den maximalen Strom des KP184 nicht überschreiten.

Bei konstanter Leistung gilt:

`I = P / V`

Dadurch steigt der Strom, wenn die Batteriespannung sinkt.

Die Software prüft deshalb gezielt, wie hoch der Strom ungefähr bei der gewählten Abschaltspannung wird.

---

## 11. CR — Konstantwiderstand

Im CR-Modus verhält sich der KP184 wie ein eingestellter Widerstand.

Der Widerstand wird in Ohm eingestellt.

Für diesen Modus gilt:

`I = V / R`

und:

`P = V² / R`

### Prüfungen vor START

Mit der aktuellen Startspannung prüft die Software:

- erwarteten Startstrom;
- erwartete Startleistung;
- maximale 40-A-Grenze;
- maximale 400-W-Grenze.

Wenn der gewählte Widerstand zu niedrig ist, wird der Test nicht gestartet und KP184control zeigt ungefähr den minimal sicheren Widerstand für die aktuelle Spannung an.

### Hardwarebestätigung

Für das getestete KP184-Profil wurde CR hardwareseitig mit folgender Skalierung bestätigt:

`0.1 Ω pro Registerschritt`

Die geschriebene CR-Einstellung wird vor LOAD ON außerdem zurückgelesen und überprüft.

---

## 12. CV — Konstantspannung

Der CV-Reiter ist sichtbar, aber **CV ist in v2.1.0 bewusst deaktiviert**.

Während der Entwicklung wurden sowohl die native CV-Einstellung als auch eine softwarebasierte strombegrenzte CV-Regelung untersucht. Die experimentelle Regelung wurde für eine Veröffentlichung als nicht stabil genug eingestuft.

START kann deshalb im CV-Modus nicht verwendet werden.

---

## 13. Sicherheitsprüfungen vor einem Test

Zusätzlich zu den Prüfungen pro Lastmodus führt KP184control allgemeine Prüfungen durch.

### Gültige Live-Spannung

Zuerst muss eine gültige Live-Batteriespannung gemessen worden sein.

### Maximale KP184-Spannung

Wenn die gemessene Batteriespannung über der maximalen Spannung des bestätigten Geräteprofils liegt, wird der Test nicht gestartet.

### Prüfung der eingegebenen maximalen Batteriespannung

Wenn die Live-Batteriespannung mehr als **1.5 V höher** ist als die vom Benutzer eingegebene maximale Batteriespannung, blockiert KP184control den Start.

Damit soll zum Beispiel eine falsch eingegebene Batteriespannung erkannt werden.

### Bestätigtes Schreibprofil

Ein unbekanntes Geräteprofil darf gelesen werden, aber die Software aktiviert LOAD nicht, solange das Schreibprofil nicht als sicher bestätigt wurde.

---

## 14. START TEST

Bei einem gültigen Start:

1. werden alte Messdaten gelöscht;
2. werden Ah und Wh auf null gesetzt;
3. wird eine neue CSV-Datei geöffnet;
4. wird LOAD zuerst ausdrücklich AUS geschaltet;
5. wird der gewählte Lastmodus an den KP184 geschrieben;
6. werden die Einstellungen geschrieben;
7. wird LOAD eingeschaltet;
8. werden die Einstellungen während des aktiven Tests gesperrt.

Die STOP-Taste wird aktiv und START vorübergehend deaktiviert.

---

## 15. STOP TEST

Bei STOP:

- wird LOAD OFF an den KP184 gesendet;
- stoppt der aktive Test;
- wird die CSV-Datei geschlossen;
- werden Endergebnisse an die CSV-Datei angehängt;
- werden die Eingabefelder wieder freigegeben;
- wird Battery Health aktualisiert.

Der Stopgrund wird aufgezeichnet, zum Beispiel:

- manuell gestoppt;
- automatische Abschaltung;
- Programm geschlossen.

---

## 16. Kapazität in Ah

Während des Tests integriert KP184control den gemessenen Strom über die Zeit.

Vereinfacht:

`Ah += Strom × vergangene Zeit in Stunden`

Dadurch wird die tatsächlich gemessene Entladekapazität aufgebaut.

---

## 17. Energie in Wh

Bei jedem Messpunkt wird zuerst berechnet:

`Leistung = Spannung × Strom`

Danach:

`Wh += Leistung × vergangene Zeit in Stunden`

Damit wird die von der Batterie während des Tests abgegebene Energie berechnet.

---

## 18. Vergleich mit der Werkskapazität

Wenn eine Werkskapazität eingegeben wurde, zeigt die Software:

- gemessene Ah;
- Prozent der Werkskapazität;
- grafischen Fortschrittsbalken.

Beispiel:

- Werk: 30 Ah
- gemessen: 26 Ah

dann beträgt die direkt gemessene Battery Health ungefähr:

`26 / 30 × 100 = 86.7 %`

---

## 19. Geschätzte verbleibende Kapazität

Nach einer **automatischen Abschaltung** kann KP184control, wenn genügend Entladedaten vorhanden sind, die Kapazität schätzen, die theoretisch noch unterhalb der eingestellten Abschaltspannung vorhanden sein könnte.

Dafür verwendet die Software den letzten Teil der tatsächlich gemessenen Entladekurve.

Eine Schätzung wird nur erstellt, wenn die Kurve ausreichend brauchbar ist. In v2.1.0 erfordert dies unter anderem:

- mindestens 30 Messpunkte;
- mindestens 0.5 Ah gemessene Kapazität;
- ausreichende Spannungsänderung in der Kurve.

Die Software zeigt dann unter anderem:

- gemessene Kapazität;
- geschätzte verbleibende Ah;
- geschätzte Gesamtkapazität;
- geschätzte Battery Health.

Dieser Wert ist eine **Schätzung auf Basis der Kurve**, keine direkt gemessene Kapazität.

---

## 20. Diagramme

Während des Tests sind mehrere Diagrammansichten verfügbar:

- Spannung (V)
- Strom (A)
- Kapazität (Ah)
- Energie (Wh)
- Alle Diagramme

Die Diagramme werden während des Tests aktualisiert.

Das Spannungsdiagramm zeigt auch die eingestellte Abschaltspannung als Referenz.

---

## 21. CSV-Protokollierung

Für jeden Test wird automatisch eine CSV-Datei erstellt in:

`Dokumente\KP184-Logs`

Der Dateiname enthält Datum und Uhrzeit.

### Geräteinformationen

Die Datei enthält unter anderem:

- Modellprofil;
- Modell-ID;
- Baudrate;
- CRC-Profil;
- Leseprofil;
- Schreibprofil;
- maximale Gerätespannung;
- maximaler Gerätestrom;
- maximale Geräteleistung.

### Testeinstellungen

Unter anderem:

- Batteriechemie;
- Werkskapazität;
- maximale Batteriespannung;
- Live-Startspannung;
- geschätzte Reihenkonfiguration;
- Lastmodus;
- CC-Strom, CP-Leistung oder CR-Widerstand;
- Abschaltspannung.

### Messzeilen

Jede Messzeile enthält:

- Zeitstempel;
- vergangene Sekunden;
- Spannung;
- Strom;
- Leistung;
- Kapazität;
- Energie.

### Endergebnisse

Nach dem Stoppen werden unter anderem hinzugefügt:

- Stopgrund;
- Startzeit;
- Endzeit;
- Endspannung;
- gemessene Kapazität;
- gemessene Energie;
- Testdauer;
- gemessene Battery Health;
- falls verfügbar: geschätzte verbleibende und gesamte Kapazität.

---

## 22. Sprachen

Die Oberfläche unterstützt:

- Nederlands
- English
- Deutsch

Die gewählte Sprache wird lokal gespeichert und beim nächsten Start wiederhergestellt.

---

## 23. Praktischer Ablauf

Für einen normalen Batterietest:

1. Batterie und KP184 korrekt anschließen.
2. KP184 per USB/seriell mit dem PC verbinden.
3. COM-Port auswählen.
4. Auf **Verbinden** klicken.
5. Richtige Batteriechemie wählen.
6. Werkskapazität eingeben.
7. Maximale vollgeladene Batteriespannung eingeben.
8. Berechnete Reihenkonfiguration prüfen.
9. Automatisch eingetragene Abschaltspannung prüfen.
10. CC, CP oder CR wählen.
11. Eine konservative Last einstellen.
12. Automatische Abschaltung vorzugsweise eingeschaltet lassen.
13. Im CC-Modus vorzugsweise Soft-Start verwenden.
14. START TEST klicken.
15. Batterie, Verkabelung, Steckverbinder und KP184 während des Tests überwachen.
16. STOP TEST verwenden, wenn etwas Unerwartetes passiert.

---

## 24. Wichtiger Sicherheitshinweis

KP184control hilft bei der Prüfung von Einstellungen, kann aber nicht bestimmen, welcher Entladestrom für eine bestimmte Batterie sicher ist.

Die Software kennt zum Beispiel die Gerätegrenzen des bestätigten KP184-Profils, aber nicht automatisch:

- maximalen Dauerstrom jeder Batterie;
- maximalen Strom jedes BMS;
- Leitungsquerschnitt;
- Sicherungswert;
- Steckverbindergrenze;
- Zelltemperatur;
- Batterieschäden oder Verschleiß.

Der Benutzer bleibt daher für sichere Testeinstellungen und Überwachung verantwortlich.

---

## 25. Grenzen von v2.1.0

In dieser Version nicht aktiv:

- CV-Regelung;
- dynamische/Pulslast;
- Innenwiderstandstest;
- OCP-Test;
- native Slew-Rate-Konfiguration;
- programmierbare Lastprofile.

Diese Funktionen können in zukünftigen Versionen untersucht werden.

---

Copyright © 2026 Richard Uilenberg. All rights reserved.
