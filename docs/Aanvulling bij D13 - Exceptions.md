# Exceptions

## Drie soorten fouten

Bij het programmeren kan je met drie soorten fouten te maken krijgen:

| Soort | Wanneer ontdekt? | Hoe erg? |
|---|---|---|
| **Compilefouten** | Tijdens het compileren | Minst erg: je kán niets fout uitvoeren, en de compiler wijst je precies aan waar het probleem zit. |
| **Logische fouten** | Tijdens het testen of gebruiken | Lastig: het programma draait, maar doet iets fout. Niets wijst je rechtstreeks naar de oorzaak. |
| **Runtimefouten (exceptions)** | Tijdens het uitvoeren | Het programma breekt zich af op het moment dat de fout optreedt. |

Compilefouten zijn in zekere zin je beste vriend: ze verhinderen dat je foute code uitvoert en geven je meteen feedback. Logische fouten zijn het moeilijkst op te sporen, want het programma *lijkt* te werken — het doet alleen niet wat je verwacht.

In dit deel gaan we dieper in op de derde soort: **runtimefouten**, beter bekend als **exceptions**.

## Programma-afbreking

Een optredende exception leidt tot **onmiddellijke programma-afbreking**. De gebruiker krijgt geen kans meer om bijvoorbeeld werk op te slaan.

```csharp
Console.Write("Geef een index: ");
int index = int.Parse(Console.ReadLine());

string tekst = "Hallo";
Console.WriteLine(tekst[index]);  // wat als index 99 is?
```

Voert de gebruiker `99` in, dan treedt een `IndexOutOfRangeException` op: er is geen karakter op die positie. Het programma stopt abrupt.

Binnen Visual Studio wordt dat netjes weergegeven met een aanduiding van de exception en de veroorzakende regel. Maar voor een eindgebruiker die de applicatie buiten Visual Studio opstart, is het effect simpelweg een crashend programma. Dat willen we vermijden.

## Exceptions vermijden

Het optreden van runtimefouten proberen we te vermijden. Daar bestaan twee aanpakken voor:

1. **Programmatorische controles** — zelf vooraf controleren met `if`-statements of een operatie veilig is.
2. **Exception handling** — met een `try`/`catch`-constructie de riskante code omringen.

### Aanpak 1: programmatorische controles

Je controleert vooraf of de operatie veilig is:

```csharp
Console.Write("Geef een index: ");
bool indexOk = int.TryParse(Console.ReadLine(), out int index);

string tekst = "Hallo";

if (indexOk && index >= 0 && index < tekst.Length)
{
    Console.WriteLine(tekst[index]);
}
else
{
    Console.WriteLine("Ongeldige index.");
}
```

Geen exception mogelijk: je hebt het probleem afgevangen vóór het zich kan voordoen.

### Aanpak 2: exception handling (try/catch)

Je *probeert* de riskante code uit te voeren, en *vangt* een eventuele exception op:

```csharp
try
{
    Console.Write("Geef een index: ");
    int index = int.Parse(Console.ReadLine());

    string tekst = "Hallo";
    Console.WriteLine(tekst[index]);
}
catch (Exception ex)
{
    Console.WriteLine($"Er ging iets mis: {ex.Message}");
}
```

Treedt er een exception op in het `try`-blok, dan springt de uitvoer onmiddellijk naar het `catch`-blok. Het programma breekt zich niet af.

### Wanneer welke aanpak?

De keuze hangt af van hoe waarschijnlijk het probleem is:

- Gebruik **programmatorische controles** als de ongeldige situatie **regelmatig kan voorkomen** en deel uitmaakt van het **normale programmaverloop**. Denk aan gebruikersinvoer: het is vrij normaal dat iemand iets intypt dat geen geldig getal is.

- Gebruik **exception handling** als de probleemsituatie **uitzonderlijk** (*exceptioneel*) is. Denk aan een bestand dat plots onbereikbaar wordt, of onvoldoende werkgeheugen. Zulke situaties zijn zeldzaam, en het zou je code onleesbaar maken om ze allemaal met `if`-controles af te dekken.

In de praktijk combineer je vaak beide: programmatorische controles voor de verwachte problemen, en een `try`/`catch` als vangnet voor de onverwachte.

## Het try/catch-mechanisme

### Sprong naar het catch-gedeelte

Zodra een exception optreedt ergens in het `try`-blok, **springt** de uitvoer onmiddellijk naar het `catch`-blok. De rest van de code in het `try`-blok wordt overgeslagen. Er wordt **niet** teruggekeerd.

