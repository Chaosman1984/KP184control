# KP184control v2.1.0 — Uitgebreide handleiding

Deze handleiding beschrijft de functies die in **KP184control v2.1.0** daadwerkelijk beschikbaar zijn.

> **Ondersteunde belastingmodi:** CC, CP/CW en CR  
> **CV:** zichtbaar in de interface, maar bewust uitgeschakeld in v2.1.0  
> **Getest apparaatprofiel:** KUNKIN KP184, model-ID `0x0730`

---

## 1. Doel van KP184control

KP184control bestuurt een KUNKIN KP184 elektronische belasting en is bedoeld voor het uitvoeren, volgen en vastleggen van ontlaadtests.

De software kan onder andere:

- de KP184 automatisch herkennen;
- live spanning, stroom en vermogen tonen;
- ontladen in CC-, CP/CW- en CR-modus;
- Ah en Wh berekenen;
- de ontlaadcurve opslaan en grafisch tonen;
- automatisch stoppen bij een ingestelde stopspanning;
- een veilige standaard-stopspanning berekenen op basis van accuchemie en maximale accuspanning;
- hardwarelimieten controleren voordat een test wordt gestart;
- testresultaten automatisch als CSV opslaan;
- gemeten capaciteit vergelijken met de fabriekscapaciteit;
- na een bruikbare automatische cutoff een schatting van resterende capaciteit maken.

---

## 2. Accugegevens

### Accuchemie

Beschikbare keuzes:

- Li-ion (NMC/NCA)
- LiPo
- LiFePO4
- NiMH

De gekozen chemie wordt gebruikt voor:

1. het schatten van het aantal cellen in serie;
2. de automatische berekening van de veilige standaard-stopspanning;
3. de latere schatting van capaciteit onder de ingestelde cutoff.

### Fabriekscapaciteit

Hier vul je de oorspronkelijke nominale accucapaciteit in Ah in, bijvoorbeeld:

`30.000 Ah`

Dit veld is niet nodig om de KP184 als belasting te laten werken, maar wel voor:

- percentage van fabriekscapaciteit;
- voortgangsbalk;
- Battery Health-weergave;
- schatting van totale capaciteit na automatische cutoff.

### Max. accuspanning

Hier vul je de spanning van de volledig geladen accu in.

Voorbeeld:

`67.200 V`

KP184control gebruikt deze waarde samen met de gekozen chemie om de serieconfiguratie te schatten.

---

## 3. Automatische celconfiguratie

De software deelt de ingevoerde maximale accuspanning door de typische volledig-geladen celspanning en rondt het resultaat af op een geheel aantal cellen.

Gebruikte waarden:

| Chemie | Volledig geladen per cel |
|---|---:|
| Li-ion (NMC/NCA) | 4.20 V |
| LiPo | 4.20 V |
| LiFePO4 | 3.65 V |
| NiMH | 1.45 V |

Voorbeeld:

- chemie: Li-ion
- maximale accuspanning: 67.2 V
- 67.2 / 4.20 = 16

De software toont dan ongeveer:

`Configuratie: 16S | 4.200 V/cell`

---

## 4. Automatische veilige stopspanning

### Belangrijk

In v2.1.0 wordt de **standaard veilige stopspanning niet uit de ingestelde stroom berekend**.

De automatische stopspanning wordt berekend uit:

- accuchemie;
- maximale accuspanning;
- daaruit afgeleid aantal seriecellen.

De ingestelde stroom wordt apart gecontroleerd tegen de stroom- en vermogenslimieten van de KP184.

### Standaard cutoff per cel

| Chemie | Automatische cutoff per cel |
|---|---:|
| Li-ion (NMC/NCA) | 3.10 V |
| LiPo | 3.20 V |
| LiFePO4 | 3.00 V |
| NiMH | 1.00 V |

Voorbeeld voor een 16S Li-ion accu:

`16 × 3.10 V = 49.6 V`

De software vult dan automatisch ongeveer **49.600 V** als stopspanning in.

De gebruiker kan de stopspanning daarna handmatig aanpassen.

---

## 5. Automatisch stoppen bij cutoff

De optie **Automatisch stoppen bij cutoff** staat standaard aan.

Tijdens een actieve test:

