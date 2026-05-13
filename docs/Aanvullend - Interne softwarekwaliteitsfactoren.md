# Interne softwarekwaliteit

> *"Iedere zot kan code schrijven die een computer begrijpt. Goeie programmeurs schrijven code die mensen begrijpen."* — vrij naar Martin Fowler

## Inleiding

Er bestaan twee soorten softwarekwaliteit:

- **Externe kwaliteit**: wat de eindgebruiker merkt. Werkt het programma? Is het snel genoeg? Crasht het niet? Is de UI bruikbaar?
- **Interne kwaliteit**: wat *wij als ontwikkelaars* merken zodra we de code openen. Begrijp ik wat er staat? Kan ik dit aanpassen zonder iets anders kapot te maken? Kan ik dit testen?

Een programma kan extern perfect lijken en intern een puinhoop zijn. Dat valt op het moment zelf niet op, maar wel binnen drie weken, wanneer jij of een collega er een feature aan moet toevoegen.

Hieronder lopen we de belangrijkste interne kwaliteitsfactoren door, van micro-niveau (één regel code) tot macro-niveau (architectuur).

## 1. Op het niveau van enkele regels code

### 1.1 Naming guidelines volgen

Een goeie naam *toont de intentie*. Iemand die je code leest, mag niet hoeven raden waar `x` voor staat.

```csharp
// ❌ Slecht
double x = p * 0.21;
double t = p + x;

// ✅ Beter
double btw = nettoprijs * BtwTarief;
double brutoprijs = nettoprijs + btw;
```

Daarnaast volg je de **conventies van de taal**. In C# is dat onder andere:

| Element | Conventie | Voorbeeld |
|---|---|---|
| Klassen, methodes, properties | PascalCase | `Klant`, `BerekenTotaal()`, `Voornaam` |
| Lokale variabelen, parameters | camelCase | `aantalProducten`, `klantId` |
| Private velden | `_camelCase` | `_repository` |
| Constanten | PascalCase | `MaxAantalPogingen` |
| Interfaces | `I` + PascalCase | `IKlantRepository` |

In JavaScript/TypeScript zijn de regels lichtjes anders (camelCase voor methodes, PascalCase voor klassen en types). Het belangrijkste is: **wees consistent binnen één project**.

### 1.2 Consistente lay-out

Indentatie, accolades, spaties rond operatoren... Allemaal kleine dingen, maar als ze inconsistent zijn wordt code moe-makend om te lezen.

```csharp
// ❌ Slecht
if(leeftijd>=18){
Console.WriteLine ("meerderjarig");
} else{
   Console.WriteLine("minderjarig" );}

// ✅ Beter in Allman style (de default in de .NET wereld) 
if (leeftijd >= 18)
{
    Console.WriteLine("meerderjarig");
}
else
{
    Console.WriteLine("minderjarig");
}

// Of in K&R (Kernighan & Ritchie) style (populair in JavaScript)
// en ook vaak gebruikt in ons leermateriaal (om wat "compacter" te zijn)
if (leeftijd >= 18) {
    Console.WriteLine("meerderjarig");
} else {
    Console.WriteLine("minderjarig");
}

// In dit geval kan je natuurlijk ook de accolades weglaten, 
// maar dan nog steeds de spaties en indentatie behouden:
if (leeftijd >= 18)
    Console.WriteLine("meerderjarig");
else
    Console.WriteLine("minderjarig");
```

Goed nieuws: dit hoef je niet manueel te doen. Visual Studio formatteert automatisch (Ctrl+K, Ctrl+D).

### 1.3 Geen *magic values*

Een "magic value" is een waarde die zomaar in de code staat, zonder uitleg.

```csharp
// ❌ Slecht: wat betekent 21? en 100?
double prijs = nettoprijs * 21 / 100;

// ✅ Beter
const double BtwTariefStandaard = 0.21;
double prijs = nettoprijs * BtwTariefStandaard;
```

Hetzelfde voor strings die ergens een betekenis hebben:

```csharp
// ❌ Slecht
if (status == "ACTIEF") { ... }

// ✅ Beter: een enum of een constante
if (status == KlantStatus.Actief) { ... }
```

