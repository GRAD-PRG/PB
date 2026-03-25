# Programmeren Basis - Deel 07 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Visual Studio project en opstartobject
### Oefening D07opstartobject
Maak voor de oefeningen *Deel 07* in *Visual Studio* een **nieuw *Console App (.NET Core)* project** met de naam ***D07***.

**Hernoem** het door *Visual Studio* opgeleverde **broncode document naar *D07cirkel.cs***.

> **Opmerking: Broncode documenten hernoemen in Visual Studio**
>
> Bestanden van je project hernoemen kan bijvoorbeeld door in de *Solution Explorer* te rechterklikken op een bestand en te kiezen voor **Rename**.
>
> Indien *Visual Studio* voorstelt het corresponderende taalelement te hernoemen mag je hierop ingaan. Dit zal ervoor zorgen dat de `class Program` in dit document ook hernoemt wordt naar `class D07cirkel`.

Pas, indien het nog noodzakelijk is, in je broncodedocument de naam van de klasse aan in `D07cirkel`.

Laat het programma alvast een tekst als *"Dit is de uitvoer van D07cirkel…​"* afdrukken. Neem hiervoor eventueel gewoon volgende code over…​

```csharp
using System;

namespace D07 {

    class D07cirkel {

        static void Main() {
            Console.WriteLine("Dit is de uitvoer van D07cirkel...");
        }

    }

}
```

Voer de applicatie uit en verifiëer of *"Dit is de uitvoer van D07cirkel…​"* wordt afgedrukt.

Voeg een tweede broncode documenten met de naam *D07veelvoudenvan9.cs* toe. Pas de inhoud telkens ook aan naar bovenstaande code, maar pas de naam van de klasse aan tot `D07veelvoudenvan9`.

Laat het programma de tekst *"Dit is de uitvoer van D07veelvoudenvan9…​"* afdrukken. Bijvoorbeeld dus…​

```csharp
using System;

namespace D07 {

    class D07veelvoudenvan9 {

        static void Main() {
            Console.WriteLine("Dit is de uitvoer van D07veelvoudenvan9...");
        }

    }

}
```

Pas bij de project eigenschappen het opstartobject aan om `D07veelvoudenvan9` uit te voeren. Controleer of effectief *"Dit is de uitvoer van D07veelvoudenvan9…​"* wordt afgedrukt.

## String bewerkingen en char datatype
### Een getal omzetten naar een geformatteerde string
#### Oefening D07veelvoudenvan9
Schrijf een programma dat onderstaande output produceert.

Gebruik een for loop om voor elke regel het resultaat te berekenen en formatteer de getallen zoals getoond.

```csharp
 1 x 9 =   9
 3 x 9 =  27
 5 x 9 =  45
 7 x 9 =  63
 9 x 9 =  81
11 x 9 =  99
13 x 9 = 117
15 x 9 = 135
```

