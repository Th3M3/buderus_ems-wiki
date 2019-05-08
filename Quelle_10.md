## zyklische Telegramme von RC310 (Adresse `10`)

- #### Typ 0x06: RCTimeMessage
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`06`**|**`00`**|          |     |                |
  |        |        |        |        | **5**    |     |                |+2000 = Jahr
  |        |        |        |        | **6**    |     |                |Monat
  |        |        |        |        | **7**    |     |                |Stunden
  |        |        |        |        | **8**    |     |                |Tag
  |        |        |        |        | **9**    |     |                |Minuten
  |        |        |        |        | **10**   |     |                |Sekunden
  |        |        |        |        | **11**   |     |                |Wochentag (0=Mo...6=So)
  |        |        |        |        | **12.**  |**0**|                |Sommerzeit
  |        |        |        |        | **13**   |     |                |bisher immer 0x10
  |        |        |        |        | **14**   |     |                |bisher immer 0xff
  |        |        |        |        | **16**   |     |                |CRC
  |        |        |        |        | **17**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x14: RC310 -?> MC110: Abfrage UBABetriebszeit
  Telegramwiederholung **alle 1Std.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`88`**|**`14`**|**`00`**|          |     |                |Ziel 08 mit gesetzem höchsten Bits bedeutet 'Polling'
  |        |        |        |        | **5**    |     |                |0x03 = die gewünsche Datenlänge
  |        |        |        |        | **6**    |     |                |CRC
  |        |        |        |        | **7**    |     |                |BREAK (0x00)

<br>

- #### Typ 0xbf: RCErrorMessage
  Telegramwiederholung **alle 10min.** und sofort bei kommendem u. gehendem Ereignis

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`bf`**|**`00`**|          |     |                |
  |        |        |        |        | **5**    |     |                |bisher immer 0x10 (eigene Adresse?)
  |        |        |        |        | **6**    |     |                |Model ID, 0x9e= RC310
  |        |        |        |        | **29**   |     |                |CRC
  |        |        |        |        | **30**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xe7: RC310 -> MC110: WW_Sollwert
  Telegramwiederholung **alle 60sec.**<br>
  Mit diesem Telegramm (de)aktiviert die Bedieneinheit die WW-Bereitung des MC110
  
  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`08`**|**`e7`**|**`00`**|          |     |                |
  |        |        |        |        |  **5**   |     |                |?_Zirkulationspumpe vorhanden? (bisher immer 0x01)
  |        |        |        |        |  **6.**  |**1**|     ja/nein    |?_WW-Bereitung aktiv (ab 5uhr 1, ab 22:59 wieder 0)
  |        |        |        |        |  **6**   |     |     Betriebsart|NOCH ZU PRÜFEN: 00= ständig aus<br>01= ständig ein<br>02= Automatik
  |        |        |        |        | **8**    |     |                |CRC
  |        |        |        |        | **9**    |     |                |BREAK (0x00)

<br>