Dat laatste veronderstelt natuurlijk dat je een `KlantStatus`-enum hebt gedefinieerd:
```csharp
public enum KlantStatus
{
    Actief,
    Inactief,
    Geblokkeerd
}
```

### 1.4 Geen dode code

Code die nergens meer gebruikt wordt, uitgecommentarieerde blokken "voor het geval dat", ongebruikte variabelen, methodes die nooit aangeroepen worden... allemaal weg.

```csharp
// ❌ Slecht
public void VerwerkBestelling(Bestelling b)
{
    // Oude versie
    // foreach (var lijn in b.Lijnen) { ... }
    // if (b.Klant.IsVip) korting = 0.10;

    VerstuurNaarMagazijn(b);
}
```

Je hebt **versiebeheer** (Git) om oude code terug te vinden. De code zelf mag opgeruimd zijn.

### 1.5 Commentaar zegt het *waarom*, niet het *wat*

Goede code lees je als een verhaal. Commentaar is enkel nodig voor zaken die niet uit de code zelf blijken: een vreemde bedrijfsregel, een workaround voor een bug, een uitleg waarom je iets *niet* zo gedaan hebt.

```csharp
// ❌ Slecht: zegt gewoon hetzelfde als de code
// Verhoog teller met 1
teller++;

// ✅ Beter: zegt waarom we dit doen
// We tellen pogingen om brute-force aanvallen na 5 pogingen te blokkeren
misluktePogingen++;
```

### 1.6 Guard clauses in plaats van geneste if's

Geneste if's zijn moeilijk te volgen. Zet uitzonderingen vooraan en handel ze meteen af.

```csharp
// ❌ Slecht (arrow anti-pattern)
public double BerekenKorting(Klant klant)
{
    if (klant != null)
    {
        if (klant.IsActief)
        {
            if (klant.Bestellingen.Count > 0)
            {
                return ...;
            }
        }
    }
    return 0;
}

// ✅ Beter
public double BerekenKorting(Klant klant)
{
    if (klant == null) return 0;
    if (!klant.IsActief) return 0;
    if (klant.Bestellingen.Count == 0) return 0;

    return ...;
}
```

Een **guard clause** is een check vooraan in een methode die een uitzonderlijk geval afhandelt en de methode meteen verlaat (via `return`, `throw` of `continue`/`break` in een lus). Het idee: de "wachter" staat aan de poort en stuurt foute gevallen meteen terug, zodat de rest van de methode op de normale weg kan focussen.

De term is wijdverspreid, vooral gepopulariseerd door Martin Fowler in zijn boek Refactoring (1999), waar het opduikt als de refactoring **"Replace Nested Conditional with Guard Clauses"**.

**Wat guard clauses doen**, drie typische rollen:

**1. Validatie van parameters (defensief programmeren)**

```csharp
public void Storten(double bedrag)
{
    if (bedrag <= 0)
        throw new ArgumentException("Bedrag moet positief zijn.");

    Saldo += bedrag;
}
```
**2. Vroeg terugkeren bij triviale gevallen**

```csharp
public double BerekenKorting(Klant klant)
{
    if (klant == null) return 0;
    if (!klant.IsActief) return 0;

    // hoofdlogica
}
```

**3. In een lus: oninteressante items overslaan**

```csharp
foreach (var bestelling in bestellingen)
{
    if (bestelling.IsGeannuleerd) continue;
    if (bestelling.Klant == null) continue;

    Verwerk(bestelling);
}
```

**Waarom dit beter is dan diep nesten**

- **Het normale geval wordt zichtbaar.** Met geneste if's verstop je de hoofdlogica binnenin lagen indentatie. Met guards staat de hoofdlogica plat in de methode.
- **Uitzonderingen staan bovenaan**, waar de lezer als eerste kijkt. Dat is ook waar je hersenen ze het makkelijkst verwerken: "deze gevallen tellen niet — al de rest van de methode handelt over het happy path".
- **Lagere cyclomatische complexiteit** in visuele zin — al blijven het natuurlijk evenveel beslispunten.
- **Makkelijker uitbreiden.** Een nieuwe randvoorwaarde? Eén regel erbij vooraan, geen herstructurering van geneste blokken.

