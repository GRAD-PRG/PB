# Programmeren Basis - Deel 04
## 1. Invoer meteen converteren
Tot dus ver hebben we voor het opvangen van de console invoer een extra `string` variabele gebruikt. In de console kan immers enkel met tekst worden gewerkt (afgedrukt of ingelezen). `Console.ReadLine()` levert dan ook altijd de ingevoerde waarde (wat je intikt voor je op Enter drukt) op in `string` vorm.

Indien…​

-   de ingevoerde waarde echter als numeriek gegeven mag en kan worden geïnterpreteerd

-   je wil beginnen rekenen met die informatie

…​converteren we deze `string` naar het gepaste numerieke datatype.

Voorbeeld van opvangen en daarna converteren

Bijvoorbeeld aan de hand van de `int.Parse()` functionaliteit…​

```csharp
Console.Write("Geef het aantal deelnemers : ");
string aantalDeelnemersAlsTekst = Console.ReadLine();
int aantalDeelnemers = int.Parse(aantalDeelnemersAlsTekst);

int helft = aantalDeelnemers / 2;
Console.WriteLine($"De helft van dat aantal is: {helft}");
```

Het opvangen van de invoer enerzijds, en het converteren anderzijds, mag ook samen gebeuren.

Voorbeeld van meteen converteren van de opgevangen invoer

De aanroep naar `Console.ReadLine()` kan meteen tussen de haakjes van onze `int.Parse(…​)` gebeuren.

```csharp
Console.Write("Geef het aantal deelnemers : ");
int aantalDeelnemers = int.Parse(Console.ReadLine());

int helft = aantalDeelnemers / 2;
Console.WriteLine($"De helft van dat aantal is: {helft}");
```

## 2. Debugging
### Soorten fouten bij het programmeren
Nu onze algoritmes steeds complexer worden, treden er steeds makkelijk fouten op.

Twee soorten van fouten hebben we reeds aangehaald…​

-   *Compilefouten* die optreden wanneer de compiler ons niet verstaat, of wanneer de compiler ons expliciet wil verhinderen een bepaalde operatie uit te voeren.

-   *Runtimefouten* (*excepties*), die niet bij het compileren, maar pas daarna bij het uitvoeren naar voor komen. Dit in moeilijk te voorziene, *exceptionele* omstandigheden.

Een derde en laatste categorie van fouten zijn de *logische fouten*. Hierbij is er geen compiler of runtime die tussenkomt om ons te melden dat iets fout loop, maar doet het programma gewoon niet wat het eigenlijk zou moeten doen. Deze laatste categorie van fouten is de moeilijkste om op te sporen, er is immers niets of niemand die ons aanwijst waar het fout loopt.

Voorbeeld van een logische fout

Wens je een programma die de som maakt van twee getallen dan functioneert volgende code niet naar behoren…​

```csharp
int getal1 = 10;
int getal2 = 20;

int som = getal1 * getal2;

Console.WriteLine("Som: " + som);
```

Indien we het voorbeeld uitvoeren krijgen we volgende **foutieve output**…​

``` nowrap
Som: 200
```

Er treedt geen compilefout, noch een runtimefout op.

In dergelijk eenvoudig algoritme is het natuurlijk niet zo moeilijk uit te maken wat het probleem veroorzaakt. In complexer stukken code kan het helpen code stap voor stap te ontleden, waarbij je steeds ook verifieert of de inhouden van je variabelen correct evolueren.

### 2.1. Debugging in Visual Studio
Visual Studio reikt ons enkele tools aan om fouten op te sporen. Het inzetten van de *debugger* in combinatie met een *Locals* toolvenster is vaak een goede start bij het opsporen van fouten.

Voorbeeld van een programma dat niet goed functioneert

Stel dat we een oppervlakte willen berekenen op basis van een ingevoerde lengte en breedte. We laten het programma op basis van die informatie het aantal vierkante meters en hectare berekenen en afdrukken…​

Je zou de ingevoerde meters kunnen vermenigvuldigen tot vierkante meters, en omzetten naar hectare. We nemen in de berekening een tussenstap, van vierkante meters gaan we eerst naar are, en van are naar hectare.

```csharp
Console.Write("Lengte in meter?: ");
double lengteInMeter = double.Parse(Console.ReadLine());

Console.Write("Breedte in meter?: ");
double breedteInMeter = double.Parse(Console.ReadLine());

double vierkanteMeter = lengteInMeter * breedteInMeter;
double are = vierkanteMeter / 10;
double hectare = are / 100;

Console.WriteLine("\nAantal vierkante meter: " + vierkanteMeter);
Console.WriteLine("Aantal hectare: " + hectare);
```

Indien we het voorbeeld uitvoeren en de gebruiker de waardes *100* en *100* invoert bekomen we volgende output…​

``` nowrap
Lengte in meter?: 100
Breedte in meter?: 100

Aantal vierkante meter: 10000
Aantal hectare: 10
```

