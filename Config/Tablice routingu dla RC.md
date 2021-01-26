# Adresy klientów

Pamiętajmy, że my adres utożsamiamy z nr portu, którym klient podłączony jest do sieci.

## SN1

| dstName: name | dst: port |
| ------------- | --------- |
| Ania          | 141       |
| Bartek        | 131       |

## SN2

| dstName: name | dst: port |
| ------------- | --------- |
| Janek         | 221       |
| Franek        | 231       |

## SN3

| dstName: name | dst: port |
| ------------- | --------- |
| Abacki        | 333       |
| Babacki       | 335       |
| Cacacki       | 321       |
| Dadacki       | 322       |
| Fafacki       | 323       |

# Routing w podsieci klienckiej

Należy zauważyć, że podsieci tworzą wielobok zupełny. To znaczy, że każdy węzeł z każdym jest połączony.

Kolejna warta uwagi rzecz, to fakt, iż węzeł wyjściowy nigdy nie jest podłączony do żadnego klienta.

Routing w takiej sytuacji jest bardzo prosty. Jakikolwiek węzeł gdy pyta o dst:

- które jest w naszej podsieci zostanie skierowany do węzła, gdzie dst jest jednym z portów
- które jest z poza naszej podsieci zostanie skierowany do routera wyjściowego

# Routing w podsieci tranzytowej

Tu należy zauważyć:

- iż węzły znowu tworzą wielobok zupełny
- że węzeł nadrzędny jakiejś podsieci nigdy nie dostanie od niej pakietu do niej
- że każda podsieć ma dwa węzły nadrzędne, więc do każdej z każdego węzła możliwe są dwie drogi
- każdy węzeł obsługuje 2 podsieci, jeśli przekazuje ruch między nimi, może to zrobić bezpośrednio, jeśli do tej trzeciej musi skorzystać z jednego z węzłów podsieci tranzytowej

Więc tak, gdy węzeł otrzyma pakiet z jakiejś podsieci, to może on być skierowany albo do podsieci, dla której węzeł jest nadrzędny albo do podsieci, dla której nie jest węzłem nadrzędnym. Gdy jest:

- nadrzędny dla podsieci docelowej
  - Ma do wyboru dwie drogi;
    - bezpośrednia o wadze 1
    - pośrednia przez drugi węzeł nadrzędny dla tej podsieci
- NIKIM dla podsieci docelowej
  - Ma do wyboru dwie pośrednie drogi o wadze 2.

Gdy nie ma awarii żadnego z łączy, to wybór nie jest jednoznaczny i router

- W pierwszym przypadku wybiera łącze bezpośrednie o wadze 1.

- W drugim losuje lub wybiera mniej obciążone

>  //TODO tu trzeba pamiętać o zasadzie, że alternatywkę wybieramy tylko raz.

> Przykład R03 dostaje pakiet do SN2, wybiera drogę alternatywną i kieruje pakiet do R02. R02 patrzy, że łącze 022-212 kaputt, też wybiera alternatywkę, więc pakiet leci do R03. pętla rutingowa.jpg

# Routing w strefie

Tu patrzymy na porty_global.png

Należy zauważyć, że połączenia można podzielić na dwa typy:

- wewnątrz podsieciowe
- między podsieciowe

Jeśli pyta węzeł kliencki i połączenie jest typu:

- pierwszego to, RC strefy zawsze odpowie portem z tej samej podsieci i na tym się skończy jego robota
- drugiego to, RC strefy zawsze skieruje połączenie na jeden z dwóch portów wyjściowych do SN0 zgodnie z zasadą podziału obciążeń.

Jeśli pyta węzeł tranzytowy (wtedy tylko typ drugi):

- RC skieruje połączenie na jeden z dwóch łączy (zgodnie z zasadą podziału obciążeń) łączących SN0 z podsiecią, gdzie znajduje się dst.

# Tablice Routingu w stanie nominalnym

Nie ma tutaj dróg alternatywnych. RC nie bierze w ogóle pod uwagę, że sieć może się zapchać.

## SN1

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 11x  | 141  |     115      |
| 12x  | 141  |     122      |
| 13x  | 141  |     132      |
| 14x  | 141  |     141      |
| 11x  | 131  |     114      |
| 12x  | 131  |     121      |
| 13x  | 131  |     131      |
| 14x  | 131  |     144      |
| 11x  | xxx  | 111 \|\| 112 |
| 12x  | xxx  |     123      |
| 13x  | xxx  |     133      |
| 14x  | xxx  |     142      |

## SN2

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 21x  | 221  |     213      |
| 22x  | 221  |     221      |
| 23x  | 221  |     233      |
| 21x  | 231  |     214      |
| 22x  | 231  |     222      |
| 23x  | 231  |     231      |
| 21x  | xxx  | 211 \|\| 212 |
| 22x  | xxx  |     223      |
| 23x  | xxx  |     232      |

## SN3

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 31x  | 321  |     314      |
| 32x  | 321  |     321      |
| 31x  | 322  |     314      |
| 32x  | 322  |     322      |
| 31x  | 323  |     314      |
| 32x  | 323  |     323      |
| 31x  | 313  |     313      |
| 32x  | 313  |     324      |
| 31x  | 315  |     315      |
| 32x  | 315  |     324      |
| 31x  | xxx  | 311 \|\| 312 |
| 32x  | xxx  |     324      |

## SN0

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 01x  | 1xx  |     011      |
| 02x  | 1xx  | 024 \|\| 023 |
| 03x  | 1xx  |     032      |
| 01x  | 2xx  | 013 \|\| 014 |
| 02x  | 2xx  |     022      |
| 03x  | 2xx  |     031      |
| 01x  | 3xx  |     012      |
| 02x  | 3xx  |     021      |
| 03x  | 3xx  | 033 \|\| 034 |

## Strefa 1

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 1xx  | 141  |     141      |
| 1xx  | 131  |     131      |
| 1xx  | xxx  | 111 \|\| 112 |
| 2xx  | 221  |     221      |
| 2xx  | 231  |     231      |
| 2xx  | xxx  | 211 \|\| 212 |
| 0xx  | 1xx  | 011 \|\| 032 |
| 0xx  | 2xx  | 031 \|\| 022 |
| 0xx  | 3xx  | 012 \|\| 021 |

## Strefa 2

| src  | dst  |   gateway    |
| :--: | :--: | :----------: |
| 3xx  | 313  |     313      |
| 3xx  | 321  |     321      |
| 3xx  | 322  |     322      |
| 3xx  | 323  |     323      |
| 3xx  | 335  |     335      |
| 3xx  | xxx  | 312 \|\| 311 |