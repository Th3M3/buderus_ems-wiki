## Einstellungen der Bedieneinheit RC310

> Im Gegensatz zum MC110 wurden hier vorab keine Einstellungsparameter gesendet, sondern nur geänderte Einstellungen auf dem Bus bekanntgegeben.

<br>

- #### Typ 0x01af/01b0: Einstellungen Heizkreise
  Gesammelter Datensatz aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **00**  |  **ff** |   **00**   | **01 af<br>01 b0** |          |     |             |           |**Heizkreis 1<br>Heizkreis2**
  |            |          |         |         06 |                    | **?**    |     |             | °C        |Sommer/Winnter-Umschaltung / Sommer ab
  |            |          |         |         07 |                    | **?**    |     |             | °C        |Sommer/Winnter-Umschaltung / Betriebsart<br>00= Ständig Sommer (aus)<br>01= Sommerbetrieb ab eingest. Temperatur<br>02= ständig Winter (aus)
  |            |          |         |            |                    | **?**    |     |             |           |CRC
  |            |          |         |            |                    | **?**    |     |             |           |BREAK (0x00)

  **Telegramme mit gleichem Header, aber mit Offset > 00**:

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Bytes (dez) inkl. CRC & Break| Offset/s aus Datensatz |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:---------------------------:|:----------------------:|:--------
  |   **10**   |  **00**  |  **ff** |   **06**   | **01 af<br>01 b0** | 9                           | 06                     |Telegramm wurde bei Änderung des Wertes gesendet
  |   **10**   |  **00**  |  **ff** |   **07**   | **01 af<br>01 b0** | 9                           | 07                     |Telegramm wurde bei Änderung des Wertes gesendet

<br>

- #### Einstellungen Warmwasser
  Gesammelter Datensatz aus den einzelnen Offsets. So als Telegramm noch nicht gesehen!

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Byte (dez)| Bit |Divisor (dez)|  Einheit  |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:--------:|:---:|:-----------:|:---------:|:--------
  |   **10**   |  **00**  |  **ff** |   **00**   |      **01 f5**     |          |     |             |           |
  |            |          |         |         05 |                    | **?**    |     |             |           |thermische Desinfektion / BA (0=manuell ff=nach Zeitprogramm)
  |            |          |         |         06 |                    | **?**    |     |             | Uhrzeit   |thermische Desinfektion / Startzeit (Dezimalwert *4 -> Dezimalzeit)
  |            |          |         |         07 |                    | **?**    |     |             | Wochentag |thermische Desinfektion / Wochentag (0=Mo, 6=So, 7=täglich)
  |            |          |         |            |                    | **?**    |     |             |           |CRC
  |            |          |         |            |                    | **?**    |     |             |           |BREAK (0x00)

  **Telegramme mit gleichem Header, aber mit Offset > 00**:

  |Sender (hex)|Ziel (hex)|Typ (hex)|Offset (hex)|EMS+ Typ (2byte hex)|Bytes (dez) inkl. CRC & Break| Offset/s aus Datensatz |Bemerkung
  |:----------:|:--------:|:-------:|:----------:|:------------------:|:---------------------------:|:----------------------:|:--------
  |   **10**   |  **00**  |  **ff** |   **05**   |      **01 f5**     | 9                           | 05                     |Telegramm wurde bei Änderung des Wertes gesendet
  |   **10**   |  **00**  |  **ff** |   **06**   |      **01 f5**     | 9                           | 06                     |Telegramm wurde bei Änderung des Wertes gesendet
  |   **10**   |  **00**  |  **ff** |   **07**   |      **01 f5**     | 9                           | 07                     |Telegramm wurde bei Änderung des Wertes gesendet

<br>
