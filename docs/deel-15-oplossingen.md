# Programmeren Basis - Deel 15 - Oplossingen
## Visibility / Properties
### Oplossing D15persoon
Persoon.cs

```csharp
class Persoon {

    public string Naam { get; set; }
    public DateTime Geboortedatum { get; set; }
    public string Woonplaats { get; set; }

    public int Leeftijd() {
        int leeftijd = 0;
        DateTime dt = Geboortedatum.Date.AddYears(1);  // (1)
        while (dt <= DateTime.Today) {
            leeftijd++;
            dt = dt.AddYears(1);
        }
        return leeftijd;
    }

}
```

1.  Hier wordt de property `Geboortedatum` in plaats van de method `GetGeboortedatum()` uitgelezen.

Program.cs

```csharp
class Program {

    static void Main() {
        DateTime geboorteDatum1 = new DateTime(2000, 1, 1);
        DateTime geboorteDatum2 = new DateTime(2002, 3, 4);
        DateTime geboorteDatum3 = new DateTime(2003, 4, 5);

        Persoon p1 = new Persoon();
        p1.Naam = "Jan";
        p1.Geboortedatum = geboorteDatum1;
        p1.Woonplaats = "Brugge";

        PrintPersoonsgegevens(p1);

        Persoon p2 = new Persoon();
        p2.Naam = "Piet";
        p2.Geboortedatum = geboorteDatum2;
        p2.Woonplaats = "Kortrijk";

        PrintPersoonsgegevens(p2);

        Persoon p3 = new Persoon();
        p3.SetNaam("Rita");
        p3.SetGeboortedatum(geboorteDatum3);
        p3.SetWoonplaats("Antwerpen");

        PrintPersoonsgegevens(p3);
    }

    static void PrintPersoonsgegevens(Persoon persoon) {
        Console.WriteLine(persoon.Naam);
        Console.WriteLine(persoon.Woonplaats);
        Console.WriteLine(persoon.Leeftijd());
    }

}
```

### Oplossing D15rechthoek
Rechthoek.cs

```csharp
class Rechthoek {

    public double Hoogte { get; set; }
    public double Breedte { get; set; }

    public double Oppervlakte() {
        return Hoogte * Breedte;
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Rechthoek rechthoek1 = new Rechthoek();
        rechthoek1.Hoogte = 4.1;
        rechthoek1.Breedte = 8;

        Rechthoek rechthoek2 = new Rechthoek();
        rechthoek2.Hoogte = 5;
        rechthoek2.Breedte = 10.2;

        PrintRechthoek(rechthoek1);
        PrintRechthoek(rechthoek2);
    }

    static void PrintRechthoek(Rechthoek r) {
        Console.WriteLine($"Rechthoek met hoogte {r.Hoogte}, breedte {r.Breedte} en oppervlakte {r.Oppervlakte()}.");
    }

}
```

### Oplossing D15cirkel
Cirkel.cs

```csharp
class Cirkel {

    public double Straal { get; set; }

    public double Oppervlakte() {
        return Straal * Straal * Math.PI;
    }
    public double Omtrek() {
        return Straal * 2 * Math.PI;
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Cirkel cirkel = new Cirkel();
        cirkel.Straal = 3.45;

        PrintCirkel(cirkel);
    }

    static void PrintCirkel(Cirkel c) {
        Console.WriteLine($"De straal is {c.Straal}");
        Console.WriteLine($"De omtrek is {c.Omtrek()}");
        Console.WriteLine($"De oppervlakte is {c.Oppervlakte()}");
    }

}
```

### Enkel uitleesbare properties
#### Oplossing D15bankrekening
Bankrekening.cs

```csharp
class Bankrekening {

    public decimal Saldo { get; private set; }

    public void Stort(decimal bedrag) {
        Saldo = Saldo + bedrag;
    }
    public void HaalAf(decimal bedrag) {
        Saldo = Saldo - bedrag;
    }

    public void SchrijfOver(decimal bedrag, Bankrekening doelRekening) {
        this.HaalAf(bedrag);
        doelRekening.Stort(bedrag);
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Bankrekening b1 = new Bankrekening();
        Bankrekening b2 = new Bankrekening();

        decimal bedrag = 100m;

        b1.SchrijfOver(bedrag, b2);

        Console.WriteLine(b1.Saldo == -100m); // zou true moeten geven
        Console.WriteLine(b2.Saldo == 100m);  // zou true moeten geven
    }

}
```