**Tegenargument: "single exit point"**

Niet iedereen is fan. Er bestaat een oudere school (deels uit Pascal/structured-programming-tijdperk) die zegt: **één enkele exit per methode**. Volgens die visie zijn vroege return's slecht omdat ze het redeneren over de methode bemoeilijken — je moet alle mogelijke exit-punten in het oog houden.

In moderne objectgeoriënteerde codebases is dit standpunt sterk in de minderheid. De consensus, ook in Microsofts eigen .NET-stijl, is dat de leesbaarheidswinst van guard clauses ruimschoots opweegt tegen het bezwaar — zolang je methodes kort houdt. Voor een methode van 200 regels met tien return's op willekeurige plekken geldt het bezwaar wél: dan ben je het overzicht kwijt. Daarom horen guard clauses **vooraan**, niet halverwege.

## 2. Op het niveau van een methode of functie

### 2.1 DRY — Don't Repeat Yourself

Dezelfde logica mag niet op meerdere plaatsen herhaald worden. Als je een wijziging moet doorvoeren, wil je dat op *één* plek doen.

```csharp
// ❌ Slecht
double prijsTrui = 25 + (25 * 0.21);
double prijsBroek = 50 + (50 * 0.21);
double prijsSchoen = 80 + (80 * 0.21);

// ✅ Beter
double prijsTrui = BerekenBrutoprijs(25);
double prijsBroek = BerekenBrutoprijs(50);
double prijsSchoen = BerekenBrutoprijs(80);

double BerekenBrutoprijs(double netto) { return netto + (netto * 0.21); }
```

**Maar pas op**: niet alle herhaling is duplicatie. Als twee stukken code *toevallig* dezelfde regels hebben maar over verschillende concepten gaan, mag je ze gerust apart laten. Een vuistregel is de *Rule of Three*: pas extraheren als je het voor de **derde keer** schrijft.

### 2.2 Korte methodes met één duidelijke taak

Een methode moet je in één oogopslag kunnen begrijpen. Vuistregel: past op één scherm zonder scrollen.

```csharp
// ❌ Slecht: een methode die alles doet
public void VerwerkBestelling(int klantId, List<int> productIds)
{
    // klant ophalen uit database
    // producten ophalen
    // voorraad controleren
    // prijs berekenen
    // korting toepassen
    // factuur aanmaken
    // mail versturen
    // ... vele regels code
}

// ✅ Beter: opgesplitst per verantwoordelijkheid
public void VerwerkBestelling(int klantId, List<int> productIds)
{
    Klant klant = HaalKlantOp(klantId);
    List<Product> producten = HaalProductenOp(productIds);
    ControleerVoorraad(producten);
    Factuur factuur = MaakFactuurAan(klant, producten);
    VerstuurBevestiging(klant, factuur);
}
```

### 2.3 Beperk het aantal parameters

Hoe meer parameters, hoe lastiger om de methode correct te gebruiken. Vuistregel: **maximaal 3 à 4**. Heb je er meer nodig? Dan ontbreekt er meestal een object dat ze samenhoudt.

```csharp
// ❌ Slecht
public void MaakKlantAan(string voornaam, string achternaam, string straat,
    string huisnummer, string postcode, string gemeente, string telefoon,
    string email, DateTime geboortedatum) { ... }

// ✅ Beter
public void MaakKlantAan(KlantGegevens gegevens) { ... }
```

### 2.4 Beperk de complexiteit

Hoe meer `if`, `else`, `switch`, `for` en `while` door elkaar in één methode, hoe moeilijker te lezen *en* te testen. Splits op in kleinere methodes.

```csharp
// ❌ Te complex: te veel beslissingen in één methode
public double BerekenKorting(Klant klant, Bestelling bestelling)
{
    double korting = 0;

    if (klant != null && klant.IsActief)
    {
        if (klant.Type == KlantType.Vip)
        {
            if (bestelling.Totaal > 500)
                korting = bestelling.Totaal * 0.15;
            else
                korting = bestelling.Totaal * 0.10;
        }
        else if (klant.Type == KlantType.Standaard)
        {
            if (bestelling.Totaal > 1000)
                korting = bestelling.Totaal * 0.05;
        }

        if (DateTime.Now.Month == 12)
            korting += bestelling.Totaal * 0.02;
    }

    return korting;
}
```

