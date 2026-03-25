# Programmeren Basis - Deel 08 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Array-variabelen en initializers
### Oefening D08feestdagen
Vul de meegegeven code aan met de ontbrekende code.

Je moet een array-variabele declareren, en de array initialiseren.

Kies de lengte zo uit dat de array net voldoende elementen heeft om alle namen van feestdagen te kunnen bewaren.

```csharp
// (1)

feestdagen[0] = "Nieuwjaar";
feestdagen[1] = "Pasen";
feestdagen[2] = "Paasmaandag";
feestdagen[3] = "Dag vd Arbeid";
feestdagen[4] = "OLH Hemelvaart";
feestdagen[5] = "Pinksteren";
feestdagen[6] = "Pinkstermaandag";
feestdagen[7] = "Nationale Feestdag";
feestdagen[8] = "OLV Hemelvaart";
feestdagen[9] = "Allerheiligen";
feestdagen[10] = "Wapenstilstand";
feestdagen[11] = "Kerstdag";

Console.WriteLine(feestdagen.Length);
```

1.  Vul hier de code aan.

Als je het programma vervolgens uitvoert, krijg je *12* te zien.

```csharp
12
```

Gebruik eventueel eens de debugger om na te gaan of de array correct is opgevuld.

### Oefening D08dagen
Vervang in onderstaande module regels *(1)* tot en met *(8)* door een declaratieregel met een array-initializer om meteen alle zeven initiële waardes voor de array-elementen op te geven.

```csharp
string[] dagen = new string[7]; // (1)
dagen[0] = "ma";                // (2)
dagen[1] = "di";                // (3)
dagen[2] = "wo";                // (4)
dagen[3] = "do";                // (5)
dagen[4] = "vr";                // (6)
dagen[5] = "za";                // (7)
dagen[6] = "zo";                // (8)

Console.Write($"({dagen.Length} elementen): ");
for (int index = 0; index < dagen.Length; index++) {
    Console.Write(dagen[index] + " ");
}
```

Uitvoer…​

```csharp
(7 elementen): ma di wo do vr za zo
```

## Aanspreken van array-elementen
### Oefening D08dagnummer
Schrijf een programma dat de gebruiker om een dagnummer vraagt van 1 t.e.m. 7 en vervolgens toont welke weekdag daarmee overeenkomt (maandag is dag 1).

Indien de gebruiker geen geldig dagnummer intypt toont het programma niks.

Bijvoorbeeld…​

```csharp
Geef een dagnummer : groen
```

Of…​

```csharp
Geef een dagnummer : -3
```

Of…​

```csharp
Geef een dagnummer : 9
```

Of…​

```csharp
Geef een dagnummer : 6
Dagnummer 6 is zaterdag.
```