Bij een lengte en breedte van *100 meter*, hebben we bijgevolg *100 are*, wat *1 hectare* is. Ons programma gaat echter *10 hectare* rapporteren.

Ofwel bij het opvangen van de ingevoerde waardes, ofwel bij het maken van de opeenvolgende berekeningen, moet iets fout gelopen zijn.

Zo meteen sporen we de fout op, en zoeken we naar een oplossing.

#### 2.1.1. Breakpoint en debugmodus
Indien er een logische fout optreedt en het niet meteen duidelijk is wat het probleem veroorzaakt, helpt het stap voor stap door de code te gaan. Om daarbij na elke stap op te volgen hoe onze variabelen evolueren. Het zijn ten slotte onze variabelen waarin de ingevoerde waardes, of de berekende resultaten worden gestockeerd.

Een *breakpoint*, die op één of meerdere regels in onze code wordt opgenomen, zal de uitvoer van de code, bij het bereiken van deze regel, onderbreken. Het brengt ons in een *breakmodus* (ook wel *debugmodus* genoemd). In deze *modus* stelt onze ontwikkelomgeving ons in staat aan de hand van verschillende tools het verloop van onze applicatie te gaan monitoren. Zo kunnen we bekijken hoe we op een bepaalde plaats in onze code zijn terechtgekomen, of kunnen we nagaan wat de actuele inhoud is van onze variabelen.

We hernemen het vorige voorbeeld…​

We zouden bijvoorbeeld kunnen beginnen met het plaatsen van een *breakpoint* op de declaratieregel van `vierkanteMeter`. Je kan dat doen door te rechterklikken op deze regel en te kiezen voor ***Breakpoint** **›** **Insert Breakpoint***.

![Visual Studio - Insert Breakpoint](images/Visual%20Studio%20-%20Insert%20Breakpoint.png)

De regel wordt in het rood gemarkeerd als hierop een breakpoint is ingesteld.

![Visual Studio - Breakpoint](images/Visual%20Studio%20-%20Breakpoint.png)

Starten we nu de applicatie, en voeren we bijvoorbeeld opnieuw dezelfde waardes in, dan merkt je op hoe Visual Studio effectief op de breakpoint-regel, de uitvoering staakt…​ Deze regel wordt in het geel gemarkeerd.

![Visual Studio - Debugmodus](images/Visual%20Studio%20-%20Debugmodus.png)

#### 2.1.2. Locals toolvenster
Graag zouden op dit punt tijdens uitvoer alvast eens controleren of onze `lengteInMeter` en `breedteInMeter` variabelen correct zijn opgevuld.

Kies voor ***Debug** **›** **Windows** **›** **Locals***.

![Visual Studio - Debug Windows menu](images/Visual%20Studio%20-%20Debug%20Windows%20menu.png)

Het *Locals* toolvenster verschijnt die ons een overzicht geeft van de inhoud van onze *lokale variabelen*.

> **Opmerking: Wat is een lokale variabele?**
>
> Lokale variabelen zijn deze die *lokaal* in de functionaliteit (hier de `Main()` method) worden gedeclareerd. Het effect van een *lokale declaratie* is dat de variabele enkel daar ter beschikking is (*beperkte scope*), en dat ze slechts tijdelijk (zolang de method in uitvoering is) in het geheugen zijn ingeladen (*korte lifespan*).

![Visual Studio - Locals toolvenster](images/Visual%20Studio%20-%20Locals%20toolvenster.png)

We stellen vast dat de inhoud van onze twee variabelen `lengteInMeter` en `breedteInMeter` correct zijn opgevuld.

#### 2.1.3. Door de code stappen
We kunnen een programma stap voor stap, regel voor regel, laten uitvoeren. Dit is zinvol om de ondertussen wijzigende inhouden van onze variabelen op te volgen.

Kies in de menu voor ***Debug** **›** **Step Into*** om de uitvoer één stap verder, in dit geval één regel verder, halt te laten houden.

![Visual Studio - Debug Step Into](images/Visual%20Studio%20-%20Debug%20Step%20Into.png)

De uitvoering wordt onderbroken nog voor uitvoer van de volgende regel.

![Visual Studio - Locals 2](images/Visual%20Studio%20-%20Locals%202.png)

> **Tip**
>
> Een *Step Into* kan via een knop op de *Debug* werkbalk. De knop met dezelfde afbeelding als weergegeven naast de menu optie *Step Into*.

Ook hier zien we hoe de uitvoer correct is verlopen, het aantal vierkante meters is correct samengesteld tot *10 000*. Deze informatie is ook terug te vinden (op regel 7) in een hierna weergegeven traceertabel.

Nemen we nog een stap, opnieuw via *Debug* en *Step Into*, dan zien we waar het probleem is opgetreden.

![Visual Studio - Locals 3](images/Visual%20Studio%20-%20Locals%203.png)

Waar we *100* are hadden verwacht, hebben we blijkbaar een fout in onze berekening gemaakt, gedeeld door *10* in plaats van *100*, waardoor we de foutieve *1000* are zijn uitgekomen. Logischerwijs zal onze verdere berekening die op basis van deze are waarde gebeurt, ook geen correct resultaat opleveren.