## Constructoren
### Default constructor
#### Oplossing D15artikel
Artikel.cs

```csharp
class Artikel {

    public Artikel() {  // (1)
        BtwPercentage = 21m;
    }

    public decimal PrijsExclusiefBtw { get; set; }
    public decimal BtwPercentage { get; set; } //= 21m;  // (2)

    public decimal PrijsInclusiefBtw() {
        return PrijsExclusiefBtw * (1 + (BtwPercentage / 100));
    }

}
```

1.  De default constructor.

2.  Ook bij de definitie van een property kan je een initiële waarde toekennen. De opgave vroeg echter met een constructor te werken.

Program.cs

```csharp
class Program {

    static void Main() {
        Artikel artikel1 = new Artikel();
        Console.WriteLine(artikel1.BtwPercentage == 21m);        // zou true moeten opleveren

        artikel1.PrijsExclusiefBtw = 1000m;
        artikel1.BtwPercentage = 6m;

        Console.WriteLine(artikel1.PrijsExclusiefBtw == 1000m);   // zou true moeten opleveren
        Console.WriteLine(artikel1.BtwPercentage == 6m);          // zou true moeten opleveren
        Console.WriteLine(artikel1.PrijsInclusiefBtw() == 1060m); // zou true moeten opleveren
    }

}
```

#### Oplossing D15encrypter ☒
Voor deze oefening is er geen voorbeeld oplossing beschikbaar.

### Verplichte initialisatie / Meerdere constructoren (die elkaar oproepen)
#### Oplossing D15artikelmetprijs
Artikel.cs

```csharp
class Artikel {

    public Artikel(decimal prijsExclusiefBtw, decimal btwPercentage) {
        this.PrijsExclusiefBtw = prijsExclusiefBtw;
        this.BtwPercentage = btwPercentage;
    }
    public Artikel(decimal prijsExclusiefBtw) : this(prijsExclusiefBtw, 21m) { }  // (1)

    public decimal PrijsExclusiefBtw { get; set; }
    public decimal BtwPercentage { get; set; }

    public decimal PrijsInclusiefBtw() {
        return PrijsExclusiefBtw * (1 + (BtwPercentage / 100));
    }

}
```

1.  Merk op hoe de constructor met één parameter, deze met twee parameters oproept. Deze constructor hoeft verder zelf niets meer te doen.

Er zijn ook alternatieve constructies te bedenken, bijvoorbeeld…​

Artikel.cs

```csharp
class Artikel {

    public Artikel(decimal prijsExclusiefBtw) {
        this.BtwPercentage = 21m;
        this.PrijsExclusiefBtw = prijsExclusiefBtw;
    }
    public Artikel(decimal prijsExclusiefBtw,
                   decimal btwPercentage) : this(prijsExclusiefBtw) {  // (1)
        this.BtwPercentage = btwPercentage;
    }

    public decimal PrijsExclusiefBtw { get; set; }
    public decimal BtwPercentage { get; set; }

    public decimal PrijsInclusiefBtw() {
        return PrijsExclusiefBtw * (1 + (BtwPercentage / 100));
    }

}
```

1.  De constructor met twee parameters roept deze keer deze met één paramter op. Het `BtwPercentage` vertrekt hier zo van *21*.

2.  Bij het aanmaken van een object via de constructor met twee parameters, wordt deze *21* dan nog eens overschreven met een opgegeven waarde.

De oorspronkelijke oplossing is daarom iets eenvoudiger.

### Immutable objecten
#### Oplossing D15immutablecirkel
Cirkel.cs

```csharp
class Cirkel {

    public Cirkel(double straal) {
        this.Straal = straal;
    }

    public double Straal { get; }

    public double Oppervlakte() {
        return Straal * Straal * Math.PI;
    }

    public double Omtrek() {
        return Straal * 2 * Math.PI;
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Cirkel cirkel1 = new Cirkel(3.45);  // (1)
        Cirkel cirkel2 = new Cirkel();      // (2)

        cirkel1.Straal = 34.5;  // (3)
    }

}
```

1.  Je moet bij het creëren van een `Cirkel` een *straal* opgeven.

2.  Deze regelt levert een compile-fout op: *"There is no argument given that corresponds to the required formal parameter 'straal' of 'Cirkel.Cirkel(double)'"*

