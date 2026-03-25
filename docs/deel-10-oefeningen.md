# Programmeren Basis - Deel 10 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Vooraf
Voordien moest je de code van de oplossingen netjes tussen de accolades van de `Main` method plakken om een volledig en werkend programma te bekomen.

In de oefeningen hieronder worden meerdere methods gebruikt, die allen samen met de `Main` method in de `class Program` moeten terechtkomen.

De code in je *Visual Studio* project zal er dus zo moeten uitzien :

```csharp
using System;

namespace D10 {
    class Program {

        // Main method
        // ...

        // Extra method(s)
        // ...

    }
}
```

## Methods
### Oefening D10toonvierkant
Schrijf een `Main` method die een `ToonVierkant` method aanroept.

Deze laatste print op het console scherm een vierkant van *5 op 5* sterretjes (`*`).

```csharp
*****
*****
*****
*****
*****
```

## Methods met een parameter
### Oefening D10toonvierkantparameters
Pas de method `ToonVierkant` aan zodat een parameter `zijde` wordt meegegeven die de afmetingen van het vierkant bepaalt.

## Methods met meerdere parameters
### Oefening D10toonrechthoek
Schrijf een method `ToonRechthoek` met 2 parameters (`breedte` en `hoogte`) die een rechthoek van sterretjes op de console zet.

Het programma vraagt de gebruiker om een `breedte` en een `hoogte`, en toont vervolgens een rechthoek van die afmeting. Indien de gebruiker geen keurige getallen invoert, blijft het programma de vraag herhalen.

