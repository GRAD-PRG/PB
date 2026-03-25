# Programmeren Basis - Deel 15 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Visibility / Properties
### Oefening D15persoon
Pas de klasse `Persoon` uit de oplossing van *D14persoon* zo aan dat je met properties werkt als `Naam`, `Geboortedatum` en \`Woonplaats´.

We gaan er hier van uit dat we net als voorheen deze eigenschappen zowel willen kunnen opvragen (*getten*) als instellen (*setten*).

Je gaat in de code van de `Leeftijd()` method ook één en andere moeten aanpassen.

Pas ook de code van de `Main` method uit de klasse `Program` aan om correct overweg te gaan met deze properties.

### Oefening D15rechthoek
Pas de klasse `Rechthoek` uit de oplossing van *D14rechthoek* zo aan dat je met properties werkt als `Hoogte` en `Breedte`.

We gaan er hier van uit dat we net als voorheen deze eigenschappen zowel willen kunnen opvragen (*getten*) als instellen (*setten*).

Je gaat in de code van de `Oppervlakte()` method ook één en andere moeten aanpassen.

Herschrijf ook de `Program` klasse uit de opgave van deze oefening.

### Oefening D15cirkel
Pas de klasse `Cirkel` uit de oplossing van *D14cirkel* zo aan dat je met een property `Straal` werkt.

We gaan er hier van uit dat we net als voorheen deze eigenschap zowel willen kunnen opvragen (*getten*) als instellen (*setten*).

Je gaat in de code van de `Oppervlakte()` en `Omtrek()` methods ook één en andere moeten aanpassen.

Herschrijf ook de `Program` klasse uit de opgave van deze oefening.

### Enkel uitleesbare properties
#### Oefening D15bankrekening
Pas de klasse `Bankrekening` uit de oplossing van *D14bankrekening* zo aan dat je met een property `Saldo` werkt.

We gaan er hier van uit dat we net als voorheen deze eigenschap enkel kunnen uitlezen (*getten*), en niet rechtstreeks instellen (niet kunnen *setten*).

Uiteraard blijft het zo dat de `Stort(decimal bedrag)` en `HaalAf(decimal bedrag)` methods de *saldo toestand* gaan manipuleren. Ook in de code van die methods ga je één en andere moeten aanpassen.

Herschrijf ook de `Program` klasse uit de opgave van deze oefening.

## Constructoren
### Default constructor
#### Oefening D15artikel
Pas de klasse `Artikel` uit de oplossing van *D14artikel* zo aan dat je met properties `PrijsExclusiefBtw` en `BtwPercentage` werkt.

We gaan er hier van uit dat we net als voorheen deze eigenschap zowel willen kunnen opvragen (*getten*) als instellen (*setten*).

Maak gebruik van een *default constructor* om het initiële `BtwPercentage` op *21* te krijgen. Ter herinnering: een default constructor is `public` en parameterloos.

Maak gebruik van dezelfde `Program` klasse als in de oplossing van *D14artikel* om uit te testen of je klasse en members correct zijn gedefinieerd.

#### Oefening D15encrypter
Neem de oplossing van *D11encrypted* erbij en stop de logica voor de versleuteling in een class `Encrypter`.

Deze klasse heeft een constructor met 1 parameter, nl. de verschuiving (offset) die moet gebruikt worden bij het opstellen van de geheime tekst. Het codewiel zit ingebakken in die klasse (in een private dataveld) en kan niet van buitenaf worden ingesteld of opgevraagd.

Ter herinnering, het codewiel was een string die er zo uitzag :

```csharp
string codewiel = "0ab1cd2ef3gh4ij5kl6m n7op8qr9st.uv,wx!yz?"; // (1)
```

1.  Maak hiervan een private dataveld in je klasse `Encrypter`

*Als de termen verschuiving, offset, en codewiel je niks zeggen, dan moet je nog eens de uitleg bij die oefening D11encrypted herlezen.*

Voorzie ook een instance method `GetCodeFor` met 1 parameter voor de tekst die moet versleuteld worden. Deze method produceert de versleutelde tekst als return value.

Een `Encrypter` object kan dan als volgt gebruikt worden :

```csharp
Encrypter e = new Encrypter(6); // (1)
string gewoneTekst = "hallo";
string geheimeTekst = e.GetCodeFor(gewoneTekst); // (2)
Console.WriteLine($"{gewoneTekst} werd versleuteld tot {geheimeTekst}");
```

1.  hier wordt een `Encrypter` object aangemaakt met het ingebakken codewiel en een verschuiving van `6`

2.  hier wordt "hallo" meegegeven als parameter dus de return value zal de versleutelde versie van `hallo` zijn.

Je kunt je class `Encrypter` testen met deze code :

```csharp
Encrypter e5 = new Encrypter(5);
string tekst = "a19z";
string code = e5.GetCodeFor(tekst);
Console.WriteLine("+ 5 " + tekst + "->" + code);