3.  En ook bij het ondernemen van een poging de `Straal` na creatie nog aan te passen levert dat een compile-fout op: *"Property or indexer 'Cirkel.Straal' cannot be assigned to — it is read only"*

#### Oplossing D15afstandtussenpunten
Punt.cs

```csharp
class Punt {

    public Punt(double x, double y) {
        this.X = x;
        this.Y = y;
    }

    public double X { get; }
    public double Y { get; }

    public static double GetAfstandTussen(Punt p1, Punt p2) {
        double x1 = p1.X;
        double x2 = p2.X;
        double y1 = p1.Y;
        double y2 = p2.Y;

        return Math.Sqrt(Math.Pow(x1 - x2, 2) + Math.Pow(y1 - y2, 2));
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Punt p1 = new Punt(4, 6);
        Punt p2 = new Punt(7, 2);

        double afstand = Punt.GetAfstandTussen(p1, p2);

        Console.WriteLine($"De afstand is {afstand}");
    }

}
```

## Verwijzingen en samengestelde objecten
### Oplossing D15cirkelpunt
Cirkel.cs

```csharp
class Cirkel {

    public Cirkel(int x, int y, double straal) {
        this.Middelpunt = new Punt(x, y);
        this.Straal = straal;
    }
    public Cirkel(double straal) : this(0, 0, straal) { }

    public double Straal { get; }
    public Punt Middelpunt { get; private set; }

    public void VerplaatsNaar(double x, double y) {
        Middelpunt = new Punt(x, y);
    }
    public bool Bevat(Punt punt) {
        double afstand = Punt.GetAfstandTussen(Middelpunt, punt);
        return (afstand <= Straal);
    }

    public double Oppervlakte() {
        return Straal * Straal * Math.PI;
    }
    public double Omtrek() {
        return Straal * 2 * Math.PI;
    }

}
```

Het object diagram zou er zo kunnen uitzien…​

![Objectdiagram - Cirkel met een Middelpunt](images/Objectdiagram%20-%20Cirkel%20met%20een%20Middelpunt.jpg)

### Oplossing D15cirkeloverlapt
Cirkel.cs

```csharp
class Cirkel {

    public Cirkel(int x, int y, double straal) {
        this.Middelpunt = new Punt(x, y);
        this.Straal = straal;
    }
    public Cirkel(double straal) : this(0, 0, straal) { }

    public double Straal { get; }
    public Punt Middelpunt { get; private set; }

    public void VerplaatsNaar(double x, double y) {
        Middelpunt = new Punt(x, y);
    }
    public bool Bevat(Punt punt) {
        double afstand = Punt.GetAfstandTussen(Middelpunt, punt);
        return (afstand <= Straal);
    }

    public double Oppervlakte() {
        return Straal * Straal * Math.PI;
    }
    public double Omtrek() {
        return Straal * 2 * Math.PI;
    }

    public static bool Overlapt(Cirkel c1, Cirkel c2) {  // (1)
        double somStralen = c1.Straal + c2.Straal;
        double afstandMiddelpunten =
            Punt.GetAfstandTussen(c1.Middelpunt, c2.Middelpunt);
        return (afstandMiddelpunten <= somStralen);
    }

}
```

1.  Enkel deze method werd toegevoegd.

### Oplossing D15figuren
Punt.cs

```csharp
namespace D15.D15figuren.Figuren {

    class Punt {  // (1)
        ...
    }

}
```

1.  Klasse `Punt` is in de namespace `D15.D15figuren.Figuren` geplaatst.

Merk op dat je natuurlijk ook steeds aan de slag kan met verschillende -in mekaar uitgeschreven- `namespace` statements…​

Punt.cs

```csharp
namespace D15 {

    namespace D15figuren {

        namespace Figuren {

            class Punt {  // (1)
                ...
            }

        }

    }

}
```

1.  Klasse `Punt` is nog steeds in dezelfde namespace `D15.D15figuren.Figuren` gedefinieerd.

De eerste aanpak oogt allicht wat eenvoudiger.

Cirkel.cs

```csharp
namespace D15.D15figuren.Figuren {

    class Cirkel {  // (1)
        ...
    }

}
```

1.  Klasse `Cirkel` is in de namespace `Figuren` geplaatst.

De opgave van deze oefening vroeg de klasse `Program` zelf in de *root* van de voor de oefening gecreëerde namespace (`D15.D15figuren`) onder te brengen…​

