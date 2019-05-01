## Einstellungen des Mischers MM50

> Wie beim MC110 werden beim Aufrufen einer Einstellung die Einstellungsparameter vom Mischer abgefragt.<br>
> Wie beim RC310 sendet der Mischer lediglich die geänderte Einstellung als Broadcast (und nicht den kompletten Einstellungsdatensatz).

<br>

- #### Typ 0xf9: RC -?> MM50: Abfrage Einstellungsparameter
  beim Aufrufen der Einstellung

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`a1`**|**`f9`**|**`00`**|          |     |                |
  |        |        |        |        | **5**    |     |                |0x11 = die gewünsche Datenlänge
  |        |        |        |        | **6-8**  |     |     Daten-Typ  |Einstellungsdatensatz, z.B.<br>`ff 01 ce`: [WW-Vorrang HK2][01ce]
  |        |        |        |        | **9**    |     |                |Offset des Einstellungsdatensatzes
  |        |        |        |        | **10**   |     |                |CRC
  |        |        |        |        | **11**   |     |                |BREAK (0x00)

  <br>
  
  #### MM50 -> RC310: Antwort Einstellungsparameter
  Antwort auf vorherige Abfrage
   
  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`10`**|**`f9`**|**`00`**|          |     |                |
  |        |        |        |        | **5-8**  |     |                |Wiederholung der Bytes 6-9 aus dem vorherigen Abfrage-Telegramm
  |        |        |        |        | **9**    |     |                |Bitmaske?
  |        |        |        |        | **13**   |     |                |minimal einstellbarer Wert
  |        |        |        |        | **17**   |     |                |Standardwert der Einstellung
  |        |        |        |        | **21**   |     |                |maximal einstellbarer Wert
  |        |        |        |        | **25**   |     |                |aktueller Wert der Einstellung
  |        |        |        |        | **26**   |     |                |CRC
  |        |        |        |        | **27**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01ce: Einstellungen WW-Vorrang HK2
  [01ce]:#typ-0x01ce-einstellungen-ww-vorrang-hk2
  Gesammelter Datensatz aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`00`**|**`ff`**|**`00`**|**`01 cc`**|          |     |                |
  |        |        |        |     03 |           | **10**   |     |                |WW-Vorrang Heizkreis 2<br>ff= Ja<br>00= Nein
  |        |        |        |        |           | **??**   |     |                |CRC
  |        |        |        |        |           | **??**   |     |                |BREAK (0x00)

  <br>

  #### RC310 -> MM50: Ändern einer Einstellung: WW-Vorrang HK2
  bei Bestätigen/Speichern einer Einstellung an RC
   
  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`10`**|**`21`**|**`ff`**| **??** |**`01 cc`**|          |     |                |Offset gibt die Einstellung an, die geändert werden soll
  |        |        |        |        |           |  **7**   |     |                |neuer Wert
  |        |        |        |        |           |  **8**   |     |                |CRC
  |        |        |        |        |           |  **9**   |     |                |BREAK (0x00)