tekst = "GROEN";
Console.WriteLine("+ 5 " + tekst + "->" + e5.GetCodeFor(tekst));

Encrypter e10 = new Encrypter(10);
tekst = "c# !";
Console.WriteLine("+10 " + tekst + "->" + e10.GetCodeFor(tekst));

Console.WriteLine();

tekst = "0allo?";
Encrypter e1 = new Encrypter(1);
Console.WriteLine("+ 1 " + tekst + "->" + e1.GetCodeFor(tekst));

Encrypter e40 = new Encrypter(40);
Console.WriteLine("+40 " + tekst + "->" + e40.GetCodeFor(tekst));

Encrypter e41 = new Encrypter(41);
Console.WriteLine("+41 " + tekst + "->" + e41.GetCodeFor(tekst));

Encrypter em1 = new Encrypter(-1);
Console.WriteLine("- 1 " + tekst + "->" + em1.GetCodeFor(tekst));

Console.WriteLine();

Encrypter em10 = new Encrypter(-10);
Console.WriteLine("-10 " + tekst + "->" + em10.GetCodeFor(tekst));

Encrypter em40 = new Encrypter(-40);
Console.WriteLine("-40 " + tekst + "->" + em40.GetCodeFor(tekst));

Encrypter em41 = new Encrypter(-41);
Console.WriteLine("-41 " + tekst + "->" + em41.GetCodeFor(tekst));

Encrypter em82 = new Encrypter(-82);
Console.WriteLine("-82 " + tekst + "->" + em82.GetCodeFor(tekst));
```

De output van dit fragment is dezelfde als bij de uitleg bij die oefening *D11encrypted*…​

```csharp
+ 5 a19z->2fv1
+ 5 GROEN->GROEN
+10 c# !->j#t2

+ 1 0allo?->ab66p0
+40 0allo?->?0kk7z
+41 0allo?->0allo?
- 1 0allo?->?0kk7z

-10 0allo?->.ueeit
-40 0allo?->ab66p0
-41 0allo?->0allo?
-82 0allo?->0allo?
```

### Verplichte initialisatie / Meerdere constructoren (die elkaar oproepen)
#### Oefening D15artikelmetprijs
Pas je oplossing van voorgaande oefening aan. Maak het verplicht om bij creatie van een \`Artikel\`op zijn minst een *prijs exclusief BTW* te voorzien.

In totaal zijn er twee mogelijkheden bij het aanmaken van `Artikel` objecten:

-   met één parameter (de *prijs exclusief BTW*), bijvoorbeeld `new Artikel(100m)`, *100* zal hier de *prijs exclusief BTW* zijn, *21* is dan het default *BTW percentage* van toepassing

-   met twee parameter (de *prijs exclusief BTW* en het *BTW percentage*), bijvoorbeeld `new Artikel(200m, 6m)`, *200* zal hier de *prijs exclusief BTW* zijn, *6* is dan het *BTW percentage* van toepassing

Test uit of je klasse en members correct gedefinieerd zijn aan de hand van volgende testcode…​

```csharp
// Test de constructor met één parameter:
Artikel artikel1 = new Artikel(100m);
Console.WriteLine(artikel1.PrijsExclusiefBtw == 100m);    // zou true moeten opleveren
Console.WriteLine(artikel1.BtwPercentage == 21m);         // zou true moeten opleveren
Console.WriteLine(artikel1.PrijsInclusiefBtw() == 121m);  // zou true moeten opleveren