Program.cs

```csharp
using D15.D15figuren.Figuren;                // (2)

namespace D15.D15figuren {

    class Program {

        static void Main() {
            Cirkel c1;                       // (1)
            Cirkel c2;                       // (1)

            Figuren.Punt p1;                 // (3)
            D15.D15figuren.Figuren.Punt p2;  // (4)

            ...
        }

    }

}
```

1.  Om compilerfouter bij het verwijzen naar het `Cirkel` datatype te vermijden (*The type or namespace name 'Cirkel' could not be found (are you missing a using directive or an assembly reference?)*) …​

2.  …​kan je een *using directive* als `using D15.D15figuren.Figuren` bovenaan je code toevoegen. Daarmee kan je veréénvoudigd (*verkort*) naar de inhoud van deze *gebruikte* namespace verwijzen.

3.  Een alternatief zou erin bestaan een meer *kwalificerende* naam te hanteren. `Figuren.Punt` bijvoorbeeld maakt duidelijk, naar de compiler toe, dat het type `Punt` in de `Figuren` namespace (van de huidige context (`D15.D15figuren`)) mag worden gezocht.

4.  Eventueel zelf *fully qualifying*, of dus de *volledige kwalificerende* naam van dat datatype. De huidige context is dan irrelevant.

De aanpak met de *using directive* is uiteraard de meest aangename. Dit zorgt immers voor eenvoudiger, of makkelijk te lezen code.

Merk wel op dat je in een `using` statement naar de volledige naam van de *gebruikte* namespace moet verwijzen. Dus iets als `D15.D15figuren.Figuren`, eerder dan eenvoudigweg `Figuren`.

Indien je ook effectief de code van de namespace `Figuren` in een folder met gelijklopende naam had gezet, ziet geeft je *Solution Explorer* ongeveer volgend overzicht…​

![Solution Explorer - Toont Figuren folder](images/Solution%20Explorer%20-%20Toont%20Figuren%20folder.png)

### Oplossing D15getafstandtussenenoverlapt
Program.cs

```csharp
class Program {

    static void Main() {
        Punt p1 = new Punt(4, 6);
        Punt p2 = new Punt(7, 2);

        //double afstand = Punt.GetAfstandTussen(p1, p2);  // (1)
        double afstand = p1.GetAfstandTussen(p2);          // (2)
        Console.WriteLine(afstand);                    // moet 5 zijn

        Cirkel c1 = new Cirkel(10, 20, 5);
        Cirkel c2 = new Cirkel(8, 12, 10);
        Cirkel c3 = new Cirkel(100, 200, 3);

        //Console.WriteLine(Cirkel.Overlapt(c1, c2));  // moet true opleveren  (3)
        Console.WriteLine(c1.Overlapt(c2));            // moet true opleveren  (4)

        //Console.WriteLine(Cirkel.Overlapt(c2, c3));  // moet false opleveren
        Console.WriteLine(c2.Overlapt(c3));            // moet false opleveren
    }

}
```

1.  In plaats van een call te maken als `Punt.GetAfstandTussen`…​

2.  roepen we de method deze keer aan op een object van type `Punt`, merk ook op dat we slechts één parameterwaarde nog overhouden.

3.  In plaats van een call te maken als `Cirkel.Overlapt`…​

4.  roepen we de method deze keer aan op een object van type `Cirkel`, merk ook op dat we slechts één parameterwaarde nog overhouden.

Punt.cs

```csharp
class Punt {

    public Punt(double x, double y) {
        X = x;
        Y = y;
    }

    public double X { get; }
    public double Y { get; }

    //public static double GetAfstandTussen(Punt p1, Punt p2) {
    //    double x1 = p1.X;  // (2)
    //    double x2 = p2.X;  // (3)
    //    double y1 = p1.Y;
    //    double y2 = p2.Y;
    //
    //    return Math.Sqrt(Math.Pow(x1 - x2, 2) + Math.Pow(y1 - y2, 2));
    //}
    public double GetAfstandTussen(Punt p) {  // (1)
        double x1 = this.X;  // (2)
        double x2 = p.X;     // (3)
        double y1 = this.Y;
        double y2 = p.Y;

        return Math.Sqrt(Math.Pow(x1 - x2, 2) + Math.Pow(y1 - y2, 2));
    }

}
```

