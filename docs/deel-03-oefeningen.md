# Programmeren Basis - Deel 03 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

### String interpolatie
#### ★ Oefening D03frietjesinterpolatie
Herwerk [de oplossing van D02frietjes](../deel-02-oplossingen/deel-02-oplossingen.html#_oplossing_d02frietjes) zodat string interpolatie gebruikt wordt.

#### ★ Oefening D03leeftijdinterpolatie
Herwerk [de oplossing D02leeftijd](../deel-02-oplossingen/deel-02-oplossingen.html#_oplossing_d02leeftijd) zodat string interpolatie gebruikt wordt.

#### Oefening D03sominterpolatie
Herwerk [de oplossing van D02som](../deel-02-oplossingen/deel-02-oplossingen.html#_oplossing_d02som) zodat string interpolatie gebruikt wordt.

### Werken met getallen
#### Oefening D03getalraden
Schrijf een programma dat een random getal bepaalt tussen 0 en 10 en de gebruiker 1 kans geeft om het te raden. Het programma toont of de gok van de gebruiker juist of fout was.

Een mogelijke uitvoering waarbij het random getal 4 is en de gebruiker 8 gokt :

```csharp
De computer denkt aan een getal tussen 0 en 10.
Welk getal denkt u dat het is : 8
Helaas, het getal was 4!
```

Een mogelijke uitvoering waarbij het random getal 4 is en de gebruiker correct gokt :

```csharp
De computer denkt aan een getal tussen 0 en 10.
Welk getal denkt u dat het is : 4
Proficiat, u heeft goed geraden!
```

#### Oefening D03kleinermetif
Schrijf een programma dat de gebruiker om twee gehele getallen vraagt en toont welk getal het kleinste is. Gebruik hiervoor - bij wijze van oefening - eens een if/else structuur en niet `Math.Min()`!

Een mogelijke uitvoering :

```csharp
Geef een getal : 12
Geef een ander getal : 56
12 is kleiner.
```

#### Oefening D03kleinermetmathmin
Herschrijf het programma uit de vorige oefening zodat dit keer wél `Math.Min()` gebruikt wordt.

#### Oefening D03cirkel
Schrijf een programma dat de gebruiker om de straal van een cirkel (een kommagetal!) vraagt en vervolgens zowel de omtrek als de oppervlakte van de cirkel toont.

Voor een cirkel geldt

-   de omtrek is 2 keer Pi keer de straal

-   de oppervlakte is Pi maal het kwadraat van de straal

#### Oefening D03pythagoras
Schrijf een programma dat de lengte van de schuine zijde berekent van een rechthoekige driehoek.

Het programma vraagt de gebruiker om de basis en de hoogte van de driehoek (beiden kommagetallen) en toont dan de lengte van de schuine zijde.

De lengte van die schuine zijde kun je berekenen met de *stelling van Pythagoras*, je kunt [hier](https://www.calculat.org/nl/oppervlakte-omtrek/stelling-van-pythagoras.html) een voorbeeld uitproberen.

Een uitvoering met basis 3 en hoogte 4 :

```csharp
Geef de basis : 3
Geef de hoogte : 4
De lengte van de schuine zijde is 5
```

#### ★ Oefening D03absolutewaarde
Herwerk [de oplossing van D02absolutewaarde](../deel-02-oplossingen/deel-02-oplossingen.html#_oplossing_d02absolutewaarde) en pas toe wat je in dit deel bijgeleerd hebt.

### Variabelen en scope
#### Oefening D03imperial
Dit programma vraagt de gebruiker om een afstand in feet en inches, en toont dan de equivalente afstand in centimeter.

```csharp
Console.Write("Geef het aantal feet : ");
string aantalFeetAlsTekst = Console.ReadLine();
double aantalFeet = double.Parse(aantalFeetAlsTekst);

Console.Write("Geef het aantal inches : ");
string aantalInchesAlsTekst = Console.ReadLine();
double aantalInches = double.Parse(aantalInchesAlsTekst);

double aantalFeetInCm = aantalFeet * 30.48;
double aantalInchesInCm = aantalInches * 2.54;

double totaalInCm = aantalFeetInCm + aantalInchesInCm;

Console.WriteLine($"Dat is {totaalInCm}cm.");
```

Vervang de *magic values* in dit programma door const variabelen.

#### ★ Oefening D03persecondewijzer
Schrijf een programma dat de gebruiker om een geheel aantal seconden vraagt en toont hoeveel uren, minuten en seconden dit is.

Tip : maak eerst voor jezelf drie voorbeelden op papier, nml.

-   8587 seconden

-   307 seconden

-   57 seconden

### Traceertabellen
#### Oefening D03traceergetalontleden
Neem [de oplossing van D02getalontleden](../deel-02-oplossingen/deel-02-oplossingen.html#_oplossing_d02getalontleden) erbij en nummer de regels in de broncode.

Maak een traceertabel voor de uitvoering waarbij de gebruiker `123` ingeeft.

#### ★ Oefening D03traceeralfabetsoep
Stel een traceertabel op van de uitvoering van onderstaand programma.

```csharp
 1 : int a = 5;
 2 : int b = 10;
 3 : int c = a / b;
 4 : c = b % a;
 5 : c = 4;
 6 : c -= 20;
 7 : b--;
 8 : c = b % a;
 9 : double e = 3.3;
10 : double f = a / b * e;
11 : double g = e * a / b;
12 : double h = 0.3;
13 : h *= 11;
14 : bool t = (a < 8);
15 : bool u = (a != b);
16 : bool v = (e == h);
```

#### ★ Oefening D03traceerpersecondewijzer
Maak de traceertabel voor [de oplossing van D03persecondewijzer](../deel-03-oplossingen/deel-03-oplossingen.html#_oplossing_d03persecondewijzer) hierboven, als de gebruiker `8587` ingeeft voor het aantal seconden.