// Test of de __setters__ nog correct functioneren:
artikel1.PrijsExclusiefBtw = 1000m;
artikel1.BtwPercentage = 6m;
Console.WriteLine(artikel1.PrijsExclusiefBtw == 1000m);   // zou true moeten opleveren
Console.WriteLine(artikel1.BtwPercentage == 6m);          // zou true moeten opleveren
Console.WriteLine(artikel1.PrijsInclusiefBtw() == 1060m); // zou true moeten opleveren

// Test de constructor met twee parameters:
Artikel artikel2 = new Artikel(200m, 6m);
Console.WriteLine(artikel2.PrijsExclusiefBtw == 200m);    // zou true moeten opleveren
Console.WriteLine(artikel2.BtwPercentage == 6m);          // zou true moeten opleveren
Console.WriteLine(artikel2.PrijsInclusiefBtw() == 212m);  // zou true moeten opleveren

// Test uit of de prijs exclusief BTW wel verplicht is,
// volgende regel code zou dan ook een compile-fout moeten opleveren:
Artikel artikel3 = new Artikel();
// Zet bovenstaande regel in commentaar indien hij daadwerkelijk een
// compile-fout oplevert, dan heb je bereikt wat de bedoeling was
```

### Immutable objecten
#### Oefening D15immutablecirkel
Herneem je oplossing van *Oefening D15cirkel* maar zorg er deze keer voor dat objecten van de klasse `Cirkel` immutable zijn. Meer specifiek: na creatie van een `Cirkel` kan de `Straal` niet meer wijzigen.

Ter herinnering…​

Indien je zelf datatype gedeeltelijk of geheel immutable wil maken, ga je typisch:

-   een constructor voorzien om initiële waardes bij creatie van het object te kunnen opgeven

-   *getters* voorzien om de nodige informatie bevraagbaar te maken

Bij creatie van een `Cirkel` gaan we de *straal* opgeven. De *straal eigenschap* is opvraagbaar (*gettable*), niet verder instelbaar (*settable*).

Test ook uit aan de hand van een `Program` of je inderdaad de *straal* niet meer kan wijzigen (wat de bedoeling dus is).

#### Oefening D15afstandtussenpunten
Herneem je oplossing van *Oefening D14afstandtussenpunten* maar zorg er deze keer voor dat objecten van de klasse `Punt` immutable zijn. Werk anderzijds ook met propertys voor de *X* en *Y* coördinaten.

Bij creatie van een `Punt` object moet de `X` en `Y` waarde worden ingesteld. Na creatie kunnen deze waardes niet meer veranderen

Pas de `Program` zo aan dat met de properties wordt gewerkt.

## Verwijzingen en samengestelde objecten
### Oefening D15cirkelpunt
Maak gebruik van de `Punt` klasse uit je oplossing van *D15afstandtussenpunten*, en herwerk de `Cirkel` klasse van *D15immutablecirkel* als volgt…​

Elke *cirkel* houdt nu de positie van z’n *middelpunt* bij, deze wordt bij constructie ingesteld door de *X* en *Y* waarden mee te geven aan de constructor (bovenop de *straal*).

Voorzie een method `VerplaatsNaar(int x, int y)` die het *middelpunt* van de *cirkel* verplaatst naar de nieuwe positie.

Voorzie ook een `Middelpunt` member die een `Punt` oplevert dat *het middelpunt van de cirkel* voorstelt.

Voorzie in de `Cirkel` klasse ook een `Bevat(Punt p)` method die *true* of *false* antwoordt al naargelang of het *punt* binnen de *cirkel* valt of niet. Hiervoor gebruik je de method `GetAfstandTussen`.

Test je programma uit met de volgende code…​

```csharp
Cirkel c = new Cirkel(10, 20, 5);   // x, y en straal
Punt p1 = new Punt(13, 25);         // x en y
Punt p2 = new Punt(8, 16);          // x en y

Console.WriteLine(c.Bevat(p1));     // toont false
Console.WriteLine(c.Bevat(p2));     // toont true

c.VerplaatsNaar(11, 27);            // x en y

Console.WriteLine(c.Middelpunt.X);  // toont 11
Console.WriteLine(c.Middelpunt.Y);  // toont 27