1.  Het `static` sleutelwoord is uit de hoofding weggehaald (zo maak je er een *instance method* van). En één parameter van type `Punt` is verwijderd.

2.  Merk op dat `p1` nu vervangen is door `this` (het object in uitvoering).

3.  En dat parameter `p2` nu hernoemt is naar `p` (er is immers slechts één parameter, we moeten ze dan ook niet meer benummeren).

Cirkel.cs

```csharp
class Cirkel {

    public Cirkel(int x, int y, double straal) {
        Middelpunt = new Punt(x, y);
        Straal = straal;
    }
    public Cirkel(double straal) : this(0, 0, straal) { }

    public double Straal { get; }
    public Punt Middelpunt { get; private set; }

    public void VerplaatsNaar(double x, double y) {
        Middelpunt = new Punt(x, y);
    }
    public bool Bevat(Punt punt) {
        //double afstand = Punt.GetAfstandTussen(Middelpunt, punt);  // (1)
        double afstand = Middelpunt.GetAfstandTussen(punt);          // (2)
        return afstand <= Straal;
    }

    public double Oppervlakte() {
        return Straal * Straal * Math.PI;
    }
    public double Omtrek() {
        return Straal * 2 * Math.PI;
    }

    //public static bool Overlapt(Cirkel c1, Cirkel c2) {
    //    double somStralen = c1.Straal + c2.Straal;  // (4)
    //    double afstandMiddelpunten =
    //        Punt.GetAfstandTussen(c1.Middelpunt, c2.Middelpunt);
    //    return afstandMiddelpunten <= somStralen;
    //}
    public bool Overlapt(Cirkel c) {  // (3)
        double somStralen = this.Straal + c.Straal;  // (4)
        double afstandMiddelpunten =
            this.Middelpunt.GetAfstandTussen(c.Middelpunt);  // (2)
        return afstandMiddelpunten <= somStralen;
    }

}
```

1.  Gezien de aanpassing in klasse `Punt` moeten nu in plaats van de *class method* `Punt.GetAfstandTussen`…​

2.  Op een object van type `Punt`, bijvoorbeeld het `Punt` object dat de `Middelpunt` property oplevert, de `GetAfstandTussen` method aanroepen.

3.  Opnieuw is het `static` sleutelwoord weggehaald. Deze keer om van de `Overlapt` method een *instance method* te maken. En één parameter van type `Cirkel` is verwijderd.

4.  In de implementatie van deze method kunnen we in plaats van naar een parameter als voorgaande `c1` te verwijzen, werken met het object in uitvoering (`this`).

Normaliter kies je tussen een *class method* of een *instance method*. Je gaat nooit beide voorzien. Welke keuze *'beter'* is, is een ontwerpbeslissing.

### Oplossing D15rekeningkantoor
Persoon.cs

```csharp
class Persoon {

    public string Voornaam { get; set; }
    public string Familienaam { get; set; }
    public Adres Adres { get; set; }

    public Persoon(string voornaam, string familienaam, Adres adres) {
        Voornaam = voornaam;
        Familienaam = familienaam;
        Adres = adres;
    }

}
```

Adres.cs

```csharp
class Adres {

    public string Straat { get; set; }
    public string Huisnummer { get; set; }
    public string Postcode { get; set; }
    public string Gemeente { get; set; }

    public Adres(string straat, string huisnummer, string postcode, string gemeente) {
        Straat = straat;
        Huisnummer = huisnummer;
        Postcode = postcode;
        Gemeente = gemeente;
    }

}
```

Rekening.cs

```csharp
class Rekening {

    public string Nummer { get; set; }
    public double Saldo { get; set; }
    public Kantoor Kantoor { get; set; }
    public Persoon Titularis { get; set; }

    public Rekening(string nummer, double saldo, Kantoor kantoor, Persoon titularis) {
        Nummer = nummer;
        Saldo = saldo;
        Kantoor = kantoor;
        Titularis = titularis;
    }

}
```

Kantoor.cs

```csharp
class Kantoor {

    public Persoon Kantoorhouder { get; set; }
    public Adres Adres { get; set; }

    public Kantoor(Persoon kantoorhouder, Adres adres) {
        Kantoorhouder = kantoorhouder;
        Adres = adres;
    }

}
```

Program.cs