Wat hier mis loopt: één methode beantwoordt drie vragen tegelijk (komt deze klant in aanmerking? welk basistarief? geldt er een seizoenstoeslag?), en de lezer moet alles tegelijk in zijn hoofd houden.

Opgesplitst per beslissing:

```csharp
// ✅ Beter: elke methode beantwoordt één vraag
public double BerekenKorting(Klant klant, Bestelling bestelling)
{
    if (!KomtInAanmerking(klant)) return 0;

    double basis = BepaalBasiskorting(klant, bestelling);
    double seizoen = BepaalSeizoenskorting(bestelling);

    return basis + seizoen;
}

private bool KomtInAanmerking(Klant klant)
{
    return klant != null && klant.IsActief;
}

private double BepaalBasiskorting(Klant klant, Bestelling bestelling)
{
    if (klant.Type == KlantType.Vip && bestelling.Totaal > 500)
        return bestelling.Totaal * 0.15;

    if (klant.Type == KlantType.Vip)
        return bestelling.Totaal * 0.10;

    if (klant.Type == KlantType.Standaard && bestelling.Totaal > 1000)
        return bestelling.Totaal * 0.05;

    return 0;
}

private double BepaalSeizoenskorting(Bestelling bestelling)
{
    if (DateTime.Now.Month == 12)
        return bestelling.Totaal * 0.02;

    return 0;
}
```

Wat is er gebeurd?

- De **hoofdmethode** leest nu als een verhaal: *komt in aanmerking → bereken basis → bereken seizoen → tel op*.
- Elke deelmethode heeft een **naam die de bedoeling toont**.
- De geneste if/else is vervangen door een rij **guard clauses** die elk één geval afhandelen en meteen terugkeren — plat in plaats van piramidaal.
- Voor **unit tests** kan je nu `BepaalBasiskorting` apart testen zonder je zorgen te maken over seizoenslogica of de aanmerking-check.

### 2.5 Command-Query Separation

Een methode doet **één** van twee dingen:

- **Command**: voert iets uit, verandert iets (return type `void`).
- **Query**: geeft een waarde terug, verandert niets.

Niet allebei tegelijk.

```csharp
// ❌ Slecht: deze "query" verandert ook de toestand
public int GeefAantalPogingen()
{
    _aantalPogingen++;
    return _aantalPogingen;
}

// ✅ Beter
public void RegistreerPoging() { _aantalPogingen++; }
public int AantalPogingen { get { return _aantalPogingen; } }
```

### 2.6 Foutafhandeling

- Geen lege `catch`-blokken — fouten "stilletjes opslokken" *doen we niet*.
- Geen exceptions misbruiken als gewone controle-flow.
- Vang enkel op wat je *kan* afhandelen.

```csharp
// ❌ Slecht
try
{
    BewerkFile(pad);
}
catch (Exception)
{
    // ssht, gewoon negeren
}

// ✅ Beter
try
{
    BewerkFile(pad);
}
catch (FileNotFoundException ex)
{
    _logger.Warning($"Bestand niet gevonden: {pad}");
    throw; // of een gepaste afhandeling
}
```


## 3. Op het niveau van een klasse

### 3.1 Single Responsibility Principle (SRP)

Een klasse heeft **één reden om te veranderen**. Een `Klant`-klasse die ook nog factuurnummers genereert én mails verstuurt, heeft drie redenen om aangepast te worden — en wordt dus drie keer zo snel een puinhoop.

```csharp
// ❌ Slecht: doet veel te veel
public class Klant
{
    public string Naam { get; set; }
    public void OpslaanInDatabase() { ... }
    public void VerstuurWelkomMail() { ... }
    public string GenereerFactuurnummer() { ... }
}

// ✅ Beter: gescheiden verantwoordelijkheden
public class Klant { public string Naam { get; set; } }
public class KlantRepository { public void Opslaan(Klant k) { ... } }
public class MailService { public void VerstuurWelkomMail(Klant k) { ... } }
```
SRP wordt door beginners vaak letterlijk genomen: *"één klasse mag maar één ding doen"*. Maar dat klopt niet. Een `KlantRepository` mag perfect zowel `Opslaan`, `Inlezen`, `Verwijderen` als `ZoekOpEmail` bevatten — dat zijn vier methodes, maar samen vormen ze **één coherente verantwoordelijkheid: persistentie** van klanten.

