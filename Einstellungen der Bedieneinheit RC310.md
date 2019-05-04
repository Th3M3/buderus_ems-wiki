## Einstellungen der Bedieneinheit RC310

> Im Gegensatz zum MC110 wurden hier vorab keine Einstellungsparameter gesendet, sondern nur geänderte Einstellungen auf dem Bus als Broadcast bekanntgegeben.

<br>

- #### Typ 0x019b/019c: Einstellungen Heizkreise
  **Gesammelter Datensatz** aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!<br>
  Telegramm wird mit Offset vom RC310 gesendet nachdem der Wert darin geändert wurde

  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 9b`<br>`01 9c`**|          |     |                |**Heizkreis 1<br>Heizkreis2**
  |        |        |        |     07 |                      | **?**    |     |                |Heizkurve: max. Vorlauftemperatur HK2 (bei EMS-Typ `01 9c` an Offset 07)<br>neuer Wert wird auch in Datensatz `01 a6` an Offset 21 geschrieben
  |        |        |        |     08 |                      | **?**    |     |                |Heizkurve: max. Vorlauftemperatur HK1 (bei EMS-Typ `01 9b` an Offset 08)<br>neuer Wert wird auch in Datensatz `01 a5` an Offset 21 geschrieben
  |        |        |        |        |                      | **?**    |     |                |CRC
  |        |        |        |        |                      | **?**    |     |                |BREAK (0x00)

  <details>
  <summary><b>gesendete Broadcast-Telegramme mit Offset, nach Ändern einer Einstellung</b></summary>

  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      |Bytes ges.|Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:--------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`07`**|             `01 9c`  |  9       |Header + Offset 07 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`08`**|  `01 9b`             |  9       |Header + Offset 08 aus Datensatz + CRC + BREAK
  </details>
  
<br>

- #### Typ 0x01af/01b0: Einstellungen Heizkreise
  **Gesammelter Datensatz** aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!<br>
  Telegramme werden mit Offset vom RC310 gesendet nachdem der Wert darin geändert wurde

  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 af`<br>`01 b0`**|          |     |                |**Heizkreis 1<br>Heizkreis2**
  |        |        |        |     04 |                      | **?**    |     |     °C         |Heizkurve: Auslegungstemperatur HK1 (bei EMS-Typ `01 af` an Offset 04)
  |        |        |        |     05 |                      | **?**    |     |     °C         |Heizkurve: Auslegungstemperatur HK2 (bei EMS-Typ `01 b0` an Offset 05)
  |        |        |        |     06 |                      | **?**    |     |     °C         |Sommer/Winnter-Umschaltung / Sommer ab
  |        |        |        |     07 |                      | **?**    |     |     °C         |Sommer/Winnter-Umschaltung / Betriebsart<br>00= Ständig Sommer (aus)<br>01= Sommerbetrieb ab eingest. Temperatur<br>02= ständig Winter (aus)
  |        |        |        |        |                      | **?**    |     |                |CRC
  |        |        |        |        |                      | **?**    |     |                |BREAK (0x00)

  <details>
  <summary><b>gesendete Broadcast-Telegramme mit Offset, nach Ändern einer Einstellung</b></summary>
  | Sender |  Ziel  |  Typ   | Offset |      EMS+ Typen      |Bytes ges.|Bemerkung
  |:------:|:------:|:------:|:------:|:--------------------:|:--------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`04`**|  `01 af`             |  9       |Header + Offset 05 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`05`**|             `01 b0`  |  9       |Header + Offset 05 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`06`**|  `01 af`<br>`01 b0`  |  9       |Header + Offset 06 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`07`**|  `01 af`<br>`01 b0`  |  9       |Header + Offset 07 aus Datensatz + CRC + BREAK
  </details>
  
<br>

- #### Typ 0x01f5: Einstellungen Warmwasser
  **Gesammelter Datensatz** aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!<br>
  Telegramm wird mit Offset vom RC310 gesendet nachdem der Wert darin geändert wurde

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`00`**|**`ff`**|**`00`**|**`01 f5`**|          |     |                |
  |        |        |        |     05 |           | **?**    |     |                |thermische Desinfektion / BA (0=manuell ff=nach Zeitprogramm)
  |        |        |        |     06 |           | **?**    |     |     Uhrzeit    |thermische Desinfektion / Startzeit (Dezimalwert *4 -> Dezimalzeit)
  |        |        |        |     07 |           | **?**    |     |     Wochentag  |thermische Desinfektion / Wochentag (0=Mo, 6=So, 7=täglich)
  |        |        |        |        |           | **?**    |     |                |CRC
  |        |        |        |        |           | **?**    |     |                |BREAK (0x00)

  <details>
  <summary><b>gesendete Broadcast-Telegramme mit Offset, nach Ändern einer Einstellung</b></summary>

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ |Bytes ges.|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:--------
  |  `10`  |  `00`  |  `ff`  |**`05`**|  `01 f5`  | 9        |Header + Offset 05 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`06`**|  `01 f5`  | 9        |Header + Offset 05 aus Datensatz + CRC + BREAK
  |  `10`  |  `00`  |  `ff`  |**`07`**|  `01 f5`  | 9        |Header + Offset 05 aus Datensatz + CRC + BREAK
  </summary>

<br>




EMS+-Telegramme:
=================

10 00 f7 00 ff 02	20byte				(?<=00 )10 00 f7 00 ff 02.{39}.00	bytes 8 - 18 auf 0xff, wenn Zirkulatios-BA auf 'Eigenes Zeitprogramm' gestellt wird