#### Oefening D07cirkel
Herschrijf [de oplossing van D03cirkel](../deel-03-oplossingen/deel-03-oplossingen.html#_oplossing_d03cirkel) zodat de omtrek en oppervlakte van de cirkel getoond worden tot op 3 cijfers na de komma.

Een uitvoering waarbij de gebruiker 2.7 invult als straal ziet er zo uit :

```csharp
Geef de straal van een cirkel : 2.7
De omtrek is 16.965.
De oppervlakte is 22.902.
```

### String bewerkingen
#### Oefening D07dotdotdot
Schrijf een programma dat de gebruiker om een tekst vraagt en vervolgens de tekst herhaalt maar een `.` achter elk karakter zet.

```csharp
Geef een tekst : hallo!
h.a.l.l.o.!.
```

#### Oefening D07karakterperpositie
Schrijf een programma dat de gebruiker om tekst vraagt en voor elke positie toont welk karakter op die positie staat.

```csharp
Geef een tekst : programmeren
 0 = p
 1 = r
 2 = o
 3 = g
 4 = r
 5 = a
 6 = m
 7 = m
 8 = e
 9 = r
10 = e
11 = n
```

#### Oefening D07achterstevoren
Schrijf een programma dat de gebruiker om tekst vraagt en vervolgens die tekst achterstevoren weergeeft.

```csharp
Geef een tekst : programmeren
neremmargorp
```

#### Oefening D07bevatleesteken
Schrijf een programma dat de gebruiker om tekst vraagt en dat weergeeft of die tekst minstens 1 leesteken bevat.

Tip : gebruik `Char.isPunctuation()` om te beslissen of een karakter al dan niet een leesteken is.

Hieronder staan twee voorbeeld uitvoeringen om te tonen hoe het er moet uitzien :

```csharp
Geef een tekst : programmeren
De tekst bevat geen enkel leesteken.
```

```csharp
Geef een tekst : Zeg 'ns AAA!
De tekst bevat minstens 1 leesteken.
```

#### Oefening D07begintmethoofdletter
Schrijf een programma dat de gebruiker om tekst vraagt en dat weergeeft of die tekst start met een hoofdletter of kleine letter.

Hieronder staan twee voorbeeld uitvoeringen om te tonen hoe het er moet uitzien :

```csharp
Geef een tekst : programmeren
De tekst start met een kleine letter.
```

```csharp
Geef een tekst : Zeg 'ns AAA!
De tekst start met een hoofdletter.
```

#### Oefening D07aantalkeere
Schrijf een programma dat de gebruiker om tekst vraagt en toont hoeveel keer de letter 'e' voorkomt (hoofdletterongevoelig).

Bijvoorbeeld :

```csharp
Geef een tekst : Edward: "Hello, hello. What's going on? What's all this shouting? We'll have no trouble here."
'e' komt 8 keer voor
```

#### Oefening D07beginmethoofdletters
Schrijf een programma dat de gebruiker om een tekst vraagt en de karakters op de eerste 5 posities omzet naar hoofdletters. Het resultaat wordt op de console getoond.

| Input        | Output       |
|--------------|--------------|
| programmeren | PROGRammeren |
| 12 monkeys   | 12 MOnkeys   |
| lol          | LOL          |

Een voorbeelduitvoering :

```csharp
Geef een tekst : programmeren
PROGRammeren
```

### String bewerkingen (moeilijker)
#### Oefening D07klinkersenmedeklinkers
Schrijf een programma dat de gebruiker om een tekst vraagt en het aantal klinkers en medeklinkers weergeeft.

We beschouwen enkel de letters uit het a-z alfabet met 5 klinkers (a, e, i, o en u).

```csharp
Geef een tekst : Hokey Cokey, pig-in-a-pokey!
9 klinker(s) en 12 medeklinker(s)
```

Hint : het is wellicht het makkelijkst om 2 strings te definieren :

```csharp
string klinkers = "aeiou";
string medeklinkers = "bcdfghjklmnpqrstvwxyz";
```

en voor elk karakter na te gaan of het in deze strings voorkomt.

#### Oefening D07tussenaccolades
Schrijf een programma dat de gebruiker om een tekst vraagt en het deel ervan toont dat tussen accolades staat.

Indien er meerdere teksten tussen accolades staan, wordt enkel naar de eerste `{` en eerste `}` gekeken.

Enkel voorbeeld uitvoeringen :

```csharp
Geef een tekst : De hazen {vliegen laag} vandaag
gevonden : vliegen laag
```

```csharp
Geef een tekst : De hazen {vliegen laag vandaag
niet gevonden
```

```csharp
Geef een tekst : De hazen {vliegen {laag vandaag
niets gevonden
```

```csharp
Geef een tekst : De hazen {vliegen} {laag} vandaag
gevonden : vliegen
```

```csharp
Geef een tekst : De hazen {vliegen {laag}} vandaag
gevonden : vliegen {laag
```

#### Oefening D07zoeken
Schrijf een programma dat de gebruiker om een tekst en een zoekwoord vraagt. Het programma toont hoe vaak het zoekwoord in de tekst voorkomt (hoofdletterongevoelig).

```csharp
Geef een tekst : De man van An haat ambetante verwanten
Geef de zoektekst : an
De zoektekst komt 5 keer voor
```

```csharp
Geef een tekst : De man van An haat ambetante verwanten
Geef de zoektekst :
De zoektekst komt 0 keer voor
```

```csharp
Geef een tekst : abababaabaabb
Geef de zoektekst : ab
De zoektekst komt 5 keer voor
```

```csharp
Geef een tekst : ananas
Geef de zoektekst : ana
De zoektekst komt 2 keer voor
```

```csharp
Geef een tekst : aaaaaa
Geef de zoektekst : aa
De zoektekst komt 5 keer voor
```

Wat vind je eigenlijk zelf van dit laatste voorbeeld? Zou je `3` verwachten of toch eerder `5`?

Probeer dit laatste voorbeeld eens uit in een paar verschillende editoren (notepad, wordpad, word, notepad++, atom, Visual Studio, …​) en kijk telkens of je `5` dan wel `3` zoekresultaten krijgt!

Hiervoor moet je in de editor een nieuw document maken waarin je enkel de tekst `aaaaaa` stopt. Open vervolgens via de menubalk van de editor het zoekvenster. Laat nu de editor zoeken naar `aa` tot het einde van het document bereikt is en tel hoe vaak de tekst gevonden wordt.

#### Oefening D07zoekennavorige
Pas nu je oplossing aan om bij het zoeken naar `aa` in `aaaaaa` toch `3` te bekomen (in plaats van `5` zoals je oplossing van de vorige oefening).

```csharp
Geef een tekst : aaaaaa
Geef de zoektekst : aa
De zoektekst komt 3 keer voor
```

Of bij het zoeken naar `ana` in `ananas` bekomen we deze keer `1` (in plaats van `2` zoals je oplossing van de vorige oefening).

```csharp
Geef een tekst : ananas
Geef de zoektekst : ana
De zoektekst komt 1 keer voor
```

Je weet niet op voorhand hoe groot je zoektekst zal zijn. Maar de lengte van deze zoektekst is wel wat je in rekening moet brengen om te bepalen waar je gaat verder zoeken.
