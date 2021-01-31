# Komutatory

Podstawowa jednostka to:

![](1.png)

Buforwanie jest wymagane na wejściu wyjścia komutatora oraz na wejściu każdej sekcji.

Jak jest pytanie, że pakiet przechodzi przez bufory w polu, to liczą się tylko bufory sekcyjne.

## 1

<img src="2.png" style="zoom:75%;" />

Ma on:

- 4 wejścia i wyjścia
- 2 sekcje
- 2 bity sterujące

## 2

<img src="3.png" style="zoom:75%;" />

- 8 wejść/wyjścia
- 3 sekcje
- 4 komutatory na sekcje
- 3 bity sterujące
- 12 komutatorów 2x2

## 3

<img src="4.png" style="zoom:50%;" />

- 16 wejść wyjść
- 4 sekcje
- 8 komutatorów na sekcje
- 4 bity sterujące
- 32 komutatory 2x2

# Pytania o te drogi

To jest tak, że liczba sposób na jakie pakiet może przejść przez cały komutator jest równa

liczba_wejść x liczba_wyjść

<span style="color:blue"> 16 x 16 </span>

A jeden komutator w danej sekcji stanowi przecież ułamek

1/liczba_komutatorów_w_sekcji

<span style="color:blue"> 1/8</span>

Więc przez ten komutator przechodzi 

liczba_wejść x liczba_wyjść / liczba_komutatorów_w_sekcji

<span style="color:blue"> 16 x 16 / 8 = 32 </span>

Czasem jest jeszcze dopytane o konkretne wyjście, wyjścia wiadomo są dwa, więc dzielimy liczbę przez 2

<span style="color:blue"> 32 / 2 = 16 </span>

# Koszt

Prawo Moore'a:

Za tą samą cenę za 18 miesięcy możemy kupić dwukrotnie lepszy sprzęt.