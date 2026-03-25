# Programmeren Basis - Deel 13 - Verdieping
## 1. var declaraties
Bij het declareren van sommige variabelen kan je, indien je ze meteen initialiseert (op een bepaalde waarde instelt), aan de slag met `var` declaraties.

In plaats van expliciet het datatype te vermelden, zoals in…​

```csharp
...
int getal = 5;
string y = x;
Random rnd = new Random();
```

Kan je deze ook weglaten, en vervangen door het `var` sleutelwoord…​

```csharp
...
var getal = 5;
var y = x;
var rnd = new Random();
```

De code leidt dat exact hetzelfde resultaat. `getal` blijft een variabele van type `int`, `y` van type `string` (indien `x` nog steeds wijst naar een `string` variabele) en `rnd` van type `Random`.

Deze keer leidt de compiler echter zelf het datatype voor deze variabelen af (*type inference*), dit op basis van het datatype van de expressie in de toekenningsclausule. Op basis dus van wat rechts van de toekenningsoperator `=` staat uitgeschreven.

In het geval van de declaraties van `getal` of `y` geeft dat eigenlijk geen enkel nut. In tegendeel, het weglaten van het datatype maakt de code minder expliciet. De lezer van deze code, moet nu zelf ook het type van deze variabelen gaan afleiden. Zeker bij de declaratie van `y` is dit extra werk voor de lezer (de declaratie van `x` moet er bijgehaald worden).

Bij het declareren van `rnd` heeft de `var` declaratie nog het voordeel dat je het type `Random` slechts éénmalig hoeft te vermelden. Maar de meerwaarde is beperkt.

Niet alle variabelen kan je zomaar declareren met een `var` statement. Bij declaraties van parameter variabelen bijvoorbeeld, moet je steeds expliciet het datatype vermelden. Iets als…​

```csharp
static void Main() {
    double x = 4.0;
    PrintMacht(3.0, x);
}

static void PrintMacht(var grondtal, var exponent = 2.0) { // (1)
    double macht = Math.Pow(grondtal, exponent);
    Console.WriteLine($"{grondtal} ^ {exponent} = {macht}");
}
```

1.  `var` ga je hier allicht vervangen door `double`.

…​kan dus niet.

Vanwege gebrek aan consitentie, en omdat het de leesbaarheid van onze algoritmes niet persé bevorderd, vermijden we grotendeels in dit materiaal het gebruik van deze `var` declaraties.

## 2. Werken met streams, bijvoorbeeld van type StreamReader en StreamWriter
### 2.1. Zelf de stream afsluiten (Close)
Het eind van het `using` statement geeft aan waar we klaar zijn met het gebruik van de *stream* of dus de onderliggende *resource* (*bestand* in ons geval).

Je kan ook op een andere manier aangeven dat de *stream* mag worden *afgesloten*, meer specifiek door zelf de `Close()` method aan te roepen op het *stream* object. Hierbij geven we expliciet aan de *stream* de opdracht zijn afsluitend gedrag te voltooien, zoals het vrijgeven van de gebruikte resource (het bestand).

```csharp
string inhoud;

{                                                          // (1)
    StreamReader sr = new StreamReader("test.txt");        // BEGIN VAN HET GEBRUIK
    inhoud = sr.ReadToEnd();
    sr.Close();                                            // EIND VAN HET GEBRUIK
}                                                          // (2)

Console.WriteLine(inhoud);
```

1.  Je kan statementblokken toevoegen (stukken code tussen accolades plaatsen) om daarbinnen gedeclareerde variabelen een lokale scope te geven.

2.  Hier zal de variabele `sr` niet meer in scope zijn na de afsluitende accolade. Dit bootst dan hier het effect na van wat in een typisch `using` statement, bijvoorbeeld `using (StreamReader sr = …​) { …​ }` wordt bereikt.

> **Tip: IDisposable interface**
>
> Telkens als het gaat om een `IDisposable` datatype (zoals `StreamReader` of -`Writer`) wordt verwacht dat je een `Close` of `Dispose` method aanroept op objecten van deze datatype. Dit op zijn minst zodra je klaar bent met het gebruik van de onderliggende resource.
>
> Zo spoedig mogelijk laten we het afsluitend gedrag voltooien. Dat *afsluitend gedrag* kan bijvoorbeeld het *doorsassen* zijn van interne buffers (bv. bij `StreamWriter` objecten), of het vrijgeven (ontgrendelen) van de bestanden waar bijvoorbeeld *streams* gebruik van maken.
>
> Tijdens het coderen in *Visual Studio* kan je vrij makkelijk nagaan of het gaat om een `IDisposable` datatype. Dergelijk datatype *implementeert* een `IDisposable` *interface*.
>
> Een *interface implementeren* is vergelijkbaar met het naleven van een contract. In dit geval een contract dat aangeeft dat er een mogelijkheid bestaat om te *disposen* (*weg te gooien*). Het is van die mogelijkheid dat het `using` statement gebruik gaat maken. Bij zijn einde (bij zijn afsluitende accolade bijvoorbeeld) wordt (op de achtergrond) code ingevoegd die van mogelijkheid gebruik maakt.
>
> Hier werken we enkel met `StreamReader` en `StreamWriter` objecten. Deze zijn effectief `IDisposable`. Deze uitleg over het afsluiten (*disposen*) van *streams* is echter bedoelt om je nu reeds te kaderen hoe je later dat *disposen* ook gaat toepassen wanneer je met andere types van *stream* objecten aan de slag gaat.
>
> Wens je na te gaan of een datatype `IDisposable` is, kijk dan naar zijn *basis(sen)* (*base* of *bases* zoals het in *Visual Studio* wordt genoemd). Rechterklik bijvoorbeeld in de declaratie van `sr` op `StreamReader` en kies voor *Go To Base*. Een apart *Bases* toolvenster opent om alle *basissen* weer te geven.
>
> Indien `interface IDisposable` daarin opduikt dan weet je dat het de bedoeling is expliciet het eind van het gebruik te markeren. Dit door ofwel zelf gebruik te maken van een `Close` of `Dispose` call. Of, en dat is eenvoudiger, gebruik te maken van een `using` statement. Deze gaat immers met zijn einde (bij zijn afsluitende accolade) aangeven dat daar resource-gebruik beëindigd wordt.
>
> Ga je later eens experimenteren met `NetworkStream`, `BinaryReader`, `BinaryWriter`, …​ *stream* objecten, dan ga je ook daar opmerken dat ze de `IDisposable` interface implementeren. Nu reeds weet je wat daarvan de conclusie moet zijn (vergeet deze *stream* objecten niet af te sluiten, en dat liefst zo spoedig mogelijk).
>
> Op het *implementeren* van *interfaces* gaan we later dieper in.