```csharp
public class KlantRepository
{
    public void Opslaan(Klant klant) { ... }
    public Klant InlezenOpId(int id) { ... }
    public List<Klant> InlezenAlle() { ... }
    public void Verwijderen(int id) { ... }
    public Klant ZoekOpEmail(string email) { ... }
}
```

Dat is niet in strijd met SRP, ook al doet de klasse vijf dingen. De vraag is niet "hoeveel methodes?" maar **"hoeveel onafhankelijke redenen tot verandering?"** Robert C. Martin, die het principe geformuleerd heeft, omschrijft het zo: *"A class should have only one reason to change."*

Voor `KlantRepository` is die reden: de manier waarop klanten worden opgeslagen wijzigt (bv. overstap van SQL Server naar PostgreSQL, of van database naar JSON-file). Alle vijf methodes veranderen mee — dus ze horen samen. Pas als je merkt dat je een klasse moet wijzigen om --fundamenteel verschillende redenen--, is er een probleem.

**Een betere formulering: "redenen om te veranderen"**

Vergelijk met het voorbeeld...

```csharp
public class Klant
{
    public string Naam { get; set; }
    public void OpslaanInDatabase() { ... }    // verandert als DB wijzigt
    public void VerstuurWelkomMail() { ... }   // verandert als mailprovider wijzigt
    public string GenereerFactuurnummer() { ... } // verandert als boekhoudregels wijzigen
}
```

Dit zijn **drie verschillende redenen tot verandering**, met drie verschillende "stakeholders": de database-administrator (DBA), de marketingmanager, en de boekhouder. Dát is een SRP-overtreding. Niet het feit dat er drie methodes zijn.

Wat verschillende methodes "bij elkaar" maakt, is **cohesie**...

### 3.2 Hoge cohesie

Een klasse heeft **hoge cohesie** als haar leden allemaal rond hetzelfde concept of dezelfde verantwoordelijkheid draaien. SRP en cohesie zijn nauw verwant: SRP voorschrijft "één reden tot verandering"; cohesie meet hoe goed je daarin slaagt.

Op grotere schaal bundelt men coherente verantwoordelijkheden in lagen of modules. Dat is wat we verderop **separation of concerns** noemden. De klassieke driedeling:

| Laag | Verantwoordelijkheid | Typische klassen |
|------|--------------------|----------------|
| **Presentation / UI** | Gebruikersinteractie, weergave | `KlantForm`, `Program` (in een console-app), ... |
| **Domain / Business** | Bedrijfsregels en -logica | `Klant`, `Bestelling`, `BerekenKorting`, `FactuurService`, ... |
| **Persistence / Data Access** | Opslaan en ophalen van gegevens | `KlantRepository`, ... |

Elke laag heeft zijn eigen "reden tot verandering": de UI verandert als de designer een knop verplaatst, de domeinlaag als de boekhouder de BTW-regels aanpast, de datalaag als de DBA naar een andere database overstapt. Door deze lagen netjes gescheiden te houden, **raken die wijzigingen elkaar niet**.

### 3.3 Encapsulatie

De interne werking van een klasse blijft binnen die klasse. Velden zijn `private`. Toegang verloopt via properties of methodes. Op die manier kan je later de implementatie wijzigen zonder dat de rest van je code breekt.

```csharp
// ❌ Slecht
public class Rekening
{
    public decimal Saldo;  // public veld => iedereen kan dit aanpassen!
    public void Stort(decimal bedrag)
    {
        if (bedrag <= 0) throw new ArgumentException("Bedrag moet positief zijn.");
        Saldo += bedrag;
    }
}

Rekening r = new Rekening();
r.Stort(1000m);
r.Saldo -= 500m;  // oeps (of toch misschien niet de bedoeling?)

// ✅ Beter
public class Rekening
{
    public decimal Saldo { get; private set; }

    public void Stort(decimal bedrag)
    {
        if (bedrag <= 0) throw new ArgumentException("Bedrag moet positief zijn.");
        Saldo += bedrag;
    }
}

Rekening r = new Rekening();
r.Stort(1000m);
r.Saldo -= 500m;  // kan niet (compilefout)
```