Een overzicht van de waardes in onze variabelen is ook terug te vinden (op regel 8) in een hierna weergegeven traceertabel.

De oorzaak van ons probleem is gevonden. De code kan worden gecorrigeerd.

> **Tip: Hoe verlaat ik de debugmodus?**
>
> Debugmodus verlaten, kan door in de menu te kiezen voor ***Debug** **›** **Stop Debugging***..
>
> Opnieuw kan je ook via de *Debug* werkbalk kiezen voor *Stop Debugging*.

### 2.2. Debuggen met een traceertabel
Ook aan de hand van een traceertabel kan je natuurlijk fouten terugvinden…​

Voorbeeld

```csharp
  1 : Console.Write("Lengte in meter?: ");
  2 : double lengteInMeter = double.Parse(Console.ReadLine());
  3 :
  4 : Console.Write("Breedte in meter?: ");
  5 : double breedteInMeter = double.Parse(Console.ReadLine());
  6 :
  7 : double vierkanteMeter = lengteInMeter * breedteInMeter;
  8 : double are = vierkanteMeter / 10;
  9 : double hectare = are / 100;
```

De tabel wordt dan…​

| Regel | `lengteInMeter` | `breedteInMeter` | `vierkanteMeter` | `are`    | `hectare` |
|------------|------------|------------|------------|------------|------------|
| 1     | /               | /                | /                | /        | /         |
| 2     | 100             |                  |                  |          |           |
| 5     |                 | 100              |                  |          |           |
| 7     |                 |                  | 10000            |          |           |
| 8     |                 |                  |                  | **1000** |           |
| 9     |                 |                  |                  |          | **10**    |

In dit overzicht wordt allicht ook vlug duidelijk dat de problemen starten na het berekenen van de `are`.

De aanpak met een zelf uitgeschreven traceertabel is uiteraard veel meer werk. Typisch bekijk je alle regels en alle variabelen terwijl je vaak wel een idee hebt waar de fout zich ongeveer bevindt.

## 3. If …​ else if …​ else …​
Ondertussen weten we hoe we op basis van *één voorwaarde* in een `if` statement met `else` block ons algoritme kunnen opslitsen in *twee uitvoeringspaden*. Op basis van die ene voorwaarde wordt een keuze gemaakt ofwel *code block 1*, ofwel *code block 2* uit te voeren.

``` nowrap
if (voorwaarde) {
    code block 1
} else {
    code block 2
}
```

Code block 1 wordt uiteraard uitgevoerd indien de voorwaarde correct is (`true`), code block 2 indien de voorwaarde niet correct is (`false`).

### 3.1. Geneste if’s
Als in één uitvoeringspad ook voorwaardelijkheid van pas komt, kan je probleemloos een nieuw `ìf` statement toevoegen. Het effect is dan geneste `if` statements.

Voorbeeld met één if

Om te bepalen of een temperatuur al dan niet *enorm warm* is, volstaat één `if` statement met één voorwaarde…​

```csharp
Console.Write("Geef de actuele temperatuur in : ");
string temperatuurAlsTekst = Console.ReadLine();
int temperatuur = int.Parse(temperatuurAlsTekst);

if (temperatuur >= 30) {
    Console.WriteLine("Het is enorm warm.");
} else {
    Console.WriteLine("Het is niet enorm warm.");
}
```

Momenteel zal bij de invoer van temperatuur onder de 30 graden worden gesteld dat het *niet enorm warm* is.

Voorbeeld met geneste if’s

Volstaat dat niet, of wens je een meer gedetailleerde conclusie, dan kan een tweede `if` statement, met een tweede voorwaarde, oplossing brengen…​

```csharp
if (temperatuur >= 30) {
    Console.WriteLine("Het is enorm warm.");
} else {
    if (temperatuur < 10) {
        Console.WriteLine("Het is koud.");
    } else {
        Console.WriteLine("Het is aangenaam warm.");
    }
}
```

Slechts indien het niet zo is dat `temperatuur >= 30`, gaat ons algoritme na of `temperatuur < 10`.

Ons programma kan 3 mogelijke conclusies trekken (*enorm warm*, *koud* of *aangenaam warm*) op basis van een combinatie van voorwaardes.

|                | temperatuur &gt;= 30 | temperatuur &lt; 10   |
|----------------|----------------------|-----------------------|
| enorm warm     | true                 | (niet van toepassing) |
| koud           | false                | true                  |
| aangenaam warm | false                | false                 |

Enkel wanneer het niet zo is dat `temperatuur >= 30`, wordt bekeken of `temperatuur < 10`.

Indien het wel klopt dat `temperatuur >= 30`, wordt de tweede voorwaarde zelfs niet geëvalueerd. Het programma hoeft immers geen tijd te verliezen aan dergelijke beslissing.

