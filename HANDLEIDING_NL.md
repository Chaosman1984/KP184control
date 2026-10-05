# KP184control v2.3.0 — Handleiding

Deze handleiding beschrijft de functies van **KP184control v2.3.0**.

> **Ondersteunde belastingmodi:** CC, CP/CW, CR en CV  
> **Getest apparaatprofiel:** KUNKIN KP184, model-ID `0x0730`  
> **Windows-release:** x64, self-contained

## 1. Doel

KP184control bestuurt een KUNKIN KP184 elektronische belasting voor het uitvoeren, volgen en vastleggen van ontlaadtests. De software toont live spanning, stroom en vermogen, berekent Ah en Wh, maakt grafieken en CSV-logs en kan tests automatisch stoppen.

## 2. Accugegevens en veilige cutoff

Selecteer de accuchemie en vul de juiste maximale volledig-geladen accuspanning in. KP184control gebruikt deze gegevens om de serieconfiguratie en een standaard veilige cutoff te schatten.

> [!WARNING]
> Vul altijd de juiste maximale accuspanning in. Een verkeerde waarde kan leiden tot een verkeerde celconfiguratie en daardoor een onjuiste automatische cutoff.

Beschikbare chemieën zijn Li-ion (NMC/NCA), LiPo, LiFePO4 en NiMH. De fabriekscapaciteit in Ah wordt gebruikt voor voortgang, capaciteitsvergelijking en Battery Health.

## 3. Verbinden met de KP184

Selecteer de juiste COM-poort. KP184control detecteert automatisch ondersteunde communicatie-instellingen en controleert het apparaatprofiel. Het hardwarematig gevalideerde profiel is de KUNKIN KP184 met model-ID `0x0730`.

Voor dit profiel worden de bekende grenzen toegepast:

- maximaal 150 V;
- maximaal 40 A;
- maximaal 400 W.

Een onbekend profiel kan waar mogelijk worden uitgelezen, maar LOAD wordt niet ingeschakeld zolang een veilig schrijfprofiel niet bevestigd is.

## 4. Belastingmodi

### CC — Constant Current

De KP184 trekt een ingestelde constante stroom. Softwarematige soft-start staat standaard aan en bouwt de stroom geleidelijk op.

### CP/CW — Constant Power

De KP184 probeert een constant vermogen te trekken. Omdat `I = P / V`, kan de stroom stijgen wanneer de accuspanning daalt. KP184control controleert daarom ook de verwachte stroom bij de gekozen cutoff.

### CR — Constant Resistance

De KP184 gedraagt zich als een ingestelde weerstand. Voor het geteste profiel is de CR-schaal hardwarematig bevestigd op 0,1 Ω per registerstap.

Bij een **verwachte CR-stroom onder ongeveer 0,15 A** geeft v2.3.0 een waarschuwing, omdat de praktische regelnauwkeurigheid bij zeer lage stroom kan afnemen.

### CV — Constant Voltage

CV is in **v2.3.0 ingeschakeld** voor het geteste KP184-profiel.

Native CV heeft geen apart instelbare hardwarematige stroomlimiet. Daarom voert KP184control extra controles uit en bewaakt de software de startfase sneller. Bij een te hoge gemeten startstroom wordt LOAD uitgeschakeld.

> [!WARNING]
> De softwarebewaking is een extra veiligheidslaag en vervangt geen externe of hardwarematige stroombegrenzing wanneer die voor de test nodig is.

## 5. START en veiligheidscontrole

Voor LOAD ON controleert KP184control onder andere:

- of een geldige live spanning beschikbaar is;
- of het KP184-profiel schrijven veilig toestaat;
- stroom-, spannings- en vermogensgrenzen;
- de verwachte belasting voor de gekozen modus;
- de ingevoerde accu- en cutoffgegevens.

Tijdens een actieve test bewaakt de software de bekende stroom- en vermogenslimieten. Waarschuwingen verschijnen voordat een echte limietoverschrijding tot LOAD OFF leidt.

## 6. Automatisch stoppen

Automatische cutoff staat standaard aan. Daarnaast kan een test worden gestopt op:

- maximale testduur;
- maximale Ah;
- maximale Wh.

De stopreden wordt in de resultaten en CSV vastgelegd.

## 7. USB-verlies en hervatten

Na meerdere opeenvolgende communicatiefouten wordt een actieve testadministratie gepauzeerd en probeert KP184control automatisch opnieuw verbinding te maken. Een test wordt **nooit automatisch hervat**.

Na een succesvolle reconnect bevestigt de software eerst een veilige toestand met LOAD OFF. Daarna kan de gebruiker de test handmatig hervatten.

## 8. Presets en historie

v2.3.0 ondersteunt lokale testpresets voor herbruikbare testinstellingen. Daarnaast wordt een compacte lokale historie van afgeronde tests bijgehouden. De volledige meetpunten blijven in de CSV-bestanden staan.

## 9. CSV, grafieken en rapportage

Nieuwe CSV-logs bevatten onder meer:

- `APP;Naam;KP184control`
- `APP;Versie;v2.3.0`
- apparaat- en communicatiegegevens;
- veiligheidsinformatie;
- testinstellingen;
- meetpunten en gebeurtenissen;
- eindresultaten en stopreden.

De software toont grafieken voor de gemeten testgegevens en kan een PDF-testrapport maken.

## 10. Mobiele/webinterface

KP184control v2.3.0 bevat een lokale webinterface die vanaf een telefoon of ander apparaat op hetzelfde netwerk kan worden gebruikt.

Beschikbare standen:

- **Uit** — webtoegang uitgeschakeld;
- **Alleen lezen** — meekijken zonder bedieningscommando's;
- **Volledige bediening** — ondersteunde instellingen en START/STOP op afstand.

Bedieningsacties gebruiken een lokaal actietoken. Het token kan vanuit Windows worden vernieuwd; bestaande mobiele pagina's moeten daarna opnieuw worden geladen.

De huidige release is bedoeld voor **lokale netwerktoegang**, niet als openbare internetdienst.

## 11. Installatie van v2.3.0

Download bij GitHub Releases:

- `KP184control-v2.3.0-windows-x64.zip`
- `KP184control-v2.3.0-windows-x64-SHA256.txt`

Pak de ZIP uit en start `KP184control.exe`.

De Windows x64-release is **self-contained**. Een losse installatie van de Microsoft .NET Desktop Runtime is voor dit pakket niet nodig.

Gebruik het SHA256-bestand om desgewenst de integriteit van de ZIP te controleren.

## 12. Veiligheid

Accu- en elektronische-belastingtests kunnen hoge stroom, warmte, vonkvorming, BMS-afschakeling en brandgevaar veroorzaken. Gebruik geschikte kabels, connectoren en zekeringen, controleer polariteit en grenzen vóór START en laat een test niet onbeheerd achter.

KP184control is een hulpmiddel en vervangt geen correcte elektrische beveiliging, toezicht of beoordeling van de accuconditie.

## 13. Licentie en broncode

KP184control is proprietary software en geen open-sourceproject. De publieke repository bevat bewust geen C#-broncode. Zie `LICENSE.txt` voor de gebruiksvoorwaarden.
