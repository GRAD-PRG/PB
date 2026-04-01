# Dependency Inversion & Dependency Injection

## Het probleem: tight coupling

Je kent ondertussen klassen, interfaces en subtype polymorfisme. Je weet hoe je met interfaces een contract definieert en hoe je verschillende implementaties achter eenzelfde type kunt aanspreken. Maar in de praktijk schrijven veel beginners (en ook ervaren programmeurs die niet opletten) code waarin klassen **rechtstreeks afhankelijk** zijn van andere concrete klassen. Dat noemen we **tight coupling** — en het levert problemen op.

Bekijk dit voorbeeld. Een `BestellingService` die bestellingen verwerkt en een bevestigingsmail stuurt:

```csharp
class EmailVerzender
{
    public void VerstuurEmail(string naar, string onderwerp, string bericht)
    {
        Console.WriteLine($"E-mail naar {naar}: {onderwerp}");
        // ... SMTP-logica ...
    }
}

class BestellingService
{
    private EmailVerzender emailVerzender = new EmailVerzender(); // ? tight coupling

    public void PlaatsBestelling(string klantEmail, string product)
    {
        Console.WriteLine($"Bestelling geplaatst: {product}");
        emailVerzender.VerstuurEmail(klantEmail, "Bevestiging", $"Je bestelde {product}.");
    }
}
```

Dit werkt. Maar er zijn problemen:

1. **`BestellingService` is vastgeklonken aan `EmailVerzender`.** Als je morgen ook via SMS wilt bevestigen, moet je `BestellingService` aanpassen. Elke nieuwe manier van notificeren vereist wijzigingen in een klasse die daar eigenlijk niets mee te maken heeft.

2. **Je kunt `BestellingService` niet los testen.** Als je in een test `PlaatsBestelling()` aanroept, wordt er echt een e-mail verstuurd (of je krijgt een fout omdat er geen mailserver draait). Je kunt de `EmailVerzender` niet vervangen door een nep-versie.

3. **De hogere-niveau klasse (`BestellingService`) is afhankelijk van een lager-niveau detail (`EmailVerzender`).** Als de implementatie van het mailversturen verandert — andere constructor, andere parameters, andere bibliotheek — moet je ook `BestellingService` aanpassen.

---

## Het Dependency Inversion Principle (DIP)

Het **Dependency Inversion Principle** is één van de vijf SOLID-principes (de "D" in SOLID) en zegt in essentie:

> **Klassen op een hoger niveau mogen niet afhankelijk zijn van klassen op een lager niveau. Beide moeten afhankelijk zijn van abstracties (interfaces).**

"Hoger niveau" wil hier zeggen: de klasse die de businesslogica bevat, de klasse die het grotere plaatje regelt. "Lager niveau" is de klasse die een specifiek technisch detail uitvoert (mails versturen, data opslaan, logging doen, ...).

Het idee is dat je de afhankelijkheid **omdraait** (*inverteert*). In plaats van dat `BestellingService` rechtstreeks afhankelijk is van `EmailVerzender`, maak je beide afhankelijk van een **interface**.

### Stap 1: definieer een interface

```csharp
interface INotificatieVerzender
{
    void VerstuurNotificatie(string naar, string onderwerp, string bericht);
}
```

### Stap 2: laat de concrete klasse de interface implementeren

```csharp
class EmailVerzender : INotificatieVerzender
{
    public void VerstuurNotificatie(string naar, string onderwerp, string bericht)
    {
        Console.WriteLine($"E-mail naar {naar}: {onderwerp}");
        // ... SMTP-logica ...
    }
}
```

### Stap 3: laat de hogere-niveau klasse afhangen van de interface

```csharp
class BestellingService
{
    private INotificatieVerzender notificatieVerzender;

    public BestellingService(INotificatieVerzender notificatieVerzender)
    {
        this.notificatieVerzender = notificatieVerzender;
    }

    public void PlaatsBestelling(string klantEmail, string product)
    {
        Console.WriteLine($"Bestelling geplaatst: {product}");
        notificatieVerzender.VerstuurNotificatie(klantEmail, "Bevestiging", $"Je bestelde {product}.");
    }
}
```

Merk op: `BestellingService` weet nu **niets** meer over `EmailVerzender`. Hij kent enkel de interface `INotificatieVerzender`. De concrete implementatie wordt van buitenaf meegegeven via de constructor.

### Wat levert dit op?

Je kunt nu zonder `BestellingService` aan te passen een compleet andere notificatiemethode gebruiken:

```csharp
class SmsVerzender : INotificatieVerzender
{
    public void VerstuurNotificatie(string naar, string onderwerp, string bericht)
    {
        Console.WriteLine($"SMS naar {naar}: {bericht}");
    }
}

class PushNotificatieVerzender : INotificatieVerzender
{
    public void VerstuurNotificatie(string naar, string onderwerp, string bericht)
    {
        Console.WriteLine($"Push naar {naar}: {onderwerp}");
    }
}
```

