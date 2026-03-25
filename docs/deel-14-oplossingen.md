# Programmeren Basis - Deel 14 - Oplossingen
## Oplossing D14persoon
Persoon.cs

```csharp
namespace D14.D14persoon {

    class Persoon {

        private string _naam;
        public string GetNaam() {
            return _naam;
        }
        public void SetNaam(string naam) {
            _naam = naam;
        }

        private DateTime _geboortedatum;
        public DateTime GetGeboortedatum() {
            return _geboortedatum;
        }
        public void SetGeboortedatum(DateTime geboortedatum) {
            _geboortedatum = geboortedatum;
        }

        public int Leeftijd() {
            int leeftijd = 0;
            DateTime dt = GetGeboortedatum().Date.AddYears(1);
            while (dt <= DateTime.Today) {
                leeftijd++;
                dt = dt.AddYears(1);
            }
            return leeftijd;
        }

        private string _woonplaats;
        public string GetWoonplaats() {
            return _woonplaats;
        }
        public void SetWoonplaats(string woonplaats) {
            _woonplaats = woonplaats;
        }

    }

}
```

Program.cs

```csharp
using System;

namespace D14.D14persoon {

    class Program {

        static void Main() {
            DateTime geboorteDatum1 = new DateTime(2000, 1, 1);
            DateTime geboorteDatum2 = new DateTime(2002, 3, 4);
            DateTime geboorteDatum3 = new DateTime(2003, 4, 5);

            Persoon p1 = new Persoon();
            p1.SetNaam("Jan");
            p1.SetGeboortedatum(geboorteDatum1);
            p1.SetWoonplaats("Brugge");

            PrintPersoonsgegevens(p1);

            Persoon p2 = new Persoon();
            p2.SetNaam("Piet");
            p2.SetGeboortedatum(geboorteDatum2);
            p2.SetWoonplaats("Kortrijk");

            PrintPersoonsgegevens(p2);

            Persoon p3 = new Persoon();
            p3.SetNaam("Rita");
            p3.SetGeboortedatum(geboorteDatum3);
            p3.SetWoonplaats("Antwerpen");

            PrintPersoonsgegevens(p3);
        }

        static void PrintPersoonsgegevens(Persoon persoon) {
            Console.WriteLine(persoon.GetNaam());
            Console.WriteLine(persoon.GetWoonplaats());
            Console.WriteLine(persoon.Leeftijd());
        }
    }

}
```

## Oplossing D14rechthoek
Rechthoek.cs

```csharp
namespace D14.D14rechthoek {

    class Rechthoek {

        private double _hoogte;
        public void SetHoogte(double value) {
            _hoogte = value;
        }
        public double GetHoogte() {
            return _hoogte;
        }

        private double _breedte;
        public void SetBreedte(double value) {
            _breedte = value;
        }
        public double GetBreedte() {
            return _breedte;
        }

        public double Oppervlakte() {
            return GetHoogte() * GetBreedte();
        }

    }

}
```

## Oplossing D14cirkel
Cirkel.cs

```csharp
namespace D14.D14cirkel {

    class Cirkel {

        private double _straal;
        public void SetStraat(double straal) {
            _straal = straal;
        }
        public double GetStraal() {
            return _straal;
        }

        public double Oppervlakte() {
            return GetStraal() * GetStraal() * Math.PI;
        }

        public double Omtrek() {
            return GetStraal() * 2 * Math.PI;
        }
    }

}
```

Program.cs

```csharp
using System;

namespace D14.D14cirkel {

    class Program {

        static void Main() {
            Cirkel cirkel = new Cirkel();
            cirkel.SetStraal(3.45);

            PrintCirkel(cirkel);
        }

        static void PrintCirkel(Cirkel c) {
            Console.WriteLine($"De straal is {c.GetStraal()}");
            Console.WriteLine($"De omtrek is {c.Omtrek()}");
            Console.WriteLine($"De oppervlakte is {c.Oppervlakte()}");
        }

    }

}
```

Een alternatieve oplossing voor klasse `Cirkel`…​

Cirkel.cs

```csharp
namespace D14.D14cirkel.Alternatief {

    class Cirkel {

        private double _straal;
        public void SetStraat(double straal) {
            _straal = straal;
            _oppervlakte = GetStraal() * GetStraal() * Math.PI;
            _omtrek = GetStraal() * 2 * Math.PI;
        }
        public double GetStraal() {
            return _straal;
        }

        private double _oppervlakte;
        public double Oppervlakte() {
            return _oppervlakte;
        }

        private double _omtrek;
        public double Omtrek() {
            return _omtrek;
        }

    }

}
```

