## zyklische Telegramme von MM50 Mischer (Adresse `21`)

- #### Typ 0xbf: MMErrorMessage
  Telegramwiederholung **alle 10min.** und sofort bei kommendem u. gehendem Ereignis

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`00`**|**`bf`**|**`00`**|          |     |                |
  |        |        |        |        |  **5**   |     |                |bisher immer 0x21 (eigene Adresse?)
  |        |        |        |        |  **6**   |     |                |bisher immer 0x9f
  |        |        |        |        | **29**   |     |                |CRC
  |        |        |        |        | **30**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xd2: MM50 -> MC110: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`08`**|**`d2`**|**`00`**|          |     |                |
  |        |        |        |        |  **5-6** |     |     °C         |?_Soll Fußbodentemperatur (exkl. Raumeinfluss-Temp), 0x0000 bei Nacht
  |        |        |        |        |  **7**   |     |                |0x64 im Heizbetrieb, 0x00 bei Nacht
  |        |        |        |        |  **9**   |     |                |bisher immer 0x01
  |        |        |        |        | **10**   |     |                |CRC
  |        |        |        |        | **11**   |     |                |BREAK (0x00)

<br>

- #### Typ 0xe2: MM50 -> MC110: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`08`**|**`e2`**|**`00`**|          |     |                |
  |        |        |        |        |  **6**   |     |                |CRC
  |        |        |        |        |  **7**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x0155: ?_
  Telegramwiederholung **alle 60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`00`**|**`ff`**|**`00`**|**`01 55`**|          |     |                |
  |        |        |        |        |           |  **8**   |     |                |CRC
  |        |        |        |        |           |  **9**   |     |                |BREAK (0x00)

<br>

- #### Typ 0x01d8: Mischer Status
  Telegramwiederholung **60sec.**

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ | Byte Nr. | Bit |Faktor & Einheit|Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:--------:|:---:|:--------------:|:--------
  |**`21`**|**`00`**|**`ff`**|**`00`**|**`01 d8`**|          |     |                |
  |        |        |        |     00 |           |  **7**   |     |                |0x01, regelmäßig für 3min. auf 0x00
  |        |        |        |     01 |           |  **8.**  |**0**|     ja/nein    |Mischer fährt auf (Relais)
  |        |        |        |     01 |           |  **8.**  |**1**|     ja/nein    |Mischer fährt zu (Relais)
  |        |        |        |     02 |           |  **9**   |     |     %          |Mischerposition
  |        |        |        |  03-04 |           | **10-11**|     | 0.1 °C         |Vorlauftemperatur Istwert
  |        |        |        |     05 |           | **12**   |     |     °C         |Vorlauftemperatur Sollwert
  |        |        |        |     06 |           | **13**   |     |                |bisher immer 0x08
  |        |        |        |     08 |           | **15**   |     |                |meist 0x08, gelegentlich 0x04 od. 0x07
  |        |        |        |        |           | **16**   |     |                |CRC
  |        |        |        |        |           | **17**   |     |                |BREAK (0x00)

  **zusätzliche Telegramme (variable Längen) mit gleichem Header** zwischendurch (bei Änderung):

  | Sender |  Ziel  |  Typ   | Offset | EMS+  Typ |  Bytes (inkl. CRC & Break)  |        Offset/s aus Datensatz         |Bemerkung
  |:------:|:------:|:------:|:------:|:---------:|:---------------------------:|:-------------------------------------:|:--------
  |**`21`**|**`00`**|**`ff`**|**`00`**|**`01 d8`**| 9<br>10                     | 00<br>00-01                           |sofort bei Änderung der Werte
  |**`21`**|**`00`**|**`ff`**|**`01`**|**`01 d8`**| 9<br>10                     | 01<br>01-02                           |sofort bei Änderung der Werte
  |**`21`**|**`00`**|**`ff`**|**`02`**|**`01 d8`**| 9                           | 02                                    |sofort bei Änderung der Werte
  |**`21`**|**`00`**|**`ff`**|**`03`**|**`01 d8`**| 10                          | 03-04                                 |sofort bei Änderung der Werte
  |**`21`**|**`00`**|**`ff`**|**`05`**|**`01 d8`**| 9                           | 05                                    |sofort bei Änderung der Werte
  |**`21`**|**`00`**|**`ff`**|**`08`**|**`01 d8`**| 9                           | 08                                    |sofort bei Änderung der Werte

<br>