Console.WriteLine(c.Bevat(p1));     // toont true
Console.WriteLine(c.Bevat(p2));     // toont false
```

Teken een object diagram dat de situatie weergeeft op het einde van de `Main` method.

### Oefening D15cirkeloverlapt
Ga nog wat verder…​

Voeg aan de `Cirkel` klasse een method 'Overlapt' toe met 2 `Cirkel` parameters die *true* of *false* retourneert al naargelang of *de cirkels* elkaar overlappen of niet.

Kijk eens goed in onderstaande `Program` of er nu verwacht wordt dat de `Overlapt` method een instance method (non-`static`) of class method (`static`) is.

Twee *cirkels overlappen* elkaar indien de afstand tussen hun *middelpunten* kleiner is dan de som van hun *stralen*.

Werk met volgende code om je `Overlapt` method uit te testen…​

```csharp
Cirkel c1 = new Cirkel(10, 20, 5);
Cirkel c2 = new Cirkel(8, 12, 10);
Cirkel c3 = new Cirkel(100, 200, 3);

Console.WriteLine(Cirkel.Overlapt(c1, c2));  // moet true opleveren
Console.WriteLine(Cirkel.Overlapt(c2, c3));  // moet false opleveren
```

### Oefening D15figuren
Opnieuw gaan we een stapje verder…​

De klassen `Cirkel` en `Punt` horen samen, ze zijn *coherent* omdat ze beiden iets met *figuren* te maken hebben.

Definieer deze keer in je namespace voor deze oefening ( `namespace D15.D15figuren { …​ }` ) een *subnamespace* met de naam `Figuren` (of dus voluit `D15.D15figuren.Figuren`). Plaats beide klassen (`Cirkel` en `Punt`) die je in deze oefening overneemt in deze namespace `Figuren`.

Vaak plaats men alle code die tot één namespace behoort ook samen in één folder. Hier zou je binnen de folder voor de oefening (`D15figuren`) een folder `Figuren` voor alle code uit die namespace kunnen aanmaken.

> **Opmerking: Een folder aanmaken inVisual Studio.**
>
> Rechterklik in het *Solution Explorer* toolvenster op de projectnaam en kies voor **Add** **›** **New Folder**.

Zorg ervoor dat de `Program` klasse met je `Main` method niet verplaatst wordt (ze komt immers niet in de `Figuren` namespace terecht).

*Welke foutmeldingen geeft de compiler je?*

*Wat moet je veranderen in de file met je Main method om de compiler tevreden te stellen?*

### Oefening D15getafstandtussenenoverlapt
Stel dat we in de klasse `Punt`, de afstand tussen twee *punten* als een *instance method* willen definiëren (in plaats van een *class method*)…​

Hoe zouden we deze *instance method* dan kunnen aanroepen?

Herschrijf de method `GetAfstandTussen` om er een *instance method* van te maken.

Stel dat we in de klasse `Cirkel`, de method die test of twee *cirkels* overlappen als een *instance method* willen definiëren (in plaats van een *class method*). ..

Hoe zouden we deze *instance method* dan kunnen aanroepen?

Herschrijf de method `Overlapt` om er een *instance method* van te maken.

Pas ook volgende code aan om opnieuw de afstand van `p1` tot `p2` te bepalen, en het al dan niet overlappen van *cirkels* `c1` en `c2`, en `c2` en `c3` na te gaan, maar deze keer aan de hand van de *instance methods*.

```csharp
Punt p1 = new Punt(4, 6);
Punt p2 = new Punt(7, 2);

double afstand = Punt.GetAfstandTussen(p1, p2);
Console.WriteLine(afstand);                  // moet 5 zijn

Cirkel c1 = new Cirkel(10, 20, 5);
Cirkel c2 = new Cirkel(8, 12, 10);
Cirkel c3 = new Cirkel(100, 200, 3);

Console.WriteLine(Cirkel.Overlapt(c1, c2));  // moet true opleveren
Console.WriteLine(Cirkel.Overlapt(c2, c3));  // moet false opleveren
```

### Oefening D15rekeningkantoor
Voorzien klassen met de volgende properties…​

``` nowrap
    class Persoon
        string Voornaam
        string Familienaam
        Adres Adres

    class Adres
        string Straat
        string Huisnummer
        string Postcode
        string Gemeente

    class Rekening
        string Nummer
        double Saldo
        Kantoor Kantoor
        Persoon Titularis

    class Kantoor
        Persoon kantoorhouder
        Adres adres
