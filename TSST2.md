# TSST 2

## Wstemp

Tu są takie początkowe idee tego wszystkiego. Wysyłam, żeby od razu skrytykować te pomysły zanim zaczniemy w to brnąć. Ogólnie strasznie pogmatwane to jest, ten projekt2, duża dowolność, czasem ASON się kłóci z tymi sieciami telefonicznymi, co AT pokazuje. Też zrobiłem tak sieć, żeby były w niej wszystkie bajery, jak poziomy hierarchii, partitioning (to jest na 100% wymagane), role węzłów i nie wiem czy one wszystkie są wymagane, oraz czy niektóre w połączeniu z pomysłami telekomunistów z lat 80 się nieco wykluczają, dlatego nie ma tu żadnego systemu podanego na wykładzie, a jedynie inspirację. To co tu jest łupane, wydaje mi się na ten moment, że jest do zrobienia, mniej więcej jakoś tam każdy problem mam przemyślany jak to może wyglądać, ale bez konkretów.

Wiedza którą tutaj szastam to są wykłady 7_201120, 8_201204, 9_201211. Ale cała teoria jaką z nich używam jest opisana. Dalsze wykłady, wydaje mi się nie są must-have do projektu, a czas nagli, dlatego zacząłem już coś rzeźbić.



Wysyłam taką małą jak na razie porcję, ~~bo więcej nie mam~~ bo:

- Większa szansa na znalezienie jakiś błędów w tym wszystkich, co trzy głowy to nie jedna
- Żebyście się mogli zapoznać z ogólną ideą. Dużo lepiej niż dostać na ryj 15 stron tekstu.
- Nie ma tu opisanych dokładnych zasad sterowania, temat jest do dyskusji i znowu co trzy głowy to nie jedna.

## No to jazda

Opisując taki scenariusz jaki będzie prezentowany na obronie projektu to...

Najpierw klient wybiera ile slotów chce zająć na swoje połączenie oraz adres docelowy.

> W świecie to ma odzwierciedlenie takie, że klient żąda połączenia o przepustowości 100Gb/s.  Czyli widmo musie mieć szerokość 200GHz.
>
> Teraz należy wybrać modulację w zależności od odległości:
>
> - 0-100 km -> 64QAM
> - 101-200 km -> 32QAM
> - 201-300 km -> 16QAM
> - 301-400 km -> 8QAM
> - 401-500 km -> 4QAM
> - 501-600 km -> BPKS
>
> Załóżmy  że adres docelowy jest 217km od klienta, no to wybieramy 16QAM. (!Ale skąd CC/RC wie, ma tablice odległości dla każdej pary klientów?)
>
> Czyli każdy baud/impuls może zakodować naraz 4 bity. Więc potrzebujemy już tak naprawdę pasma o szerokości 50GHz.
>
> Z racji, że szczelina/slot ma 12.5 GHz oraz potrzebujemy jakiś odstęp między innymi połączeniami (W tym przykładzie wszystko na jednej podnośnej!), to połączenie żądane przez klienta zajmie 5 szczelin/slotów, które muszą być obok siebie, na całej długości.
>
> 
>
> Ale myślę, że my sobie pominiemy tą szampańską zabawę i klient będzie wpisywał ile szczelin potrzebuje.

## RSA - Routing and Spectrum Allocation

Zakres długości fal jaki zajmuje jedno połączenie, nie zmienia się na całej drodze danego połączenia.

Czyli tak: III oknie mamy zakres fal 1480-1660nm. 

Załóżmy, że jakieś połączenie ze swojego rutera brzegowego zajęło pasmo (w przeliczeniu na zakres długości fali) 1500-1520nm. To jak po drodze przejdzie ono przez łącze, gdzie już jest połączenie, którego zakres długość fal się pokrywa to jest lipa.

My sobie załóżmy, że na łączu jest dostępnych 100 szczelin. Które sobie ponumerujemy od 0 do 99.

I trzeba będzie robić drogę tak, żeby dwa zakresy (np. 7-12) nie najechały na siebie.

///TODO Trzeba wymyślić na to sposób.

Np. że RC będą wiedziały przecież od LRM jakie są szczeliny zajęte, i tym info można się jakoś powymieniać z sąsiednimi RC.

## Struktura sieci

### Czysta struktura sieci

<img src="img/czysta.png" alt="czysta" style="zoom:100%;" />

Szare to hosty, niebieskie to routery. Niektóre routery są ciemniejsze i nieco większe. W ten sposób zaznaczone są routery tranzytowe.

Co nam wprowadza pierwszy podział węzłów, czyli podział na **płaszczyzny hierarchii**.

