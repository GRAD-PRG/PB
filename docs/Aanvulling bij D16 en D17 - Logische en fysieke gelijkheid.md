# Logische gelijkheid: `Equals` en `GetHashCode`

## Waarom dit hoofdstuk?

Tot nu toe heb je `List<T>`, `HashSet<T>` en `Dictionary<TKey, TValue>` gebruikt met eenvoudige types zoals `int`, `string` of `double`. Voor die types "werkt het gewoon": `list.Contains(5)` geeft `true` als 5 in de lijst zit, een `HashSet<string>` laat geen duplicaten toe, een `Dictionary<int, ...>` vindt zijn sleutel terug.

Zodra je diezelfde collecties gebruikt met **je eigen objecten**, breekt dat vanzelfsprekende gedrag. Een `List<Klant>.Contains(...)` vindt een klant die er nochtans in zit niet terug. Een `HashSet<Factuur>` laat plots duidelijke duplicaten toe. Een `Dictionary<Klant, ...>` kan een sleutel niet opzoeken, zelfs als je exact dezelfde gegevens meegeeft.

De oorzaak is altijd dezelfde: C# weet niet wanneer twee objecten van jouw klasse *inhoudelijk* hetzelfde zijn. Dat moet je zelf vertellen, via `Equals` en `GetHashCode`.

---

## 1. Referentiegelijkheid vs. logische gelijkheid

Bekijk deze klasse:

```csharp
public class Klant
{
    public int KlantNummer { get; }
    public string Naam { get; }

    public Klant(int klantNummer, string naam)
    {
        KlantNummer = klantNummer;
        Naam = naam;
    }
}
```

En nu:

```csharp
Klant a = new Klant(12345, "Jan Peeters");
Klant b = new Klant(12345, "Jan Peeters");

Console.WriteLine(a == b);        // False
Console.WriteLine(a.Equals(b));   // False
```

Twee objecten, volledig identieke inhoud, en toch zijn ze "niet gelijk". Dat is geen bug. Het is de **standaardbetekenis van gelijkheid voor referentietypes in .NET**:

> Twee referenties zijn gelijk als ze naar *hetzelfde object in het geheugen* verwijzen.

Dat heet **referentiegelijkheid** (*reference equality*). De standaard `Equals` die elke klasse van `object` erft, vergelijkt gewoon geheugenadressen. `a` en `b` zijn twee verschillende objecten op twee verschillende plaatsen in het heap-geheugen, dus ze zijn niet gelijk.

Wat we eigenlijk *willen* is **logische gelijkheid** (*value equality* of *structural equality*): twee `Klant`-objecten zijn gelijk als ze hetzelfde klantnummer hebben, ongeacht waar ze in het geheugen staan.

Die logische gelijkheid moet je **zelf definiëren**. C# weet niet welke velden bepalend zijn voor identiteit. Is dat het klantnummer? De combinatie van naam en adres? Alle velden samen? Dat is een *domeinkeuze*, geen technische.

---

## 2. `Equals` overriden

Het signaal aan C# dat jouw klasse zijn eigen gelijkheidsregel heeft, is het overschrijven van `object.Equals`:

```csharp
public class Klant
{
    public int KlantNummer { get; }
    public string Naam { get; }

    public Klant(int klantNummer, string naam)
    {
        KlantNummer = klantNummer;
        Naam = naam;
    }

    public override bool Equals(object? obj)
    {
        if (obj is not Klant andere) return false;
        return KlantNummer == andere.KlantNummer;
    }

    public override int GetHashCode()
    {
        return KlantNummer.GetHashCode();
    }
}
```

Merk op:

- Het parametertype is `object?`, niet `Klant`. Dat is verplicht, want we overschrijven de methode van `object`.
- We starten altijd met een typecontrole. `obj is not Klant andere` combineert de typecheck én de cast naar een variabele `andere` in één stap (*pattern matching*).
- We kiezen hier bewust om gelijkheid te definiëren op basis van `KlantNummer`. Een klant met een ander nummer is een andere klant, ook al heet hij toevallig "Jan Peeters". Twee klanten met hetzelfde nummer zijn dezelfde klant, ook als hun naam ergens verkeerd is ingetikt.
- We overschrijven meteen ook `GetHashCode`. Waarom, lees je verder.