- #### Typ 0x0167: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 67`**|          |     |                |
  |        |        |        |        |           | **9**    |     |                |CRC
  |        |        |        |        |           | **10**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01a5/0x01a6: ?_Heizkreise
  Datensatz wird in zwei Telegrammen **zyklisch alle 60sec.** gesendet

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 a5`**|          |     |                |Header für **Heizkreis 1**
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 a6`**|          |     |                |Header für **Heizkreis 2**
  |        |        |        |  00-01 |           | **7-8**  |     | 0.1 °C         |Istwert Raumfühler<br>(0x8000 falls Fühler fehlt)
  |        |        |        |     02 |           | **9**    |     |                |wechselt zw. 0x00, 0x01, 0x03 (0x11 wenn Sommer/Winter-Umschaltung auf ständig Sommer gestellt wird)
  |        |        |        |     03 |           | **10**   |     | 0.5 °C         |Raum-Solltemperatur
  |        |        |        |     04 |           | **11**   |     |     °C         |Vorlauftemp. Sollwert
  |        |        |        |     06 |           | **13**   |     | 0.5 °C         |aktuelle Raumtemp. lt. Programm (gleicher Wert wie in Byte 10)
  |        |        |        |     07 |           | **14**   |     | 0.5 °C         |nachfolgende Raumsolltemp lt. Programm (0x00 = Heizen Aus)
  |        |        |        |  08-09 |           | **15-16**|     |     min        |Restzeit bis Betriebsart gewechselt wird (z.B. Heizen > Absenken), max. Anzeige 24h 
  |        |        |        |     0a |           | **17.**  |**0**|                |Betriebsart: 0= manuell, 1= auto
  |        |        |        |     0a |           | **17.**  |**1**|                |Betriebsart: 0= heizen, 1= absenken
  |        |        |        |     0b |           | **18**   |     |                |aktuelle Betriebsart lt. Programm (gleicher Wert wie Byte 17)
  |        |        |        |     0c |           | **19**   |     |                |nachfolgende Betriebsart lt. Programm
  |        |        |        |  0d-0e |           | **20-21**|     |     min        |wie Bytes 15-16
  |        |        |        |  0f-10 |           | **22-23**|     |     min        |verstrichene Zeit in aktueller Betriebsart
  |        |        |        |     13 |           | **26**   |     |                |bisher immer 0x11
  |        |        |        |     14 |           | **27**   |     |                |bisher immer 0x01
  |        |        |        |     15 |           | **28**   |     |                |bei BA Heizen auf 0x03, abends auf 0x00
  |        |        |        |     16 |           | **29**   |     |                |bisher immer 0xff
  |        |        |        |     17 |           | **30**   |     |                |bisher immer 0xff
  |        |        |        |        |           | **32**   |     |                |CRC
  |        |        |        |        |           | **33**   |     |                |BREAK (0x00)

  **Erweiterung mit Offset 0x19**, direkt im Anschluss:<br>

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`19`**|**`01 a5`**|          |     |                |Header für **Heizkreis 1**
  |**`10`**|**`00`**|**`ff`**|**`19`**|**`01 a6`**|          |     |                |Header für **Heizkreis 2**
  |        |        |        |     19 |           | **7**    |     |                |6:00-23:50 auf 0x06, ansonsten HK1 auf 0x05 & HK2 auf 0x04
  |        |        |        |     1a |           | **8**    |     |                |HK1 6:00 auf 0x04, 22:59 auf 0x06, HK2 bisher immer 0x04
  |        |        |        |     1b |           | **9**    |     |                | 
  |        |        |        |     1f |           | **13**   |     |                |bisher immer 0xff
  |        |        |        |     20 |           | **14**   |     |                |HK1 um 6:00 auf 0x64, um 22:59 auf 0x00, HK2 bisher immer 0x64
  |        |        |        |     21 |           | **15**   |     |     °C         |Heizkurve: max. Vorlauftemp<br>HK1 0x4b= 75°C<br>HK2 0x30= 48°C
  |        |        |        |     23 |           | **17**   |     |                |bisher immer 0x3c
  |        |        |        |     24 |           | **18**   |     |                |bisher immer 0x01
  |        |        |        |     25 |           | **19**   |     |                |bisher immer 0xff
  |        |        |        |     26 |           | **20**   |     |                |bisher immer 0x01
  |        |        |        |     27 |           | **21**   |     |                |bisher immer 0x02
  |        |        |        |        |           | **22**   |     |                |CRC
  |        |        |        |        |           | **23**   |     |                |BREAK (0x00)

  <details>
  <summary><b>zusätzliche Telegramme (variable Längen) mit gleichem Header</b> zwischendurch (bei Änderung)</summary>

  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      |  Bytes (inkl. CRC & Break)  |        Offset/s aus Datensatz         |Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:---------------------------:|:-------------------------------------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`02`**|  `01 a5`<br>`01 a6`  | 9                           | 02                                    |Telegramm bei Änderung
  |  `10`  |  `00`  |  `ff`  |**`03`**|  `01 a5`<br>`01 a6`  | 9<br>12<br>13<br>22<br>27   | 03<br>03-06<br>03-07<br>03-10<br>03-15|Telegramm bei BA-Wechsel (durch Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`04`**|  `01 a5`<br>`01 a6`  | 9                           | 04                                    |Telegramm bei Änderung
  |  `10`  |  `00`  |  `ff`  |**`06`**|  `01 a5`<br>`01 a6`  | 10                          | 06-07                                 |Telegramm um 23°° (Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`07`**|  `01 a5`<br>`01 a6`  | 9                           | 07                                    |Telegramm um 23°° (Zeitprogramm), bisher aber nur einmal in 3Tagen aufgezeichnet
  |  `10`  |  `00`  |  `ff`  |**`08`**|  `01 a5`<br>`01 a6`  | 10<br>15<br>17              | 08-09<br>08-0e<br>08-10               |Telegramm alle 5 Min.
  |  `10`  |  `00`  |  `ff`  |**`0a`**|  `01 a5`<br>`01 a6`  | 9                           | 0a                                    |Telegramm um 23°° (Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`0b`**|  `01 a5`<br>`01 a6`  | 10<br>14                    | 0b-0c<br>0b-16                        |bei BA-Wechsel (Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`0d`**|  `01 a5`<br>`01 a6`  | 10<br>12                    | 0d-0e<br>0d-10                        |alle 60sec.
  |  `10`  |  `00`  |  `ff`  |**`0f`**|  `01 a5`<br>`01 a6`  | 10                          | 0f-10                                 |unregelmäßig, alle 1-12min
  |  `10`  |  `00`  |  `ff`  |**`15`**|  `01 a5`<br>`01 a6`  | 9<br>13<br>27               | 15<br>15-19<br>15-27                  |bei BA-Wechsel (durch Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`19`**|  `01 a5`<br>`01 a6`  | 9                           | 19                                    |sporadisch bei Schaltpunkt (Zeitprogramm)
  |  `10`  |  `00`  |  `ff`  |**`1a`**|  `01 a5`<br>`01 a6`  | 9                           | 1a                                    |bei BA-Wechsel d. Zeitprogramm
  |  `10`  |  `00`  |  `ff`  |**`20`**|  `01 a5`<br>`01 a6`  | 9                           | 20                                    |bei BA-Wechsel d. Zeitprogramm
  </details>

<br>

- #### Typ 0x01b9/0x01ba: ?_
  **Gesammelter Datensatz** aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 b9`**|          |     |                |Header für **?_Heizkreis 1**
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 ba`**|          |     |                |Header für **?_Heizkreis 2**
  |        |        |        |     08 |           | **?**    |     |                |bisher immer 0xff
  |        |        |        |     12 |           | **?**    |     |                |bisher immer 0x00
  |        |        |        |        |           | **?**    |     |                |CRC
  |        |        |        |        |           | **?**    |     |                |BREAK (0x00)

  <details>
  <summary><b>zusätzliche Telegramme mit gleichem Header</b> zwischendurch (bei Änderung)</summary>

  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      |  Bytes (inkl. CRC & Break)  |        Offset/s aus Datensatz         |Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:---------------------------:|:-------------------------------------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`08`**|  `01 b9`<br>`01 ba`  | 9                           | 08                                    |bei Zeitschaltpunkt (5:59 & 22:59)
  |  `10`  |  `00`  |  `ff`  |**`12`**|  `01 b9`<br>`01 ba`  | 9                           | 12                                    |bei Zeitschaltpunkt (5:59 & 22:59)
  </details>

<br>

- #### Typ 0x01e0: RC310 -> MC110: HK1_Sollwert
  Telegramwiederholung **alle 60sec.**<br>
  Mit diesem Telegramm steuert die Bedieneinheit den Vorlauf-Sollwert des UBA für Heizkreis 1

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`08`**|**`ff`**|**`00`**|**`01 e0`**|          |     |                |
  |        |        |        |        |           | **7**    |     |                |bisher immer 0x01
  |        |        |        |        |           | **8**    |     |     °C         |?_Vorlauf_Sollwert (6:00 bis 22:59 Werte 0x30 - 0x2f, nachts 0x00)
  |        |        |        |        |           | **9**    |     |     %          |?_Leistungsanforderung (6:00 bis 22:59 0x64, nachts 0x00)
  |        |        |        |        |           | **11**   |     |                |bisher immer 0x01
  |        |        |        |        |           | **12**   |     |                |CRC
  |        |        |        |        |           | **13**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01e2: RC310 -> HK2_Sollwert
  Telegramwiederholung **alle 60sec. oder bei Änderung**<br>
  Mit diesem Telegramm steuert die Bedieneinheit den Vorlauf-Sollwert des Mischers für Heizkreis 2

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`21`**|**`ff`**|**`00`**|**`01 e2`**|          |     |                |
  |        |        |        |        |           |  **7**   |     |                |bisher immer 0x01
  |        |        |        |        |           |  **8**   |     |     °C         |?_Vorlauf_Sollwert (0x17 - 0x25)
  |        |        |        |        |           |  **9**   |     |     %          |?_Leistungsanforderung (bisher immer 0x64)
  |        |        |        |        |           | **11**   |     |                |bisher immer 0x01
  |        |        |        |        |           | **12**   |     |                |CRC
  |        |        |        |        |           | **13**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01ea: RC310 -> MC110: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`08`**|**`ff`**|**`00`**|**`01 ea`**|          |     |                |
  |        |        |        |        |           | **7**    |     |                |sporadisch 2h auf 0x01, ansonsten 0x00
  |        |        |        |        |           | **8**    |     |                |CRC
  |        |        |        |        |           | **9**    |     |                |BREAK (0x00)

<br>

- #### Typ 0x01ec: RC310 -> Mischer: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`21`**|**`ff`**|**`00`**|**`01 ec`**|          |     |                |
  |        |        |        |        |           |  **7**   |     |                |gleich wie 10 08 ff 00 01 ea, Byte 7
  |        |        |        |        |           |  **8**   |     |                |CRC
  |        |        |        |        |           |  **9**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x021d: ?_Warmwasser
  Datensatz wird als Telegramm **zyklisch alle 60sec.** gesendet

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`02 1d`**|          |     |                |
  |        |        |        |     02 |           |  **9**   |     |                |ab 22:59 0x08, ab 4:59 0x0a
  |        |        |        |     03 |           | **10**   |     |                |bisher immer 0x04
  |        |        |        |        |           | **11**   |     |                |CRC
  |        |        |        |        |           | **12**   |     |                |BREAK (0x00)

  <details>
  <summary><b>zusätzliche Telegramme mit gleichem Header</b> zwischendurch (bei Änderung)</summary>

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ |  Bytes (inkl. CRC & Break)  |        Offset/s aus Datensatz         |Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:---------------------------:|:-------------------------------------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`02`**|  `02 1d`  | 9                           | 02                                    |bei Änderung (durch Zeitschaltuhr)
  </details>
