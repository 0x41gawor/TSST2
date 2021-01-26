# Konfiguracja NCC

## Struktury danych

- Directory.clientPortByClientName: Dict - Serwer Directory ma słownik, mapujący nazwę klienta na port, którym jest dołączony do sieci.

- Port styku ConnectionRequest CC_strefy
- Port styku CallCoordination NCC innych stref

## Co trzeba wczytać z konfiguracji

Wszystko.

### Directory.clientPortByClientName: Dict

| dstName: name | dst: port |
| ------------- | --------- |
| Ania          | 141       |
| Bartek        | 131       |
| Janek         | 221       |
| Franek        | 231       |
| Abacki        | 333       |
| Babacki       | 335       |
| Cacacki       | 321       |
| Dadacki       | 322       |
| Fafacki       | 323       |

### Port styku ConnectionRequest CC_strefy 

Dla NCC_s1: 42811

Dla NCC_s2: 42821

### Port styku CallCoordination NCC innych stref

Dla NCC_s1: 41822

Dla NCC_s2: 41812