```

Alle properties mogen *gettable* en *settable* zijn.

Geef elke klasse één constructor die de nodige parameters heeft om elke property van een beginwaarde te voorzien.

Schrijf een `Main` method die objecten maakt en aanéénknoopt voor de volgende voorbeelddata:

``` nowrap
    Jan Janssens, Koekoekstraat 70, 9090 Melle

    Jan heeft een rekening met nummer BE11 2222 3333 4444 met daarop 120Eur

    Deze rekening is bij het kantoor van Mieke Mickelsen, Kerkstraat 12, 8000 Brugge

    Mieke woont in haar kantoor
```

Teken ook een object diagram van de toestand op het eind van deze `Main` method.

### Oefening D15rekeningkantoormieke
Voeg in je oplossing van de vorige oefening, wat code toe om *Mieke* te doen verhuizen naar *huisnummer 99* in dezelfde straat..

```csharp
Persoon mieke = new Persoon(...);
...andere code...
mieke.Adres.HuisNummer = 99;
```

Wat gebeurt er met het *kantoor*? Zet eens het *huisnummer* op de console…​

```csharp
Console.WriteLine(kantoorMieke.Adres.HuisNummer);  // geeft?
```

Indien je de afgedrukt waarde niet goed begrijpt, grijp je terug naar je object diagram uit vorige oefeningen. Pas daar het `Huisnummer` van het `Adres` van `mieke` aan in *99*.

Dan kan je misschien beter volgen welke waarde wordt afgedrukt.

### Oefening D15stellingverhuur
Definiëer de noodzakelijke datatypes (klassen), en hun verwachte/vereiste members om volgende clientcode mogelijk te maken…​

```csharp
using System;

namespace D15.D15stellingverhuur {

    class Program {

