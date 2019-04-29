## Einstellungen des Regelgeräts MC110

#### Ablauf bei Ändern von Einstellungen<br>
Telegrammmitschnitt am Beispiel der Einstellung `Notbetrieb Vorlauftemperatur`:

  1. Einstellung an RC310 aufrufen:<br>
    -> RC fragt die Einstellung vom MC110 ab:<br>
    `10 88 f9 00 11 e6 00 00 13 c2 00`.<br>
    Das Regelgerät antwortet umgehend und sendet die Einstellungsparameter:<br>
    `08 10 f9 00 e6 00 00 13 0f 00 00 00 1e 00 00 00 2d 00 00 00 5a 00 00 00 3a 8d 00`<br>
    (Aktualwert= 58°C, min.= 30°C, max= 90°C).

  2. Wert (ungeändert, also weiterhin mit 58°C) an RC speichern:<br>
    -> RC teilt dem Regelgerät den neuen Wert der Einstellung mit:<br>
    `10 08 e6 13 3a f6 00`<br>
    Dieser quittiert den Empfang mit `01 00`.

  3. Danach sendet das Regelgerät den vollständigen Telegrammtyp, bei dem ein Wert geändert wurde:<br>
    `08 00 e6 00 01 5a 1e 5a 64 05 64 00 06 fa 0a 03 05 64 00 00 00 28 00 3a 03 00 00 00 00 00 00 59 00`<br>
    und<br>
    `08 00 e6 1b 09 09 00 22 00`.

<br>

- #### Typ 0xf7: Sichtbar/Unsichtbarschalten von Einstellungen am RC
  ggf. steuert das Regelgerät die Sichtbarkeit von Einstellungen in Abhängigkeit anderer Einstellungen
   
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **00**  |  **f7** |   **00**   |          |     |             |           |
  |            |          |         |            | **5-7**  |     |             | Daten-Typ |Einstellungsdatensatz, z.B.<br>`ff 07 d0`: [ext. Wärmeanforderung][07d0]
  |            |          |         |            |  **8**   |     |             |           |Bitmaske sichtbare Einstellungen (nach Offset des Datensatzes)
  |            |          |         |            |  **9**   |     |             |           |CRC
  |            |          |         |            | **10**   |     |             |           |BREAK (0x00)

<br>

- #### Typ 0xf9: RC -?> MC110: Abfrage Einstellungsparameter
  beim Aufrufen der Einstellung

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **88**  |  **f9** |   **00**   |          |     |             |           |
  |            |          |         |            | **5**    |     |             |           |bisher immer 0x11
  |            |          |         |            | **6-8**  |     |             | Daten-Typ |Einstellungsdatensatz, z.B.<br>`e6 00 00`: [Heizung][e6]<br>`ea 00 00`: [Warmwasser][ea]<br>`ff 07 d0`: [ext. Wärmeanforderung][07d0]
  |            |          |         |            | **9**    |     |             |           |Offset des Einstellungsdatensatzes
  |            |          |         |            | **10**   |     |             |           |CRC
  |            |          |         |            | **11**   |     |             |           |BREAK (0x00)

  <br>

  #### MC110 -> RC310: Antwort Einstellungsparameter
  Antwort auf vorherige Abfrage
   
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **10**  |  **f9** |   **00**   |          |     |             |           |
  |            |          |         |            | **5-8**  |     |             |           |Wiederholung der Bytes 6-9 aus dem vorherigen Abfrage-Telegramm
  |            |          |         |            | **9**    |     |             |           |Bitmaske?
  |            |          |         |            | **13**   |     |             |           |minimal einstellbarer Wert
  |            |          |         |            | **17**   |     |             |           |Standardwert der Einstellung
  |            |          |         |            | **21**   |     |             |           |maximal einstellbarer Wert
  |            |          |         |            | **25**   |     |             |           |aktueller Wert der Einstellung
  |            |          |         |            | **26**   |     |             |           |CRC
  |            |          |         |            | **27**   |     |             |           |BREAK (0x00)

<br>

