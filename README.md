Hier sind meine Erkenntnisse aus dem EMS-Bus unserer Buderus-Heizung:

### Eckdaten der Heizung:
- Typ **GB125** mit **30kW Öl**-Brenner
- Regelgerät **MC110**
- Bedieneinheit **RC310** an der Heizung
- Mischermodul **MM50** für zweiten Heizkreis (Fußbodenheizung)
- Warmwasserspeicher mit Ladepumpe, am Kessel angeschlossen
- Baujahr 2018


### Ausrüstung zum Telegramm-Mitschnitt:
- **Hardware**: 
  - Pegelwandlung per [_Level-shifter_](https://github.com/Th3M3/ems-bus-HW)-Platine
  - _NanoPi Neo_ (Level-shifter am UART-Eingang)
- **Software**:
  - Diverse Progrämmchen in Python 2.7
  - serielle Kommunikation mit EMS-Level-shifter (bisher nur lesend):
    - 9600 Baud, 8 Datenbits, 1 Stopbit, ohne Parität
---

### EMS-Bus, Teilnehmer, Telegramme:
- **Hardware**:
  - **Anschluss an der Heizung** erfolgt am Regelgerät (MC110), an der Anschlussklemme für den Raumregler. Die Klemme ist orange und trägt die Bezeichnung 'BUS'. Die Polarität muss nicht beachtet werden.<br>Bei einem Test an der links daneben sitzenden, weißen Klemme (ebenfalls mit 'BUS' bezeichnet, an der der Mischer angeschlossen ist), hat ergeben dass dort ebenfalls die gleichen Telegramme kommen.<br>Somit gibt es einen gemeinsamen Bus für alle Teilnehmer.

  - Die **Spannung auf dem Bus** beträgt je nach Signalzustand ca. 10 oder 15V.<br>Kleine Verbraucher können direkt aus dem Bus gespeist werden.

- **Teilnehmer auf dem Bus**:

  | Adresse  | Bezeichnung                  |
  |:---------|:-----------------------------|
  | **0x08** | MC110 Regelgerät             |
  | **0x10** | RC310 Bedieneinheit          |
  | **0x21** | MM50 Mischer für Heizkreis 2 |

- Die übermittelten Daten werden nach dem Empfang als **Hexadezimal**zahlen dekodiert. Hexzahlen werden üblicherweise mit vorangestelltem `0x` gekennzeichnet. Zur einfacheren Lesbarkeit wird hier diese Kennzeichnung meist weggelassen.

- Auf dem Bus werden EMS und EMSplus-Telegramme übertragen.

- Ein Telegramm besteht aus **Header**, **Nutzdaten**, **CRC**-Prüfcode und **Break**.<br>
  Der **Header** beinhaltet **Sender**adresse, **Ziel**adresse, Telegramm**typ** und einen Daten**offset**.<br>
  Im Vergleich zu einem EMS-Telegramm kennzeichnet ein `ff` in Byte 3 ein EMSplus-Telegramm. Nach dem Datenoffset in Byte 4 folgt dann der EMSplus-Telegrammtyp in Bytes 5 & 6.<br>
  Bei EMSplus sind variable Telegrammlängen möglich und auch schon beobachtet worden.

  Aufbau EMS-Telegramm:
  
  |Byte 1|Byte 2|Byte 3|Byte 4|Byte 5 ... Byte n-2|Byte n-1|Byte n|
  |:----:|:----:|:----:|:----:|:-----------------:|:------:|:----:|
  |Sender|Ziel  |Typ   |Offset|Nutzdaten          |CRC     |BREAK |
    
  Aufbau EMSplus-Telegramm:
  
  |Byte 1|Byte 2|Byte 3|Byte 4|Byte 5 & Byte 6| Byte 7... Byte n-2|Byte n-1|Byte n|
  |:----:|:----:|:----:|:----:|:-------------:|:-----------------:|:------:|:----:|
  |Sender|Ziel  |`ff`  |Offset|EMSplus-Typ    |Nutzdaten          |CRC     |BREAK |

  Mit der **Zieladresse** kann ein Teilnehmer direkt angesprochen werden. Oft senden die Teilnehmer an Ziel `00`, was als Broadcast an alle Teilnehmer zu verstehen ist.<br>
  Setzt der Absender zusätzlich in der Zieladresse das höchste Bit, dann ist dies eine Abfrage nach einen Wert/Wertebereich aus einem Datenblock.<br>

  Der **Offset** referenziert die Nutzdaten im gesamten Datensatz. Ein Offset von 0 gibt z.B. an, dass der erste Wert der Nutzdaten an Offset 0 im gesamten Dateblock steht.<br>
  Bei Lesebefehlen ist der Offset hilfreich, um nur einen Teil eines Datensatzes abzugragen.<br>
  Bei Schreibbefehlen wird über den Offset festgelegt, an welche Stelle der oder die neuen Wert(e) geschrieben werden sollen.

  **CRC** ist ein über Bytes 1 bis n-2 errechnetes Polynom zur Redundanzprüfung beim Empfänger.<br>
  
  **Break** ist eigentlich kein gesendetes Zeichen. Bei einem Break hält der Teilnehmer die Leitung länger als eine Frame-Zeit auf logisch 0 und signalisiert damit das Ende der Übertragung.<br>Auf Grund der fehlenden korrekten Break-Erkennung wird dies jedoch über die serielle Schnittstelle als `00` eingelesen.
  > Obwohl viele EMS-Telegramme im [EMS wiki] dekodiert wurden, tauchen hier weitestgehend neue, nicht dokumentierte Telegramme auf.

### Ablauf:
- Das Regelgerät MC110 ist Bus-Master und fordert zyklisch nacheinander alle möglichen Teilnehmer auf zum Senden auf.<br>
  Dabei sendet der Master die Adresse des abzugragenden Teilnehmers inkl. gesetztem höchsten Bit und nachfolgendem Break (Polling), z.B. `90 00`.<br>Ist der Teilnehmer am Bus, so darf er jetzt senden. Hat er aktuell keine Sendedaten, dann beantwortet das Polling nur mit seiner eigenen Adresse und Break, im Beispiel mit `10 00`.<br>Hat der Teilnehmer jedoch Daten zu senden, dann werden diese gesendet und erst dann sendet der Teilnehmer abschließend seine eigene Adresse und Break und gibt damit den Bus wieder frei fürs Polling des nächsten Teilnehmers.<br>


### Telegramme:
Meine Erkenntnisse aus den Telegrammen habe ich nach Absender und Typ, bzw. nach Funktion gegliedert.
- [zyklische Telegramme von Sender 0x08](Quelle_08.md)
- [zyklische Telegramme von Sender 0x10](Quelle_10.md)
- [zyklische Telegramme von Sender 0x21](Quelle_21.md)
- [Einstellungen der Bedieneinheit RC310](Einstellungen%20der%20Bedieneinheit%20RC310.md)
- [Einstellungen des Regelgeräts MC110](Einstellungen%20des%20Regelgeräts%20MC110.md)

---
### Weiterführende Links und Quellen:
- [EMS wiki]
- [Understanding the EMS Bus of Buderus / Nefit boilers]
- [SmartHome-Projekt EMS-Bus]
- [Buderus Unterlagen Download]


[EMS wiki]: https://emswiki.thefischer.net
[Understanding the EMS Bus of Buderus / Nefit boilers]: https://domoticproject.com/ems-bus-buderus-nefit-boiler
[SmartHome-Projekt EMS-Bus]: http://www.kabza.de/MyHome/EMSbus.html
[Buderus Unterlagen Download]: https://www.buderus.de/de/technische-dokumentation
