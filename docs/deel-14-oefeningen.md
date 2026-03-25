# Programmeren Basis - Deel 14 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Oefening D14persoon
Breid de klasse `Persoon` uit zodat we ook de *woonplaats* (tekst) van een *persoon* kunnen bijhouden, aanpassen en opvragen.

Schrijf een `Main` (niet in de `Persoon` klasse!) die eerst twee `Persoon` objecten maakt, en opvult.

Vraag al hun gegevens nu op en zet deze op de console.

Pas vervolgens van één van de objecten alle gegevens aan, en zet opnieuw alle gegevens van alle personen op de console.

Persoon.cs

```csharp
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

    ... (1)
}
```

1.  Hier uitbreiden.

## Oefening D14rechthoek
Maak zelf een klasse waarvan de objecten een eenvoudige voorstelling zijn van een *rechthoek*.

Van of met een *rechthoek* willen we:

-   de *hoogte* kunnen instellen en opvragen

-   de *breedte* kunnen instellen en opvragen

-   de *oppervlakte* kunnen opvragen

Zorg ervoor dat de meegegeven testcode volgend programmaverloop kent…​

```csharp
Rechthoek met hoogte 4,1, breedte 8 en oppervlakte 32,8.
Rechthoek met hoogte 5, breedte 10,2 en oppervlakte 51.
```

Meegegeven testcode…​

Program.cs

```csharp
class Program {
    static void Main() {
        Rechthoek rechthoek1 = new Rechthoek();
        rechthoek1.SetHoogte(4.1);
        rechthoek1.SetBreedte(8);

        Rechthoek rechthoek2 = new Rechthoek();
        rechthoek2.SetHoogte(5);
        rechthoek2.SetBreedte(10.2);

        PrintRechthoek(rechthoek1);
        PrintRechthoek(rechthoek2);
    }
    static void PrintRechthoek(Rechthoek r) {
        Console.WriteLine($"Rechthoek met hoogte {r.GetHoogte()}, breedte {r.GetBreedte()} en oppervlakte {r.Oppervlakte()}.");
    }
}
```

## Oefening D14cirkel
Schrijf een klasse `Cirkel` zodat we `Cirkel` objecten een *straal* kunnen geven en deze ook opvragen.

Bovendien moet een `Cirkel` ons kunnen vertellen wat zijn *oppervlakte* en *omtrek* is.

Schrijf een `Main` method (niet in de `Cirkel` klasse!) die een *cirkel* aanmaakt met *straal 3.45* en diens informatie op de console zet.

## Oefening D14artikel
Creëer een klasse waarvan de objecten een eenvoudige voorstelling zijn van een *artikel*.

Van of met een *artikel* willen we:

-   de *prijs exclusief BTW* kunnen instellen en opvragen

-   de *BTW* kunnen instellen, als de waarde niet wordt ingesteld is 21% van toepassing

-   de *BTW* kunnen opvragen

-   de *prijs inclusief BTW* kunnen opvragen

Maak ook zelf de nodige testcode aan om voorvermelde verantwoordelijkheden uit te testen.

## Oefening D14counter
Schrijf een `Counter` klasse die een *teller* voorstelt.

Elke *counter* heeft een *value* en een *step value*.

Voorzie een `Advance` method die de *teller* bij elke oproep met de *step value* verhoogt.

De *value* van de teller is `0` bij het begin. Je kunt deze waarde instellen met `SetValue` of opvragen met de `GetValue` method.

De *step value* is by default `1`, kan je opvragen met `GetStep`, en instellen met `SetStep`.

Test je `Counter` klasse met een `Main` method met volgende inhoud…​

```csharp
Counter c1 = new Counter();

Counter c2 = new Counter();
c2.SetValue(100);

Counter c3 = new Counter();
c3.SetValue(1000);
c3.SetStep(10);

for (int i = 0; i < 10; i++) {
    c1.Advance();
    c2.Advance();
    c3.Advance();
}

Console.WriteLine(c1.GetValue());  // toont 10
Console.WriteLine(c2.GetValue());  // toont 110
Console.WriteLine(c3.GetValue());  // toont 1100
```

## Oefening D14selecteerwinnaar
Gebruik je laatste `Persoon` klasse en schrijf een `Main` method die objecten aanmaakt voor *vijf* van je *vrienden* en deze in een array bijhoudt. Geef al je vrienden op zijn minst een *naam*.

In dezelfde class als je `Main` method, stop je onderstaande method `SelecteerWinnaar` en vult deze verder aan: `static Persoon SelecteerWinnaar(Persoon[] personen) { …​ }`

Deze method selecteert een willekeurige persoon uit de array en retourneert deze.

Gebruik `SelecteerWinnaar` in je `Main` method om één van je *vrienden* tot *winnaar* te kronen en zet diens naam op de console.

## Oefening D14afstandtussenpunten
Schrijf een klasse `Punt` met een *x* en een *y coordinaat*, beiden van type `double`. Voorzie geschikte datavelden alsook `Get` en `Set` methods.