We hadden reeds in *selecties* gebruik gemaakt van *sequentie controlestructuren*. Nu merken we op hoe je net zo makkelijk binnen één selectie, andere selecties kan inbouwen.

Anders gesteld, een `ìf` kan in een andere `if` worden uitgeschreven.

#### 3.1.1. Met else if block
Vaak worden de accolades rond een `if` statement in een `else` block weggelaten.

Voorbeeld met geneste if zonder accolades

Voorgaande geneste if’s…​

```csharp
if (temperatuur >= 30) {
    Console.WriteLine("Het is enorm warm.");
} else {
    if (temperatuur < 10) {
        Console.WriteLine("Het is koud.");
    } else {
        Console.WriteLine("Het is aangenaam warm.");
    }
}
```

Kan dus ook zo…​

```csharp
if (temperatuur >= 30) {
    Console.WriteLine("Het is enorm warm.");
} else if (temperatuur < 10) {
    Console.WriteLine("Het is koud.");
} else {
    Console.WriteLine("Het is een matige temperatuur.");
}
```

Ook hierbij zal slechts indien het niet zo is dat `temperatuur >= 30`, ons programma nagaan of `temperatuur < 10`.

> **Tip**
>
> Het weglaten van de accolades kan de leesbaarheid van onze code bevorderen.
>
> Zoals je ziet, start men typisch de tweede `if` meteen volgend op `else` (op dezelfde regels).

#### 3.1.2. Meerdere else if blocks
Er staan geen limieten op het aantal `if` statements die je in elkaar kan nestelen.

Om nog meer gedetailleerde conclusies te trekken over de ingevoerde temperatuur voegen we extra `if` statements toe…​

```csharp
if (temperatuur >= 30) {
    Console.WriteLine("Het is enorm warm.");
} else if (temperatuur >= 15) {
    Console.WriteLine("Het is aangenaam warm.");
} else if (temperatuur < 10) {
    Console.WriteLine("Het is koud.");
} else {
    Console.WriteLine("Het is eerder fris.");
}
```

Enkel indien het

-   niet zo is dat `temperatuur >= 30`

-   én niet zo is dat `temperatuur >= 15`

-   én niet is dat `temperatuur < 10`

zal ons algoritme concluderen dat het *eerder fris* is.

|                | temperatuur &gt;= 30 | temperatuur &gt;= 15  | temperatuur &lt; 10   |
|------------------|------------------|------------------|------------------|
| enorm warm     | true                 | (niet van toepassing) | (niet van toepassing) |
| aangenaam warm | false                | true                  | (niet van toepassing) |
| koud           | false                | false                 | true                  |
| fris           | false                | false                 | false                 |

> **Opmerking: Meerdere else if blocks, maar slechts één else block…​**
>
> Samengevat kan je stellen dat we nul of meerdere `else if` blocks mogen inzetten.
>
> Het `else` gedeelte is optioneel, maar kan maximaal één keer voorkomen. Daar wordt de logica opgenomen die wordt uitgevoerd indien aan geen enkel van bovenstaande voorwaardes is voldaan.

### 3.2. Vergelijking met if’s in sequentie
Merk op dat…​

``` nowrap
if (a < b) {
    code block 1
} else if (b < a) {
    code block 2
} else {
    code block 3
}
```

**Niet** hetzelfde is als…​

``` nowrap
if (a < b) {
    code block 1
}
if (b < a) {
    code block 2
}
if (a == b) {
    code block 3
}
```

Indien, zoals deze laatste aanpak, de verschillende if’s na elkaar worden uitgeschreven, zullen ze ook eenvoudigweg in opeenvolging -onvoorwaardelijk- allen tot uitvoering worden gebracht.

Voorbeeld met 3 if’s in sequentie

```csharp
Console.Write("Aantal personen?: ");
int personen = int.Parse(Console.ReadLine());
Console.Write("Aantal glazen?: ");
int glazen = int.Parse(Console.ReadLine());

if (glazen < personen) {
    // code block 1
    Console.WriteLine("We hebben glazen tekort, we openen een nieuwe doos glazen...");
    glazen = glazen + 6;
}
if (glazen > personen) {
    // code block 2
    Console.WriteLine("We hebben meer dan voldoende glazen.");
}
if (glazen ==  personen) {
    // code block 3
    Console.WriteLine("We hebben voor elke persoon precies één glas.");
}
```

Bij de invoer van *10 personen* en *5 glazen* bekomen we…​

```csharp
Aantal personen?: 10
Aantal glazen?: 5
We hebben glazen tekort, we openen een nieuwe doos glazen...
We hebben meer dan voldoende glazen.
```

Dat de laatste zin werd opgenomen in de uitvoer is op zijn minst gesteld vreemd.

De aanpassing van het aantal glazen in codefragment 1, leidde ook tot het uitvoeren van codefragement 3.

Door verschillende beslissingen in sequentie uit te schrijven, worden ze hoe dan ook allen uitgevoerd, en wordt elke voorwaarde geëvalueerd.

Voorbeeld met geneste if’s