- #### Typ 0xe6: Einstellungen Heizung
  [e6]:#typ-0xe6-einstellungen-heizung
  Datensatz wird vom MC110 in zwei Telegrammen gesendet nachdem ein Wert daraus geändert wurde
    
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **00**  |  **e6** |   **00**   |          |     |             |           |
  |            |          |         |         00 | **5**    |     |             |           |Heizung ein/aus<br>00= aus<br>01= ein
  |            |          |         |         01 | **6**    |     |             | °C        |Heizung max. Temperatur
  |            |          |         |            | **11**   |     |             | %         |Kesselleistung max.
  |            |          |         |            | **12**   |     |             | %         |Kesselleistung min.
  |            |          |         |            | **13**   |     |             | °K        |feste Brenner-Ausschalthysterese (positiver Wert +6K)
  |            |          |         |            | **14**   |     |             | °K        |feste Brenner-Einschalthysterese (negativer Wert -6K)
  |            |          |         |         0a | **15**   |     |             | min       |Zeitintervall/Taktsperre
  |            |          |         |         0b | **16**   |     |             |           |Konfig. Warmw. am Kessel<br>00= kein WW<br>03= Ladepumpe
  |            |          |         |         13 | **24**   |     |             | °C        |Notbetrieb Vorlauftemperatur
  |            |          |         |         14 | **25**   |     |             |           |Konfig. Heizkr. 1 am Kessel<br>00= kein HK<br>03= eigene Pumpe
  |            |          |         |            | **32**   |     |             |           |CRC
  |            |          |         |            | **33**   |     |             |           |BREAK (0x00)

  **Erweiterung mit Offset 0x1b**, direkt im Anschluss an `08 00 e6 00`:
    
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **00**  |  **e6** |   **1b**   |          |     |             |           |
  |            |          |         |            | **8**    |     |             |           |CRC
  |            |          |         |            | **9**    |     |             |           |BREAK (0x00)

  <br>

  #### RC310 -> MC110: Ändern einer Einstellung: Heizung
  bei Bestätigen/Speichern einer Einstellung an RC
   
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **08**  |  **e6** |   **??**   |          |     |             |           |Offset gibt die Einstellung an, die geändert werden soll
  |            |          |         |            | **5**    |     |             |           |neuer Wert
  |            |          |         |            | **6**    |     |             |           |CRC
  |            |          |         |            | **7**    |     |             |           |BREAK (0x00)
  

<br>

- #### Typ 0xea: Einstellungen Warmwasser
  [ea]:#typ-0xea-einstellungen-warmwasser
  Datensatz wird vom MC110 als Telegramm gesendet nachdem ein Wert daraus geändert wurde
    
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **00**  |  **ea** |   **00**   |          |     |             |           |
  |            |          |         |         05 | **10**   |     |             |           |Warmwasser ein/aus<br>00= aus<br>01=ein
  |            |          |         |         06 | **11**   |     |             | °C        |Sollwert Wassertemperatur
  |            |          |         |         0b | **16**   |     |             |           |Einschalthäufigkeit Zirkulationspumpe<br>01= 1x3Min/h ... 06= 6x3Min/h<br>07= Dauerhaft Ein
  |            |          |         |         0c | **17**   |     |             | °C        |Sollwert thermische Desinfektion
  |            |          |         |         10 | **21**   |     |             | °C        |Sollwert Einmalladung Temperatur
  |            |          |         |         12 | **23**   |     |             | °C        |Sollwert Warmwasser reduziert
  |            |          |         |            | **31**   |     |             |           |CRC
  |            |          |         |            | **32**   |     |             |           |BREAK (0x00)

  <br>

  #### RC310 -> MC110: Ändern einer Einstellung: Warmwasser
  bei Bestätigen/Speichern einer Einstellung an RC
   
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **08**  |  **ea** |   **??**   |          |     |             |           |Offset gibt die Einstellung an, die geändert werden soll
  |            |          |         |            | **5**    |     |             |           |neuer Wert
  |            |          |         |            | **6**    |     |             |           |CRC
  |            |          |         |            | **7**    |     |             |           |BREAK (0x00)
  
<br>

- #### Typ 0x07d0: Einstellungen ext. Wärmeanforderung
  [07d0]:#typ-0x07d0-einstellungen-ext-wärmeanforderung
  Datensatz wird vom MC110 als Telegramm gesendet nachdem ein Wert daraus geändert wurde

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **08**   |  **00**  |  **ff** |   **00**   |      **07 d0**     |          |     |             |           |
  |            |          |         |         00 |                    |  **7**   |     |             |           |Signal ext. Wärmeanforderung<br>00= 0-10V<br>01= ein/aus<br>Bei 01 wird Sichtbarkeit von Sollwerteinstellung durch Telegramm f7 ausgeblendet
  |            |          |         |         01 |                    |  **8**   |     |             |           |Sollwert ext. Wärmeanforderung bei 0-10V<br>00= Vorlauftemperatur<br>01= Leistung
  |            |          |         |            |                    |  **9**   |     |             |           |CRC
  |            |          |         |            |                    | **10**   |     |             |           |BREAK (0x00)

  <br>
  
  #### RC310 -> MC110: Ändern einer Einstellung: ext. Wärmeanforderung
  bei Bestätigen/Speichern einer Einstellung an RC
   
  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **08**  |  **ff** |   **??**   |      **07 d0**     |          |     |             |           |Offset gibt die Einstellung an, die geändert werden soll
  |            |          |         |            |                    |  **7**   |     |             |           |neuer Wert
  |            |          |         |            |                    |  **8**   |     |             |           |CRC
  |            |          |         |            |                    |  **9**   |     |             |           |BREAK (0x00)