Vuistregel: velden steeds **hidden** (`private` of `protected` access modifiers), en toegang via **properties** of **methodes**. Dat geeft je later ook de vrijheid om validatie, logging, berekeningen of andere logica toe te voegen zonder dat de rest van je code er iets van merkt.

### 3.4 Lage koppeling

Hoe minder een klasse over andere klassen weet, hoe makkelijker je iets kan wijzigen. Werk waar mogelijk met **interfaces** in plaats van concrete types, of pas wat men noemt **dependency inversion** toe (lees meer over dependency inversion verderop in het leermateriaal).

### 3.5 Tell, Don't Ask

Vraag een object geen data om er dan zelf iets mee te doen. Laat het object het zelf doen.

```csharp
// ❌ Slecht: we trekken data uit het object om er dan iets mee te doen
if (rekening.Saldo >= bedrag)
{
    rekening.Saldo -= bedrag;
}

// ✅ Beter: het object regelt het zelf
rekening.Afhalen(bedrag);
```

### 3.6 Law of Demeter — "praat enkel met je vrienden"

Vermijd lange ketens van punten. Hoe langer de keten, hoe meer kennis je code heeft over de interne structuur van andere objecten.

```csharp
// ❌ Slecht
string postcode = bestelling.Klant.Adres.Postcode.ToString();

// ✅ Beter: laat Bestelling of Klant het werk doen
string postcode = bestelling.GeefPostcode().ToString();
```

### 3.7 Vermijd "primitive obsession"

Als een `string` of `int` eigenlijk een *concept* voorstelt, geef het dan een eigen type. Zo verplaats je validatie naar één plek en kan de compiler je helpen.

```csharp
// ❌ Slecht: e-mail is overal een string, validatie overal opnieuw
public void StuurMail(string email) { ... }

// ✅ Beter
public class EmailAdres
{
    public string Waarde { get; }
    public EmailAdres(string waarde)
    {
        if (!waarde.Contains("@")) throw new ArgumentException("Ongeldig e-mailadres.");
        Waarde = waarde;
    }
}
public void StuurMail(EmailAdres email) { ... }
```

## 4. SOLID

 SOLID is een set ontwerpregels die ervoor moeten zorgen dat OO-code onderhoudbaar en uitbreidbaar blijft.

 - **S**: Single Responsibility Principle (SRP) — een klasse heeft één reden om te veranderen.
 - **O**: Open/Closed Principle (OCP) — software-entiteiten zijn open
 - **L**: Liskov Substitution Principle (LSP) — subtypes moeten substitueerbaar zijn voor hun basistype.
 - **I**: Interface Segregation Principle (ISP) — liever veel kleine interfaces dan één grote.
 - **D**: Dependency Inversion Principle (DIP) — afhankelijk van abstracties, niet van concrete implementaties.

 ### 4.1 Open/Closed Principle (OCP)

 Klassen moeten open zijn voor uitbreiding maar gesloten voor wijziging. In de praktijk: gedrag uitbreiden via nieuwe types (subklassen, nieuwe implementaties van een interface) in plaats van bestaande code aan te passen.

 ### 4.2 Liskov Substitution Principle (LSP)

 Een subtype moet z'n basistype kunnen vervangen zonder dat het programma stuk gaat. 

 ```csharp
public class Rekening
{
    protected decimal _saldo;

    // Postconditie: saldo is na afloop verhoogd met bedrag.
    public virtual void Storten(decimal bedrag)
    {
        _saldo = _saldo + bedrag;
    }

    public decimal Saldo()
    {
        return _saldo;
    }
}

public class SpaarRekening : Rekening
{
    // Verzwakt de postconditie:
    // bij bedragen onder 10 euro gebeurt er niets.
    public override void Storten(decimal bedrag)
    {
        if (bedrag < 10)
        {
            return;
        }
        _saldo = _saldo + bedrag;
    }
}
```

