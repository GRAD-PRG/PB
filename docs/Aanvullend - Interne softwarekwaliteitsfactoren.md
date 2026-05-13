# Interne softwarekwaliteit

> *"Iedere zot kan code schrijven die een computer begrijpt. Goeie programmeurs schrijven code die mensen begrijpen."* — vrij naar Martin Fowler

## Inleiding

Er bestaan twee soorten softwarekwaliteit:

- **Externe kwaliteit**: wat de eindgebruiker merkt. Werkt het programma? Is het snel genoeg? Crasht het niet? Is de UI bruikbaar?
- **Interne kwaliteit**: wat *wij als ontwikkelaars* merken zodra we de code openen. Begrijp ik wat er staat? Kan ik dit aanpassen zonder iets anders kapot te maken? Kan ik dit testen?

Een programma kan extern perfect lijken en intern een puinhoop zijn. Dat valt op het moment zelf niet op, maar wel binnen drie weken, wanneer jij of een collega er een feature aan moet toevoegen.

Hieronder lopen we de belangrijkste interne kwaliteitsfactoren door, van micro-niveau (één regel code) tot macro-niveau (architectuur).

---

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

// ✅ Beter
if (leeftijd >= 18)
{
    Console.WriteLine("meerderjarig");
}
else
{
    Console.WriteLine("minderjarig");
}
```

Goed nieuws: dit hoef je niet manueel te doen. Visual Studio formatteert automatisch (Ctrl+K, Ctrl+D), en met een `.editorconfig` leg je teamregels vast.

### 1.3 Geen *magic numbers* of *magic strings*

Een "magic number" is een getal dat zomaar in de code staat, zonder uitleg.

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
mislukte Pogingen++;
```

### 1.6 Guard clauses in plaats van geneste if's

Een *piramide van doom* is moeilijk te volgen. Zet uitzonderingen vooraan en handel ze meteen af.

```csharp
// ❌ Slecht
public double BerekenKorting(Klant klant)
{
    if (klant != null)
    {
        if (klant.IsActief)
        {
            if (klant.Bestellingen.Count > 0)
            {
                return klant.Bestellingen.Sum(b => b.Totaal) * 0.05;
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

    return klant.Bestellingen.Sum(b => b.Totaal) * 0.05;
}
```

---

## 2. Op het niveau van een methode of functie

### 2.1 DRY — Don't Repeat Yourself

Dezelfde logica mag niet op meerdere plaatsen herhaald worden. Als je een wijziging moet doorvoeren, wil je dat op *één* plek doen.

```csharp
// ❌ Slecht
double prijsTrui = 25 + (25 * 0.21);
double prijsBroek = 50 + (50 * 0.21);
double prijsSchoen = 80 + (80 * 0.21);

// ✅ Beter
double BerekenBrutoprijs(double netto) => netto + (netto * 0.21);

double prijsTrui = BerekenBrutoprijs(25);
double prijsBroek = BerekenBrutoprijs(50);
double prijsSchoen = BerekenBrutoprijs(80);
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
    // ... 80 regels code
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
public int AantalPogingen => _aantalPogingen;
```

### 2.6 Foutafhandeling

- Geen lege `catch`-blokken — fouten "stilletjes opslokken" is een doodzonde.
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

---

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

### 3.2 Encapsulatie

De interne werking van een klasse blijft binnen die klasse. Velden zijn `private`. Toegang verloopt via properties of methodes. Op die manier kan je later de implementatie wijzigen zonder dat de rest van je code breekt.

```csharp
// ❌ Slecht
public class Rekening
{
    public double Saldo;  // iedereen kan dit aanpassen!
}

var r = new Rekening();
r.Saldo = -1000000;  // oeps

// ✅ Beter
public class Rekening
{
    public double Saldo { get; private set; }

    public void Storten(double bedrag)
    {
        if (bedrag <= 0) throw new ArgumentException("Bedrag moet positief zijn.");
        Saldo += bedrag;
    }
}
```

### 3.3 Hoge cohesie

Alle leden van een klasse horen logisch bij elkaar. Een klasse `Klant` met properties `Voornaam`, `Achternaam`, `Adres` is cohesief. Een klasse `Helper` met methodes voor PDF-generatie, datumberekeningen *en* string-manipulatie is dat niet.

### 3.4 Lage koppeling

Hoe minder een klasse over andere klassen weet, hoe makkelijker je iets kan wijzigen. Werk waar mogelijk met **interfaces** in plaats van concrete types.

```csharp
// ❌ Slecht: BestellingService is vastgenageld aan SqlKlantRepository
public class BestellingService
{
    private SqlKlantRepository _repo = new SqlKlantRepository();
}

// ✅ Beter: hangt af van een abstractie
public class BestellingService
{
    private readonly IKlantRepository _repo;
    public BestellingService(IKlantRepository repo) { _repo = repo; }
}
```

Het voordeel: voor het testen kan je nu een nep-implementatie (mock) doorgeven.

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
string straat = bestelling.Klant.Adres.Straat.ToUpper();

// ✅ Beter: laat Bestelling of Klant het werk doen
string straat = bestelling.GeefAfleverStraatInHoofdletters();
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

---

## 4. Op het niveau van module of architectuur