1. wacht KP184control de eerste 2 seconden voordat de cutoffbeveiliging actief wordt;
2. wordt de accuspanning ongeveer eenmaal per seconde gecontroleerd;
3. moet de spanning 3 opeenvolgende metingen op of onder de ingestelde cutoff liggen;
4. daarna wordt LOAD OFF gestuurd en de test gestopt.

Dit voorkomt dat één zeer korte spanningsdip meteen de hele test beëindigt.

Als de automatische cutoff handmatig wordt uitgeschakeld, geeft de software eerst een waarschuwing.

---

## 6. Verbinding met de KP184

### COM-poort en adres

Selecteer de COM-poort waarop de KP184 is aangesloten. Het Modbus-adres is instelbaar; standaard wordt adres 1 gebruikt.

### Automatische communicatie-detectie

KP184control probeert automatisch meerdere baudrates:

- 9600
- 115200
- 57600
- 38400
- 19200
- 4800
- 2400

Daarnaast test de software beide gebruikte CRC-bytevolgordes.

### Modeldetectie

Bij model-ID `0x0730` wordt het apparaat als KP184 herkend.

Voor dit bevestigde profiel gebruikt de software de volgende apparaatlimieten:

- maximaal 150 V;
- maximaal 40 A;
- maximaal 400 W.

Als een apparaat wel kan worden uitgelezen maar het schrijfprofiel niet bevestigd is, start KP184control uit veiligheid geen belastingstest.

---

## 7. Live meting

Na verbinding toont de software continu:

- spanning in V;
- stroom in A;
- vermogen in W.

De meting wordt ongeveer eenmaal per seconde bijgewerkt.

Tijdens een test worden dezelfde waarden gebruikt voor:

- capaciteit in Ah;
- energie in Wh;
- CSV-logging;
- grafieken;
- cutoffcontrole.

---

## 8. CC — Constant Current

In CC-modus probeert de KP184 een constante ontlaadstroom te trekken.

Je stelt in:

- ontlaadstroom in ampère;
- stopspanning;
- soft-start aan/uit.

### Controles vóór START

KP184control controleert onder andere:

- stroom moet groter zijn dan 0 A;
- ingestelde stroom mag de modelgrens niet overschrijden;
- `spanning × stroom` mag het maximale KP184-vermogen niet overschrijden.

Voorbeeld:

Bij 66 V en 10 A zou ongeveer 660 W gevraagd worden. Omdat het bevestigde KP184-profiel maximaal 400 W toestaat, wordt de test niet gestart.

De software berekent in zo'n geval ook ongeveer welke stroom bij de actuele spanning nog binnen de 400 W blijft.

---

## 9. CC soft-start

Soft-start staat in v2.1.0 standaard aan.

De soft-start:

1. stelt eerst maximaal 0.100 A in;
2. schakelt LOAD ON;
3. verhoogt daarna de CC-stroom met 0.100 A;
4. wacht 100 ms tussen iedere stap;
5. stopt met verhogen zodra de ingestelde eindstroom bereikt is.

Voorbeeld bij een doel van 1.000 A:

`0.1 → 0.2 → 0.3 → ... → 1.0 A`

Dit vermindert een abrupte belastingssprong bij het starten.

Soft-start kan handmatig worden uitgeschakeld. In dat geval wordt de gekozen CC-stroom direct ingesteld voordat LOAD ON wordt geactiveerd.

---

## 10. CP/CW — Constant Power

In CP/CW-modus probeert de KP184 een constant vermogen te trekken.

Je stelt het gewenste vermogen in watt in.

### Controles vóór START

KP184control controleert:

- vermogen moet groter zijn dan 0 W;
- vermogen mag de modelgrens van de KP184 niet overschrijden;
- de verwachte stroom bij de ingestelde cutoff mag de maximale stroom van de KP184 niet overschrijden.

Bij constant vermogen geldt:

`I = P / V`

Daardoor loopt de stroom op wanneer de accuspanning daalt.

De software controleert daarom specifiek wat de stroom ongeveer wordt bij de gekozen stopspanning.

---

## 11. CR — Constant Resistance

In CR-modus gedraagt de KP184 zich als een ingestelde weerstand.

Je stelt de weerstand in ohm in.

Bij deze modus gelden:

`I = V / R`

en:

`P = V² / R`

### Controles vóór START