**Waarom dit LSP schendt**

De basis belooft: na `Storten(b)` is het saldo met `b` verhoogd. Cliëntcode mag daarop rekenen:

```csharp
void VoerTransactieUit(Rekening r)
{
    decimal voor = r.Saldo();
    r.Storten(5);
    decimal na = r.Saldo();

    Console.WriteLine($"Verschil: {na - voor}");
    // Bij Rekening:      5
    // Bij SpaarRekening: 0  ← contract gebroken
}
```

De methode werkt correct met `Rekening` en faalt stilzwijgend met `SpaarRekening`. Geen exception, geen compilerfout — gewoon verkeerd gedrag. Dat is precies wat LSP-schendingen zo verraderlijk maakt.

Hier verzwakt de subklasse de postconditie van `Storten`: in plaats van "saldo is verhoogd met bedrag" wordt het "saldo is verhoogd met bedrag, tenzij bedrag < 10". Dat is een contractbreuk. De subklasse voldoet niet aan de verwachtingen die de basis stelt.

Andere typische LSP-violations in C#:

- Subklasse versterkt precondities.
- Subklasse retourneert null waar de basis dat niet doet.

LSP is niet "erf alleen als is-a klopt in natuurlijke taal" — het is een gedragscontract.

### 4.3 Interface Segregation Principle (ISP)

 Klanten moeten niet gedwongen worden afhankelijk te zijn van interfaces die ze niet gebruiken. In de praktijk: liever veel kleine, specifieke interfaces dan één grote, algemene interface.

 ```csharp
 // Schending
public interface IWerknemer {
    void Werken();
    void Eten();
    void Slapen();
}
// Een Agent moet Eten() en Slapen() implementeren — onzin.

// Beter: gesplitst per rol
public interface IWerker  { void Werken(); }
public interface IEter    { void Eten(); }
public interface ISlaper  { void Slapen(); }
```

In C# is ISP relatief goedkoop dankzij multiple interface implementation. Wel waakzaam zijn voor interface-explosie.

### 4.4 Dependency Inversion Principle (DIP)

 Hoog-niveau modules mogen niet afhankelijk zijn van laag-niveau modules; beide moeten afhankelijk zijn van abstracties. In de praktijk: code afhankelijk maken van interfaces of abstracte klassen in plaats van concrete implementaties.

## 5. Algemen principes 

- **YAGNI**: *You Ain't Gonna Need It.* Bouw geen flexibiliteit in "voor het geval dat".
- **KISS**: *Keep It Simple, Stupid.* De simpelste oplossing die werkt, is meestal de beste.

## 5. Waarom dit allemaal? (Ja, ook in tijden van AI)

Een legitieme vraag: *"Mijn AI-assistent genereert code voor mij. Moet ik dit nog kennen?"* — Ja, en eigenlijk **net méér dan vroeger**. Een paar redenen:

1. **Je leest meer code dan je schrijft.** Onderzoek schat de verhouding al jaren op zo'n 10:1. Met AI in de mix wordt dat eerder 20:1, want jij wordt vooral *reviewer* van wat de AI produceert. Wie code-kwaliteit niet kan beoordelen, kan AI-output niet beoordelen.

2. **AI maakt vrolijk dezelfde fouten als beginners**, alleen sneller en met meer zelfvertrouwen: nodeloze duplicatie, klassen die te veel doen, magic numbers, geneste if's, exceptions die opgeslokt worden, premature abstracties. Iemand moet dat detecteren.

3. **Verantwoordelijkheid ligt bij jou.** Als jouw code op productie de mist ingaat, kan je niet zeggen "GitHub Copilot heeft het zo geschreven". Jij hebt het gecommit, jij hebt de pull request gemerged.

4. **AI-suggesties verbeteren als jij verbetert.** Een netter geformuleerde prompt, betere namen in bestaande code en een duidelijke projectstructuur → betere suggesties. Garbage in, garbage out.

5. **Onderhoud is waar de echte kost zit.** Studies schatten dat 60 tot 80% van de levenskost van software in onderhoud kruipt, niet in de initiële ontwikkeling. Interne kwaliteit *is* die onderhoudskost.

