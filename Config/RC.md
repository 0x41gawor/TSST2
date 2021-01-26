# Konfiguracja RC

## Struktury danych

- routeTable - tabela z kolumnami `src | dst | gateway`
- links - lista połączeń w podsieci, nad którą RC sprawuje opiekę
  - Gdzie Link to obiekt `[port1: port, port2: port, slotsArray: ArrayOf<slots>]`
- connections - lista połączeń w podsieci, żeby RC wiedział jakie dawać slots, dla połączeń zapytanych !pierwszy raz

## Co trzeba wczytać z konfiguracji

Tylko routeTable.

```xml
<routeTable>
	<row> 
		<src>11x</src>
		<dst>141</dst>
		<gateway>115</gateway>
	</row>
	.
	.
	.
</routeTable>
```

Jak je wypełnić można znaleźć w pliku `Tablice Routingu dla RC.md`



Routery strefowe muszą znać nawzajem swoje porty stków NetworkTopology

Tak samo RC podsieciowe każdy musi znać 3 pozostałe.