Baseer je hierbij op de oplossing van oefening [oefening D06rechthoekopmaat](../deel-06-oefeningen/deel-06-oefeningen.html#_oefening_d06rechthoekopmaat).

D06rechthoekopmaat.

```csharp
Geef de breedte: 10
Geef de hoogte: 3

**********
**********
**********
```

### Oefening D10toonsomtussen
Schrijf een method `ToonSomTussen` met 2 parameters `min` en `max` die de som toont van alle getallen tussen `min` en `max` (grenzen inclusief).

### Oefening D10printreeks
```csharp
static void Main() {
    PrintReeks(10, 15);
    PrintReeks(8, 3);
    PrintReeks(4, 4);
}

... // (1)
```

1.  Vul hier de ontbrekende method(s) aan.

De `PrintReeks` method gaat alle gehele getallen van het *kleinste* naar het *grootste* afdrukken, dit met een stap van *1*.

De uitvoer zou moeten opleveren…​

```csharp
10 > 11 > 12 > 13 > 14 > 15
3 > 4 > 5 > 6 > 7 > 8
4
```

## Methods en arrays
### Oefening D10toongetallenmetmethod
Schrijf een method `ToonGetallen` die een `int[]` als parameter heeft en de inhoud van deze array op de console zet. Dit is een variatie op [oefening D09toongetallenmetjoin](../deel-09-oefeningen/deel-09-oefeningen.html#_oefening_d09toongetallenmetjoin).

Probeer dit uit met volgende code in de `Main` method…​

```csharp
int[] getallen = { 4, 7, 9, 34, 2, 56, 34, 78 };
ToonGetallen(getallen);
```

Het programma toont…​

```csharp
4, 7, 9, 34, 2, 56, 34, 78
```

## Methods als query
### Oefening D10vraaggebruikerompositiefgetal
Pas de oplossing van D10ToonRechthoek aan zodanig dat er een method `VraagGebruikerOmPositiefGetal` gebruikt wordt om de input af te handelen.

Deze method heeft 1 parameter `vraag` en produceert een `int` waarde.

De method stelt de meegegeven vraag, leest van de console en retourneert de ingegeven waarde.

Indien de gebruiker geen getal intypt (bijvoorbeeld *Hallo*) of een negatief getal ingeeft, zal de method de vraag herhalen totdat er een positief getal is ingevoerd.

Indien je gebruiker bijvoorbeeld *hallo*, *10*, *-1* en *3* invoert, ziet de uitvoer er zo uit…​

```csharp
Geef de breedte : hallo
Geef de breedte : 10
Geef de hoogte : -1
Geef de hoogte : 3

**********
**********
**********
```

### Oefening D10bevat
Schrijf een method `Bevat` met 2 parameters, een `string[] woorden` en een `string zoekwoord`.

De method retourneert `true` indien het zoekwoord in de array voorkomt en `false` indien dit niet het geval is.

```csharp
string[] dieren = {"hond", "kat", "olifant", "krokodil"};

Console.WriteLine(Bevat(dieren, "papegaai")); // (1)
Console.WriteLine(Bevat(dieren, "olifant"));  // (2)
```

1.  drukt False af

2.  drukt True af

Herschrijf de oplossing van oefening D09zoekdier zodat deze `Bevat` method gebruikt wordt.

### Oefening D10faculteit
Schrijf een programma dat de gebruiker om een getal vraagt en de faculteit van dat getal afbeeldt.

De faculteit van een getal is het product van alle getalle van 1 t.e.m. dat getal.

Men noteert dit wel eens met een uitroepteken.

Bijvoorbeeld bij invoer van *3*…​

```csharp
Geef een getal : 3
3! is 6
```

Of bij invoer van *5*…​

```csharp
Geef een getal : 5
5! is 120
```

Ter info : de faculteit van *3* is *(1 \* 2 \* 3)* en die van *5* is *(1 \* 2 \* 3 \* 4 \* 5)*.

Voorzie in het programma een method `GetFaculteit` met een parameter van type `int` die een `int` waarde produceert.

### Oefening D10intervaloverlap
Schrijf een method `IsOverlappend` die ons kan vertellen of twee intervallen overlappen.

Een interval is een verzameling getallen, bepaald door een ondergrens en een bovengrens en alle getallen daartussen.

Bijvoorbeeld het interval `[34,39]` bevat 34, 39 en alle getallen daartussen.

De method `IsOverlappend` heeft 4 parameters : twee grenzen van interval 1 en twee grenzen van interval 2. De return value geeft aan met `true/false` of de intervallen minstens 1 gemeenschappelijk getal hebben.

```csharp
static bool IsOverlappend(int minInterval1, int maxInterval1, int minInterval2, int maxInterval2) { ... }
```

Je mag er hierbij van uitgaan dat geldt dat `minInterval1 <= maxInterval1` en `minInterval2 <= maxInterval2`.

Probeer je method uit met de volgende code en controleer of de output klopt :

```csharp
Console.WriteLine( IsOverlappend(3, 6, 4, 10) ); // toont true
Console.WriteLine( IsOverlappend(4, 10, 3, 6) ); // toont true
Console.WriteLine( IsOverlappend(3, 6, 6, 10) ); // toont true
Console.WriteLine( IsOverlappend(6, 10, 3, 6) ); // toont true
Console.WriteLine( IsOverlappend(3, 6, 7, 10) ); // toont false
Console.WriteLine( IsOverlappend(7, 10, 3, 6) ); // toont false
Console.WriteLine( IsOverlappend(6, 6, 7, 7) );  // toont false
Console.WriteLine( IsOverlappend(7, 7, 6, 6) );  // toont false
Console.WriteLine( IsOverlappend(6, 6, 3, 10) ); // toont true
Console.WriteLine( IsOverlappend(3, 10, 6, 6) ); // toont true
Console.WriteLine( IsOverlappend(6, 6, 6, 10) ); // toont true
Console.WriteLine( IsOverlappend(6, 10, 6, 6) ); // toont true
```

### Oefening D10temperatuur
Herschrijf oplossing D02temperatuur (*input Fahrenheit, output Celsius*) zodat een method `ConvertFahrenheitToCelsius` gebruikt wordt.

Deze method heeft een parameter voor de temperatuur in *Fahrenheit* en produceert de temperatuur in *Celsius*.

### Oefening D10dagenfebruari
D1011

Maak zelf een method die antwoord op de vraag hoeveel dagen er in februari zijn van een bepaald jaar.

```csharp
static void Main()
{
    do
    {
        Console.Write("Jaar?: ");
        int jaar = int.Parse(Console.ReadLine());
        Console.WriteLine($"In februari van {jaar} zijn er {...} dagen.");  // (1)
        Console.WriteLine();
    } while (true);
}

... // (2)

static bool IsSchrikkeljaar(int jaartal)
{
    return (jaartal % 400 == 0 || jaartal % 4 == 0 && jaartal % 100 != 0);
}
```

1.  Vervang de `…​` door de nodige method call.

2.  Vervang de `…​` door de nodige method definitie.

Bij invoer van *2016* krijgen we…​

```csharp
Jaar?: 2016
In februari van 2016 zijn er 29 dagen.
```

Bij invoer van *2017* krijgen we…​

```csharp
Jaar?: 2017
In februari van 2017 zijn er 28 dagen.
```

Bij invoer van *2100* krijgen we…​

```csharp
Jaar?: 2100
In februari van 2100 zijn er 28 dagen.
```

### Oefening D10dagen
Breid nu het programma uit.

Zorg ervoor dat de gebruiker ook zelf de maand kan uitkiezen.

Werk met een extra method, die voor eender welke maand zal opleveren hoeveel dagen deze heeft in een bepaald jaar.

```csharp
static void Main()
{
    do
    {
        Console.Write("Maand?: ");
        int maand = int.Parse(Console.ReadLine());
        Console.Write("Jaar?: ");
        int jaar = int.Parse(Console.ReadLine());
        string[] maanden = {"januari", "februari", "maart", "april", "mei", "juni", "juli",
                        "augustus", "september", "oktober", "november", "december"};
        Console.WriteLine($"In {maanden[maand - 1]} van {jaar} zijn er {...} dagen."); // (1)
        Console.WriteLine();
    } while (true);
}

... // (2)

static bool IsSchrikkeljaar(int jaartal)
{
    return (jaartal % 400 == 0 || jaartal % 4 == 0 && jaartal % 100 != 0);
}
```

1.  Vervang de `…​` door de nodige method call.

2.  Vervang de `…​` door de nodige method definitie.

Maak eventueel voor een stuk gebruik van je oplossing van voorgaande oefening.

Bij invoer van *4* en *2017* krijgen we…​

```csharp
Maand?: 4
Jaar?: 2017
In april van 2017 zijn er 30 dagen.
```

Bij invoer van *2* en *2017* krijgen we…​

```csharp
Maand?: 2
Jaar?: 2017
In februari van 2017 zijn er 28 dagen.
```

Bij invoer van *2* en *2016* krijgen we…​

```csharp
Maand?: 2
Jaar?: 2016
In februari van 2016 zijn er 29 dagen.
```