Of, kort: AI maakt het schrijven van code goedkoper, maar het *lezen* en *aanpassen* van code blijft net zo duur. Interne kwaliteit verlaagt die kost.


## 6. Tip: gebruik je AI-assistent als peer developer

Veel studenten gebruiken een AI-assistent enkel om code te **genereren**. Je haalt er minstens evenveel uit door hem ook als *code reviewer* in te zetten. Een paar bruikbare prompts:

> *"Review onderstaande C#-code op interne kwaliteit. Focus op naming, methode-grootte, SRP en duplicatie. Wijs concreet aan wat beter kan, en leg uit waarom."*

> *"Welke 'code smells' herken je in deze klasse? Stel per smell een refactoring voor."*

> *"Stel dat je deze code binnen 6 maanden moet aanpassen. Wat zou je nu al frustreren?"*

> *"Welke verantwoordelijkheden zitten er in deze klasse? Zou je hem opsplitsen, en hoe?"*

Een paar bedenkingen daarbij:

- **Wees kritisch**, ook op de review zelf. AI verzint soms verbeteringen die geen verbetering zijn, of die je code juist abstracter en complexer maken dan nodig.
- **Vraag motivatie**, niet alleen suggesties. "Waarom is dit beter?" Als je het antwoord niet begrijpt of niet overtuigend vindt, mag je de suggestie negeren.
- **Twee passes** werken vaak goed: één pass met de AI, daarna zelf nog eens kijken met de checklist uit dit hoofdstuk.
- **De checklist zelf laten meegeven** in de prompt verhoogt de kwaliteit van de review. Plak gerust een stuk van dit cursusmateriaal in de prompt mee.


## 7. Een waarschuwing

De principes hierboven zijn **richtlijnen**, geen wetten. Een veelgemaakte fout bij beginners is ze té strikt toepassen:

- Methodes opsplitsen tot tientallen one-liners waar je het overzicht door verliest.
- Voor elke klasse meteen een interface introduceren, ook al is er maar één implementatie.
- DRY zo doortrekken dat twee toevallig gelijkaardige stukken code samengevoegd worden, terwijl ze verschillende concepten voorstellen.

De ultieme toets blijft: **kan iemand anders (of jij over zes maanden) deze code begrijpen en aanpassen zonder te vloeken?** Als het antwoord ja is, doe je het goed — onafhankelijk van welke regels je daarvoor wel of niet gevolgd hebt.


## Kader — ISO/IEC 25010

Voor wie nieuwsgierig is: er bestaat een internationale standaard die softwarekwaliteit formeel definieert: **ISO/IEC 25010** (de opvolger van ISO 9126). Die standaard splitst kwaliteit op in een aantal hoofdkenmerken, zoals:

- **Functional suitability** — doet het wat het moet doen?
- **Performance efficiency** — is het snel genoeg?
- **Compatibility** — werkt het samen met andere systemen?
- **Usability** — is het bruikbaar?
- **Reliability** — werkt het betrouwbaar?
- **Security** — is het veilig?
- **Maintainability** — is het onderhoudbaar?
- **Portability** — kan je het naar andere omgevingen verplaatsen?

Bijna alles wat we in dit hoofdstuk besproken hebben, valt onder **Maintainability**, met als sub-kenmerken:

| Sub-kenmerk | Waar gaat het over? |
|---|---|
| Modularity | Is de code opgesplitst in losse, herbruikbare stukken? |
| Reusability | Kan ik delen hergebruiken in een andere context? |
| Analysability | Kan ik snel achterhalen waar een bug zit of wat impact heeft? |
| Modifiability | Kan ik een wijziging doorvoeren zonder veel kapot te maken? |
| Testability | Kan ik dit los testen? |

Met andere woorden: alle micro-praktijken in dit hoofdstuk — naming, DRY, SRP, encapsulatie, lage koppeling — dienen één of meerdere van deze vijf onderhoudbaarheidskenmerken. Het is geen verzameling losse "regeltjes", maar een samenhangend systeem dat ervoor zorgt dat software ook over jaren leefbaar blijft.