De software controleert met de actuele startspanning:

- verwachte startstroom;
- verwacht startvermogen;
- maximale 40 A-grens;
- maximale 400 W-grens.

Als de gekozen weerstand te laag is, wordt de test niet gestart en geeft KP184control een indicatie van de minimale veilige weerstand voor de actuele spanning.

### Hardwarebevestiging

Voor het geteste KP184-profiel is CR hardwarematig bevestigd met een schaal van:

`0.1 Ω per registerstap`

De geschreven CR-instelling wordt vóór LOAD ON ook teruggelezen en gecontroleerd.

---

## 12. CV — Constant Voltage

De CV-tab is zichtbaar, maar **CV is bewust uitgeschakeld in v2.1.0**.

Tijdens ontwikkeling is zowel de native CV-instelling als softwarematige current-limited CV onderzocht. De experimentele regeling werd niet stabiel genoeg bevonden voor opname in de release.

Daarom kan START niet in CV-modus worden gebruikt.

---

## 13. Veiligheidscontroles vóór een test

Naast de controles per belastingmodus voert KP184control algemene controles uit.

### Geldige live spanning

Er moet eerst een geldige live accuspanning zijn gemeten.

### KP184 maximale spanning

Als de gemeten accuspanning boven de maximale spanning van het bevestigde apparaatprofiel ligt, start de test niet.

### Controle ingevoerde max. accuspanning

Als de live accuspanning meer dan **1.5 V hoger** is dan de door de gebruiker ingevoerde maximale accuspanning, blokkeert KP184control de start.

Dit is bedoeld om bijvoorbeeld een verkeerd ingevoerde accuspanning te signaleren.

### Bevestigd schrijfprofiel

Een onbekend apparaatprofiel mag wel worden uitgelezen, maar de software activeert LOAD niet zolang het schrijfprofiel niet als veilig bevestigd is.

---

## 14. START TEST

Bij een geldige start:

1. worden oude meetgegevens gewist;
2. worden Ah en Wh op nul gezet;
3. wordt een nieuw CSV-bestand geopend;
4. wordt LOAD eerst expliciet UIT gezet;
5. wordt de gekozen belastingmodus naar de KP184 geschreven;
6. worden de instellingen geschreven;
7. wordt LOAD aangezet;
8. worden de instellingen tijdens de actieve test vergrendeld.

De STOP-knop wordt actief en START wordt tijdelijk uitgeschakeld.

---

## 15. STOP TEST

Bij STOP:

- wordt LOAD OFF naar de KP184 gestuurd;
- stopt de actieve test;
- wordt het CSV-bestand afgesloten;
- worden eindresultaten aan het CSV-bestand toegevoegd;
- worden de invoervelden opnieuw beschikbaar;
- wordt Battery Health bijgewerkt.

De stopreden wordt vastgelegd als bijvoorbeeld:

- handmatig gestopt;
- automatische cutoff;
- programma afgesloten.

---

## 16. Capaciteit in Ah

Tijdens de test integreert KP184control de gemeten stroom over de tijd.

In vereenvoudigde vorm:

`Ah += stroom × verstreken tijd in uren`

Hierdoor wordt de werkelijk gemeten ontlaadcapaciteit opgebouwd.

---

## 17. Energie in Wh

Op ieder meetmoment wordt eerst berekend:

`vermogen = spanning × stroom`

Daarna:

`Wh += vermogen × verstreken tijd in uren`

Hiermee wordt de afgegeven energie van de accu tijdens de test berekend.

---

## 18. Vergelijking met fabriekscapaciteit

Als een fabriekscapaciteit is ingevuld, toont de software:

- gemeten Ah;
- percentage van fabriekscapaciteit;
- grafische voortgangsbalk.

Voorbeeld:

- fabriek: 30 Ah
- gemeten: 26 Ah

dan is de direct gemeten Battery Health ongeveer:

`26 / 30 × 100 = 86.7 %`

---

## 19. Geschatte resterende capaciteit

Na een **automatische cutoff** kan KP184control, als voldoende ontlaadgegevens beschikbaar zijn, een schatting maken van de capaciteit die theoretisch nog onder de ingestelde cutoff aanwezig zou kunnen zijn.

De software gebruikt daarvoor het laatste deel van de werkelijk gemeten ontlaadcurve.