```csharp
class Program {

    static void Main() {
        Adres adresJan = new Adres("Koekoekstraat", "70", "9090", "Melle");
        Persoon jan = new Persoon("Jan", "Janssens", adresJan);

        Adres adresMieke = new Adres("Kerkstraat", "12", "8000", "Brugge");
        Persoon mieke = new Persoon("Mieke", "Mickelsen", adresMieke);
        Kantoor kantoorMieke = new Kantoor(mieke, adresMieke);

        Rekening rekeningJan = new Rekening("BE11 2222 3333 4444", 120, kantoorMieke, jan);
    }

}
```

Object diagram…​

![Object diagram - Jan en Mieke](images/Object%20diagram%20-%20Jan%20en%20Mieke.jpg)

### Oplossing D15rekeningkantoormieke
Vermits het `Persoon` object voor *Mieke* en het `Kantoor` object een verwijzing naar hetzelfde `Adres` object hebben, zal elke wijziging beiden doen verhuizen! Het afgedrukt *huisnummer* is dus ook *99*.

![Object diagram - Mieke verhuist](images/Object%20diagram%20-%20Mieke%20verhuist.png)

Indien dat niet de bedoeling is kan je dit vermijden door elk een eigen `Adres` object te geven (met initieel dezelfde data in)…​

```csharp
Adres adresMieke = new Adres("Kerkstraat", "12", "8000", "Brugge");
Persoon mieke = new Persoon("Mieke", "Mickelsen", adresMieke);

Adres adresKantoor = new Adres("Kerkstraat", "12", "8000", "Brugge");
Kantoor kantoorMieke = new Kantoor(mieke, adresKantoor);
```

### Oplossing D15stellingverhuur ☒
Voor deze oefening is er geen voorbeeld oplossing beschikbaar.

## Expressies en static typing
### Oplossing D15expressies1
**`5` is een expressie** omdat deze de waarde voor de eerste parameter van `GetFactuur` aanduidt.

→ `int` expressie die gehele numerieke waarde 5 voorstelt.

→ Correct want de call naar `GetFactuur` verwacht als eerste waarde een `id` die een `int` moet zijn.

**`d` is een expressie** omdat deze de waarde voor de tweede parameter van GetFactuur aanduidt.

→ `DateTime` expressie die datum *12 maart 2017* voorstelt.

→ Correct want `GetFactuur` verwacht als tweede waarde een `creationDate` die een `DateTime` moet zijn.

**`GetFactuur(5, d)` is een expressie** omdat deze hier gebruikt wordt om aan te duiden van wat (van welk `GetFactuur` object) je een aspect als de `CreatieDatum` gaat opvragen.

→ `GetFactuur` expressie die een nieuwe *factuur* voorstelt met `id` *5* en *creatiedatum 12 maar 2017*, deze expressie zal evalueren naar de referentie van het gecreëerde `GetFactuur` object.

→ Correct want `GetFactuur` objecten beschikken over een publieke member `CreatieDatum` die je op deze wijze kan gebruiken.

**`GetFactuur(5, d).CreatieDatum` is een expressie** omdat deze hier gebruikt wordt om aan te duiden van wat (van welke `DateTime` object) je een aspect als de `Day` gaat opvragen.

→ `DateTime` expressie die *datum 12 maart 2017* voorstelt.

→ Correct want `DateTime` objecten beschikken over een publieke member `Day` die je op deze wijze kan gebruiken.

**`GetFactuur(5, d).CreatieDatum.Day` is een expressie** omdat deze hier gebruikt wordt om aan te duiden welke waarde op de `Console` wordt geschreven (parameterwaarde voor `WriteLine` method).

→ `int` expressie die *getal (dag) 12* voorstelt.

→ Correct want aan de `WriteLine` method kan je een `int` waarde doorgeven.

### Oplossing D15expressies2
**`(new Persoon())` is een expressie** (met en zonder de omsluitende haakjes) omdat deze hier gebruikt wordt om aan te duiden van wat (van welke `Persoon` object) je een aspect als `Vip` gaat opvragen.