Deze alternatieve oplossing heeft als voordeel dat `Oppervlakte` en `Omtrek` zijn heel *rap* zijn. Er wordt immers niks meer berekend. Interessant als deze heel vaak worden opgeroepen en het programma te traag is hierdoor (niet erg waarschijnlijk).

Er zijn ook nadelen. Elk `Cirkel` object is drie keer zo groot (drie `double` waardes in plaats van één). De *oppervlakte* en *omtrek* worden voor elk object sowieso berekend, ook al hebben we ze soms niet eens nodig. Deze alternatieve oplossing geeft de voorkeur aan uitvoeringstijd boven opslagplaats.

Normaliter schrijf je echter je klassen zodanig dat je de voorkeur geeft aan opslagplaats boven uitvoeringstijd. Alles wat kan afgeleid worden uit andere informatie, wordt niet in een dataveld bijgehouden. De eerste klasse `Cirkel` is dus doorgaans de betere keuze.

## Oplossing D14artikel
Artikel.cs

```csharp
namespace D14.D14artikel {

    class Artikel {

        private decimal _prijsExclusiefBtw;
        public void SetPrijsExclusiefBtw(decimal value) {
            _prijsExclusiefBtw = value;
        }
        public decimal GetPrijsExclusiefBtw() {
            return _prijsExclusiefBtw;
        }

        private decimal _btwPercentage = 21m;  // (1) (2)
        public void SetBtwPercentage(decimal value) {
            _btwPercentage = value;
        }
        public decimal GetBtwPercentage() {
            return _btwPercentage;
        }

        public decimal PrijsInclusiefBtw() {
            return GetPrijsExclusiefBtw() * (1 + (GetBtwPercentage() / 100));
        }

    }

}
```

1.  Merk op hoe je datavelden kan initialiseren.

2.  Een `m` maakt meteen duidelijk (aan de compiler en lezer van de code) dat het hier gaat om een `deciMal` waarde, en bijvoorbeeld niet om een `int` of `double` literal.

Program.cs

```csharp
using System;

namespace D14.D14artikel {

    class Program {

        static void Main() {
            Artikel artikel1 = new Artikel();
            Console.WriteLine(artikel1.GetBtwPercentage() == 21m);       // true

            artikel1.SetPrijsExclusiefBtw(1000m);
            artikel1.SetBtwPercentage(6m);

            Console.WriteLine(artikel1.GetPrijsExclusiefBtw() == 1000m); // true
            Console.WriteLine(artikel1.GetBtwPercentage() == 6m);        // true
            Console.WriteLine(artikel1.PrijsInclusiefBtw() == 1060m);    // true
        }

    }

}
```

De testcode is hier zo opgesteld dat je op basis van de uitvoer (waar we een aantal keer *"true"* op de console moeten krijgen) heel snel ziet of de `Artikel` klasse, en zijn members, correct zijn opgesteld.

Je vergelijkt hiervoor (met de `==` operator bijvoorbeeld) de eigenlijke waarde (bijvoorbeeld `artikel1.PrijsInclusiefBtw()`) met de verwachte waarde (bijvoorbeeld `1060m`). Is het resultaat van die vergelijking `true` dan weet je dat de geteste query correct functioneert.

Indien er een *"false"* op de console verschijnt, werkt iets dus niet naar behoren.

Het werken met dergelijk testcode kan zorgen voor wat efficiëntie: Kijken of alle testen slagen (of er *"true"* werd afgedrukt) gaat veel sneller, dan lezen wat de waardes zijn die de queries produceren, en zelf nadenken over het feit of deze de verwachte waardes zijn. (Wat de techniek is die je allicht voorheen gebruikt bij testen van je klassen).

Test na elke *manipulatie* van het object (na elke call naar een *commando*) op basis van alle *queries* na of het object zijn in een correct *toestand* bevindt.

## Oplossing D14counter
Counter.cs

```csharp
namespace D14.D14counter {

    class Counter {

        private int _value;
        public void SetValue(int value) {
            _value = value;
        }
        public int GetValue() {
            return _value;
        }

        private int _stepValue = 1;
        public void SetStep(int stepValue) {
            _stepValue = stepValue;
        }
        public int GetStep() {
            return _stepValue;
        }

        public void Advance() {
            _value += _stepValue;
        }

    }

}
```

