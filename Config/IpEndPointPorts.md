# Typy komponentów ASON

## CPCC

### Listenery:

- CallAccept

### Instancje CPCC

Liczba ich wynosi: 9

- CPCC_1419
- CPCC_1319
- CPCC_2319
- CPCC_2219
- CPCC_3139
- CPCC_3159
- CPCC_3219
- CPCC_3229
- CPCC_3239

## NCC

### Listenery:

- ConnectionRequest
- CallCoordination
- CallTeardown

### Instancje NCC:

Liczba ich wynosi 2:

- NCC_s1
- NCC_s2

## CC

### Listenery:

- ConnectionRequest
- PeerCoordination
- CallTeardown

### Instancje CC:

Liczba ich wynosi 18:

- CC_s1
- CC_s2
- CC_SN0
- CC_SN1
- CC_SN2
- CC_SN3
- CC_R01
- CC_R02
- CC_R03
- CC_R11
- CC_R12
- CC_R13
- CC_R14
- CC_R21
- CC_R22
- CC_R23
- CC_R31
- CC_R32

## RC

### Listenery:

- RouteTableQuery
- LocalTopology
- NetworkTopology

### Instancje RC:

Liczba ich wynosi: 6

- RC_s1
- RC_s2
- RC_SN0
- RC_SN1
- RC_SN2
- RC_SN3

## LRM

### Listenery:

- LinkConnectionRequest

### Instancje LRM:

Liczba ich wynosi: 16+5+6+12+6+2 = 47

// Strefa 1

- LRM_111
- LRM_112
- LRM_011
- LRM_032
- LRM_211
- LRM_212
- LRM_031
- LRM_022
- LRM_312
- LRM_311
- LRM_012
- LRM_021
- LRM_141
- LRM_131
- LRM_231
- LRM_221

// Strefa 2

- LRM_313
- LRM_315
- LRM_321
- LRM_322
- LRM_323

// SN0

- LRM_014
- LRM_033
- LRM_034
- LRM_023
- LRM_024
- LRM_013

// SN1

- LRM_142
- LRM_143
- LRM_144
- LRM_132
- LRM_133
- LRM_134
- LRM_123
- LRM_122
- LRM_121
- LRM_115
- LRM_114
- LRM_113

// SN2

- LRM_232
- LRM_233
- LRM_222
- LRM_223
- LRM_213
- LRM_214

// SN3

- LRM_314
- LRM_324

## Podsumowanie

Łącznie do rezerwacji jest:

1x9 + 3x2 + 3x18 + 3x6 + 1x47 = 134 porty

# Konwencje przydzielania portów

Zawłaszczymy sobie zakres: 40 000 - 49 151

## Ogólnie

No więc jest sobie kombinacja pięciu cyfr.

- Pierwsza to 1
- Druga identyfikuje typ komponentu ASON:
  - 1 - NCC
  - 2 - CC
  - 3 - RC
  - 4 - CPCC
  - 5 - LRM
- Trzecia i czwarta to identyfikator konkretnego komponentu wewnątrz klasy komponentów
  - Tu zasady są różne dla różnych klas komponentów
- Piąta konkretny styk, który komponent oferuje

## NCC

Numer portu dla NCC w ogólności wygląda tak:

**11xyz**

Gdzie:

- xy, to id konkretnej instancji:
  - NCC_s1 --> xy = 81
  - NCC_s2 --> xy = 82
- z, to id konkretnego styku:
  - ConnectionRequest --> z= 1
  - CallCoordination --> z = 2
  - CallTeardown --> z = 3

> Przykład 41812
>
> Styk CallCoordination komponentu NCC_s1.

## CC

Numer portu dla CC w ogólności wygląda tak:

**12xyz**

Gdzie:

- xy, to id konkretnej instancji

  - CC_strefy

    - x=8,  y = numer strefy

    - > Np. dla CC_s1, xy = 81

  - CC_podsieciowe

    - x=9, y = numer podsieci

    - > Np. dla CC_SN0, xy =90

  - CC_routerowe

    - xy = id rutera

    - > Np. dla CC_R12, xy = 12

- z, to id konkretnego styku:

  - ConnectionRequest --> z = 1
  - PeerCoordination --> z = 2
  - CallTeardown -->  z = 3

> Przykład 42812
>
> Styk PeerCoordination komponentu CC_s1

> Przykład 42913
>
> Styk CallTeardown komponentu CC_SN1

> Przykład 42222
>
> Styk PeerCoordination komponentu CC_R22

//TODOTO wprowadza ograniczenie na 8 podsieci!!!

## RC

Numer portu dla RC w ogólności wygląda tak:

**13xyz**

Gdzie:

- xy, to id konkretnej instancji

  - RC_strefy

    - x=8, y = numer strefy

    - > Np. dla RC_s2, xy = 82

  - RC_podsieciowe

    - x=9, y= numer podsieci

    - > Np. dla RC_SN3, xy = 93

- z to id konkretnego styku

  - RouteTableQuery --> z = 1
  - LocalTopology --> z = 2
  - NetworkTopology --> z = 3

> Przykład 43813
>
> Styk NetworkTopology komponentu RC_s1

> Przykład 43911
>
> Styk RouteTableQuery komponentu RC_SN1

## CPCC

Numer portu dla CPCC w ogólności wygląda tak:

**14xyz**

Gdzie:

- xyz, to nr portu (tego sieciowego naszego), którym CPCC jest podłączony do sieci.

> Przykład 44221
>
> Styk CallAccept komponentu CPCC_2219 (Janek)

## LRM

Numer portu dla LRM w ogólności wygląda tak:

**15xyz**

Gdzie:

- xyz, to id LRM

> Przykład 45221
>
> Styk LinkConnectionRequest komponentu LRM_221.