```csharp
try
{
    Console.WriteLine("Stap 1");       // wordt uitgevoerd
    int x = int.Parse("geen getal");   // exception hier!
    Console.WriteLine("Stap 3");       // wordt NIET uitgevoerd
}
catch (Exception ex)
{
    Console.WriteLine($"Fout: {ex.Message}");
}

Console.WriteLine("Na het try/catch");  // wordt gewoon uitgevoerd
```

Output:

```
Stap 1
Fout: Input string was not in a correct format.
Na het try/catch
```

Code die in het `try`-blok volgt na een instructie die een exception opgooit, wordt dus enkel uitgevoerd als die instructie *géén* exception veroorzaakt. Dat spronggedrag is net de kracht van het mechanisme: je hoeft niet zelf geneste `if`-controles te schrijven.

### Meerdere try-statements na elkaar

Soms wil je dat een fout bij de ene operatie niet verhindert dat de andere operatie alsnog geprobeerd wordt. Dan gebruik je **meerdere aparte `try`-statements**:

```csharp
try
{
    string[] regels = File.ReadAllLines("bestand1.csv");
    Console.WriteLine($"Bestand 1: {regels.Length} regels");
}
catch (Exception ex)
{
    Console.WriteLine($"Bestand 1 mislukt: {ex.Message}");
}

try
{
    string[] regels = File.ReadAllLines("bestand2.csv");
    Console.WriteLine($"Bestand 2: {regels.Length} regels");
}
catch (Exception ex)
{
    Console.WriteLine($"Bestand 2 mislukt: {ex.Message}");
}
```

Faalt het eerste bestand, dan wordt het tweede alsnog geprobeerd.

### Meerdere catch-blokken

Eén `try`-blok kan meerdere `catch`-blokken hebben, elk voor een ander type exception:

```csharp
try
{
    string inhoud = File.ReadAllText("data.csv");
    int aantal = int.Parse(inhoud);
    Console.WriteLine($"Aantal: {aantal}");
}
catch (FileNotFoundException ex)
{
    Console.WriteLine($"Bestand niet gevonden: {ex.Message}");
}
catch (FormatException ex)
{
    Console.WriteLine($"Ongeldige inhoud: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Onverwachte fout: {ex.Message}");
}
```

De uitvoer springt naar het **eerste** `catch`-blok dat past bij het type van de opgetreden exception. Daarna gaat de uitvoer verder na het volledige `try`/`catch`-statement.

!!! warning "Let op de volgorde"
    Plaats specifieke exception-types **vóór** algemene. Een `FileNotFoundException` is een subtype van `IOException`, dat op zijn beurt een subtype is van `Exception`. Zet je `catch (Exception ex)` bovenaan, dan zal dat blok *alle* exceptions opvangen en komen de specifiekere blokken er nooit aan te pas.

### Catch zonder variabele

Als je de details van de exception niet nodig hebt, mag je de variabele weglaten:

```csharp
try
{
    File.WriteAllText("log.txt", "test");
}
catch
{
    Console.WriteLine("Wegschrijven mislukt.");
}
```

## Verschillende soorten exceptions

Er bestaan tientallen ingebouwde exception-types in .NET. Enkele die je als beginner regelmatig tegenkomt:

| Exception | Typische oorzaak |
|---|---|
| `FormatException` | `int.Parse("abc")` — de string is geen geldig getal |
| `OverflowException` | `int.Parse("99999999999")` — het getal past niet in het datatype |
| `IndexOutOfRangeException` | Toegang tot een onbestaande index in een array of string |
| `DivideByZeroException` | Deling door nul (bij gehele getallen) |
| `FileNotFoundException` | Het bestand bestaat niet |
| `DirectoryNotFoundException` | De map in het pad bestaat niet |
| `IOException` | Algemene input/output-fout (bv. bestand vergrendeld) |
| `ArgumentException` | Een ongeldig argument werd aan een method doorgegeven |
| `NullReferenceException` | Aanspreken van een lid op een `null`-referentie |

Merk op dat sommige types verwant zijn. `FileNotFoundException` is een specifiekere variant van `IOException`. Dat verklaart waarom de volgorde van `catch`-blokken ertoe doet.

## Documentatie raadplegen

Van elke method of property die je gebruikt, kan je opzoeken welke exceptions ze kan opwerpen. Dat kan op twee manieren:

1. **In Visual Studio**: hang met de muis boven de method-naam. De tooltip toont onder "Exceptions" welke runtimefouten mogelijk zijn.

2. **Op Microsoft Learn (docs.microsoft.com)**: zoek de method op. Onder het kopje "Exceptions" staat per exception-type uitgelegd *wanneer* die optreedt.

Zo zie je bijvoorbeeld bij `File.ReadAllLines` dat naast `FileNotFoundException` ook `DirectoryNotFoundException`, `IOException`, `UnauthorizedAccessException` en andere kunnen optreden.