We kunnen dit vermijden door de `ìf` statements in elkaar te nestelen…​

```csharp
if (glazen < personen) {
    // code block 1
    Console.WriteLine("We hebben glazen tekort, we openen een nieuwe doos glazen...");
    glazen = glazen + 6;
} else if (glazen > personen) {
    // code block 2
    Console.WriteLine("We hebben meer dan voldoende glazen.");
} else {
    // code block 3
    Console.WriteLine("We hebben voor elke persoon precies één glas.");
}
```

Deze keer bekomen we bij invoer van *10 personen* en *5 glazen* een ander resultaat…​

```csharp
Aantal personen?: 10
Aantal glazen?: 5
We hebben glazen tekort, we openen een nieuwe doos glazen...
```

Er heerst *exclusiviteit* tussen codefragment 1, 2 en 3. Ten hoogste één van deze codefragmenten wordt uitgevoerd.

## 4. Logische bewerkingen
### 4.1. And
Indien niet aan één, maar aan meerdere voorwaardes moet voldaan zijn, alvorens bepaalde logica uit te voeren, kan je deze voorwaardes combineren met een logische *and* (`&&`) operator.

Voorbeeld met een and operator

Om na te gaan of een getal zich in een bepaald bereik valt, bijvoorbeeld *1 tot en met 100*, moet het

-   minstens even groot zijn dan de ondergrens (`getal >= ondergrens`)

-   maximaal even groot zijn dan de bovengrens (`getal <= bovengrens`)

```csharp
int ondergrens = 1;
int bovengrens = 100;

Console.Write("Getal?: ");
int getal = int.Parse(Console.ReadLine());

if (getal >= ondergrens && getal <= bovengrens) {
    Console.Write($"{getal} ligt tussen {ondergrens} en {bovengrens}");
}
```

Een logische *and* (`&&`) operator is bruikbaar om twee `bool` expressies te combineren.

Het resultaat van de combinatie is ook een `bool`. Meer specifiek enkel `true` indien beide onderdelen `true` zijn.

> **Belangrijk: And logica**
>
> | voorwaarde 1 | voorwaarde 2 | voorwaarde 1 and voorwaarde 2 |
> |--------------|--------------|-------------------------------|
> | **true**     | **true**     | **true**                      |
> | true         | false        | false                         |
> | false        | true         | false                         |
> | false        | false        | false                         |

> **Opmerking**
>
> Merk op dat *voorwaarde 2* totaal irrelevant is voor de uitkomst indien *voorwaarde 1* alvast `false` is. De uitkomst is dan immers steeds `false`.
>
> Deze tweede voorwaarde wordt in zo’n geval dan ook *kortgesloten*. Er wordt geen tijd verloren met het evalueren van deze voorwaarde.

#### Meer dan 2 voorwaardes combineren
Je bent niet beperkt in het aantal voorwaardes die je gaat combineren.

Voorbeeld met meer dan twee voorwaardes

Indien we bijkomend moeten verifiëren of de `ondergrens` effectief wel *onder* de `bovengrens` ligt, kan je een bijkomende voorwaarde toevoegen…​

```csharp
if (ondergrens <= bovengrens && getal >= ondergrens && getal <= bovengrens) {
    ...
}
```

Hier hebben we drie voorwaardes die steeds met *and* logica worden gecombineerd.

In theorie is er geen limiet.

#### Alternatief zonder and operator
Ook zonder gebruik te maken van een *and* operator (`&&`) kunnen we hetzelfde resultaat bereiken.

Neem in het `if` block van een eerste controle, waar je alvast de eerste voorwaarde kan verifiëren, een tweede `if` statement op. Daarmee kan je ook de tweede voorwaarde afdwingen.

Alternatief voorbeeld zonder and operator

```csharp
if (getal >= ondergrens) {
    if (getal <= bovengrens) {
        Console.Write($"{getal} ligt tussen {ondergrens} en {bovengrens}");
    }
}
```

Zowel in deze, al bij voorgaande oplossing zal de `Write` opdracht enkel worden voltooid indien aan beide voorwaardes is voldaan…​

|                  | `getal >= ondergrens` | `getal <= bovengrens` |                         |
|------------------|------------------|------------------|------------------|
| `Write` opdracht | **true**              | **true**              | bv. bij *getal 50*      |
| (geen output)    | true                  | false                 | bv. bij bij *getal 500* |
| (geen output)    | false                 | true                  | bv. bij bij *getal 0*   |
| (geen output)    | false                 | false                 | (\*)                    |

(\*) Niet mogelijk zolang de `ondergrens` effectief lager ligt dan de `bovengrens`.

Onze oorspronkelijke aanpak, met *and* operator (`&&`) is eleganter (beter leesbaar), en dus een veel beter keuze.

Er zijn situaties mogelijk waar je toch voor een geneste `if` aanpak zou opteren. Bij elke voorwaarde kan je hier immers meteen een alternatief (`else`) scenario uitschrijven.

Voorbeeld