        static void Main(string[] args) {
            DateTime startVerhuur = new DateTime(2022, 1, 1, 8, 0, 0);
            DateTime eindVerhuur = new DateTime(2022, 2, 1, 16, 30, 0);

            // Op basis van een start- en eindmoment kan een StellingVerhuring worden aangemaakt...
            StellingVerhuring sv1 = new StellingVerhuring(startVerhuur, eindVerhuur);

            // Van een StellingVerhuring kan je het aantal uur opbouw en afbraak opvragen...
            int aantalUurOpbouw = sv1.AantalUurOpbouw;
            int aantalUurAfbraak = sv1.AantalUurAfbraak;
            Console.WriteLine(aantalUurOpbouw);   // by default is dit 8u opbouw
            Console.WriteLine(aantalUurAfbraak);  // by default is dit 4u afbraak

            // Van een StellingVerhuring kan je de NettoVerhuurPeriode opvragen...
            Periode verhuurPeriode = sv1.NettoVerhuurPeriode();

            // Deze verhuurPeriode heeft een Start en Eind tijdstip...
            // Het start tijdstip is het startmoment van de StellingVerhuring verhoogd met het
            // aantal uren opbouw...
            DateTime startTijdstip = verhuurPeriode.Start;
            Console.WriteLine(startTijdstip);  // 1 jan 2022 16u (8u na het startmoment)
            // Het eind tijdstip is het eindmoment van de StellingVerhuring verlaagd met het
            // aantal uren afbraak...
            DateTime eindTijdstip = verhuurPeriode.Eind;
            Console.WriteLine(eindTijdstip);   // 1 feb 2022 12u30 (4u voor het eindmoment)

            // Deze verhuurPeriode heeft dan ook een bepaald aantal uur...
            // Dit aantal uur is in geval van .5 naar boven afgerond...
            int aantalUur = verhuurPeriode.AantalUur();
            Console.WriteLine(aantalUur);  // 741 (in plaats van 740.5)

            // De prijs van een StellingVerhuring kan worden opgevraagd...
            // De prijs wordt minimaal berekend op aantal...
            // . uren opbouw (90 per uur)         90 x   8 =  720
            // . netto uren verhuur (5 per uur)    5 x 741 = 3705
            // . uren afbraak (60 per uur)        60 x   4 =  240
            decimal prijs = sv1.Prijs();  //               + -----
            Console.WriteLine(prijs);     //                 4665   (= 720 + 3705 + 240)

            // Het aantal uur opbouw of afbraak van een StellingVerhuur kan worden aangepast...
            sv1.AantalUurOpbouw = 5;
            sv1.AantalUurAfbraak = 3;
            // Dit zal zijn impact hebben op de NettoVerhuurPeriode, of op zijn minst op
            // het start en eind tijdstip van deze NettoVerhuurPeriode...
            verhuurPeriode = sv1.NettoVerhuurPeriode();
            Console.WriteLine(verhuurPeriode.Start);  // 1 jan 2022 13u (3u vroeger dan voorheen)
            Console.WriteLine(verhuurPeriode.Eind);   // 1 feb 2022 13u30 (1u later dan voorheen)
            // En zal zijn impact hebben op de prijs...
            Console.WriteLine(sv1.Prijs());  // 4355

            // Je kan ook Leveringen aanmaken...
            // De zijn elk naar een bepaald Adres, en betreffen een bepaalde AfstandInKm...
            Levering leveringX = new Levering("Antwerpen", 62);
            Levering leveringY = new Levering("Gent", 43);
            // Het Adres en de AfstandInKm kan uiteraard van een Levering opgevraagd worden...
            string adres = leveringX.Adres;
            Console.WriteLine(adres);        // Antwerpen
            int afstandInKm = leveringY.AfstandInKm;
            Console.WriteLine(afstandInKm);  // 43

            // Van een StellingVerhuur kan de Levering worden ingesteld...
            sv1.Levering = leveringY;
            // Deze kan naderhand uiteraard ook worden opgevraagd...
            Levering leveringSv1 = sv1.Levering;
            Console.WriteLine(leveringSv1.Adres);        // Gent
            Console.WriteLine(leveringSv1.AfstandInKm);  // 43
            // De prijs van de StellingVerhuring zal hiermee ook verhogen op basis van het aantal...
            //                              4355
            // . km (10 per km)   10 x 43 =  430
            //                            + ----
            //                              4785
            Console.WriteLine(sv1.Prijs());
        }

    }

}
```

In commentaar zie je wat de verwachtingen zijn.

De uitvoer zou er ongeveer zo kunnen uitzien.

```csharp
8
4
1/01/2022 16:00:00
1/02/2022 12:30:00
741
4665
1/01/2022 13:00:00
1/02/2022 13:30:00
4355
Antwerpen
43
Gent
43
4785
```

Mogelijks zien de datum/tijd-formaten er uiteraard in jouw oplossing wat anders uit. Dit is afhankelijk van de, op besturingssysteem niveau, ingestelde datum- en tijdsformaten.

## Expressies en static typing
```csharp
double iets = 4.0;
Console.WriteLine(5 * iets); (1)
```

1.  Wordt deze regel code aanvaard? Of zal deze compilerfouten veroorzaken?

Om in te schatten of een bepaalde grammaticale constructie aanvaard wordt (geen compilerfouten oplevert) kan je jezelf enkele vragen stellen…​

-   *Welke mogelijke expressies herken ik?*

Bijvoorbeeld `4.0`, `5` en `iets` zouden expressies kunnen zijn. Ook `5 * iets` zou samen een expressie kunnen vormen.

Heb je het moeilijk om op voorgaande vraag te antwoorden, herfomuleer het dan als volgt: *Waar geef ik in de code aan dat met een bepaalde waarde, met bepaalde informatie wordt gewerkt?*

Een expressie is immers steeds een stukje code die dat probeert mee te geven. Die aangeeft met welke *waarde* je aan de slag gaat.

-   *Waarom zou dat stukje code (die ik nomineer als expressie) een expressie moeten zijn?* Of anders uitgedrukt: *Hoe (of waarvoor) wordt dat stukje code (grammaticaal gezien) gebruikt?*

`4.0` zou een expressie moeten zijn omdat het in de context van een toekenning rechts staat van de toekenningsoperator `=`. Rechts van die operator zou altijd een expressie moeten staan, meer specifiek ééntje die aangeeft welke waarde je aan de target van deze toekenning wil toewijzen.

`5` en `iets` zouden expressies moeten zijn omdat dat stukje code (wat het ook is die daar staat) aangeeft wat de waardes zijn die we aan de hand van een `*` operator met elkaar combineren.

`5 * iets` zou een expressie moeten zijn omdat dat stukje code aangeeft welke waarde wordt meegegeven (toegekend) aan de parameter van de `WriteLine` method. Er wordt immers verwacht dat we opgeven welke waarde we op de `Console` willen drukken.

-   *Wat (als dat dan al een expressie zou zijn) zou dan het datatype zijn van die expressie?*

`iets` zou dan een expressie zijn van het type `double`.

`4.0` en `5` zouden respectievelijk expressies zijn van types `double` en `int`.

`5 * iets` zou dan een expressie zijn van type `double`

-   *Waarom zou dat door de compiler worden aanzien als een expressie van dat bepaald datatype?*

`iets` als `double` expressie omdat we immers net bij wijze van de declaratie (`double iets;`) aan de compiler vertelt dat als hij (de compiler) naderhand `iets` als expressie ziet gebruikt worden, dit mag bekijken als een expressie van type `double`.

`4.0` en `5` van types `double` en `int` omdat gebruik is gemaakt van de `double` en `int` literal notatiewijzes. In de compiler is simpelweg vastgelegd dat bij het voorkomen van letterlijk opgegeven getallen, deze als `int` of `double` expressies worden beschouwd. Afhankelijk van het al dan niet voorkomen van de *decimal separator* (`.`).

`5 * iets` van type `double` omdat de `*` operator van toepassing op operanden van types `double` en `int` zijn resultaat (*het product*) zal opleveren in `double` vorm.

-   *Kan dat dan (zal de compiler dat toelaten)? Kunnen we daar op die plaats (grammaticaal gezien) een expressie van dat datatype hanteren?*

Ja absoluut.

-   *Waarom zal de compiler dat toelaten?*

Bij het toekennen van een waarde aan een `double` variabele `iets` is het enkel maar correct aan de rechterkant van de toekenningsoperator `=` een `double` expressie te gebruiken. Aan een `double` variabele ken je immers logischerwijs `double` waardes toe.

De gebruikte `*` operator kan overweg met operanden van types `double` en `int`.

Er bestaat een overloads (*versie*) van de `WriteLine` method die een `double` argumentwaarde aanvaardt.

Probeer het nu maar eens zelf…​

### Oefening D15expressies1
```csharp
using System;