> **Płaszczyzny/poziomy hierarchii** opisują kategorie węzłów. Im węzeł jest niżej w hierarchii tym bliżej jest end-usera, im wyżej tym bardziej tranzytuje ruch na coraz większe odległości.
>
> Zasada jest taka, że jeżeli mamy dwa węzły na tym samym poziomie hierarchii, to one z punktu widzenia kierowania ruchu zachowują się bardzo podobnie.
>
> - Jeżeli znajdują się one w tej samej strefie/podsieci, to ruch między nimi jest kierowany bezpośrednio ewentualnie przez jakiś węzeł z tej samej strefy/podsieci z tej samej płaszczyzny hierarchii.
> - Jeżeli są od siebie odległe (inne strefy/podsieci) to korzystam z węzłów hierarchicznie poziomu wyższego.

My zainspirowani sieciami telefonii stacjonarnej z lat 80/90 ustalamy dwa poziomy hierarchii.

### Podsieci i strefy(domeny)

<img src="img/podsieci_i_strefy.png" alt="czysta" style="zoom:100%;" />

>**Strefa/domena*** - służą one po to, by rozdzielić fragmenty sieci. Ponieważ np: należą one do różnych operatorów, są zarządzane przez inny dział danego operatora, pełnią w sieci różne funkcję. W strefę mogą wchodzić węzły różnych poziomów hierarchii.
>
>*(raczej strefa, bo słowo "domena" jest bardzo dobrze umiejscowione w Internecie, ma tam nadane wyraźnie znaczenie, strefa jest bardziej ogólne).

My zakładamy, że strefy należą do różnych operatorów, stąd każda strefa ma swoje NCC i globalne CC.

> **Podsieć** - zbiór węzłów w obrębie jednej strefy o jednakowym poziomie w hierarchii, które są z jakiegoś powodu wyróżnione, czy to szczególna struktura, czy funkcja w sieci.

U nas podsieci są wyróżnione:

- ze względu na strukturę
  - SN1 - wielobok zupełny
  - SN2 - pierścień
- ze względu na funkcję
  - SN0 - obsługuje ruch tranzytowy (między SN1 i SN2 oraz między strefami A i B)
  - SN3 - obsługuje węzły klienckie operatora strefy B

### Role węzłów

<img src="img/role_wezlow.png" alt="czysta" style="zoom:100%;" />

> **Role węzłów** - zdefiniowane są, ażeby łatwiej organizować ruch.
>
> **Węzeł wyjściowy** - zdefiniowany dla podsieci oznacza, że wszystkie połączenia poza tą podsieć muszą prowadzić przez ten węzeł. Pojęcie gateway w Internecie. Oczywiście podsieć może mieć dwa węzły wyjściowe, wtedy połączenia poza podsieć, muszą iść przez jeden lub przez drugi. Można też zdefiniować węzeł wyjściowy dla całej strefy.
>
> **Węzeł nadrzędny** - obsługuje zbiór węzłów, które pod niego podlegają w sensie poziomu hierarchii. Można być węzłem nadrzędnym dla jednego węzła lub dla całej podsieci. Może być tak, że podsieć ma dwa węzły nadrzędne (posiadanie tylko jednego <=> single point of failure).

### Partitioning

> **Partitioning** - hierarchiczny podział sieci na podsieci. Wykorzystywany tam, gdzie nie chcemy lub nie możemy widzieć całej podsieci. Korzystam tylko z wiedzy na temat jej portów.

Potraktujemy naszą sieć globalną jako podsieć, jedną z SNx. Bo globalnie oraz w podsieciach będą obowiązywać te same zasady routingu.

SNy w partitionigu inaczej się przedstawia, ale z uwagi na zdanie wyżej narysuje rutery.

<img src="img/partitioning.png" alt="czysta" style="zoom:100%;" />

## ASON

Na najbardziej globalnej strukturze sieci pokażmy elementy ASON.

<img src="img/ason_global.png" alt="czysta" style="zoom:100%;" />

A teraz jak to wygląda w podsieci.

W tym przypadku SN1

<img src="img/ason_sn.png" alt="czysta" style="zoom:100%;" />

## Zasady sterowania

### Inspiracja nr 1

<img src="img/wyklad.png" alt="czysta" style="zoom:100%;" />*Szablon prezentacji* slajd 67/91 - Cele, kryteria i rozwiązania.

Omawiane na wykładzie 8_201204.