Een schatting wordt alleen gemaakt als de curve voldoende bruikbaar is. In v2.1.0 vereist dit onder andere:

- minimaal 30 meetpunten;
- minimaal 0.5 Ah gemeten capaciteit;
- voldoende spanningsverandering in de curve.

De software toont dan onder andere:

- gemeten capaciteit;
- geschat resterend Ah;
- geschatte totale capaciteit;
- geschatte Battery Health.

Deze waarde is een **schatting op basis van de curve**, geen rechtstreeks gemeten capaciteit.

---

## 20. Grafieken

Tijdens de test zijn meerdere grafiekweergaven beschikbaar:

- Spanning (V)
- Stroom (A)
- Capaciteit (Ah)
- Energie (Wh)
- Alle grafieken

De grafieken worden tijdens de test bijgewerkt.

De spanningsgrafiek toont ook de ingestelde cutoff als referentie.

---

## 21. CSV logging

Voor iedere test wordt automatisch een CSV-bestand gemaakt in:

`Documenten\KP184-Logs`

De bestandsnaam bevat datum en tijd.

### Apparaatinformatie

Het bestand bevat onder andere:

- modelprofiel;
- model-ID;
- baudrate;
- CRC-profiel;
- leesprofiel;
- schrijfprofiel;
- maximale apparaatspanning;
- maximale apparaatstroom;
- maximaal apparaatvermogen.

### Testinstellingen

Onder andere:

- accuchemie;
- fabriekscapaciteit;
- maximale accuspanning;
- live startspanning;
- geschatte serieconfiguratie;
- belastingmodus;
- CC-stroom, CP-vermogen of CR-weerstand;
- stopspanning.

### Meetregels

Iedere meetregel bevat:

- tijdstip;
- verstreken seconden;
- spanning;
- stroom;
- vermogen;
- capaciteit;
- energie.

### Eindresultaten

Na stoppen worden onder andere toegevoegd:

- stopreden;
- starttijd;
- eindtijd;
- eindspanning;
- gemeten capaciteit;
- gemeten energie;
- testduur;
- gemeten Battery Health;
- indien beschikbaar: geschatte resterende en totale capaciteit.

---

## 22. Talen

De interface ondersteunt:

- Nederlands
- English
- Deutsch

De gekozen taal wordt lokaal opgeslagen en bij een volgende start opnieuw geladen.

---

## 23. Praktische werkwijze

Voor een normale accutest:

1. Sluit accu en KP184 correct aan.
2. Verbind de KP184 via USB/serieel met de pc.
3. Selecteer de COM-poort.
4. Klik **Verbinden**.
5. Kies de juiste accuchemie.
6. Vul de fabriekscapaciteit in.
7. Vul de maximale volledig-geladen accuspanning in.
8. Controleer de berekende serieconfiguratie.
9. Controleer de automatisch ingevulde stopspanning.
10. Kies CC, CP of CR.
11. Vul een conservatieve belasting in.
12. Laat automatische cutoff bij voorkeur aan.
13. Gebruik bij CC bij voorkeur soft-start.
14. Klik START TEST.
15. Houd accu, bekabeling, connectoren en KP184 tijdens de test in de gaten.
16. Gebruik STOP TEST als iets onverwachts gebeurt.

---

## 24. Belangrijke veiligheidsopmerking

KP184control helpt instellingen controleren, maar kan niet bepalen wat de veilige ontlaadstroom van een specifieke accu is.

De software kent bijvoorbeeld de apparaatlimieten van het bevestigde KP184-profiel, maar niet automatisch:

- maximale continue stroom van iedere accu;
- maximale stroom van iedere BMS;
- draaddikte;
- zekeringwaarde;
- connectorlimiet;
- celtemperatuur;
- accuschade of slijtage.

De gebruiker blijft daarom verantwoordelijk voor veilige testinstellingen en toezicht.

---

## 25. Grenzen van v2.1.0

Niet actief in deze release:

- CV-regeling;
- dynamische/pulslast;
- interne-weerstandtest;
- OCP-test;
- native slew-rate configuratie;
- programmeerbare belastingprofielen.

Deze functies kunnen in toekomstige versies worden onderzocht.

---

Copyright © 2026 Richard Uilenberg. All rights reserved.