→ `Persoon` expressie die een nieuwe persoon voorstelt met naam *John*, deze expressie zal evalueren naar de referentie van het gecreëerde \`Persoon\`s object.

→ Correct want `` Persoon`s objecten beschikken over een publieke member `Vip `` die je op deze wijze kan gebruiken.

**`(new Persoon()).Vip` is een expressie** omdat deze hier gebruikt wordt om aan te duiden welke waarde wordt toegekend aan de variabele `v`.

→ `bool` expressie die correct of niet-correct voorstelt.

→ Correct want aan de variabele `v` moet je een `bool` waarde toekennen.

### Oplossing D15expressies3
**`1`, `2` en `3` zijn expressies** omdat deze hier gebruikt worden om aan te duiden welke waardes de array heeft.

→ `int` expressies die de gehele numerieke waardes *1*, *2* en *3* voorstellen

**`new int[]{ 1, 2, 3 }` is een expressie** omdat deze hier gebruikt wordt om aan te duiden van wat (van welke `int[]` object) je de lengte wil opvragen.

→ `int[]` expressie die een nieuwe `int` array voorstelt, deze expressie zal evalueren naar de referentie van het gecreëerde `int[]` object.

→ Correct want `int[]` objecten beschikken over een publieke member `Length` die je op deze wijze kan gebruiken.

**`new int[]{ 1, 2, 3 }.Length` is een expressie** omdat deze hier gebruikt worden om aan te duiden welke waarde wordt gebruikt in de vermenigvuldiging

→ `int` expressie omdat de *lengte* door de `Length` property in deze vorm wordt opgeleverd, dit zal hier de gehele numerieke *waarde 3* voorstellen\`

→ Correct omdat er ondersteuning is om een int waarde met een andere `int` te vermenigvuldigen.

**`5` is een expressie** omdat deze hier gebruikt worden om aan te duiden van welke waarde hier wordt gebruikt in de vermenigvuldiging.

→ `int` expressie die de gehele numerieke *waarde 5* voorstelt.

→ Correct omdat er ondersteuning is om een `int` waarde met een andere `int` te vermenigvuldigen.

**`new int[]{ 1, 2, 3 }.Length * 5` is een expressie** omdat deze hier gebruikt worden om aan te duiden welke lengte wordt gebruikt voor de nieuw te creëren array `a`.

→ `int` expressie omdat in de definitie van operator `*` is aangegeven dat resultaat van de vermenigvuldiging van twee `` int`s opnieuw een `int `` zal zijn.

→ Correct want een lengte voor een nieuw te creëren array moet in `int` vorm worden opgegeven.

### Oplossing D15expressies4
**`g > 5` zou een expressie kunnen zijn** omdat deze hier gebruikt wordt om aan te duiden welke waarde je wil combineren met de `&&` operator.

→ `bool` expressie die hier *niet-correct (`false`)* zal voorstellen

→ Correct want de `&&` operator verwacht `bool` operanden.

**`5` zou een expressie kunnen zijn** omdat deze hier gebruikt wordt om aan te duiden welke waarde je wil vergelijken met de `>` operator.

→ `int` expressie die de gehele numerieke *waarde 5* voorstelt.

→ Correct omdat een `int` kan vergeleken worden met een andere `int` via de `>` operator.

**`g` zou een expressie kunnen zijn** omdat deze hier gebruikt wordt om aan te duiden welke waarde je wil vergelijken met de `>` operator.

→ `int` expressie omdat de variabele met deze naam van type `int` is gedeclareerd, de expressie zal hier *waarde 4* voorstellen.

→ Correct omdat een `int` kan vergeleken worden met een andere `int` via de `>` operator.

**`6` zou een expressie kunnen zijn** omdat deze hier gebruikt wordt om aan te duiden welke waarde je wil combineren met de `&&` operator.

→ `int` expressie die de gehele numerieke *waarde 6* voorstelt.

→ Niet correct want de `&&` operator verwacht twee `bool` operanden.

Dat laatste is hier dus relevant. Aan de rechterkant an de `&&` operator wordt dus ook iets van type `bool` verwacht. Dat is hier niet het geval. Deze regel zou bijgevolg een compilerfout veroorzaken.

Grammaticaal gezien is deze constructie dus niet correct. Een compilerfout *Operator '&&' cannot be applied to operands of type 'bool' and 'int'.* treedt op.

Hopelijk hebben deze oefeningen je wat gesterkt in een beter begrip van dergelijke compilerfouten (die te maken hebben met de inzet van iets wat een expressie zou kunnen zijn, maar die dan één of ander foutief datatype zou dragen). Je zal ze nu hopelijk minder spontaan maken, en indien ze toch optreden ga je ze nu hopelijk ook vlotter begrijpen, en kunnen oplossen.