Nu werkt:

```csharp
Klant a = new Klant(12345, "Jan Peeters");
Klant b = new Klant(12345, "Jan P.");       // naam iets anders

Console.WriteLine(a.Equals(b));   // True — zelfde klantnummer
Console.WriteLine(a == b);        // False — want == vergelijkt nog steeds referenties
```

> **Let op:** `==` roept niet automatisch jouw `Equals` op voor klassen. Als je ook `==` en `!=` wil laten werken, moet je die operatoren apart overladen — daarover meer in [sectie 8](#8-operator-overloading--en-). Voor de secties hiertussen werken we met `Equals`, omdat dat is wat collecties onder de motorkap gebruiken.

---

## 3. Welke methoden leunen op `Equals`?

Een heleboel methoden in .NET stellen de vraag "zit dit object in deze collectie?" of "is dit object gelijk aan dat object?". Al die methoden leunen op `Equals`. Als jij `Equals` niet overschrijft, vallen ze terug op referentiegelijkheid — en dan krijg je "verkeerde" antwoorden.

### Op `List<T>` (en arrays)

| Methode | Wat doet ze? |
|---|---|
| `Contains(item)` | `true` als er een element in de lijst zit dat gelijk is aan `item` |
| `IndexOf(item)` | Index van het eerste element dat gelijk is aan `item`, of `-1` |
| `LastIndexOf(item)` | Zelfde, maar vanaf achter |
| `Remove(item)` | Verwijdert het *eerste* element dat gelijk is aan `item` |

Deze methoden doorlopen intern de lijst en testen elk element met `Equals`.

**Zonder override:**

```csharp
List<Klant> klanten = new();
klanten.Add(new Klant(12345, "Jan Peeters"));

bool zitErin = klanten.Contains(new Klant(12345, "Jan Peeters"));
// Resultaat: False — twee verschillende objecten in het geheugen
```

**Met override (gebaseerd op KlantNummer):**

```csharp
bool zitErin = klanten.Contains(new Klant(12345, "Jan P."));
// Resultaat: True — logische gelijkheid op klantnummer
```

### In LINQ

Bijna alle LINQ-methoden die ergens "gelijkheid" impliceren, gebruiken onder water `Equals`:

- `Contains`
- `Distinct`
- `Except`, `Intersect`, `Union`
- `SequenceEqual`
- `GroupBy` (voor het groeperen van sleutels)
- `ToDictionary` en `ToHashSet` (om duplicaten te detecteren)

Voorbeeld:

```csharp
var duplicaten = new List<Klant>
{
    new Klant(12345, "Jan Peeters"),
    new Klant(67890, "Marie Janssens"),
    new Klant(12345, "Jan P."),    // zelfde nummer, andere naam
};

var uniek = duplicaten.Distinct().ToList();
// Zonder Equals-override: 3 elementen (alle referenties verschillend)
// Met Equals-override op KlantNummer: 2 elementen
```

### Voor collecties van waarde-achtige types

Een belangrijke les: **als je een klasse schrijft waarvan je vermoedt dat ze ooit in een `List`, `Set` of `Dictionary` terechtkomt, denk dan na over `Equals` voor je de klasse ergens in gebruik neemt**. Anders krijg je later bugs die moeilijk te traceren zijn, omdat `Contains` "gewoon" `false` blijft teruggeven zonder foutmelding.

---

## 4. `GetHashCode`: waarom ook dat nog?

Je hebt hierboven al gezien dat we naast `Equals` ook `GetHashCode` overschreven. Voor een gewone `List<T>` is `GetHashCode` niet nodig — die doorloopt gewoon elk element. Maar voor **hashingstructuren** is `GetHashCode` *essentieel*.

### Hoe werkt een hashingstructuur?

Een `HashSet<T>` of `Dictionary<TKey, TValue>` slaat zijn elementen niet sequentieel op zoals een lijst. Ze gebruiken een techniek die **hashing** heet:

1. Bij het toevoegen van een element wordt eerst `GetHashCode()` van dat element opgevraagd.
2. Die hashcode bepaalt in welk "bakje" (*bucket*) het element terechtkomt.
3. Bij het opzoeken wordt opnieuw `GetHashCode()` berekend om meteen het juiste bakje te vinden — zonder de hele collectie te doorlopen.
4. In dat bakje worden dan de elementen vergeleken met `Equals`, om te zien of het gezochte element er effectief in zit.

Dat is waarom `HashSet<T>.Contains` en `Dictionary<TKey, ...>.ContainsKey` bijna constante tijd (*O(1)*) halen, terwijl `List<T>.Contains` lineair is (*O(n)*).

### Het contract tussen `Equals` en `GetHashCode`

Die werkwijze legt een harde regel op:

> **Als `a.Equals(b)` gelijk is aan `true`, dan moet `a.GetHashCode()` exact gelijk zijn aan `b.GetHashCode()`.**

Breek je die regel, dan gaat het mis:

- `a` wordt toegevoegd aan een `HashSet` en belandt in bakje 7 (op basis van hash van `a`).
- Je vraagt `set.Contains(b)`. `b` is logisch gelijk aan `a`, maar heeft een andere hash — bijvoorbeeld bakje 12.
- Het `HashSet` kijkt in bakje 12, vindt niets, en antwoordt `false`.

Je object is "onvindbaar geworden" in zijn eigen collectie.

Het omgekeerde mág wel: twee objecten met dezelfde hashcode hoeven niet gelijk te zijn (dat heet een *hashcollisie*, en daarvoor is precies de extra `Equals`-check bedoeld). Maar twee gelijke objecten móéten dezelfde hash hebben.

### Praktische regel

Overschrijf `Equals` en `GetHashCode` **altijd samen**, gebruik **dezelfde velden** in allebei:

```csharp
public override bool Equals(object? obj)
{
    if (obj is not Klant andere) return false;
    return KlantNummer == andere.KlantNummer;   // 1 veld: KlantNummer
}

public override int GetHashCode()
{
    return KlantNummer.GetHashCode();           // zelfde veld
}
```

De C#-compiler waarschuwt je trouwens als je `Equals` overschrijft zonder `GetHashCode`: **CS0659**. Negeer die waarschuwing nooit.

### Waarschuwing: muteerbare velden

Gebruik in `GetHashCode` **geen velden die kunnen veranderen** zolang het object in een hashingstructuur zit. Als de hash van een object wijzigt terwijl het al in een `HashSet` of als `Dictionary`-sleutel gebruikt wordt, raakt het object zijn eigen bakje kwijt en wordt het onvindbaar — zelfs voor zichzelf. Kies daarom altijd voor **onveranderlijke** velden (`readonly` of `init`-only properties) als basis voor gelijkheid. In het `Klant`-voorbeeld is `KlantNummer` `get`-only; dat is bewust.

---

## 5. Voorbeeld: facturen opzoeken per klant

Stel: een facturatiesysteem houdt per klant bij welke facturen er openstaan. De voor de hand liggende datastructuur is een `Dictionary` met de klant als sleutel:

```csharp
Dictionary<Klant, List<Factuur>> facturenPerKlant = new();
```

We vullen die dictionary op een bepaald moment in het programma:

```csharp
Klant jan   = new Klant(12345, "Jan Peeters");
Klant marie = new Klant(67890, "Marie Janssens");

facturenPerKlant[jan] = new List<Factuur>
{
    new Factuur("F-2025-001", 245.00m, "Consultancy januari"),
    new Factuur("F-2025-017", 120.00m, "Licenties"),
};

facturenPerKlant[marie] = new List<Factuur>
{
    new Factuur("F-2025-002", 590.00m, "Consultancy januari"),
};
```

Ergens anders in het programma — bijvoorbeeld in een web-API die een factuuroverzicht opvraagt — krijgen we een `Klant`-object van elders (uit de database, uit een sessie, uit een andere module). Dat is technisch gezien een *ander object* dan `jan` uit het fragment hierboven, maar logisch gezien gaat het om dezelfde persoon:

```csharp
Klant aangevraagdeKlant = new Klant(12345, "Jan Peeters");

if (facturenPerKlant.TryGetValue(aangevraagdeKlant, out var facturen))
{
    Console.WriteLine($"Openstaande facturen voor {aangevraagdeKlant.Naam}:");
    foreach (Factuur f in facturen)
    {
        Console.WriteLine($" - {f.Nummer}: {f.Bedrag:C} ({f.Omschrijving})");
    }
}
else
{
    Console.WriteLine("Geen facturen gevonden.");
}
```

### Wat gebeurt er afhankelijk van wat je overschreven hebt?

**Zonder `Equals`- en `GetHashCode`-override** op `Klant`:

```
Geen facturen gevonden.
```

De `Dictionary` heeft `jan` opgeslagen in bakje X (op basis van een hash afgeleid van zijn geheugenadres). `aangevraagdeKlant` heeft een *ander* geheugenadres, dus een *andere* hash, en wijst naar bakje Y. Het zoeken lukt niet, hoewel de inhoud identiek is.

**Met enkel `Equals` overschreven** (en `GetHashCode` vergeten):

Dit lijkt misschien te werken, maar is gevaarlijk. Het `Dictionary` gebruikt nog steeds de standaard `GetHashCode` (afgeleid van de referentie). `jan` en `aangevraagdeKlant` hebben dus verschillende hashcodes, belanden in verschillende bakjes, en `TryGetValue` vindt niets. Opnieuw:

```
Geen facturen gevonden.
```

En je krijgt compilerwaarschuwing CS0659 die je niet mag negeren.

**Met beide overschreven**, correct op `KlantNummer`:

```
Openstaande facturen voor Jan Peeters:
 - F-2025-001: € 245,00 (Consultancy januari)
 - F-2025-017: € 120,00 (Licenties)
```

Nu pas gedraagt de `Dictionary` zich zoals je logisch zou verwachten.

### Zelfde principe met `HashSet`

Als we willen bijhouden *welke klanten reeds betaald hebben*, kunnen we een `HashSet<Klant>` gebruiken:

```csharp
HashSet<Klant> betaaldeKlanten = new();
betaaldeKlanten.Add(jan);

// ergens anders
bool heeftBetaald = betaaldeKlanten.Contains(new Klant(12345, "Jan Peeters"));
```

Exact dezelfde dynamiek: zonder overrides is `heeftBetaald` altijd `false`, ook al zit Jan er wel degelijk in. Met correcte overrides werkt het.

---

## 6. `IEquatable<T>`

Door `IEquatable<T>` te implementeren, kan je een **typed** `Equals` toevoegen. Collecties gebruiken die als ze beschikbaar is, wat een boxing-operatie en een typecast uitspaart:

```csharp
public class Klant : IEquatable<Klant>
{
    // ...

    public bool Equals(Klant? andere)
    {
        if (andere is null) return false;
        return KlantNummer == andere.KlantNummer;
    }

    public override bool Equals(object? obj) => Equals(obj as Klant);

    public override int GetHashCode() => KlantNummer.GetHashCode();
}
```

Voor klassen die frequent in hashingstructuren gebruikt worden, is dit de aangeraden stijl.

---

## 7. `record` in plaats van `class`

Als jouw type **waarde-achtig** is — twee objecten met dezelfde data zijn gewoon hetzelfde — dan is een `record` bijna altijd beter dan een `class`. Een `record` genereert automatisch een `Equals`, een `GetHashCode`, een `==`/`!=` operator en zelfs een leesbare `ToString`, op basis van **alle** velden:

```csharp
public record Factuur(string Nummer, decimal Bedrag, string Omschrijving);

Factuur f1 = new("F-2025-001", 245m, "Consultancy januari");
Factuur f2 = new("F-2025-001", 245m, "Consultancy januari");

Console.WriteLine(f1 == f2);       // True — records vergelijken op inhoud
Console.WriteLine(f1.Equals(f2));  // True
```

Merk het verschil met een gewone `class`: bij een `record` werkt zelfs `==` uit de doos op basis van inhoud. Alle vervelende boilerplate uit de vorige secties kan je voor waarde-achtige types dus volledig overslaan.

**Wanneer géén record?** Zodra identiteit niet gedefinieerd is door *alle* velden, maar bijvoorbeeld enkel door een ID. Een `Klant` is typisch zo'n geval: de naam kan wijzigen, het adres ook, maar het klantnummer blijft dezelfde persoon. Daar is een `class` met manuele `Equals`/`GetHashCode` op het klantnummer logischer, omdat het domein zegt: "de klant wordt *gedefinieerd* door zijn nummer, niet door zijn huidige gegevens".

Vuistregel:

| Karakter van het type | Keuze |
|---|---|
| **Waarde-object** — alle velden samen definiëren identiteit (Factuur, Bedrag, Coördinaat, Datum) | `record` |
| **Entiteit** — identiteit zit in een ID, andere velden kunnen wijzigen (Klant, Product, Gebruiker) | `class` met manuele overrides |

---

## 8. Operator overloading: `==` en `!=`

Tot nu toe werkte `==` op klassen altijd als referentievergelijking, ook al hadden we `Equals` overschreven. Voor **waarde-achtige** types voelt dat onnatuurlijk aan: als twee `Temperatuur`-objecten dezelfde temperatuur voorstellen (ook al is de ene opgegeven in Celsius en de andere in Fahrenheit), dan wil je `==` kunnen gebruiken.

Je kan `==` (en `!=`) overladen voor je eigen klasse:

```csharp
public enum Schaal { Celsius, Fahrenheit }

public class Temperatuur : IEquatable<Temperatuur>
{
    // Intern altijd in hele graden Celsius
    private readonly int _celsius;

    public Temperatuur(int waarde, Schaal schaal)
    {
        _celsius = schaal switch
        {
            Schaal.Celsius    => waarde,
            Schaal.Fahrenheit => (waarde - 32) * 5 / 9,
            _ => throw new ArgumentException($"Onbekende schaal: {schaal}")
        };
    }

    public int InCelsius    => _celsius;
    public int InFahrenheit => _celsius * 9 / 5 + 32;

    public bool Equals(Temperatuur? andere)
    {
        if (andere is null) return false;
        return _celsius == andere._celsius;
    }

    public override bool Equals(object? obj) => Equals(obj as Temperatuur);

    public override int GetHashCode() => _celsius.GetHashCode();

    public static bool operator ==(Temperatuur? a, Temperatuur? b)
    {
        if (a is null) return b is null;
        return a.Equals(b);
    }

    public static bool operator !=(Temperatuur? a, Temperatuur? b) => !(a == b);
}
```

Nu werkt het zoals je het intuïtief zou schrijven:

```csharp
Temperatuur vriespunt          = new(0, Schaal.Celsius);
Temperatuur zelfdeInFahrenheit = new(32, Schaal.Fahrenheit);

Console.WriteLine(vriespunt == zelfdeInFahrenheit);        // True
Console.WriteLine(vriespunt.Equals(zelfdeInFahrenheit));   // True
```

### Belangrijke aandachtspunten bij operator overloading

1. **Consistent houden met `Equals`.** `==` moet exact hetzelfde resultaat geven als `Equals`. Als je ze laat afwijken, krijg je een onleesbare codebase.
2. **Null-veilig schrijven.** Gebruik `is null` (niet `== null`) binnen de operator zelf, anders krijg je oneindige recursie: jouw `==` roept zichzelf op.
3. **`!=` altijd samen met `==` overladen.** Laat je er eentje weg, dan geeft de compiler een fout.
4. **Enkel doen voor waarde-achtige types.** Voor entiteiten (zoals `Klant`) is het meestal een slecht idee, omdat lezers van je code `==` vaak als referentievergelijking verwachten voor langlevende objecten met een identiteit.
5. **Records doen dit automatisch.** Als je toch een record kan gebruiken, hoef je `==` niet zelf te schrijven.

> **Floating point in het echt.** In bovenstaand voorbeeld gebruiken we `int` voor de eenvoud, zodat 0 °C en 32 °F exact gelijk zijn. Zodra je met `double` werkt, wordt gelijkheid van temperaturen een tolerantie-vraagstuk (`Math.Abs(a - b) < epsilon`) — en dan is een `==` die naar exact-gelijk kijkt meestal *niet* wat je wil.

---

## 9. `IEqualityComparer<T>`: gelijkheid zonder de klasse aan te passen

Soms kan je `Equals` en `GetHashCode` **niet** overschrijven, of je **wil** dat niet. Bijvoorbeeld:

- De klasse komt uit een externe library en je kan haar niet aanpassen.
- De klasse heeft al een `Equals`-regel, maar voor deze specifieke situatie heb je een *andere* regel nodig (bv. "twee klanten zijn hetzelfde als ze in dezelfde postcode wonen").
- Je werkt met `string` en wil bijvoorbeeld hoofdletter-ongevoelig vergelijken.

Voor die gevallen bestaat de interface `IEqualityComparer<T>`. Het is een **apart object** dat de gelijkheidsregel meedraagt, los van de klasse zelf.

```csharp
public class KlantOpPostcodeComparer : IEqualityComparer<Klant>
{
    public bool Equals(Klant? x, Klant? y)
    {
        if (x is null) return y is null;
        if (y is null) return false;
        return string.Equals(x.Postcode, y.Postcode, StringComparison.OrdinalIgnoreCase);
    }

    public int GetHashCode(Klant obj)
    {
        return obj.Postcode.ToUpperInvariant().GetHashCode();
    }
}
```

Deze comparer geef je mee aan de methode of de collectie die je wil beïnvloeden:

```csharp
List<Klant> klanten = new() { /* ... */ };

// LINQ Distinct met custom comparer
var uniekePerPostcode = klanten
    .Distinct(new KlantOpPostcodeComparer())
    .ToList();

// HashSet met custom comparer
HashSet<Klant> perPostcode = new(new KlantOpPostcodeComparer());

// Dictionary met custom comparer
Dictionary<Klant, decimal> omzetPerPostcode =
    new(new KlantOpPostcodeComparer());
```

De rest van je programma blijft onaangetast: `klant1.Equals(klant2)` gebruikt nog altijd de gewone regel (op `KlantNummer`), maar *deze specifieke collectie* gebruikt de postcode-regel.

### Ingebouwde comparers voor strings

Voor strings heb je die comparers niet zelf nodig — .NET biedt ze kant-en-klaar:

```csharp
HashSet<string> woorden = new(StringComparer.OrdinalIgnoreCase);
woorden.Add("Hello");
woorden.Contains("HELLO");   // True
```

`StringComparer.OrdinalIgnoreCase`, `CurrentCultureIgnoreCase`, `InvariantCulture`... allemaal implementaties van `IEqualityComparer<string>`.

### Wanneer klasse-override vs. comparer?

| Scenario | Aanpak |
|---|---|
| Dé gelijkheidsregel van het type (één, domein-bepaald) | `Equals`/`GetHashCode` overschrijven |
| Tijdelijke of alternatieve regel (per collectie of per operatie) | `IEqualityComparer<T>` |
| Type uit externe library, regel die je zelf wil bepalen | `IEqualityComparer<T>` |

---

## 10. Alternatief: identity map / object registry

Er is een radicaal ander antwoord op hetzelfde probleem: **zorg dat er voor elke logische klant maar één fysiek object in het geheugen bestaat**. Dan valt logische gelijkheid sowieso samen met referentiegelijkheid en heb je `Equals` en `GetHashCode` niet meer nodig.

Dit patroon heeft meerdere namen:

- **Identity Map** (Martin Fowler, *Patterns of Enterprise Application Architecture*)
- **Object Registry** of **Object Cache**
- Voor strings heet het **interning** (via `string.Intern`)

Het idee: een centrale factory houdt een register bij van reeds aangemaakte objecten, en geeft hetzelfde exemplaar terug wanneer hetzelfde klantnummer opnieuw wordt opgevraagd.

```csharp
public class Klant
{
    public int KlantNummer { get; }
    public string Naam { get; set; }

    private static readonly Dictionary<int, Klant> _registry = new();

    // Constructor is private — niemand kan rechtstreeks een Klant maken
    private Klant(int klantNummer, string naam)
    {
        KlantNummer = klantNummer;
        Naam = naam;
    }

    // Enige manier om een Klant te bekomen
    public static Klant GetOrCreate(int klantNummer, string naam)
    {
        if (!_registry.TryGetValue(klantNummer, out Klant? bestaande))
        {
            bestaande = new Klant(klantNummer, naam);
            _registry[klantNummer] = bestaande;
        }
        return bestaande;
    }
}
```

Gebruik:

```csharp
Klant jan1 = Klant.GetOrCreate(12345, "Jan Peeters");
Klant jan2 = Klant.GetOrCreate(12345, "Jan Peeters");

Console.WriteLine(ReferenceEquals(jan1, jan2));  // True — letterlijk hetzelfde object
Console.WriteLine(jan1 == jan2);                  // True — zonder enige override
```

In dit model is er voor klantnummer 12345 maar één `Klant`-instantie in het hele programma. Elke verwijzing naar "die klant" is dezelfde referentie. `Dictionary<Klant, ...>` werkt, `HashSet<Klant>` werkt, `List<Klant>.Contains` werkt — allemaal zonder ook maar één `Equals`- of `GetHashCode`-override.

### Afwegingen

**Voordelen:**

- Geen `Equals`/`GetHashCode`-boilerplate nodig.
- Gegarandeerd consistent: er is maar één "waarheid" per klant in het geheugen.
- Updates aan de klantgegevens worden automatisch overal zichtbaar — elke referentie wijst immers naar hetzelfde object.

**Nadelen:**

- **Geheugenlek**: objecten in het register worden nooit opgeruimd door de garbage collector. Voor langlopende applicaties moet je zelf een opruimstrategie voorzien (bv. met `WeakReference`).
- **Thread-safety**: de `Dictionary` is niet thread-safe. In een multi-threaded context moet je locks of `ConcurrentDictionary` gebruiken.
- **Globale state**: een statisch register maakt je code moeilijker te testen en te isoleren (twee tests kunnen elkaars `Klant`-objecten zien).
- **Serialisatie**: als je klanten uit een database haalt of over een netwerk stuurt, moet je expliciet door je factory gaan, anders krijg je tóch dubbele objecten.
- **Wie "wint" bij tegenstrijdige gegevens?** In het voorbeeld hierboven: als iemand `GetOrCreate(12345, "Jan Peeters")` oproept en later `GetOrCreate(12345, "Jan P.")`, welke naam hou je dan? Dat zijn niet-triviale ontwerpkeuzes.

### Waar zie je dit in de echte wereld?

- **Entity Framework Core** implementeert een identity map in zijn `DbContext`: binnen één context krijg je voor elke primary key exact hetzelfde object terug. Dat is waarom je binnen één EF-transactie geen zorgen moet maken over `Equals`-overrides op je entities.
- **`string.Intern`** in .NET zelf: `string`-literals met dezelfde inhoud delen één object in de *intern pool*.
- ORM-frameworks in het algemeen (Hibernate in Java, ActiveRecord in Ruby) steunen op varianten van dit patroon.

### Wanneer wél, wanneer niet?

Voor **klassiek cursuswerk** en de meeste business-applicaties is `Equals`/`GetHashCode` overschrijven het eenvoudigere en flexibelere antwoord. Een identity map is voornamelijk zinvol wanneer:

- Je al met een persistentielaag werkt die het patroon aanbiedt (EF Core, ...) — dan gebruik je het gratis.
- Identiteit delen over je hele applicatie heen een *ontwerpprincipe* is, niet alleen een technisch detail.

Wees vooral op je hoede voor het handmatig implementeren ervan als "oplossing" voor Equals-problemen. De complexiteit die erbij komt kijken (geheugen, threads, levenscycli) is meestal groter dan het probleem dat je ermee oplost.

---

## 11. Samenvatting

| Situatie | Wat je nodig hebt |
|---|---|
| `List<T>.Contains`, `IndexOf`, `Remove` met eigen objecten | `Equals` overriden |
| LINQ `Distinct`, `Except`, `Intersect`, `Union`, `SequenceEqual`, `Contains` | `Equals` overriden (en bij voorkeur ook `GetHashCode`) |
| `HashSet<T>.Contains`, `Add` (duplicaatdetectie) | `Equals` **én** `GetHashCode` overriden |
| `Dictionary<TKey, TValue>.ContainsKey`, `TryGetValue`, `[indexer]` | `Equals` **én** `GetHashCode` overriden op de *sleutel*-klasse |
| `==` en `!=` als logische vergelijking | Operator overloading |
| Waarde-achtig type (alle velden samen = identiteit) | `record` gebruiken |
| Alternatieve of tijdelijke gelijkheidsregel | `IEqualityComparer<T>` |
| Eén fysiek object per logische identiteit garanderen | Identity map / object registry (of een ORM die dit regelt) |

**Het contract in één zin:**

> Wie `Equals` overschrijft, overschrijft ook `GetHashCode`, en allebei steunen op exact dezelfde (onveranderlijke) velden.

Negeer compilerwaarschuwing **CS0659** nooit: dat is letterlijk de compiler die zegt "dit gaat vroeg of laat mis".

---

## Oefeningen

1. Schrijf een klasse `Boek` met velden `ISBN`, `Titel`, `Auteur`. Overschrijf `Equals` en `GetHashCode` zodat twee `Boek`-objecten als gelijk beschouwd worden wanneer ze dezelfde `ISBN` hebben. Test met `List<Boek>.Contains` en met `HashSet<Boek>`.

2. Schets wat er mis gaat in onderstaand fragment. Verklaar wat de output is en waarom:

    ```csharp
    public class Punt
    {
        public int X { get; set; }
        public int Y { get; set; }

        public override bool Equals(object? obj)
        {
            if (obj is not Punt p) return false;
            return X == p.X && Y == p.Y;
        }
        // Geen GetHashCode-override
    }

    HashSet<Punt> set = new();
    set.Add(new Punt { X = 1, Y = 2 });
    Console.WriteLine(set.Contains(new Punt { X = 1, Y = 2 }));
    ```

3. Breid het voorbeeld van `facturenPerKlant` uit: voeg een methode toe die voor elke klant het totaalbedrag van de openstaande facturen berekent. Gebruik daarvoor `Dictionary.TryGetValue` en LINQ's `Sum`.

4. Converteer de klasse `Factuur` naar een `record`. Wat verandert er aan het gedrag van `==` en `Equals`? Welke eigenschappen van `Factuur` moeten dan verplicht bij constructie worden opgegeven?

5. Breid de `Temperatuur`-klasse uit met de operatoren `<`, `>`, `<=` en `>=`. Wat is het natuurlijke gedrag voor `new Temperatuur(0, Celsius) < new Temperatuur(32, Fahrenheit)`?

6. Schrijf een `IEqualityComparer<Klant>` die twee klanten als gelijk beschouwt wanneer hun naam (hoofdletter-ongevoelig) gelijk is. Gebruik die comparer in een `HashSet<Klant>` en in `klanten.Distinct(...)`. Waarom is dit géén goede kandidaat voor een override van `Klant.Equals` zelf?

7. *Denkoefening.* Neem het identity-map-voorbeeld uit sectie 10. Wat zijn concrete scenario's waarin het patroon tot vreemde bugs kan leiden (denk aan: unit tests die na elkaar lopen, een klant waarvan de naam in de database wijzigt terwijl het programma draait, multi-threaded webapplicaties)? Formuleer voor elk scenario één zin uitleg.
