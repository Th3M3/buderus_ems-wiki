## zyklische Telegramme von MC110 (Adresse `08`)

- #### Typ 0x07: ?
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`07`**|**`00`**|          |     |                |
  |        |        |        |        |  **5**   |     |                |bisher immer 0x01
  |        |        |        |        |  **6**   |     |                |bisher immer 0x01
  |        |        |        |        |  **8**   |     |                |bisher immer 0x02
  |        |        |        |        | **18**   |     |                |CRC
  |        |        |        |        | **19**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x14: MC110 -> RC310: Antwort UBABetriebszeit
  Antwort auf Anfrage durch RC (`10 88 14 00 03 6e 00`)

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`10`**|**`14`**|**`00`**|          |     |                |
  |        |        |        |        | **5-7**  |     |     min        |Gesamtbetriebszeit
  |        |        |        |        | **8**    |     |                |CRC
  |        |        |        |        | **9**    |     |                |BREAK (0x00)

<br>

- #### Typ 0xbf: UBAErrorMessage
  Telegramwiederholung **alle 10min.** und sofort bei kommendem u. gehendem Ereignis 

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`bf`**|**`00`**|          |     |                |
  |        |        |        |        | **5**    |     |                |bisher immer 0x08 (eigene Adresse?)
  |        |        |        |        | **6**    |     |                |bisher immer 0x85
  |        |        |        |        | **8**    |     |     Dezimal    |bei blockierendem Fehler: bisher immer 0x0a (Störungsklasse, siehe Servicehandbuch) 
  |        |        |        |        | **9**    |     |                |bei blockierendem Fehler: bisher immer 0x11 (evtl. 0x10 = verriegelnder Fehler, 0x11 = blockierender Fehler, 0x12 = Anlagenfehler, 0x13 = zurückgesetzter Anlagenfehler)
  |        |        |        |        | **10**   |     |     ASCII      |Störungscode 0. Zeichen (ggf. 0x0a = Leerzeichen)
  |        |        |        |        | **11**   |     |     ASCII      |Störungscode (Betriebscode) 1. Zeichen
  |        |        |        |        | **12**   |     |     ASCII      |Störungscode (Betriebscode) 2. Zeichen
  |        |        |        |        | **13-14**|     |     Dezimal    |Zusatzcode (Statuscode)
  |        |        |        |        | **29**   |     |                |CRC
  |        |        |        |        | **30**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xd1: UBAOutdoorTempMessage
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`d1`**|**`00`**|          |     |                |
  |        |        |        |        | **5-6**  |     | 0.1 °C         |Außentemperatur
  |        |        |        |        | **7**    |     |                |CRC
  |        |        |        |        | **8**    |     |                |BREAK (0x00)

<br>