Voeg een class method `GetAfstandTussen` toe aan deze klasse met twee `Punt` parameters. Deze method produceert de afstand tussen de 2 meegegeven punten.

Zie eventueel: https://nl.wikipedia.org/wiki/Afstand#Afstand_tussen_twee_punten

Voorzie een `Main` method die de afstand berekent tussen de *punten (4,6)* en *(7,2)* en op de console zet. (Deze afstand is trouwens gelijk aan *5*.)

## Oefening D14uitlening
Maak het mogelijk verschillende instanties te maken van een datatype als `Uitlening`.

Over welke members dit datatype moet beschikken zal je moeten afleiden uit de meegegeven clientcode. Uit het meegegeven programma-verloop kan je anderzijds het gedrag afleiden.

Program.cs

```csharp
using System;

namespace D14.D14uitlening {

    class Program {
        static void Main() {
            Uitlening[] uitleningen = new Uitlening[10];
            int aantal = 0;
            do {
                PrintUitleningen(uitleningen, aantal);

                Console.Write("Nieuwe ontlening op?: ");
                DateTime d = DateTime.Parse(Console.ReadLine());
                Console.Write("Omschrijving?: ");
                string o = Console.ReadLine();

                aantal = Toevoegen(uitleningen, aantal, o, d);

                Console.WriteLine();
            } while (true);
        }

        static void PrintUitleningen(Uitlening[] uitleningen, int aantal) {
            for (int index = 0; index < aantal; index++) {
                Uitlening u = uitleningen[index];
                Console.WriteLine($"- {u.GetOmschrijving()}: ontleent op {u.GetOntleendatum().ToString("dd/MM/yyyy")} binnen ten laatste op {u.UitersteInleverdatum().ToString("dd/MM/yyyy")}.");
            }
            Console.WriteLine();
        }

        static int Toevoegen(Uitlening[] uitleningen, int aantal, string omschrijving, DateTime ontleendatum) {
            Uitlening nieuweUitlening = new Uitlening();
            nieuweUitlening.SetOmschrijving(omschrijving);
            nieuweUitlening.SetOntleendatum(ontleendatum);
            aantal++;

            uitleningen[aantal - 1] = nieuweUitlening;

            return aantal;
        }
    }

}
```

1.  Een `Array.Resize` method kan gebruikt worden om de oorspronkelijk array-instantie waar de array-variabele naartoe verwijst te vervangen door een nieuwe array-instantie (met aangepaste lengte).

We krijgen bij invoer van *15/12/2017, *Uitlening Moby Dick*, *21/12/2017**, *Uitlening Ivanhoe*, *22/12/2017*\_ en *Uitlening Nana* volgend resultaat…​

```csharp
Nieuwe ontlening op?: 15/12/2017
Omschrijving?: Uitlening Moby Dick

- Uitlening Moby Dick: ontleent op 15/12/2017 binnen ten laatste op 29/12/2017.

Nieuwe ontlening op?: 21/12/2017
Omschrijving?: Uitlening Ivanhoe

- Uitlening Moby Dick: ontleent op 15/12/2017 binnen ten laatste op 29/12/2017.
- Uitlening Ivanhoe: ontleent op 21/12/2017 binnen ten laatste op 04/01/2018.

Nieuwe ontlening op?: 22/12/2017
Omschrijving?: Uitlening Nana

- Uitlening Moby Dick: ontleent op 15/12/2017 binnen ten laatste op 29/12/2017.
- Uitlening Ivanhoe: ontleent op 21/12/2017 binnen ten laatste op 04/01/2018.
- Uitlening Nana: ontleent op 22/12/2017 binnen ten laatste op 05/01/2018.

Nieuwe ontlening op?:
```

## Oefening D14bankrekening
Vertrek van de meegegeven code…​.

Maak het met één *commando* `SchrijfOver`, dat je op een `Bankrekening` object aanroept (de *bronrekening*), mogelijk een bepaald *bedrag* naar een *andere bankrekening* (de *doelrekening*) over te schrijven.

Meegegeven code…​

Program.cs

```csharp
using System;

namespace D14.D14bankrekening {

    class Program {
        static void Main() {
            Bankrekening b1 = new Bankrekening();
            Bankrekening b2 = new Bankrekening();

            decimal bedrag = 100m;

            ...SchrijfOver...  // (1)

            Console.WriteLine(b1.Saldo() == -100m); // zou true moeten geven
            Console.WriteLine(b2.Saldo() == 100m);  // zou true moeten geven
        }
    }

}
```

1.  Met één commando maak je het mogelijk `bedrag` van `b1` naar `b2` over te schrijven:

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

        ... (1)
    }

}
```

1.  Voeg hier je `SchrijfOver` method toe.

Het *overschrijven* van bedrag *a* van rekening *x* naar *y* mag je implementeren als het afhalen van bedrag *a* van *x* en het storten van bedrag *a* op *y*.