Op dit niveau bewegen we ons in de richting van *software design*. Een paar kernideeën:

### 4.1 De rest van SOLID

Naast Single Responsibility (zie 3.1) bestaan er nog vier principes. Je hoeft ze nu niet allemaal te beheersen, maar onthoud dat ze bestaan:

- **O**pen/Closed: open voor uitbreiding, gesloten voor wijziging.
- **L**iskov Substitution: een subklasse moet overal kunnen waar de superklasse staat.
- **I**nterface Segregation: liever meerdere kleine interfaces dan één grote.
- **D**ependency Inversion: hang af van abstracties, niet van concrete klassen.

### 4.2 Separation of Concerns

Splits je applicatie in **lagen** met elk hun eigen rol: presentatie (UI), business (regels), data (database). De UI mag niets weten van SQL, de business mag niets weten van knoppen.

### 4.3 Modulariteit

Duidelijke grenzen tussen namespaces en projecten. Geen kruisverwijzingen waarbij A van B afhangt én B van A.

### 4.4 Testbaarheid

Kan je een stuk code los testen, zonder een echte database, internetverbinding of `DateTime.Now`? Als het antwoord nee is, zit er waarschijnlijk te veel verstrengeld in dezelfde klasse.

### 4.5 Geen overengineering — YAGNI & KISS

- **YAGNI**: *You Ain't Gonna Need It.* Bouw geen flexibiliteit in "voor het geval dat".
- **KISS**: *Keep It Simple, Stupid.* De simpelste oplossing die werkt, is meestal de beste.

Design patterns en abstracties zijn waardevol, maar enkel als ze een **bestaand probleem** oplossen. Een interface met één implementatie, een factory die één klasse aanmaakt, een abstracte basisklasse "voor later"... allemaal voorbeelden van overengineering die je code juist *slechter* maakt.

---

## 5. Waarom dit allemaal? (Ja, ook in tijden van AI)

Een legitieme vraag: *"Mijn AI-assistent genereert code voor mij. Moet ik dit nog kennen?"* — Ja, en eigenlijk **net méér dan vroeger**. Een paar redenen:

1. **Je leest meer code dan je schrijft.** Onderzoek schat de verhouding al jaren op zo'n 10:1. Met AI in de mix wordt dat eerder 20:1, want jij wordt vooral *reviewer* van wat de AI produceert. Wie code-kwaliteit niet kan beoordelen, kan AI-output niet beoordelen.

2. **AI maakt vrolijk dezelfde fouten als beginners**, alleen sneller en met meer zelfvertrouwen: nodeloze duplicatie, klassen die te veel doen, magic numbers, geneste if's, exceptions die opgeslokt worden, premature abstracties. Iemand moet dat detecteren.

3. **Verantwoordelijkheid ligt bij jou.** Als jouw code op productie de mist ingaat, kan je niet zeggen "GitHub Copilot heeft het zo geschreven". Jij hebt het gecommit, jij hebt de pull request gemerged.

4. **AI-suggesties verbeteren als jij verbetert.** Een netter geformuleerde prompt, betere namen in bestaande code en een duidelijke projectstructuur → betere suggesties. Garbage in, garbage out.

5. **Onderhoud is waar de echte kost zit.** Studies schatten dat 60 tot 80% van de levenskost van software in onderhoud kruipt, niet in de initiële ontwikkeling. Interne kwaliteit *is* die onderhoudskost.

Of, kort: AI maakt het schrijven van code goedkoper, maar het *lezen* en *aanpassen* van code blijft net zo duur. Interne kwaliteit verlaagt die kost.

---

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

---

## 7. Een waarschuwing

De principes hierboven zijn **richtlijnen**, geen wetten. Een veelgemaakte fout bij beginners is ze té strikt toepassen:

- Methodes opsplitsen tot tientallen one-liners waar je het overzicht door verliest.
- Voor elke klasse meteen een interface introduceren, ook al is er maar één implementatie.
- DRY zo doortrekken dat twee toevallig gelijkaardige stukken code samengevoegd worden, terwijl ze verschillende concepten voorstellen.

De ultieme toets blijft: **kan iemand anders (of jij over zes maanden) deze code begrijpen en aanpassen zonder te vloeken?** Als het antwoord ja is, doe je het goed — onafhankelijk van welke regels je daarvoor wel of niet gevolgd hebt.

---

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

---

## Samenvatting

| Niveau | Belangrijkste principes |
|---|---|
| Regel | Naming, lay-out, geen magic numbers, geen dode code, commentaar = waarom, guard clauses |
| Methode | DRY, kort, weinig parameters, lage complexiteit, command/query gescheiden, propere foutafhandeling |
| Klasse | SRP, encapsulatie, hoge cohesie, lage koppeling, Tell-Don't-Ask, Law of Demeter, geen primitive obsession |
| Architectuur | Rest van SOLID, separation of concerns, modulariteit, testbaarheid, YAGNI/KISS |

Onthoud vooral: deze principes zijn niet bedoeld om je af te remmen of je code "mooi" te maken om de mooie. Ze bestaan omdat **de meeste tijd in software niet zit in het schrijven, maar in het lezen, begrijpen en aanpassen** van code die er al staat. Wie daar nu in investeert, betaalt zichzelf — en zijn toekomstige teamgenoten — later terug.