Stel je voor dat je een gepaste foutmelding wil opleveren. *Te klein* of *te groot* bijvoorbeeld. Dan kan onze laatste aanpak, met geneste `if` statements daar vrij makkelijk op worden voorzien…​

```csharp
if (getal >= ondergrens) {
    if (getal <= bovengrens) {
        Console.Write($"{getal} ligt tussen {ondergrens} en {bovengrens}");
    } else {
        Console.Write("te groot");
    }
} else {
    Console.Write("te klein");
}
```

Ook in onze eerste aanpak, met *and* operator, kan dergelijke logica worden geïntegreerd. Het zal echter meer vereisen dan louter een `else` block voorzien voor onze `if (getal >= ondergrens && getal <= bovengrens)`. Als niet voldaan was aan de combinatie van voorwaardes moet het programma immers nog steeds uitzoeken welke specifieke foutmelding wordt weergegeven.

### 4.2. Or
Indien aan minstens één van meerdere voorwaardes moet voldaan zijn, alvorens bepaalde logica uit te voeren, kan je deze voorwaardes combineren met een logische *or* (`||`) operator.

Voorbeeld met een or operator

Voorwaardes als `getal < ondergrens` en `getal > bovengrens` kunnen we combineren met een `||` operator om aan te geven dat vanaf er aan één van beide voldaan is, een melding mag worden opgeleverd…​

```csharp
int ondergrens = 1;
int bovengrens = 100;

Console.Write("Getal?: ");
int getal = int.Parse(Console.ReadLine());

if (getal < ondergrens || getal > bovengrens) {
    Console.Write($"{getal} valt buiten het bereik {ondergrens} tot en met {bovengrens}");
}
```

Een logische *or* (`||`) operator is gebruiken we opnieuw om twee `bool` expressies te combineren.

Het resultaat van de combinatie is ook een `bool`. Meer specifiek `true` vanaf één van beide onderdelen `true` zijn.

> **Belangrijk: Or logica**
>
> | voorwaarde 1 | voorwaarde 2 | voorwaarde 1 \|\| voorwaarde 2 |
> |--------------|--------------|--------------------------------|
> | **true**     | **true**     | **true**                       |
> | **true**     | **false**    | **true**                       |
> | **false**    | **true**     | **true**                       |
> | false        | false        | false                          |

> **Waarschuwing**
>
> In ons dagelijk taalgebruik wordt met *of* vaak gesuggereerd dat het gaat om ofwel het ene, ofwel het andere, bemerkt dat dit hier niet het geval is.

> **Opmerking**
>
> Merk op dat *voorwaarde 2* totaal irrelevant is voor de uitkomst indien *voorwaarde 1* alvast `true` is. De uitkomst is dan immers steeds `true`.
>
> Deze tweede voorwaarde wordt in zo’n geval dan ook *kortgesloten*. Er wordt geen tijd verloren met het evalueren van deze voorwaarde.

#### Alternatief zonder or operator
Ook deze keer kunnen we zonder gebruik te maken van een *or* operator (`||`) hetzelfde resultaat bereiken.

Alternatief voorbeeld zonder or operator

Een verbetering kunnen we het echter niet noemen…​

```csharp
if (getal < ondergrens) {
    Console.Write($"{getal} valt buiten het bereik {ondergrens} tot en met {bovengrens}");
} else {
    if (getal > bovengrens) {
        Console.Write($"{getal} valt buiten het bereik {ondergrens} tot en met {bovengrens}");
    }
}
```

Bij deze aanpak gaan we in beide `if` codeblocks dezelfde logica moeten uitschrijven.

#### Don’t Repeat Yourself
Jezelf herhalen is nooit een goed idee. Men spreekt ook wel een over het *DRY* principe (*Don’t Repeat Yourself*).

Centraliseer zo vaak als mogelijk logica. Dit maakt je algoritmes niet alleen beter leesbaar, je kan ze zo later ook makkelijk aanpassen. Als je op meerdere plaatsen dezelfde code ziet, moet je je afvragen of je dit niet kan herformuleren zodat de code er maar één keer staat.

Stel…​

``` nowrap
if (voorwaarde) {
    doe A
    doe B
    doe D
} else {
    doe A
    doe C
    doe D
}
```

Vermits elke uitvoering van dit programma steeds eerst A zal doen en steeds achteraf D zal doen, kunnen we dit herleiden tot…​

``` nowrap
doe A
if (voorwaarde) {
    doe B
} else {
    doe C
}
doe D
```

Voor de computer maakt dit niet uit, de beide fragmenten zijn immers equivalent, maar voor de toekomstige lezer is het tweede heel wat duidelijker.

### 4.3. Not
Een `bool` waarde (*true* of *false*) kan met een logische *not* (`!`) operator makkelijk worden geïnverteerd. *Not true* is gelijk aan *false*, en *not false* uiteraard gelijk aan *true*.

> **Belangrijk: Not logica**
>
> | voorwaarde | not voorwaarde |
> |------------|----------------|
> | true       | false          |
> | false      | true           |