!!! tip "Exceptions zien is niet per se exception handling gebruiken"
    Het feit dat je in de documentatie ziet dat exceptions *kunnen* optreden, betekent niet dat je altijd `try`/`catch` moet gebruiken. Soms is een programmatorische controle (zoals `File.Exists`) de betere keuze. Gebruik de documentatie om te *begrijpen* wat kan falen, en kies dan de gepaste aanpak.

## Exceptions bij File IO

Bij het werken met de convenience methods uit `System.IO.File` — denk aan `ReadAllText`, `ReadAllLines`, `WriteAllText`, `WriteAllLines`, `AppendAllText` en `AppendAllLines` — heb je te maken met **externe resources** die je niet volledig onder controle hebt. Het bestand kan ontbreken, het pad kan ongeldig zijn, de schijf kan vol staan, of je hebt onvoldoende rechten.

Omdat de lijst van mogelijke exceptions bij deze methods lang is, en het bovendien *uitzonderlijke* situaties betreft, is `try`/`catch` hier een voor de hand liggende keuze:

```csharp
try
{
    string[] regels = File.ReadAllLines("klanten.csv");

    foreach (string regel in regels)
    {
        Console.WriteLine(regel);
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine("Het bestand 'klanten.csv' werd niet gevonden.");
}
catch (Exception ex)
{
    Console.WriteLine($"Onverwachte fout bij het inlezen: {ex.Message}");
}
```

Hetzelfde geldt voor schrijfbewerkingen:

```csharp
string[] resultaten = { "Jan;85", "Piet;72", "Rita;91" };

try
{
    File.WriteAllLines("resultaten.csv", resultaten);
    Console.WriteLine("Wegschrijven gelukt.");
}
catch (Exception ex)
{
    Console.WriteLine($"Wegschrijven mislukt: {ex.Message}");
}
```

!!! tip "Vuistregel"
    Elke call naar `File.ReadAllText`, `ReadAllLines`, `WriteAllText`, `WriteAllLines`, `AppendAllText` of `AppendAllLines` hoort thuis in het `try`-blok van een `try`/`catch`-statement.

## Het Try-patroon

In .NET kom je regelmatig een patroon tegen waarbij een method in twee varianten bestaat:

- Een versie die een **exception opgooit** als de operatie mislukt (bv. `int.Parse`).
- Een versie die een **`bool` teruggeeft** die aangeeft of het lukte, en het resultaat via een `out`-parameter oplevert (bv. `int.TryParse`).

Dit noemt men het **Try-patroon** (soms *Try-Parse pattern* genoemd).

### Parse vs. TryParse

Je kent dit al van het omzetten van strings naar getallen:

```csharp
// Gooit FormatException als de string geen geldig getal is:
int getal = int.Parse("abc");

// Geeft false terug als het niet lukt, geen exception:
bool gelukt = int.TryParse("abc", out int getal);
```

De `TryParse`-variant is de betere keuze wanneer ongeldige invoer **regelmatig** kan voorkomen — en dat is bij gebruikersinvoer vrijwel altijd het geval.

```csharp
Console.Write("Geef een geheel getal: ");
string invoer = Console.ReadLine();

if (int.TryParse(invoer, out int getal))
{
    Console.WriteLine($"Je gaf {getal} in.");
}
else
{
    Console.WriteLine("Dat is geen geldig geheel getal.");
}
```

### Het patroon herkennen in de documentatie

Het Try-patroon bestaat niet alleen voor `Parse`. Je vindt het op meerdere plaatsen in .NET:

| Methode met exception | Try-variant | Wat doet de Try-variant? |
|---|---|---|
| `int.Parse(s)` | `int.TryParse(s, out int result)` | Geeft `false` bij ongeldig formaat |
| `dictionary[key]` | `dictionary.TryGetValue(key, out var value)` | Geeft `false` als de key ontbreekt |

Telkens dezelfde structuur: de method-naam begint met `Try`, geeft een `bool` terug, en levert het resultaat op via een `out`-parameter.

### Wanneer Try-patroon, wanneer exception handling?

Het Try-patroon is eigenlijk een ingebakken **programmatorische controle**: je test of de operatie lukt, zonder dat er ooit een exception aan te pas komt. Dat maakt het sneller en leesbaarder voor situaties die regelmatig voorkomen.

- Bestaat er een `Try`-variant en kan het probleem regelmatig optreden? → Gebruik de `Try`-variant.
- Bestaat er geen `Try`-variant, of is het probleem echt uitzonderlijk? → Gebruik `try`/`catch`.

## Foutafhandeling in de aanroepende method