namespace D15.D15expressies1 {

    class Factuur {
        public int Id { get; set; }
        public DateTime CreatieDatum { get; set; }
    }

    class Program {
        static void Main() {
            DateTime d = new DateTime(2017, 3, 12);
            Console.WriteLine(GetFactuur(5, d).CreatieDatum.Day);  // (1)
        }
        static Factuur GetFactuur(int id, DateTime creatieDatum) {
            Factuur f = new Factuur();
            f.Id = id;
            f.CreatieDatum = creatieDatum;
            return f;
        }
    }

}
```

1.  Is de code op deze regel grammaticaal correct?

Welke expressies (van welke datatypes, die wat voorstellen) herken je op die regel?

### Oefening D15expressies2
```csharp
namespace D15.D15expressies2 {

    class Persoon {
        public bool Vip { get; set; }
        public string Naam { get; set; }
    }

    class Program {
        static void Main() {
            bool v = (new Persoon()).Vip;  // (1)
        }
    }

}
```

1.  Is de code op deze regel grammaticaal correct?

Welke expressies (van welke datatypes, die wat voorstellen) herken je op die regel?

### Oefening D15expressies3
```csharp
namespace D15.D15expressies3 {

    class Program {
        static void Main() {
            string[] a = new string[new int[]{ 1, 2, 3 }.Length * 5];  // (1)
        }
    }

}
```

1.  Is de code op deze regel grammaticaal correct?

Welke expressies (van welke datatypes, die wat voorstellen) herken je op die regel?

### Oefening D15expressies4
```csharp
namespace D15.D15expressies4 {

    class Program {
        static void Main() {
            int g = 4;
            while (g > 5 && 6)  // (1)
            {
                /* ... */
            }
        }
    }

}
```

1.  Is de code op deze regel grammaticaal correct?

Welke expressies (van welke datatypes, die wat voorstellen) herken je op die regel?