En bij het aanmaken kies je welke implementatie je meegeeft:

```csharp
// Met e-mail:
BestellingService service1 = new BestellingService(new EmailVerzender());
service1.PlaatsBestelling("klant@mail.be", "Laptop");

// Met SMS:
BestellingService service2 = new BestellingService(new SmsVerzender());
service2.PlaatsBestelling("0471234567", "Laptop");

// Met push notificaties:
BestellingService service3 = new BestellingService(new PushNotificatieVerzender());
service3.PlaatsBestelling("user123", "Laptop");
```

`BestellingService` is nu **open voor uitbreiding** (je kunt nieuwe notificatietypes toevoegen) maar **gesloten voor wijziging** (je hoeft `BestellingService` zelf niet aan te passen). Dat is trouwens nog een ander SOLID-principe: het *Open/Closed Principle*.

---

## Dependency Injection (DI)

In het voorbeeld hierboven heb je eigenlijk al **dependency injection** toegepast, misschien zonder het te beseffen. Dependency injection is geen ingewikkeld concept — het is simpelweg de techniek waarbij je een afhankelijkheid (*dependency*) **van buitenaf meegeeft** (*injecteert*) in plaats van ze binnenin de klasse aan te maken.

Vergelijk:

| Zonder DI | Met DI |
|---|---|
| De klasse maakt zelf zijn afhankelijkheden aan (`new EmailVerzender()`) | De afhankelijkheden worden van buitenaf meegegeven |
| De klasse bepaalt zelf *welke* implementatie hij gebruikt | De aanroeper bepaalt welke implementatie wordt gebruikt |
| Tight coupling | Loose coupling |

### Vormen van dependency injection

Er zijn drie veelgebruikte manieren om een dependency te injecteren:

#### 1. Constructor injection (aanbevolen)

De dependency wordt meegegeven via de **constructor**. Dit is veruit de meest gebruikte en aanbevolen vorm, omdat de klasse zo vanaf het begin in een geldige staat verkeert — hij kan niet bestaan zonder zijn afhankelijkheid.

```csharp
class BestellingService
{
    private INotificatieVerzender notificatieVerzender;

    public BestellingService(INotificatieVerzender notificatieVerzender)
    {
        this.notificatieVerzender = notificatieVerzender;
    }

    public void PlaatsBestelling(string klantEmail, string product)
    {
        Console.WriteLine($"Bestelling geplaatst: {product}");
        notificatieVerzender.VerstuurNotificatie(klantEmail, "Bevestiging", $"Je bestelde {product}.");
    }
}
```

#### 2. Property injection

De dependency wordt ingesteld via een **property**. Dit gebruik je wanneer de dependency optioneel is, of wanneer je ze na het aanmaken nog wilt kunnen wijzigen.

```csharp
class BestellingService
{
    public INotificatieVerzender NotificatieVerzender { get; set; }

    public void PlaatsBestelling(string klantEmail, string product)
    {
        Console.WriteLine($"Bestelling geplaatst: {product}");
        NotificatieVerzender?.VerstuurNotificatie(klantEmail, "Bevestiging", $"Je bestelde {product}.");
        // Let op de ?. operator: als er geen verzender is ingesteld, wordt er geen notificatie verstuurd.
    }
}

// Gebruik:
BestellingService service = new BestellingService();
service.NotificatieVerzender = new EmailVerzender();
service.PlaatsBestelling("klant@mail.be", "Laptop");
```

Het nadeel: je kunt `PlaatsBestelling()` aanroepen zonder dat er een `NotificatieVerzender` is ingesteld. De klasse is dus niet gegarandeerd in een geldige staat.

#### 3. Method injection

De dependency wordt meegegeven als **parameter van de methode**. Dit doe je wanneer de dependency per aanroep kan verschillen.

```csharp
class BestellingService
{
    public void PlaatsBestelling(string klantEmail, string product, INotificatieVerzender verzender)
    {
        Console.WriteLine($"Bestelling geplaatst: {product}");
        verzender.VerstuurNotificatie(klantEmail, "Bevestiging", $"Je bestelde {product}.");
    }
}

// Gebruik:
BestellingService service = new BestellingService();
service.PlaatsBestelling("klant@mail.be", "Laptop", new EmailVerzender());
service.PlaatsBestelling("0471234567", "Muis", new SmsVerzender());
```

---

## Een groter voorbeeld: een logboek

Laten we het principe toepassen op een iets uitgebreider voorbeeld. Stel: je bouwt een systeem dat logberichten schrijft. Waar die logberichten naartoe gaan — een bestand, de console, een database — dat wil je flexibel houden.

### De interface

```csharp
interface ILogger
{
    void Log(string bericht);
}
```

### Verschillende implementaties