> Rysunek odpowiada sieci optycznej, gdzie nie tylko trzeba znaleźć trasę prowadzącą do adresu docelowego, ale również bezkonfliktowy zakres długości fal, dobrać modulację. 
>
> Zasada (przekładając na terminy z ASON) jest taka:
>
> - W węzłach są tylko CC, funkcja RC jest wyniesiona do pojedynczego komponentu, wspólnego dla całej sieci.
>
> - RC jest to po prostu jakiś serwer, który ma bazę danych, różne algorytmy i interfejsy, na których każdy z CC może zapytać o trasę do określonego adresu.
>
> - Scentralizowane RC często jest nazywane Path Computation Server, żeby podkreślić, że w tej sieci jest oddzielny komponent do wyliczania ścieżek.
>
> - Więc po prostu to jest tak, że CC węzła dostaje przedłużenie ConnectionRequest do jakiegoś adresu, zwraca się więc z prośbą do RC, wysyłając mu adres docelowy. To co może otrzymać to:
>
>   - Cała ścieżka tzw. Explicit Path. Wtedy węzły po drodze nie będą musiały pytać RC drugi raz.
>   - Pierwsze łącze ze ścieżki.
>
>   I to jest chyba, tak że pierwsza opcja to wynik jakiegoś algorytmu, a druga to prosta tablica kierowania połączeń w RC. Ma ona następujące kolumny:
>
>   `który_węzeł_pyta` | `o_jaki_adres_docelowy`  | `które_łącze_mu_odesłać`
>
>   Przykład
>
>   <img src="img/przyklad.png" style="zoom:50%;" />
>
>   CC węzła K dostaje ConnectionRequest do adresu docelowego A. Pyta on więc o drogę RC, które odsyła mu łącze X.
>
>   Na tej podstawie węzeł K zaktualizuję swoją tablice połączeń. Czyli, że ustawi, że teraz sygnał o tym zakresie długości fali (taki jaki było w ConnectionRequest wymagany), który przyszedł na ten port (ten, na który ConnectionRequest przyszło), ma być kierowany na ten port (ten, z którego wychodzi łącze jakie dostaliśmy od RC jako odpowiedź).
>
>   Tablica kierowania połączeń w RC może być wzbogacona czy łącze jest sprawne i wtedy może być kilka opcji dla danej pary węzeł_pytający-adres_docelowy. Mogą one być uporządkowane (wg. info jakie RC dostaje od LRM) lub losowo przydzielane.
>
> Proszę tylko nie mylić tablicy kierowania połączeń z tablicą połączeń. Tą pierwszą ma RC i na jej podstawie wypełnia tablice połączeń w węzłach, czyli ona jest w Control Plane. Ta druga jest w węźle i mówi, o tym jakie połączenia przechodzą przez dany węzeł i jak jest komutować, czyli ona jest w Data Plane, ma wpływ na to co jest ustawione w polu komutacyjnym.
>
>
> Skąd się bierze tablica kierowania połączeń w PCS? 
>
> - Jest MS i ludzie z niego wpisują taką tablicę i wsadzają do RC
> - RC informacjami od LRM, wyobraża sobie topologię i robi sobie tablicę sam
> - Nie ma żadnej tablicy, a odpowiedź to wynik jakiegoś algorytmu.



### Inspiracja nr 2

<img src="img/podzial_obciazen.png" alt="czysta" style="zoom:100%;">

> Wyróżniamy 2 płaszczyzny hierarchii. Każda podsieć ma 2 lub 3 węzły nadrzędne, tranzytowe. Każdy węzeł podsieci jest podłączany do węzłów tranzytowych.
>
>
> Zasada:
>
> Jeśli mamy pokierować połączenie z węzła A do Z to losujemy jeden węzeł tranzytowy przez który to zrobimy. Np. A ma do wyboru dwa łącza.
>
> To jest tzw. **Mechanizm podziału obciążeń**.
>
> Widać, że w płaszczyźnie tranzytowej węzły nadrzędne podsieci lewej mają po 2 łączą do węzłów nadrzędnych sieci prawej, wobec tego tu znowu można zastosować mechanizm podziału obciążeń.
> **Wzajemny przelew**
>
> W węźle X, gdzie jest już tylko jeden wybór, może się okazać, że łącze XZ jest niedostępne (czy to awaria czy przeciążęnie) i wtedy można zrobić tzw. wzajemny przelew, który w tym przypadku skieruje połączenie do węzła Y (do drugiego węzła nadrzędnego) i on zrealzuje połączenie do węzła docelowego.



### Nasze zasady

Z inspiracji 1 bierzemy kierowanie ruchu w podsieci (gdzie sieć globalną też traktujemy jako podsieć).

Z inspiracji 2 bierzemy 2 płaszczyzny hierarchii oraz mechanizm podziału obciążeń (wzajemny przelew sam się zaimplementuje, bo w podsieci tranzytowej robimy inspirację 1). Zmieniamy tą rzecz, że nie wszystkie węzły podsieci są połączone z nadrzędnymi, a jedynie węzeł pełniący rolę węzła wyjściowego.