- #### Typ 0xe3: ?
  Telegramwiederholung **alle 10sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e3`**|**`00`**|          |     |                |
  |        |        |        |        | **5.**   |**0**|     ja/nein    |?_1 solange Byte 23 = 0x64 (ab ca. 23°° Wechsel von Bit 0 auf...
  |        |        |        |        |          |**2**|     ja/nein    |?_1 solange Byte 23 = 0x00 (...Bit 2. Um ca. 0:30 wieder zurück)
  |        |        |        |        | **7.**   |**0**|     ja/nein    |?_Wärmeanforderung (1 wenn auch 5.0 auf 1; Ansonsten 0)
  |        |        |        |        | **9.**   |**1**|     ja/nein    |?_
  |        |        |        |        | **10.**  |**1**|     ja/nein    |?_Warmwasserbereitung
  |        |        |        |        | **12.**  |**2**|                |?_
  |        |        |        |        | **16-17**|     | 0.1 °C         |Kessel Isttemperatur
  |        |        |        |        | **18**   |     |     %          |Brennerleistung-Ist
  |        |        |        |        | **19**   |     |     %          |bisher immer 0x64 (=100%)
  |        |        |        |        | **20**   |     |     °C         |Kessel max. Temperatur
  |        |        |        |        | **22**   |     |                |?_
  |        |        |        |        | **23**   |     |     %          |?_0x64 = 100%, wenn 5.1 auf 1. Ansonsten 0
  |        |        |        |        | **24**   |     |     °C         |?_Kesselsolltemperatur bei WW-Bereitung
  |        |        |        |        | **25**   |     |                |CRC
  |        |        |        |        | **26**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xe4: UBAMonitorFast
  Telegramwiederholung **ca. alle 3sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e4`**|**`00`**|          |     |                |
  |        |        |        |        | **7**    |     |     ASCII      |Betriebscode 1. Zeichen
  |        |        |        |        | **8**    |     |     ASCII      |Betriebscode 2. Zeichen
  |        |        |        |        | **9-10** |     |     numerisch  |Statuscode
  |        |        |        |        | **11**   |     |     °C         |Kessel Solltemperatur
  |        |        |        |        | **12-13**|     | 0.1 °C         |Kessel-Isttemperatur
  |        |        |        |        | **14**   |     |     %          |0x64 (=100%) im Heizbetrieb, 0x00 bei WW-Bereitung
  |        |        |        |        | **15**   |     |     %          |Brennerleistung-Ist
  |        |        |        |        | **16.**  |**0**|     ja/nein    |?_Flamme da
  |        |        |        |        | **16.**  |**1**|     ja/nein    |?_Tagbetrieb
  |        |        |        |        | **16.**  |**2**|     ja/nein    |?_WW-Bereitung
  |        |        |        |        | **24-25**|     | 0.1 µA         |Flammenstrom
  |        |        |        |        | **28-29**|     | 0.1 °C         |gleich wie 12+13
  |        |        |        |        | **32**   |     |                |CRC
  |        |        |        |        | **33**   |     |                |BREAK (0x00)

  **Erweiterung mit Offset 0x1b**, direkt im Anschluss an `08 00 e4 00`:

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e4`**|**`1b`**|          |     |                |
  |        |        |        |        | **5-6**  |     | 0.1 °C         |?_2. Kesseltemperatur/Ansauglufttemperatur     
  |        |        |        |        | **9-10** |     | 0.1 °C         |Abgastemperatur
  |        |        |        |        | **13**   |     |                |CRC
  |        |        |        |        | **14**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xe5: UBAMonitorSlow
  Telegramwiederholung **bei Änderung in Byte 7, ansonsten alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e5`**|**`00`**|          |     |                |
  |        |        |        |        | **7.**   |**0**|     ja/nein    |Brennstoffventil 1 auf
  |        |        |        |        | **7.**   |**1**|     ja/nein    |?_Brennstoffventil 2 auf
  |        |        |        |        | **7.**   |**2**|     ja/nein    |Gebläse ein
  |        |        |        |        | **7.**   |**3**|     ja/nein    |Zündung ein
  |        |        |        |        | **7.**   |**4**|     ja/nein    |?_Ölvorwärmer ein
  |        |        |        |        | **7.**   |**5**|     ja/nein    |Kesselkreispumpe ein
  |        |        |        |        | **7.**   |**6**|     ja/nein    |3-Wege-Ventil auf WW
  |        |        |        |        | **7.**   |**7**|     ja/nein    |Zirkulationspumpe ein
  |        |        |        |        | **13.**  |**0**|     ja/nein    |?_Feuerungsautomat gestartet
  |        |        |        |        | **15-17**|     |     numerisch  |Brennerstarts (gesamt)
  |        |        |        |        | **18-20**|     |     min        |Brenner Betr.stunden (gesamt)
  |        |        |        |        | **21-23**|     |     min        |?_Brenner Stufe 2 Betr.stunden (gesamt)
  |        |        |        |        | **24-26**|     |     min        |Brenner Betr.stunden durch Heizanforderung (exkl. WW-Bereitung)
  |        |        |        |        | **27-29**|     |     numerisch  |Brennerstarts durch Wärmeanforderung (exkl. WW-Bereitung)
  |        |        |        |        | **30**   |     |     %          |?_Heizkreispumpenmodulation
  |        |        |        |        | **32**   |     |                |CRC
  |        |        |        |        | **33**   |     |                |BREAK (0x00)

  **Erweiterung mit Offset 0x1b**, direkt im Anschluss an `08 00 e5 00`:

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e5`**|**`1b`**|          |     |                |
  |        |        |        |        | **10**   |     |                |CRC
  |        |        |        |        | **11**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xe9: Warmwasser Status
  Telegramwiederholung **alle 10sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`e9`**|**`00`**|          |     |                |
  |        |        |        |        | **5**    |     |     °C         |Warmwasser Sollwert (bei Soll="aus": 0x0a = 10°C)
  |        |        |        |        | **6-7**  |     | 0.1 °C         |Warmwasser Istwert
  |        |        |        |        | **8-9**  |     | 0.1 °C         |Warmwasser Istwert
  |        |        |        |        | **14**   |     |     °C         |Sollwert thermische Desinfektion
  |        |        |        |        | **15**   |     |     °C         |Sollwert tägl. Aufheizung Temperatur
  |        |        |        |        | **17.**  |**0**|     ja/nein    |WW-Bereitung durch Automatikprogramm aktiv
  |        |        |        |        |          |**2**|     ja/nein    |WW-Bereitung durch Einmalladung aktiv
  |        |        |        |        |          |**3**|     ja/nein    |WW-Bereitung durch thermische Desinfektion aktiv
  |        |        |        |        |          |**4**|     ja/nein    |WW-Bereitung aktiv (immer 1 wenn Bit 0, 2 oder 3 auch 1)
  |        |        |        |        | **18.**  |**0**|     ja/nein    |Zirkulation Tagbetrieb (oder durch Einmalladung/th. Desinfektion aktiviert)
  |        |        |        |        |          |**2**|     ja/nein    |Zirkulationspumpe ein
  |        |        |        |        |          |**4**|     ja/nein    |?_WW-Bereitung (bei Wechsel in Tagbetrieb manchmal kurz 0, dann wieder 1)
  |        |        |        |        |          |**5**|     ja/nein    |?_WW-Bereitung
  |        |        |        |        | **19-21**|     |     numerisch  |?_Warmwasserbereitungen
  |        |        |        |        | **22-24**|     |     min        |?_Betr.stunden Warmwasserbereitung
  |        |        |        |        | **25**   |     |                |?_Warmwasserbereitung (0x64 = 100%)
  |        |        |        |        | **28**   |     |                |?_Sollwert von Zeitprogramm (bei Soll="aus": 0x00)
  |        |        |        |        | **30**   |     |     °C         |gleich wie Byte 5
  |        |        |        |        | **31**   |     |                |CRC
  |        |        |        |        | **32**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01d6: ?_UBA Status
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`ff`**|**`00`**|**`01 d6`**|          |     |                |
  |        |        |        |        |           | **7**    |     |                |wechselt zw. 0x00 & 0x01
  |        |        |        |        |           | **12**   |     |                |wechselt zw. 0x00, 0x30, 0x2f, 0xe3
  |        |        |        |        |           | **13**   |     |                |CRC
  |        |        |        |        |           | **14**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x07da: ?Einstellungen
  Telegramm wird 1x gesendet nachdem Wert(e) (händisch) geändert wurden!
  Telegramme kamen zur gleichen Zeit wie 08 00 ea 00

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`ff`**|**`00`**|**`07 da`**|          |     |                |
  |        |        |        |        |           | **17**   |     |                |CRC
  |        |        |        |        |           | **18**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x07e4: ?_UBA Status
  Telegramwiederholung **alle 10sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`08`**|**`00`**|**`ff`**|**`00`**|**`07 e4`**|          |     |                |
  |        |        |        |        |           | **10**   |     |     ASCII      |Betriebscode 1. Zeichen
  |        |        |        |        |           | **11**   |     |     ASCII      |Betriebscode 2. Zeichen
  |        |        |        |        |           | **14**   |     |     °C         |Kessel Solltemperatur bei Heizbetrieb (0x00 bei WW-Bereitung)
  |        |        |        |        |           | **15**   |     |     %          |0x64 (=100%) im Heizbetrieb, 0x00 bei WW-Bereitung oder wenn Nacht
  |        |        |        |        |           | **16**   |     |     °C         |Kessel Solltemperatur bei WW-Bereitung (0x00 bei Heizbetrieb oder wenn Nacht)
  |        |        |        |        |           | **17**   |     |                |CRC
  |        |        |        |        |           | **18**   |     |                |BREAK (0x00)