```csharp
class ConsoleLogger : ILogger
{
    public void Log(string bericht)
    {
        Console.WriteLine($"[LOG] {bericht}");
    }
}

class BestandLogger : ILogger
{
    private string bestandspad;

    public BestandLogger(string bestandspad)
    {
        this.bestandspad = bestandspad;
    }

    public void Log(string bericht)
    {
        File.AppendAllText(bestandspad, $"[{DateTime.Now}] {bericht}\n");
    }
}
```

### Een klasse die de logger gebruikt

```csharp
class GebruikerService
{
    private ILogger logger;

    public GebruikerService(ILogger logger)
    {
        this.logger = logger;
    }

    public void Registreer(string naam)
    {
        // ... registratielogica ...
        logger.Log($"Gebruiker '{naam}' geregistreerd.");
    }

    public void Verwijder(string naam)
    {
        // ... verwijderlogica ...
        logger.Log($"Gebruiker '{naam}' verwijderd.");
    }
}
```

### Gebruik

```csharp
// Tijdens ontwikkeling: loggen naar de console
GebruikerService devService = new GebruikerService(new ConsoleLogger());
devService.Registreer("Amir");

// In productie: loggen naar een bestand
GebruikerService prodService = new GebruikerService(new BestandLogger("app.log"));
prodService.Registreer("Amir");
```

`GebruikerService` weet niet *hoe* er gelogd wordt. Hij weet enkel *dat* er gelogd kan worden, via de `ILogger`-interface. De beslissing over het *hoe* wordt genomen op het moment dat je het object aanmaakt — niet binnenin de klasse zelf.

---

## Testen met dependency injection

Een van de grootste voordelen van dependency injection is **testbaarheid**. Als je afhankelijkheden via interfaces injecteert, kun je in een test een **nep-implementatie** (een *mock* of *stub*) meegeven die niets echt doet, maar wel laat controleren of de juiste methoden werden aangeroepen.

```csharp
class NepLogger : ILogger
{
    public List<string> Berichten { get; } = new List<string>();

    public void Log(string bericht)
    {
        Berichten.Add(bericht); // Slaat het bericht op in plaats van het te printen of weg te schrijven
    }
}
```

In een test kun je dan schrijven:

```csharp
// Arrange
NepLogger logger = new NepLogger();
GebruikerService service = new GebruikerService(logger);

// Act
service.Registreer("Fatima");

// Assert
Console.WriteLine(logger.Berichten.Count == 1);                               // True
Console.WriteLine(logger.Berichten[0] == "Gebruiker 'Fatima' geregistreerd."); // True
```

Zonder dependency injection zou `GebruikerService` intern een `ConsoleLogger` of `BestandLogger` aanmaken, en zou je geen controle hebben over wat er met de logberichten gebeurt. Met DI geef je een nep-logger mee en kun je exact verifiëren wat er gelogd werd — zonder dat er iets naar de console of een bestand wordt geschreven.

---

## Visueel: de afhankelijkheden omgekeerd

Zonder dependency inversion zien de afhankelijkheden er zo uit:

```
BestellingService ??depends on??? EmailVerzender
```

`BestellingService` (hoger niveau) hangt af van `EmailVerzender` (lager niveau). Als `EmailVerzender` verandert, moet `BestellingService` mee veranderen.

Met dependency inversion wordt dat:

```
BestellingService ??depends on??? INotificatieVerzender (interface)
                                         ?
EmailVerzender ????implements?????????????
SmsVerzender ??????implements?????????????
```

Beide — zowel `BestellingService` als `EmailVerzender` — hangen nu af van de **interface**. De interface is het stabiele punt. De concrete implementaties kunnen veranderen of worden vervangen zonder dat `BestellingService` daar iets van merkt. De afhankelijkheid is *omgekeerd* (geïnverteerd): niet langer van hoog naar laag, maar van hoog naar abstractie, en van laag naar diezelfde abstractie.

---

## Samengevat

| Concept | Wat is het? |
|---|---|
| **Dependency** | Een object dat een klasse nodig heeft om zijn werk te doen |
| **Tight coupling** | Een klasse die rechtstreeks afhankelijk is van een concrete andere klasse |
| **Dependency Inversion Principle** | Hogere-niveau klassen mogen niet afhangen van lagere-niveau klassen; beide hangen af van abstracties |
| **Dependency Injection** | De techniek om afhankelijkheden van buitenaf mee te geven (via constructor, property of methode) |
| **Constructor injection** | De meest gebruikte vorm: dependencies worden via de constructor meegegeven |
| **Loose coupling** | Klassen kennen enkel elkaars interface, niet de concrete implementatie |

> **Vuistregel:** als je in een klasse `new ConcreetIets()` schrijft voor een afhankelijkheid, vraag je dan af: *"Zou ik dit ooit willen vervangen door iets anders?"* Als het antwoord ja is — en dat is het vaker dan je denkt — definieer dan een interface en injecteer de dependency.