Een `try`/`catch` hoeft niet altijd te staan op de plek waar de exception optreedt. Wanneer een exception niet wordt opgevangen in de method waar ze ontstaat, **bubbelt** ze automatisch omhoog naar de method die de aanroep deed. Dit gaat door tot er ergens een passend `catch`-blok wordt gevonden — of, als dat nergens het geval is, tot het programma zich afbreekt.

Dat is vaak ook zinvol. Een method die een bestand inleest, weet niet noodzakelijk *hoe* een fout afgehandeld moet worden. Misschien wil de ene aanroeper een foutmelding tonen, terwijl een andere aanroeper liever een standaardwaarde gebruikt. De foutafhandeling overlaten aan de aanroepende code is dan de betere keuze.

```csharp
static string[] LeesRegels(string pad)
{
    // Geen try/catch hier: als het misgaat,
    // bubbelt de exception naar de aanroeper.
    return File.ReadAllLines(pad);
}
```

De ene aanroeper toont een foutmelding:

```csharp
static void ToonKlanten()
{
    try
    {
        string[] regels = LeesRegels("klanten.csv");
        foreach (string regel in regels)
        {
            Console.WriteLine(regel);
        }
    }
    catch (FileNotFoundException)
    {
        Console.WriteLine("Klantenbestand niet gevonden.");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Fout bij inlezen: {ex.Message}");
    }
}
```

Een andere aanroeper kiest voor een andere afhandeling:

```csharp
static void ToonProducten()
{
    try
    {
        string[] regels = LeesRegels("producten.csv");
        foreach (string regel in regels)
        {
            Console.WriteLine(regel);
        }
    }
    catch (Exception)
    {
        Console.WriteLine("Geen producten beschikbaar, we werken met een lege lijst.");
    }
}
```

Dezelfde `LeesRegels`-method, maar telkens een andere foutafhandeling — aangepast aan de context van de aanroeper. De method zelf hoeft daar niets van te weten.

!!! tip "Vuistregel"
    Handel een exception af op de plek waar je voldoende context hebt om een zinvolle beslissing te nemen. Dat is vaak niet in de method waar de exception ontstaat, maar in de method die de aanroep doet.

## Zelf exceptions opwerpen

Tot nu toe heb je exceptions enkel *opgevangen*. Maar je kan ze ook zelf **opwerpen** met het `throw`-sleutelwoord. Dat doe je wanneer code wordt aangeroepen op een manier die niet toegestaan is — een situatie waarvan jij als ontwikkelaar vindt dat ze niet mag voorkomen.

### Guard clauses

Stel je een `Bankrekening`-klasse voor met een `Stort`-method. Een negatief bedrag storten slaat nergens op. In plaats van stilletjes niets te doen (of erger: een negatief bedrag effectief te verwerken), gooi je een exception op:

```csharp
class Bankrekening
{
    public decimal Saldo { get; private set; }

    public void Stort(decimal bedrag)
    {
        if (bedrag <= 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(bedrag),
                "Het te storten bedrag moet positief zijn."
            );
        }

        Saldo += bedrag;
    }
}
```

De `if`-controle bovenaan de method noemt men een **guard clause**: ze bewaakt de voorwaarden waaronder de method mag uitgevoerd worden. Wordt de voorwaarde geschonden, dan stopt de method onmiddellijk door een exception op te gooien. De aanroepende code kan die exception vervolgens opvangen:

```csharp
Bankrekening rekening = new Bankrekening();

try
{
    rekening.Stort(-50);
}
catch (ArgumentOutOfRangeException ex)
{
    Console.WriteLine($"Ongeldige storting: {ex.Message}");
}
```

### Welk exception-type kiezen?

.NET voorziet enkele standaard exception-types voor ongeldige argumenten:

| Exception | Wanneer gebruiken? |
|---|---|
| `ArgumentException` | Het argument is ongeldig (bv. een lege string waar tekst verwacht wordt) |
| `ArgumentOutOfRangeException` | Het argument valt buiten het toegelaten bereik (bv. een negatief bedrag) |
| `ArgumentNullException` | Het argument is `null` terwijl dat niet mag |

Kies het type dat het best de aard van het probleem beschrijft. Dat maakt het voor de aanroeper (of voor jezelf, later) makkelijker om te begrijpen wat er precies misging.

!!! tip "Waarom niet gewoon negeren?"
    Je zou de `Stort`-method ook kunnen schrijven met een stille `if`-controle die simpelweg niets doet bij een ongeldig bedrag. Maar dan merkt de aanroepende code niet dat er iets fout is. Een exception maakt het probleem **expliciet zichtbaar** — en dat is bijna altijd te verkiezen boven stilte.