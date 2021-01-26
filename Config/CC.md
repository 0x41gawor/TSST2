# Konfiguracja CC

## Struktury danych

- TYPE: enum - Typ CC, czy jest to CC:
  - Strefy
  - Podsieciowe
  - Routerowe
- Port styku CC PeerCoordination innych stref.
- ConnectionControllerByPort: Dict - mapuje numer portu w podsieci, CC, będące kluczem do następnych dwóch słowników
- PeerCoordinationPort: Dict - mapuje nazwę CC na jego port PeerCoordination
- ConnectionRequestPort: Dict - mapuje nazwę CC na jego port RequestConnection

> Czyli gdzieś tam w kodzie, musimy zrobić np. PeerCoordinationPYT do jakiegoś CC. Dostaliśmy od LRM odpowiedź `end` i poszukujemy portu, na który musimy się zwrócić.

```C#
string CC = ConnectionControllerByPort[end.toString];
int port = PeerCoordination[CC]; 
```

### Np. podsieć SN1

|   CC   | ConnectionRequest port | PeerCoordination port |
| :----: | :--------------------: | :-------------------: |
| CC_R11 |         42111          |         42112         |
| CC_R12 |         42111          |         42112         |
| CC_R13 |         42131          |         42132         |
| CC_R14 |         42141          |         42142         |

No i wiadomo, że w tej podsieci wszystkie porty postaci 11x należą do CC_R11. Więc to do niego należy zwrócić się gdy:

- CC_SN11 dostanie zlecenie zestawienia połączenia między portem 11x a jakimkolwiek.
- CC_R1x dostanie od swojego LRM w odpowiedź port 11x, to wtedy wie, gdzie posłać PeerCoordination.

### Tabelka dla wszystkich

|   CC   | ConnectionRequest port | PeerCoordination port |
| :----: | :--------------------: | :-------------------: |
| CC_s1  |         42811          |         42812         |
| CC_s2  |         42821          |         42822         |
| CC_SN0 |         42901          |         42902         |
| CC_SN1 |         42911          |         42912         |
| CC_SN2 |         42921          |         42922         |
| CC_SN3 |         42931          |         42932         |
| CC_R01 |         42011          |         42012         |
| CC_R02 |         42021          |         42022         |
| CC_R03 |         42031          |         42032         |
| CC_R11 |         42111          |         42112         |
| CC_R12 |         42111          |         42112         |
| CC_R13 |         42131          |         42132         |
| CC_R14 |         42141          |         42142         |
| CC_R21 |         42211          |         42212         |
| CC_R22 |         42221          |         42222         |
| CC_R23 |         42231          |         42232         |
| CC_R31 |         42311          |         42312         |
| CC_R32 |         42321          |         42322         |

## Co trzeba wczytać z konfiguracji

Umiejętność poznawania konkretnego CC po porcie.

### CC_s1

CC_s1 musi wiedzieć, dostając `src` z ConnectionRequest od NCC, do którego węzła należy dany port, żeby od niego zacząć zestawianie połączenia.

> Strefa 1

| src  | CC podsieci |
| :--: | :---------: |
| 1xx  |   CC_SN1    |
| 2xx  |   CC_SN2    |
| 0xx  |   CC_SN0    |

### CC_podsieciowe

CC_podsieciowe musi wiedzieć, dostając `src` z ConnectionRequest od CC, do którego węzła należy dany port, żeby od niego zacząć zestawianie połączenia w swojej podsieci.

> SN1

| src  | CC podsieci |
| :--: | :---------: |
| 11x  |   CC_R11    |
| 12x  |   CC_R12    |
| 13x  |   CC_R13    |
| 14x  |   CC_R14    |

Następnie CC po otrzymaniu od RC `gateway`, rezerwuje łącze tranzytowe i dostaje odpowiedź od LRM, jaki jest port na drugim końcu tego łącza, po czym musi poznać do którego CC_podsieci zrobić PeerCoordination.

> Strefa 1

| end  | CC podsieci |
| :--: | :---------: |
| 0xx  |   CC_SN0    |
| 1xx  |   CC_SN1    |
| 2xx  |   CC_SN2    |
| 3xx  |   CC_SN3    |

### CC_routerowe

CC routerowe nie zleca połączenia niżej do CC, więc musi tylko znać swoich peer'ów, żeby wiedzieć to do kogo zrobić PeerCoordination.

> SN1

| end  | CC podsieci |
| :--: | :---------: |
| 11x  |   CC_R11    |
| 12x  |   CC_R12    |
| 13x  |   CC_R13    |
| 14x  |   CC_R14    |