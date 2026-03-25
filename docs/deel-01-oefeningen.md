# Programmeren Basis - Deel 01 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Oefening D01codepositioneren
In de cursus wordt aangegeven waar je code moet plaatsen om ze uit te voeren. Probeer het eens uit met volgende code…​ .

Kopieer ze en plak ze in op de juist plaats.

```csharp
Random r = new Random();
int getal = r.Next(1, 101);
int gok = 0;
Console.WriteLine("De computer denkt aan een getal van 1 tem 100, kun je raden welk?");
do
{
    Console.Write("Wat gok je?: ");
    string input = Console.ReadLine();
    gok = int.Parse(input);
    if (gok < getal)
    {
        Console.WriteLine("Hoger!");
    }
    else if (gok > getal)
    {
        Console.WriteLine("Lager!");
    }
    else
    {
        Console.WriteLine("Disco!");
    }
} while (gok != getal);
```

Test nu uit, door het programma uit te voeren, wat die code voor ons doet.

> **Opmerking**
>
> Het programma illustreert wat je over een aantal lessen zelf kan schrijven. Naast uitvoer op de console, werken we binnenkort ook met invoer.
>
> Bepaalde code gaan we meermaals laten uitvoeren, en sommige stukjes code zijn voorwaardelijk. Ze worden enkel uitgevoerd indien voldaan is aan bepaalde condities.
>
> In het voorbeeld wordt met een willekeurig getal gewerkt, het eerste gegenereerde willekeurige getal is gebaseerd op de systeemklok. Verderop in de cursus komen we hierop terug.
>
> Zelfs zonder dat we al die onderwerpen hebben behandeld, kan je allicht inschatten welk stukjes code hiervoor verantwoordelijk zijn.
>
> Broncode, of althans grote stukken ervan, is veel programmeertalen vrij goed leesbaar, ook in C# is dat het geval. De set aan gebruikte symbolen is echter indrukwekkend, `=`, (`` , `) ``, `;`, `,`, `"`, `{`, `}`, `<`, `>`, `!`, …​ . Laat je hierdoor niet intimideren, van elk zal snel duidelijk worden wat hun betekennis is.

## Oefening D01centimetersnaarmeters
Vul volgend programma aan met de ontbrekende code.

De code die je toevoegt berekent het volledig aantal meters en het aantal resterende centimers.

```csharp
int lengteInCm = 456;
Console.WriteLine(lengteInCm);
Console.WriteLine("cm = ");

int volledigAantalMeters = (1)
int aantalResterendeCm = (2)

Console.WriteLine(volledigAantalMeters);
Console.WriteLine("m en ");
Console.WriteLine(aantalResterendeCm);
Console.WriteLine("cm");
```

1.  bedenk de nodige formule om de lengte in cm om te zetten naar een volledig aantal meters (uitgedrukt in een geheel getal)

2.  plaats hier de formule die bepaald hoeveel er overschiet nadat het totaal aantal cm verminderd werd met alle volledige meters

De uitvoer van het programma zou moeten zijn…​

```csharp
456
cm =
4
m en
56
cm
```

Wijzig je de *456* in *713* dan krijgen we…​

```csharp
713
cm =
7
m en
13
cm
```

## Oefening D01totalebedrag
Vul volgend programma aan met de ontbrekende code.

De code die je toevoegt berekent eerst het totale bedrag, daarna wordt dit bedrag op de console afgedrukt.

```csharp
int biljettenVan10Euro = 7;
int biljettenVan5Euro = 6;
int muntstukkenVan2Euro = 5;
int muntstukkenVan1Euro = 4;
int muntstukkenVan50Cent = 3;

(1)

(2)
```

1.  bereken het totale bedrag, en schrijf die weg in een nieuwe variabele

2.  druk hier de inhoud van deze variabele af op de console

De uitvoer van het programma zou moeten zijn…​

```csharp
115,5
```

Aan de meegegeven code pas je niets aan. Maar uiteraard zou je een ander getal te zien krijgen indien je de waardes *7*, *6*, *5*, *4* of *3* zou wijzigen.

## Oefening D01bmi
Vul volgend programma aan met de ontbrekende code.

De code die je toevoegt berekent de *body mass index* op basis van een lengte uitgedrukt in cm, en gewicht uitgedrukt in kg.

> **Opmerking**
>
> De body mass index (*bmi*) kan je bepalen door het gewicht in kilogram te delen door het kwadraat van de lengte uitgedrukt in meters.
>
> Het kwadraat van een bepaald getal kan je uiteraard bepalen door dat getal met zichzelf te vermenigvuldigen.

```csharp
int lengteInCm = 182;
int gewichtInKg = 72;

(1)

Console.WriteLine(bmi);
```

1.  bereken het bmi, maak eventueel gebruik van extra variabelen, en zorg ervoor dat de bmi waarde in de juiste variabele terechtkomt

De uitvoer van het programma zou moeten zijn…​

```csharp
21,7365052529888
```

Aan de meegegeven code pas je niets aan. Maar uiteraard zou je een ander getal te zien krijgen indien je de waardes *182* of *72* zou wijzigen.

## Oefening D01waardesomwisselen
Vul volgend programma aan met de ontbrekende code.

De code die je toevoegt wisselt de inhoud van de variabelen om.

```csharp
int a = 5;
int b = 13;

(1)

Console.WriteLine(a);
Console.WriteLine(b);
```

1.  verwissel hier de inhoud van de variabelen `a` en `b`

De bedoeling is dat wel degelijk eerst *13* wordt afgedrukt (de nieuwe inhoud van de `a` variabele, pas daarna *5* (de nieuwe inhoud van de `b` variabele.

```csharp
13
5
```

> **Tip**
>
> Maak eventueel gebruik van extra variabelen.

Aan de meegegeven code pas je niets aan. Maar uiteraard zou je een ander getal te zien krijgen indien je de waardes *5* of *13* zou wijzigen.

## Oefening D01euronaarpound
Vul volgend programma aan met de ontbrekende code.

Maak een programma dat een *Euro* bedrag omzet in *Pound Sterling*.

We vertrekkende van een gekende Euro waarde, en gaan uit van volgende wisselkoers: *1 Euro = 0,88 Pound Sterling*

```csharp
double euroBedrag = 105.4;
(1) (2)

Console.Write(euroBedrag);
Console.Write("EUR = ");
Console.Write(poundBedrag);
Console.Write("GPB");
```

1.  bekijk of je nog variabelen ontbreekt (nog variabelen moet declareren)

2.  ken de juiste waardes toe aan alle variabelen, eventueel gebaseerd op de juist formule

> **Opmerking**
>
> In tegenstelling tot de `WriteLine` method zal de `Write` method na de waarde geen *newline* (lees: *enter*) afdrukken.

De uitvoer van het programma zou moeten zijn…​

```csharp
105.4EUR = 92.75200000000001GPB
```