### Oefening D08twister
Herschrijf [de oplossing van D06twister](../deel-06-oplossingen/deel-06-oplossingen.html#_oplossing_d06twister) zodat er twee arrays gebruikt worden :

```csharp
string[] kleuren = {"rood", "groen", "blauw", "geel"};
string[] lichaamsdelen = {"linkerhand", "rechterhand", "linkervoet", "rechtervoet"};
```

Om een willekeurige kleur te bekomen, zal het programma een willekeurige positie in het `kleuren` array bepalen en dan de kleur op die positie gebruiken. Bijvoorbeeld, indien het random getal `2` is zal de kleur `blauw` op positie `2` gekozen worden. Voor een willekeurig lichaamsdeel wordt dezelfde aanpak gebruikt.

## Itereren met for en foreach
### Oefening D08gemeenten
In volgende code is reeds logica opgenomen om een array `gemeenten` op te vullen met enkele gemeentenamen en hun bijhorende postcode.

Vul de code nu zelf aan om met een `for` alle postcodes af te drukken.

Het is de bedoeling om telkens het element twee posities verder te benaderen.

```csharp
string[] gemeenten = new string[8];

gemeenten[0] = "Brussel";
gemeenten[1] = "1000";
gemeenten[2] = "Antwerpen";
gemeenten[3] = "2000";
gemeenten[4] = "Brugge";
gemeenten[5] = "8000";
gemeenten[6] = "Gent";
gemeenten[7] = "9000";

// (1)
```

1.  Vul hier aan.

Uitvoer…​

```csharp
1000
2000
8000
9000
```

### Oefening D08dagentotaal
Vul onderstaand voorbeeld aan met de nodige code die elk element uit de `dagen` array gaat benaderen om de waarde van dit element bij het `totaal` op te tellen.

```csharp
int[] dagen = new int[12];

dagen[0] = 31;
dagen[1] = 28;
dagen[2] = 31;
dagen[3] = 30;
dagen[4] = 31;
dagen[5] = 30;
dagen[6] = 31;
dagen[7] = 31;
dagen[8] = 30;
dagen[9] = 31;
dagen[10] = 30;
dagen[11] = 31;

int totaal = 0;
// (1)

Console.WriteLine("Totaal: " + totaal);
```

1.  Vul hier aan.

Het afgedrukte totaal zou uiteraard *365* moeten zijn.

### Oefening D08toongetallen
Begin met `int[] a = {5, 3, 1, -1, -3};` en schrijf een programma dat de waarden netjes achtereen op het scherm zet met komma’s en spaties ertussen:

``` nowrap
5, 3, 1, -1, -3
```

Merk op dat er na de laatste waarde (*-3*) geen komma is opgenomen.

Gebruik hiervoor een loop die zich aanpast aan de lengte van het array, dus als we array `a` zouden opvullen met meer of minder waarden, dan moet het programma nog steeds correct werken.

### Oefening D08netto
Pas volgende voorbeeld aan om met een `foreach` (in plaats van de `for`) elke waarde uit de `kortingen` array van het `brutoBedrag` af te trekken.

Controleer of je *920,6* (*1000 - 10 - 50 - 19.4*) uitkomt.

```csharp
double[] kortingen = { 10, 50, 19.4 };
double brutoBedrag = 1000;

double nettoBedrag = brutoBedrag;
for (int index = 0; index < kortingen.Length; index++) {
    nettoBedrag -= kortingen[index];
}

Console.Write("Netto bedrag: " + nettoBedrag);
```

### Oefening D08woordslang
Schrijf een programma waarbij de gebruiker 5 woorden moet ingeven en het programma controleert of deze een woordslang vormen.

Een woordslang is een reeks woorden waarbij elk woord begint met de laatste letter van het vorige woord.

-   bv. groe**n**, **n**ooi**t**, **t**rot**s**, **s**laa**p**, **p**unt is een woordslang

-   bv. groe**n**, **n**ooi**t**, **k**atro**l**, **l**eu**k**, **k**amer is geen woordslang ("nooit" eindigt met "t" maar "katrol" begint niet met "t")

Het programma bewaart alle woorden in een array en begint daarna met de controle.

De uitvoer is als volgt :

-   indien het een woordslang is, dan worden alle woorden na elkaar getoond

-   indien het geen woordslang is, dan worden de eerste twee woorden getoond die niet voldoen

Aan de voorbeelduitvoeringen hieronder kun je precies zien wat de output moet zijn :

```csharp
Geef een woord : groen
Geef een woord : nooit
Geef een woord : trots
Geef een woord : slaap
Geef een woord : punt
de woordslang is groen-nooit-trots-slaap-punt
```

```csharp
Geef een woord : groen
Geef een woord : nooit
Geef een woord : katrol
Geef een woord : leuk
Geef een woord : kamer
het is geen woordslang : nooit-katrol
```

## Opvullen en samenstellen van arrays
### Oefening D08aansprekingen
Vul volgend voorbeeld aan met de nodige code om elke waarde in de `aansprekingen` array aan te passen, en hiervoor de tekst *"Dag "* te plakken.

Controleer of bij het afdrukken van de arrayinhoud nu effectief blijkt dat de waardes *"Dag Jan"*, *"Dag Piet"* en *"Dag Pol"* zijn geworden.

```csharp
string[] aansprekingen = { "Jan", "Piet", "Pol" };

// (1)

foreach (string aanspreking in aansprekingen) {
    Console.WriteLine(aanspreking);
}
```

1.  Hier aanvullen.

### Oefening D08arrayopvullen
Vul onderstaande code aan om de array `getallen` op te vullen met getallen *101* tot en met *109*.

```csharp
int[] getallen = new int[9];

// opvullen
// (1)

// afdrukken
for (int index = 0; index < getallen.Length; index++) {
    Console.Write(getallen[index] + " ");
}
```

1.  Vul hier aan.

Uitvoer…​

```csharp
101 102 103 104 105 106 107 108 109
```

### Oefening D08inhoudwisselen
Gegeven is onderstaand programma met twee arrays `a` en `b`.

Voeg op de plek van het commentaar de juiste code toe om de inhoud van beide arrays om te wisselen.

```csharp
int[] a = { 12, 34, 56, 78, 90 };
int[] b = { 31, 42, 53, 64, 75 };

Console.Write("De inhoud van a voor de wissel : ");
Console.WriteLine( String.Join(',', a) );

// SCHRIJF HIER JE CODE OM DE ARRAY INHOUDEN OM TE WISSELEN

Console.Write("De inhoud van a na de wissel : ");
Console.WriteLine( String.Join(',', a) );
```

Let op : je moet zelf de nodige code schrijven om daadwerkelijk alle elementen te overlopen en om te wisselen, je mag bij deze oefening niet simpelweg `Array.Copy()` gebruiken of andere simpelere truken gebruiken (bv. enkel de variabelen vd arrays wisselen).

De uitvoering van dit programma is als volgt :

```csharp
De inhoud van a voor de wissel : 12,34,56,78,90
De inhoud van a na de wissel : 31,42,53,64,75
```

### Oefening D08omgekeerdevolgorde
Schrijf een programma dat de gebruiker om 4 namen vraagt en deze vervolgens in de omgekeerde volgorde toont op de console.

Bijvoorbeeld bij invoer van *Jan*, *Piet*, *Joris* en *Corneel*…​

```csharp
Geef naam 1 : Jan
Geef naam 2 : Piet
Geef naam 3 : Joris
Geef naam 4 : Corneel
Corneel
Joris
Piet
Jan
```

Let op: het moet heel eenvoudig zijn om het programma aan te passen naar bijvoorbeeld 6 namen door slechts op *1 plaats* in het programma een 4 naar een 6 aan te passen!

### Oefening D08omgekeerdevolgordehoeveel
Pas het vorige programma aan zodat in het begin aan de gebruiker gevraagd wordt hoeveel namen hij/zij wil ingeven.

Bijvoorbeeld…​

```csharp
Hoeveel namen wil je ingeven : 2
Geef naam 1 : Bassie
Geef naam 2 : Adriaan
Adriaan
Bassie
```

### Oefening D08volgordeomwisselen
Om bij de oefeningen [D08omgekeerdevolgorde](#_oefening_d08omgekeerdevolgorde) en [D08omgekeerdevolgordehoeveel](#_oefening_d08omgekeerdevolgordehoeveel) de waardes in omgekeerde volgorde af te drukken, heb je allicht de waardes in omgekeerde volgorde uitgelezen. Dit startende op de positie van de laatste waarde, dan op de voorlaatste positie, en zo telkens één positie lager.

Deze keer pas je [de oplossing van D08omgekeerdevolgordehoeveel](../deel-08-oplossingen/deel-08-oplossingen.html#_oplossing_d08omgekeerdevolgordehoeveel) zo aan dat de inhoud van de array ook effectief wordt gespiegeld. De laatste waarde wissel je om met de eerste waarde, de voorlaatste met de tweede, enzovoort. Daarna kan je de inhoud van de gespiegelde array eenvoudigweg van voor naar achter afdrukken, om een identiek resultaat te bekomen.

Let op : je moet zelf de nodige code schrijven om alles te overlopen en de volgorde om te draaien, je mag bij deze oefening niet simpelweg `Array.Reverse()` gebruiken!

### Oefening D08rijen
Schrijf een programma dat kan nagaan of een rij getallen een rekenkundige dan wel meetkundige rij is.

Een **rekenkundige rij** is een rij getallen waarbij het verschil tussen 2 opeenvolgende getallen steeds gelijk is

-   bv. 2, 5, 8, 11, 14 is een rekenkundige rij want het verschil is steeds +3

-   bv. 84.5, 74.5, 64.5, 54.5 is een rekenkundige rij want het verschil is steeds -10

-   bv. 5, 7, 10, 14 is geen rekenkundige rij want het verschil is niet steeds hetzelfde

Een **meetkundige rij** is een rij getallen waarbij een getal steeds gelijk is aan het vorige getal maal eenzelfde factor.

-   bv. 3, 6, 12, 24 is een meetkundige rij met factor 2 (volgende getal is steeds vorige getal maal twee)

-   bv. 100, 50, 25, 12.5, 6.25, 3.125 is een meetkundig rij met factor 0.5 (volgende getal is steeds vorige getal maal 0.5)

-   bv. 5, 10, 30, 120 is geen meetkundige rij

Het programma vraagt de gebruiker om max. 6 getallen, bewaart deze in een array en controleert achteraf of het een rekenkundige of een meetkundige rij is. Indien geen van beiden is het een "gewone" rij getallen.

Indien de gebruiker minder dan 6 getallen wil invoeren kan dit door een lege regel in te geven (gewoon op Enter drukken zonder iets te typen), dan stopt het programma met vragen naar nog meer getallen.

Je mag ervan uitgaan dat de gebruiker steeds correcte getallen ingeeft en altijd minstens 3 getallen invoert.

Indien de gebruiker een rij van identieke getallen ingeeft, zullen we dit (enkel) zien als een rekenkundige rij met delta 0.

Hieronder zie je enkele voorbeelduitvoeringen, let erop dat de kommagetallen er op jouw computer misschien anders uitzien (3.5 met punt versus 3,5 met komma).

```csharp
Geef een getal : 2
Geef een getal : 5
Geef een getal : 8
Geef een getal : 11
Geef een getal : 14
Geef een getal :      (1)
Dit is een rekenkundige rij met delta 3
```

1.  Gebruiker drukt hier gewoon op Enter zonder iets in te voeren.

```csharp
Geef een getal : 100
Geef een getal : 50
Geef een getal : 25
Geef een getal : 12.5
Geef een getal : 6.25
Geef een getal : 3.125 (1)
Dit is een meetkundige rij met factor 0.5
```

1.  Het programma vraagt geen verdere getallen omdat de gebruiker er al 6 ingaf.

```csharp
Geef een getal : 5
Geef een getal : 10
Geef een getal : 35
Geef een getal : 120
Geef een getal :      (1)
Dit is een gewone rij
```

1.  Gebruiker drukt hier gewoon op Enter zonder iets in te voeren.

### Oefening D08fibonacci
In volgend voorbeeld is een array `fibonacci` met *10* elementen aangemaakt.

De eerste twee elementen zijn alvast opgevuld met waarde *1*.

De bedoeling is de array verder op te vullen met de getallen uit de *fibonacci reeks*.

[WIKIPEDIA: Rij van Fibonacci](https://nl.wikipedia.org/wiki/Rij_van_Fibonacci)

Vul nu zelf de code aan om het *3de* tot en met het *10de* element gelijk te stellen aan de som van de vorige twee elementen.

Zo moet bijvoorbeeld het derde element *2* worden (*1 plus 1*), het vierde element *3* worden (*1 plus 2*), enzovoort.

```csharp
int[] fibonacci = new int[10];

fibonacci[0] = 1;
fibonacci[1] = 1;

// overige elementen gelijkstellen aan som van de vorige twee ...
// (1)

foreach (int getal in fibonacci) {
    Console.Write(getal + " ");
}
```

1.  Hier aanvullen.

Uitvoer…​

```csharp
1 1 2 3 5 8 13 21 34 55
```

## Zoeken in arrays
### Oefening D08positieszoeken
Begin met `int[] a = {5, 3, 1, -1, -3, 3, 9, -4};` en schrijf een programma dat de gebruiker om een waarde vraagt en die waarde zoekt in het array.

Telkens de waarde gevonden wordt, toont het programma de array index (*positie*) waarop dit gebeurde.

Bijvoorbeeld: indien de gebruiker `3` intypt, toont het programma `1 5`.

Indien de waarde niet gevonden werd, toont het programma niets.

### Oefening D08positieszoekenmooier
Breid de vorige oefening uit zodat het programma wat meer uitleg toont : `waarde 3 gevonden op positie(s) 1 5` of `waarde niet gevonden`.