## Oplossing D14selecteerwinnaar
Program.cs

```csharp
using System;

namespace D14.D14selecteerwinnaar {

    class Program {

        static void Main() {
            Persoon[] personen = new Persoon[5];
            personen[0] = new Persoon(); personen[0].SetNaam("Jan");
            personen[1] = new Persoon(); personen[1].SetNaam("Piet");
            personen[2] = new Persoon(); personen[2].SetNaam("Joris");
            personen[3] = new Persoon(); personen[3].SetNaam("Corneel");
            personen[4] = new Persoon(); personen[4].SetNaam("Mieke");

            Persoon winnaar = SelecteerWinnaar(personen);

            Console.WriteLine($"De winnaar is {winnaar.GetNaam()}");
        }

        static Persoon SelecteerWinnaar(Persoon[] kandidaten) {
            Random rnd = new Random();
            int index = rnd.Next(kandidaten.Length);
            return kandidaten[index];
        }

    }

}
```

## Oplossing D14afstandtussenpunten
Punt.cs

```csharp
namespace D14.D14afstandtussenpunten {

    class Punt {

        private double _x;
        public void SetX(double x) {
            _x = x;
        }
        public double GetX() {
            return _x;
        }

        private double _y;
        public void SetY(double y) {
            _y = y;
        }
        public double GetY() {
            return _y;
        }

        public static double GetAfstandTussen(Punt p1, Punt p2) {
            double x1 = p1.GetX();
            double x2 = p2.GetX();
            double y1 = p1.GetY();
            double y2 = p2.GetY();

            return Math.Sqrt(Math.Pow(x1 - x2, 2) + Math.Pow(y1 - y2, 2));
        }

    }

}
```

Program.cs

```csharp
using System;

namespace D14.D14afstandtussenpunten {

    class Program {

        static void Main() {
            Punt p1 = new Punt();
            p1.SetX(4);
            p1.SetY(6);

            Punt p2 = new Punt();
            p2.SetX(7);
            p2.SetY(2);

            double afstand = Punt.GetAfstandTussen(p1, p2);

            Console.WriteLine($"De afstand is {afstand}");
        }

    }

}
```

## Oplossing D14uitlening
```csharp
using System;

namespace D14.D14uitlening {

    class Uitlening {

        private string _omschrijving;
        public void SetOmschrijving(string omschrijving) {
            _omschrijving = omschrijving;
        }
        public string GetOmschrijving() {
            return _omschrijving;
        }

        private DateTime _ontleenDatum;
        public void SetOntleendatum(DateTime datum) {
            _ontleenDatum = datum;
        }
        public DateTime GetOntleendatum() {
            return _ontleenDatum;
        }

        public DateTime UitersteInleverdatum() {
            return GetOntleendatum().AddDays(14);
        }

    }

}
```

## Oplossing D14bankrekening
Program.cs

```csharp
using System;

namespace D14.D14bankrekening {

    class Program {

        static void Main() {
            Bankrekening b1 = new Bankrekening();
            Bankrekening b2 = new Bankrekening();

            decimal bedrag = 100m;

            b1.SchrijfOver(bedrag, b2);  // (1)

            Console.WriteLine(b1.Saldo() == -100m); // zou true moeten geven
            Console.WriteLine(b2.Saldo() == 100m);  // zou true moeten geven
        }

    }

}
```

1.  Deze regel werd toegevoegd.

Bankrekening.cs

```csharp
namespace D14.D14bankrekening {

    class Bankrekening {

        private decimal _saldo;
        public void Stort(decimal bedrag) {
            _saldo = _saldo + bedrag;
        }
        public void HaalAf(decimal bedrag) {
            _saldo = _saldo - bedrag;
        }
        public decimal Saldo() {
            return _saldo;
        }

        public void SchrijfOver(decimal bedrag, Bankrekening doelRekening) {  // (1)
            this.HaalAf(bedrag);  // (2)
            doelRekening.Stort(bedrag);
        }

    }

}
```

1.  Deze method werd toegevoegd.

2.  Merk op dat het gebruik van `this` hier kan benadrukken dat het van het object in uitvoering is (bijvoorbeeld object `b1` van bovenstaande `Main`) dat het *bedrag* wordt afgehaald.