### 2.2. Finaal (try…​finally…​) de stroom afsluiten
Een stuk code als…​

```csharp
string inhoud;

{
    StreamReader sr = new StreamReader("test.txt");        // OPENEN VAN DE STROOM
    inhoud = sr.ReadToEnd();
    sr.Close();                                            // AFSLUITEN VAN DE STROOM
}

Console.WriteLine(inhoud);
```

…​is eigenlijk niet volledig identiek aan…​

```csharp
string inhoud;

using (StreamReader sr = new StreamReader("test.txt")) {   // OPENEN VAN DE STROOM
    inhoud = sr.ReadToEnd();
}                                                          // AFSLUITEN VAN DE STROOM

Console.WriteLine(inhoud);
```

Het aanroepen van de `Close()` method, wat bij het gebruik van onze `using` automatisch gebeurt op het eind van het statement, hoeft enkel te gebeuren indien het openen, en de verdere handelingen (zoals de leesbewerkingen), succesvol verliepen.

Het komt dus eerder neer op…​

```csharp
string inhoud;

{
    StreamReader sr;
    try {
        sr = new StreamReader("test.txt");                 // OPENEN VAN DE STROOM
        inhoud = sr.ReadToEnd();
    } finally {
        if (sr != null) {                                  // (1)
            sr.Close();                                    // AFSLUITEN VAN DE STROOM
        }
    }
}
```

1.  Deze controle gaat na of het openen op zijn minst dan wel gelukt was. Is dat niet het geval dan bevat `sr` `null`, wat vervolgens een `NullReferenceException` zou veroorzaken bij een call als `sr.Close()`. Er is dan immers geen *af te sluiten stream*.

Code in het `finally` gedeelte wordt *op het eind* (*finaal*) altijd nog uitgevoerd. Hier zal de code in het `finally` gedeelte (die door gebruik van het `using` statement op de achtergrond voor ons wordt gegenereerd) dus zeker nog proberen de *stream* af te sluiten.

**Code die in het `finally` gedeelte van een `try` statement wordt opgenomen, is dus code die *ten allen tijde* moet worden uitgevoerd**:

-   Treedt er **geen exceptie** op in het `try` gedeelte, dan wordt na uitvoer van de code in dat `try` gedeelte `(1)`, de code in het `finally` gedeelte uitgevoerd `(3)`, dus volgende verloop in onderstaand voorbeeld: `(1)` → `(3)`

-   Treed er **wel een exception** op, dan nog zal uiteindelijk de code in het `finally` gedeelte worden uitgevoerd.

    1.  Ofwel: na de uitvoer van een **gepast `catch` gedeelte `(2a)` opgenomen in hetzelfde `try` statement** van het `try` gedeelte `(1)` waar de exception optreed. Bijvoorbeeld indien in onderstaand voorbeeld bij `(1)` een exception van type `ExceptionTypeA` optreedt, dus volgende verloop: `(1)` → `(2a)` → `(3)`

    2.  Ofwel: nog voor de uitvoer van een **gepast `catch` gedeelte `(2b)` opgenomen in** een `try` statement van **aanroepende logica** (in een overkoepeld `try` statement of lager op de *callstack*). Bijvoorbeeld indien in onderstaand voorbeeld bij `(1)` een exception van type `ExceptionTypeB` optreedt, dus volgende verloop: `(1)` → `(3)` → `(2b)`

    3.  Ofwel: meteen na het `try` gedeelte indien **geen gepast `catch` gedeelte** wordt gevonden. Bijvoorbeeld indien in onderstaand voorbeeld bij `(1)` een exception van type `ExceptionTypeC` optreedt, dus volgende verloop: `(1)` → `(3)`

```csharp
static void AanroependeLogica() {
    try {
        AangeroepenLogica()
    } catch (ExceptionTypeB ex) {
        ... (2b)
    }
}

static void AangeroepenLogica() {
    try {
        ... (1)
    } catch (ExceptionTypeA ex) {
        ... (2a)
    } finally {
        ... (3)
    }
}
```

> **Tip: Wanneer heb je een try statement met finally gedeelte nodig?**
>
> Hoe dan ook wordt altijd het `finally` gedeelte uitgevoerd. Er zijn niet zoveel scenario’s waarin zoiets echt noodzakelijk is.
>
> Bij het werken met *resources* echter, waar je bijvoorbeeld *stream* objecten gebruikt die we zeker moeten *disposen*, komt het goed van pas.
>
> Maar omdat *C#* beschikt over `using` statements, die bij gebruik de `finally` code voor ons op de achtergrond laten toevoegen, gaan we eigenlijk zelf nog zelden *rechtstreeks* gebruik van `finally` tegenkomen.