Voorbeeld met not operator

Om niet meermaals te moeten definiëren wat het betekent om te vriezen (`temperatuur <= 0`) zou je een `bool` variabele als `hetVriest` kunnen herbruiken.

```csharp
Console.Write("Temperatuur?: ");
int temperatuur = int.Parse(Console.ReadLine());

bool hetVriest = (temperatuur <= 0);
if (hetVriest) {
    Console.WriteLine("Bij deze temperatuur vriest het.");
}

Console.Write("Dank je voor het invoeren van de ");
if (!hetVriest) { // (1)
    Console.Write("warme ");
}
Console.Write("temperatuur.");
```

1.  Of althans een geïnverteerde versie ervan als je niet wil doen wanneer het niet vriest.

> **Opmerking**
>
> In het geval van numerieke vergelijkingsoperatoren hadden we reeds aangehaald dat er voor elke operator een tegenovergestelde variant bestaat. Niet `<` is hetzelfde als `>=`, niet `>` is identiek aan `<=`, niet `==` is dan `!=`, enzovoort.
>
> Een expressie als `!(temperatuur <= 0)` kan je natuurlijk ook formuleren als `temperatuur > 0`.

## 5. Interval checking
Soms moet je bepalen in welk *interval* (*bereik*) een waarde voorkomt.

Voorbeeld

Je zou verschillende leeftijdscategorie als volgt kunnen definiëren:

-   *kind* indien de leeftijd kleiner is dan 10

-   *gepensioneerd* vanaf 65 jaar

-   *tiener* indien de leeftijd minstens 10 is, én kleiner dan 18

-   *volwassene* bij een leeftijd van 25 of meer, maar onder de 65

-   *jongvolwassene* indien de leeftijd minstens 18 is, én kleiner dan 25

Letterlijk in code omgezet zou dit iets geven als…​

```csharp
if (leeftijd < 10) {
    // kind
} else if (leeftijd >= 65) {
    //gepensioneerd
} else if (leeftijd >= 10 && leeftijd < 18) {
    // tiener
} else if (leeftijd >= 25 && leeftijd < 65) {
    // volwassene
} else if (leeftijd >= 18 && leeftijd < 25) {
    // jongvolwassene
}
```

In totaal hebben we hier een zestal voorwaardes moeten uitschrijven. Sommigen daarvan werden gecombineerd (aan de hand *and* logica) met andere voorwaardes.

Als intervallen netjes op elkaar aansluiten (geen gaten ertussen), kan je dergelijke *interval controles* beter sorteren. Dit vereenvoudigt de vereiste voorwaardes.

Zelfde voorbeeld maar met gesorteerde controles

Je zou van de jongste leeftijd kunnen vertrekken, en zo opwerken naar de oudste…​

```csharp
if (leeftijd < 10) {
    // kind
} else if (leeftijd < 18) {
    // tiener
} else if (leeftijd < 25) {
    // jongvolwassene
} else if (leeftijd < 65) {
    // volwassene
} else {
    // gepensioneerd
}
```

Bij een ***tiener*** bijvoorbeeld hoef je al niet meer te controleren of de leeftijd ook wel minstens *10* is. Er wordt immers pas gecontroleerd of het om een *tiener* gaat indien reeds duidelijk was dat het geen kind was.

|                  | `leeftijd < 10` | `leeftijd < 18` | `leeftijd < 25` | `leeftijd < 65` |
|---------------|---------------|---------------|---------------|---------------|
| *kind*           | true            |                 |                 |                 |
| ***tiener***     | **false**       | **true**        |                 |                 |
| *jongvolwassene* | false           | false           | true            |                 |
| *volwassene*     | false           | false           | false           | true            |
| *gepensioneerd*  | false           | false           | false           | false           |

In aflopende volgorde kan het natuurlijk ook…​

```csharp
if (leeftijd >= 65) {
    // gepensioneerd
} else if (leeftijd >= 25) {
    // volwassene
} else if (leeftijd >= 18) {
    // jongvolwassene
} else if (leeftijd >= 10) {
    // tiener
} else {
    // kind
}
```

In beide gevallen volstaat een viertal voorwaardes. Zelfs logische operatoren waren in deze voorbeelden niet noodzakelijk.

> **Opmerking**
>
> Je kunt dit een beetje vergelijken met een *coin sorter*. Zoals hier ook in beeld werd gebracht: https://www.youtube.com/watch?v=A94h19O7_fo.
>
> Verschillende mogelijkheden worden uitgeprobeerd. De eerste die past wordt verkozen. Let erop dat de gaatjes ook gesorteerd zijn volgens grootte.

## 6. Invoer robuust opvangen
### 6.1. Strings vergelijken
Strings kan je vergelijken met `==` of `!=` operatoren.

Voorbeeld van string vergelijking

```csharp
Console.Write("Antwoord?: ");
string input = Console.ReadLine();

if (input == "ja") {
    Console.WriteLine("U koos voor JA.");
} else if (input == "nee") {
    Console.WriteLine("U koos voor NEE.");
} else {
    Console.WriteLine("U koos voor nog iets anders.");
}
```

Let op, wordt bij de uitvoer door de gebruiker iets als *"Ja"* ingevoerd, dan bekommen we…​

```csharp
Antwoord?: Ja
U koos voor nog iets anders.
```

In het geval van invoer door de gebruiker weet je vaak niet hoe hij/zij zijn informatie gaat invoeren. Bijvoorbeeld qua hoofdletter, omsluitende spaties e.d.

Voorbeeld van string vergelijking

De tekst *ja*, dat kan worden opleverd als `"ja"`, `"Ja"`, `"JA"`, `"jA"`, `" ja"`, `"ja "`, `" ja "`, …​ .

Rekening houden met al die mogelijkheden is op zijn minst gesteld omslachtig…​

```csharp
if (input == "ja" || input == "Ja" || input == "JA" || ...) {
    ...
}
```

Een betere aanpak volgt hieronder.

### 6.2. Omzetten naar kleine letters of hoofdletters
Tekst omzetten in kleine letters of hoofdletters kan met functionaliteiten als `.ToLower()` of `.ToUpper()`.

Voorbeeld van omzetting in kleine letters

Voor de punt (*dot*) verwijs je (met een `string` expressie), naar de tekst waarvan een variant in kleine letters of hoofdletters wordt opgeleverd.

```csharp
Console.Write("Geef wat tekst: ");
string input = Console.ReadLine();

Console.WriteLine("In kleine letters: " + input.ToLower());
Console.WriteLine("In hoofdletters  : " + input.ToUpper());

Console.WriteLine("Hello".ToLower());
Console.WriteLine("world".ToUpper());
```

Als hier *"Ja"* wordt ingevoerd, bekomen we…​

```csharp
Geef wat tekst:  Ja
In kleine letters: ja
In hoofdletters  : JA
hello
WORLD
```

Zo kan je ook tekst omzetten in kleine letters of hoofdletters om éénduidig af te toetsen met de referentiewaarde.

Voorbeeld van omzetting in kleine letters bij invoer

Zet bijvoorbeeld de input om in kleine letters of hoofdletters voor je begint te vergelijken…​

```csharp
string input = Console.ReadLine();

if (input.ToLower() == "ja") {
    Console.WriteLine("U koos voor JA.");
} else if (input.ToLower() == "nee") {
    Console.WriteLine("U koos voor NEE.");
} else {
    Console.WriteLine("U koos voor nog iets anders.");
}
```

Deze code werkt ongeacht de gebruiker `"Ja"`, `"JA"`, `"ja"`, `"NeE"`, `"NEe"`, …​ invoert.

### 6.3. Omsluitende spaties wegknippen
Om spaties vooraan of achteraan in een tekst weg te knippen kan je gebruik maken van een `Trim()` functionaliteit.

Voorbeeld van spaties wegknippen

Voor de punt (*dot*) verwijs je (met een `string` expressie), naar de tekst waaruit de je *leading* of *trailing* spaties wil verwijderen…​

```csharp
string begroeting = " Hello World!   ".Trim();

Console.Write($"[{begroeting}]");  // (1)
```

1.  De tekst `[Hello World!]` verschijnt op de console (en NIET `[ Hello World! ` ` ``]`!)

Je ziet aan de output dat `.Trim()` de spaties daadwerkelijk verwijderd heeft : de tekst in variabele `begroeting` bevatte geen spaties aan het begin of einde.

`Trim()` komt ook goed van pas bij het verwerken van tekst die de gebruiker intypt. Denk bv. aan een stukje informatie dat mensen doorgaans copy&pasten i.p.v intypen, bv. de tracking code van een poststuk, daar durft wel eens (per ongeluk) een spatie voor of achter meegekopieerd worden.

Voorbeeld van spaties wegknippen in invoer

Links van de `.Trim()` verwijs je met een `string` expressie naar de tekst waarvan de begin- en eindspaties moeten verwijderd worden. Het resultaat is een nieuwe tekst, inhoudelijk gelijk aan de begintekst maar zonder begin- en eindspaties.

Zo’n string expressie kan bv. een string *literal* zijn, een string variabele of het string resultaat van een andere bewerking. In het voorbeeld hieronder laten we `.Trim()` werken met het resultaat van `input.ToLower()`.

```csharp
string input = Console.ReadLine();
string verwerkteInput = input.ToLower().Trim();

if (verwerkteInput == "ja") {
    Console.WriteLine("U koos voor JA.");
} else if (verwerkteInput == "nee") {
    Console.WriteLine("U koos voor NEE.");
} else {
    Console.WriteLine("U koos voor nog iets anders.");
}
```

Zelfst indien er heel wat spaties op *Ja* volgen, of eraan voorafgaan, bekomen we…​

```csharp
Geef wat tekst:         Ja
U koos voor JA.
```

Invoer opvangen kan zo een stuk robuuster gebeuren.
