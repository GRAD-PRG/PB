# Programmeren Basis - Deel 12 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Documentatie
### Oefening D12documentatie
Zoek de webpagina met de officiële .NET API documentatie over

-   De `Sleep()` method waarmee we ons programma konden laten wachten

-   De `ReadKey()` method met een `bool` parameter

en schrijf de links op waar je deze informatie vond.

## Debugging
### Oefening D12palindroom
Stel dat we onderstaande code zouden overnemen…​

```csharp
 1 : static void Main() {
 2 :    Console.WriteLine("Geef een tekst : ");
 3 :    string tekst = Console.ReadLine();
 4 :
 5 :    Console.WriteLine(IsPalindroom(tekst));
 6 : }
 7 :
 8 : static bool IsPalindroom(string tekst) {
 9 :    string reverse = ReverseText(tekst);
10 :    if (tekst == reverse) {
11 :        return true;
12 :    } else {
13 :        return false;
14 :    }
15 : }
16 :
17 : static string ReverseText(string tekst) {
18 :    string result = "";
19 :    foreach(char c in tekst) {  // (1)
20 :        result = c.ToString();
21 :    }
22 :    return result;
23 : }
```

1.  Hier zouden we een breakpoint plaatsen.

…​en een breakpoint zou plaatsen op *regel 19*.

**Hoe zal dan de call stack er dan uitzien** wanneer je de code tot daar zou laten uitvoeren? Dit in de veronderstelling dat de gebruiker de waarde *hallo* had ingevoerd.

Neem bij elke activatie op je call stack ook de parameterwaardes op. En geef daarnaast ook nog eens van de andere lokale variabelen (`result`, `reverse` en `input`) tijdens die activaties de waardes weer.

Voorbeeld

Bij een breakpoint in onderstaande code op *regel 11*…​

```csharp
 1 : static void Main() {
 2 :    int a = 123;
 3 :    PrintDubbele(a);
 4 :
 5 :    int b = 200;
 6 :    PrintDubbele(200);
 7 : }
 8 :
 9 : static void PrintDubbele(int getal) {
10 :    int dubbele = getal * 2;
11 :    Console.WriteLine($"Het dubbele van {getal} is {dubbele}.");  // (1)
12 : }
```

1.  Hier zouden we een breakpoint plaatsen.

Zal bij uitvoer tot op die regel de *call stack*, en *overige variabelen op de verschillende activatierecords* er zo uitzien:

| Call Stack                                             | Overige variabelen op het activatie record |
|-------------------------------------------|-----------------------------|
| `PrintDubbele(int getal = 123)` gebroken op *regel 11* | `int getal = 123` en `int dubbele = 246`   |
| `Main()` invoceert op *regel 3* bovenstaande activatie | `int a = 123` en `int b = 0`               |

Merk op dat de inhoud van `b` nog op *0* staat. `PrintDubbele` mag dan al twee keer worden aangeroepen, er is wel gebroken tijdens de eerste activatie. Met andere woorden de uitvoer is gepauzeerd nog voor aan `b` een waarde kon worden toegekend. Om die reden staat hij nog op zijn *defaultwaarde 0*.

Neem de code nu ook effectief over, je kan ze van hieronder kopiëren (zonder lijnnummers deze keer)…​

```csharp
static void Main() {
    Console.WriteLine("Geef een tekst : ");
    string tekst = Console.ReadLine();

    Console.WriteLine(IsPalindroom(tekst));
}

static bool IsPalindroom(string tekst) {
    string reverse = ReverseText(tekst);
    if (tekst == reverse) {
        return true;
    } else {
        return false;
    }
}

static string ReverseText(string tekst) {
    string result = "";
    foreach(char c in tekst) {
        result = c.ToString();
    }
    return result;
}
```

…​en **zoek uit wat er fout loopt**.

Indien de gebruiker bijvoorbeeld *kajak* invoert wordt *false* afgedrukt, dit ondanks er wel degelijk sprake is van een *palindroom*.

Ga op zoek naar de fout. Start het programma opnieuw op en onderbreek de code op een gepast tijdstip. We hebben gezien dat je dit bijvoorbeeld kan met breakpoint(s) of aan de hand van de *'Run execution to here'* optie.

Maak eventueel gebruik van debugger opties als *Step Into*, *Over* of *Out* om stap voor stap door de code te gaan, en zo hopelijk uit te komen op het stukje code dat de *logische fout* veroorzaakt.

## Kommagetallen en afronden
### Oefening D12soortenafrondingen
Voorspel **eerst** wat de verschillende afgeronde waarden voor getal `g` zijn in onderstaande tabel.

| `g`    | `Math.Ceiling(g)` | `Math.Floor(g)` | `Math.Round(g)` | `Math.Round(g, MidpointRounding.AwayFromZero)` |
|--------|-------------------|-----------------|-----------------|------------------------------------------------|
| `7.4`  |                   |                 |                 |                                                |
| `7.5`  |                   |                 |                 |                                                |
| `7.6`  |                   |                 |                 |                                                |
| `-7.4` |                   |                 |                 |                                                |
| `-7.5` |                   |                 |                 |                                                |
| `-7.6` |                   |                 |                 |                                                |

Schrijf **daarna** een programma dat deze waarden voor je berekent.

## Afronden
### Oefening D12gasmaatschappij
Een gasmaatschappij installeert meters die terug naar nul springen eenmaal ze 999999 kubieke meter passeren.

Schrijf een programma dat toelaat om de begin- en eindstand van de meter voor een facturatieperiode in te lezen (gehele getallen) en daarna de totale kostprijs weergeeft voor het verbruik (afgerond tot 2 cijfers na de komma).

De berekening is als volgt :

-   voor de eerste 1000 kubieke meter is de kostprijs 0.34 Euro per kubieke meter

-   alle verbruik boven 1000 kubieke meter kost 0.31 Euro per kubieke meter

Hou er rekening mee dat de meter terug naar nul kan springen middenin een facturatieperiode!

Je mag ervan uit gaan dat men nooit meer dan 999999 kubieke meter verbruikt in dezelfde periode.

Voorbeeld

Bijvoorbeeld, bij een beginstand van 8900 kubiek en een eindstand van 14783 kubiek, zal de totale kost 1853,73 bedragen.

Het verbruik is dan immers 5883 kubiek waarvan 1000 aan 0.34Eur per kubiek (=340 Eur) en 4883 aan 0.31Eur per kubiek (=1513,73 Eur).

De uitvoering voor deze meterstanden ziet er zo uit :

```csharp
Geef de beginstand : 8900
Geef de eindstand : 14783
De totale kostprijs is 1853,73 Eur
```

Voorbeeld Een uitvoering waarbij de teller terug naar nul sprong tijdens de facturatieperiode :

```csharp
Geef de beginstand : 999920
Geef de eindstand : 120
De totale kostprijs is 68,00 Eur
```

Het totale verbruik is 80+120 = 200 kubiek :

-   80 kubiek werd verbruikt totdat de teller terug op nul sprong (omdat 999999 werd overschreden)

-   120 kubiek werd verbruikt nadat de teller terug op nul sprong

### Oefening D12kapitaalnietafgerond
Schrijf een programma dat de gebruiker om het startkapitaal vraagt en een intrestvoet (in %). Je mag ervan uitgaan dat de gebruiker braafjes de gevraagde waarden invoert.

Daarna toont het op de console hoeveel kapitaal er is in de eerste 20 jaar.

Ga ervan uit dat de bank de bedragen intern niet afrondt, maar enkel de getallen in de output afrondt. De bank rekent dus elk jaar verder met het niet-afgeronde kapitaal.

De uitvoering voor 100Eur en 5% intrest ziet er zo uit :

```csharp
Geef een bedrag : 100
Geef de intrestvoet (in %) : 5
jaar 0 : 100.00
jaar 1 : 105.00
jaar 2 : 110.25
jaar 3 : 115.76
jaar 4 : 121.55
jaar 5 : 127.63
jaar 6 : 134.01
jaar 7 : 140.71
jaar 8 : 147.75
jaar 9 : 155.13
jaar 10 : 162.89
jaar 11 : 171.03
jaar 12 : 179.59
jaar 13 : 188.56
jaar 14 : 197.99
jaar 15 : 207.89
jaar 16 : 218.29
jaar 17 : 229.20
jaar 18 : 240.66
jaar 19 : 252.70
jaar 20 : 265.33
```

### Oefening D12kapitaalwelafgerond
Herschrijf het programma uit de vorige oefening zodat de bank dit keer wel voor elk jaar verder rekent met het afgeronde kapitaal.

De uitvoering voor 100Eur en 5% intrest ziet er zo uit :

```csharp
Geef een bedrag : 100
Geef de intrestvoet (in %) : 5
jaar 0 : 100.00
jaar 1 : 105.00
jaar 2 : 110.25
jaar 3 : 115.76
jaar 4 : 121.55
jaar 5 : 127.63
jaar 6 : 134.01
jaar 7 : 140.71
jaar 8 : 147.75
jaar 9 : 155.14
jaar 10 : 162.90
jaar 11 : 171.04
jaar 12 : 179.59
jaar 13 : 188.57
jaar 14 : 198.00
jaar 15 : 207.90
jaar 16 : 218.30
jaar 17 : 229.22
jaar 18 : 240.68
jaar 19 : 252.71
jaar 20 : 265.35
```

> **Belangrijk**
>
> Merk op dat er na 20 jaar `0.02` Euro verschil is naargelang of de bank wel/niet tussenresultaten afrondt!

### Oefening D12kortingplusbtw
Schrijf een programma dat de gebruiker vraagt om een prijs excl. BTW, een kortingspercentage en een BTW-percentage. Je mag ervan uitgaan dat de gebruiker braafjes de gevraagde waarden invoert.

Het programma toont de bedragen excl. BTW, korting, BTW en incl. BTW. De BTW wordt berekend nadat de korting al is afgetrokken. Je mag ervan uitgaan dat de bedragen altijd positief zijn en onder de 1000000 Euro blijven.

Toon de bedragen netjes rechts uitgelijnd zoals op een kassaticket en gebruik bij het afronden `MidpointRounding.AwayFromZero`.

Bv. als de gebruiker 123,45Eur met 10,25% korting en 21% btw ingeeft, verschijnt er

```csharp
Geef een bedrag excl. BTW (2 cijfers na de komma) : 123.45
Geef de korting (in %) : 10.25
Geef het BTW-tarief (in %) : 21

excl. BTW : ..123.45 // (1)
  korting : ...12.65 // (1)
      BTW : ...23.27 // (1)
incl. BTW : ..134.07 // (1)
```

1.  In dit document worden hier puntjes `.` getoond i.p.v. spaties zodat je het aantal spaties kunt tellen voor het rechts uitlijnen van de getallen. Je programma toont natuurlijk spaties, geen puntjes.

Naargelang je taalinstellingen kan het zijn dat je de getallen met een komma moet schrijven (bv. `123,45` i.p.v. `123.45`).

> **Waarschuwing**
>
> LET OP : als je programma voor `incl. BTW` op `134,06` uitkomt klopt er iets niet!

## DateTime
### Oefening D12tonenmetonderdelen
Schrijf een programma dat de huidige datum toont, gebruik hierbij de verschillende onderdelen van een `DateTime` waarde opvraagt zoals `.Month` en `.Hour`. Het aantal seconden en fractie van een seconde laat je achterwege.

Bijvoorbeeld als het vandaag 12 november 2019 is om 10:49:50,567 toont het programma

```csharp
De datum van vandaag is 12/11/2019 en het is nu 10u49.
```

### Oefening D12tonenmetformaatstring
Dit is een (betere) variant van de vorige oefening.

Schrijf weer een programma dat de huidige datum toont, maar gebruik dit keer de `ToString()` method om de tekst op te bouwen a.d.h.v. een formaat string. Dit is natuurlijk een veel makkelijkere manier om de tekstvoorstelling te bekomen dan de individuele onderdelen aaneen te plakken!

Het aantal seconden en fractie van een seconde laat je weerom achterwege.

Bijvoorbeeld als het vandaag 12 november 2019 is om 10:49:50,567 toont het programma

```csharp
De datum van vandaag is 12/11/2019 en het is nu 10u49.
```

### Oefening D12seizoen
Schrijf een programma dat de gebruiker om een datum vraagt en aangeeft in welk weerkundig(!) seizoen deze datum valt.

Ter info : elk jaar begint de de lente op 01/03, de zomer op 01/06, de herfst op 01/09 en de winter op 01/12.

Enkele mogelijke uitvoeringen :

```csharp
Geef een datum : 09/04
Lente
```

```csharp
Geef een datum : 01/07
Zomer
```

```csharp
Geef een datum : 01/02
Winter
```

### Oefening D12verjaardagen
Schrijf een programma dat de gebruiker om 10 geboortedata vraagt en toont hoeveel verjaardagen er elke maand zijn.

Je mag ervan uitgaan dat de gebruiker altijd een correct datum invoert. Maanden zonder verjaardagen worden niet getoond.

Een mogelijke uitvoering :

```csharp
Geef een geboortedatum : 23/12/1997
... (stuk niet getoond)
Geef een geboortedatum : 12/01/1993
In maand 1, 2 verjaardag(en)
In maand 4, 6 verjaardag(en)
In maand 7, 4 verjaardag(en)
In maand 12, 1 verjaardag(en)
```

### Oefening D12bertbeverzondertimespan
Schrijf een programma dat de gebruiker vraagt om zo snel mogelijk 2x na elkaar op de dezelfde toets te duwen.

Het programma toont hoeveel milliseconden er tussen de 2 toetsaanslagen zaten.

Zorg ervoor dat je programma ook werkt indien de eerste net voor en de tweede net na middernacht gebeuren (tip : denk aan `.Ticks`).

We zullen deze oefening verderop met `TimeSpan` oplossen, doe het deze keer zonder.

Een mogelijke uitvoering waarbij de gebruiker 2x op dezelfde toets drukte :

```csharp
Druk 2x na elkaar op dezelfde toets, zo snel mogelijk..
De tijd ertussen bedroeg 87ms
```

Een mogelijke uitvoering waarbij de gebruiker op verschillende toetsen drukte :

```csharp
Druk 2x na elkaar op dezelfde toets, zo snel mogelijk..
Dat waren 2 verschillende toetsen
```

### Oefening D12feestdagen
Schrijf een programma dat de gebruiker om een datum vraagt en aangeeft of die datum dit jaar een feestdag is.

-   indien het een feestdag is, wordt de naam van de feestdag getoond tussen aanhalingstekens

-   indien het geen feesdag is toont het programma `Dat is geen feestdag`

-   indien het geen geldige datum is, toont het programma `Ongeldige datum`

Baseer je hiervoor op [de lijst met feestdagen](https://www.wettelijke-feestdagen.be/) en werk met arrays (dus geen ellenlange if/elseif structuur!)

**Het programma vraagt om een datum zonder jaartal** en zowel het dag- als het maandgedeelte moet uit 2 cijfers bestaan.

Enkele mogelijke uitvoeringen :

```csharp
Geef een datum : 25/12
Dat is "Kerstmis" (1)
```

1.  let op de aanhalingstekens in de output!

```csharp
Geef een datum : 39/65
Ongeldige datum
```

```csharp
Geef een datum : 05/02
Dat is geen feestdag
```

```csharp
Geef een datum : 01/05
Dat is "Dag van de Arbeid" (1)
```

1.  let op de aanhalingstekens in de output!

### Oefening D12leeftijdinjaren
Schrijf een programma dat de geboortedatum van de gebruiker vraagt en vervolgens de huidige datum en de leeftijd van de gebruiker toont.

Een mogelijke uitvoering op 12 november 2020 :

```csharp
Geef uw geboortedatum (dd/mm/jjjj) : 23/11/2000
Vandaag is het 12/11/2020, dus u bent 19 jaar oud
----
```

Als je klaar bent, kijk eens op

-   https://stackoverflow.com/questions/9/in-c-how-do-i-calculate-someones-age-based-on-a-datetime-type-birthday

om te zien waar je zoal rekening mee had kunnen/moeten houden ;)

### Oefening D12periodeoverlapt
Schrijf de methods `DatumVanaf`, `Overlapt` en `PeriodeLabel`.

Method `DatumVanaf` zal de gebruiker om een datum vragen en deze `DateTime` opleveren. De gebruiker zal steeds opnieuw een datum moeten opgeven zolang deze niet gelijk is aan, of volgt op de meegegeven *minimum datum*. Bij de call `DatumVanaf(datum1, "einddatum van periode 1")` bijvoorbeeld is `datum1` deze *minimum datum*. Hier zal de gebruiker dus een datum gelijk aan `datum1`, of volgend op `datum1` moeten opgeven.

Method `Overlapt` krijgt vier datums mee (vier `DateTime` parameters). Per twee vormen ze een *periode*. De eerste is de start van deze *periode*, de tweede het einde. Start en einde maken beide deel uit van deze *periode*. Bij een call als `Overlapt(datum1, datum2, datum3, datum4)` loopt de *eerste periode* vanaf `datum1` tot en met `datum2`, de *tweede periode* vanaf `datum3` tot en met `datum4`. Je weet niet welke *periode*, de *eerste* of de *tweede*, vroeger start. De *tweede periode* zou net zo goed vroeger kunnen starten dan de *eerste*. De `Overlapt` method antwoordt in `bool` vorm op de vraag of de *eerste periode* en *tweede periode* overlappen. Indien de startdatum van de *laatste periode* ligt tussen de start- en einddatum van de *vroegste periode* overlappen ze.

Method `PeriodeLabel` genereert een eenvoudig `string` label die een *periode*, afgebakend door de twee meegegeven datums, voorstelt. Bijvoorbeeld *20/11/2022 - 18/12/2022* of *24/12/2022 - 08/01/2023*. Je kan hier, voor de omzetting van `DateTime` naar `string`, de *formatstring "dd/MM/yyyy"* inzetten.

Vul volgende code aan…​

```csharp
    class Program
    {
        static void Main(string[] args)
        {
            // Opvragen start- en einddatum van periode 1...
            DateTime datum1 = Datum("startdatum van periode 1");
            DateTime datum2 = DatumVanaf(datum1, "einddatum van periode 1");

            // Opvragen start- en einddatum van periode 2...
            DateTime datum3 = Datum("startdatum van periode 2");
            DateTime datum4 = DatumVanaf(datum3, "einddatum van periode 2");

            // Controleren of deze periodes overlappen...
            string overlappen = "overlappen";
            if (!Overlapt(datum1, datum2, datum3, datum4)) overlappen += " niet";

            // Afprinten van de output...
            Console.Write($"Periode 1 ({PeriodeLabel(datum1, datum2)}) en periode 2 ({PeriodeLabel(datum3, datum4)}) {overlappen}.");
        }

        static DateTime Datum(string omschrijving)
        {
            return DatumVanaf(DateTime.MinValue, omschrijving);  // (4)
        }

        // (1)

    // (2)

    // (3)
    }
```

1.  Vul hier aan met method `DatumVanaf`.

2.  Vul hier aan met method `Overlapt`.

3.  Vul hier aan met method `PeriodeLabel`.

4.  Method `Datum` zal net als `DatumVanaf` een `DateTime` value opleveren die ten vroegste een bepaalde "minimum datum" zal zijn. In dit geval wordt met `DateTime.MinValue` als argumentwaarde gewerkt bij de call naar `DatumVanaf`. `DateTime.MinValue` zal de vroegst mogelijk datum reprensenteerbaar aan de hand van het `DateTime` opleveren.

De uitvoer is…​

```csharp
Geef startdatum van periode 1 (dd/mm/jjjj) : 24/12/2022
Geef einddatum van periode 1 (dd/mm/jjjj) : 08/01/2023
Geef startdatum van periode 2 (dd/mm/jjjj) : 20/11/2022
Geef einddatum van periode 2 (dd/mm/jjjj) : 18/12/2022
Periode 1 (24/12/2022 - 08/01/2023) en periode 2 (20/11/2022 - 18/12/2022) overlappen niet.
```

Hierbij is nagegaan of de kerstvakantie van 2022 (periode 1) *overlapt met* het WK voetbal 2022 (periode 2).

Zelfs al zou hier het *WK* lopen tot en met *24/12/2022* dan nog is er geen sprake van overlap. Dat zou wel het geval zijn bij einddatum *25/12/2022*. Test het ook eens uit met die datums.

Zolang de gebruiker een einddatum opgeeft die voor de startdatum (*minimum datum*) valt, krijgt hij een foutmelding te zien, en moet hij een nieuw datum opgeven…​

```csharp
Geef startdatum van periode 1 (dd/mm/jjjj) : 24/12/2022
Geef einddatum van periode 1 (dd/mm/jjjj) : 08/01/2023
Geef startdatum van periode 2 (dd/mm/jjjj) : 20/11/2022
Geef einddatum van periode 2 (dd/mm/jjjj) : 18/12/2020
De datum moet minstens 20/11/2022 zijn (of verder).
Geef einddatum van periode 2 (dd/mm/jjjj) : 18/12/2021
De datum moet minstens 20/11/2022 zijn (of verder).
Geef einddatum van periode 2 (dd/mm/jjjj) : 18/12/2022
Periode 1 (24/12/2022 - 08/01/2023) en periode 2 (20/11/2022 - 18/12/2022) overlappen niet.
```

## TimeSpan
### Oefening D12bertbevermettimespan
Neem [de oplossing van D12bertbeverzondertimespan](../deel-12-oplossingen/deel-12-oplossingen.html#_oplossing_d12bertbeverzondertimespan) erbij. Dit was de oefening waarin de tijd gemeten werd tussen twee toetsdrukken van de gebruiker.

Herwerk de oplossing en probeer de `TimeSpan` mogelijkheden optimaal te benutten.

### Oefening D12leeftijdindagen
Schrijf een programma dat de geboortedatum van de gebruiker vraagt en toont hoeveel dagen oud de gebruiker is.

Je kan het eerste gedeelte van [de oplossing van D12leeftijdinjaren](../deel-12-oplossingen/deel-12-oplossingen.html#_oplossing_d12leeftijdinjaren) overnemen, zodat je enkel nog de berekening moet schrijven. Gebruik hierbij een `TimeSpan` waarde.

De uitvoering op 14/11/2020 als de gebruiker `23/11/2000` invoert :

```csharp
Geef uw geboortedatum (dd/mm/jjjj) : 23/11/2000
U bent 7296 dagen oud
```

### Oefening D12gethuurbedrag
Schrijf een method `GetHuurBedrag` die de huurprijs van een gehuurd apparaat kan berekenen :

```csharp
static double GetHuurBedrag(DateTime afgehaald, DateTime teruggebracht, double dagPrijs) { ... }
```

De method heeft 2 parameters die de momenten aanduiden waarop het apparaat werd afgehaald en teruggebracht, alsook een parameter die de prijs per gehuurde dag voorstelt.

De method berekent het aantal dagen tussen die 2 momenten en vermenigvuldigt dit met de prijs per dag. Elke begonnen "dag" moet **volledig** betaald worden, er worden geen stukjes van dagen verrekend. Je mag ervan uitgaan dat het `teruggebracht` moment altijd NA het `afgehaald` moment ligt.

Schrijf een `Main` method waarin je de return value van de volgende oproepen op de console zet :

```csharp
GetHuurBedrag( new DateTime(2022, 11, 20, 16, 45, 00), new DateTime(2022, 11, 20, 17, 15, 00), 10 ); (1)
GetHuurBedrag( new DateTime(2022, 11, 20, 16, 45, 00), new DateTime(2022, 11, 23, 16, 15, 00), 10 ); (2)
GetHuurBedrag( new DateTime(2022, 11, 20, 16, 45, 00), new DateTime(2022, 11, 23, 17, 15, 00), 10 ); (3)
```

1.  resultaat is 10

2.  resultaat is 30

3.  resultaat is 40

## Arrays en methods
### Oefening D12getresultaat
Schrijf een method `GetResultaat` die de uitslag van een partij spelletjes van twee spelers kan bepalen.

De method heeft 2 parameters die de scores van de twee spelers voorstellen en een return value die het resultaat van de partij aangeeft :

```csharp
static int GetResultaat(int[] scoresSpeler1, int[] scoresSpeler2) { ... }
```

Je mag ervan uitgaan dat de beide arrays steeds evenveel elementen bevatten. Elk array bevat de scores van een speler voor elk van de gespeelde spelletjes.

Per gespeeld spel, wint de hoogste score. De method moet dus tellen hoe vaak speler 1 won en hoe vaak speler 2 won (bij gelijke score wint er niemand).

De return value geeft aan wie er won, en wel als volgt :

-   return value is `< 0` : speler 1 heeft de meeste spelletjes gewonnen

-   return value is `> 0` : speler 2 heeft de meeste spelletjes gewonnen

-   return value is `0` : er is geen winnaar

Je mag zelf kiezen welke waarden je method retourneert. Indien bv. speler 1 wint mag de return value `-1` zijn, of `-100`, of `-33`, .. zolang de return value maar `< 0` is.

Je kunt je method uitproberen met de volgende code in de `Main` method :

```csharp
int[] scoresA = {3, 7, 10};
int[] scoresB = {4, 6, 9};
int[] leeg = {};

Console.WriteLine( GetResultaat(scoresA, scoresB) ); // toont negatief getal want speler 1 wint
Console.WriteLine( GetResultaat(scoresB, scoresA) ); // toont positief getal want speler 2 wint
Console.WriteLine( GetResultaat(scoresA, scoresA) ); // toont zero want gelijkspel
Console.WriteLine( GetResultaat(leeg, leeg)       ); // toont zero want gelijkspel
```

### Oefening D12doorsnede
Schrijf een method `Doorsnede` die een array kan opleveren opgevuld met alle waardes die in twee andere arrays met getallen voorkomen.

Deze method krijgt *twee parameters*, meer specifiek de twee arrays waarvan de **doorsnede** wordt bepaald.

Je moet de code voor `Doorsnede` zelf schrijven, je mag hiervoor geen ingebouwde methods gebruiken die min of meer hetzelfde doen. Je mag ook enkel gebruik maken van arrays, niet van overige *collectietypes*. Later gaan we inderdaad zien hoe dergelijke functionaliteit reeds vervat zit in voorgedefinieerde logica.

Maak ook een method `ToonDoorsnede` die je zoals in onderstaande code geïllustreerd, kan inzetten…​

```csharp
class Program
{
    static void Main()
    {
        double[] temperaturenMeetpunt1 = { 10.1, 20.2, 15.5, 12.3, 28.7 };
        double[] temperaturenMeetpunt2 = { 10.0, 20.2, 15.6, 12.3, 28.8, 11.1 };

        double[] doorsnede = Doorsnede(temperaturenMeetpunt1, temperaturenMeetpunt2);
        ToonDoorsnede(doorsnede);                       // toont de tekst "20,2 | 12,3"

        double[] getallen1 = { 1.23, 2.34, 3.45 };
        double[] getallen2 = { 1.99, 2.34 };
        ToonDoorsnede(Doorsnede(getallen1, getallen2)); // toont de tekst "2,34"

        double[] getallen3 = { 1.99, 2.99, 3.99 };
        ToonDoorsnede(Doorsnede(getallen1, getallen3)); // toont de tekst "geen doorsnede"
    }

    ...  // (1)

    ...  // (2)
}
```

1.  Vul hier aan met method `ToonDoorsnede`.

2.  Vul hier aan met method `Doorsnede`.

De uitvoer is…​

```csharp
Doorsnede: 20,2 | 12,3
Doorsnede: 2,34
geen doorsnede
```

### Oefening D12frequenties
Maak een console applicatie dat **de gebruiker om *10 getallen* vraagt**.

Alle ongeldige input (tekst die niet als `int` te interpreteren valt) worden genegeerd.

Druk na de invoer in het programma de **som** en het **gemiddelde** van alle getallen af. Druk ook af **hoe vaak elk getal werd ingevoerd**.

Maak gebruik van arrays en methods daar waar je zelf nuttig vindt.

Bij de invoer van de waardes *123*, *45*, *45*, *test*, *89*, *45*, *789*, *789*, *789*, *789* en *123* bekomen we…​

```csharp
Getal 1?: 123
Getal 2?:
Getal 2?: 45
Getal 3?: 45
Getal 4?: test
Getal 4?: 89
Getal 5?: 45
Getal 6?: 789
Getal 7?: 789
Getal 8?: 789
Getal 9?: 789
Getal 10?: 123

Som: 3626
Gemiddelde: 362
Frequenties:
  123 komt 2 voor
  45 komt 3 voor
  89 komt 1 voor
  789 komt 4 voor
```

### ★ Oefening D12gemeenteraad
Elke gemeente verkies bij de gemeenteraadsverkiezing een aantal raadsleden. Elk raadslid krijgt een *zetel* in de gemeenteraad.

**Totaal aantal raadsleden:**

Creëer een method `Raadsleden`. De method wordt gebruikt om voor een bepaald *aantal inwoners* het correct *aantal raadsleden* te bevragen.

Het aantal raadsleden (of dus zetels) is afhankelijk van het aantal inwoners. Er zijn minstens *7 raadsleden*. Vanaf *1'000 inwoners* zijn dit er *9*, vanaf *2'000* zijn het er *11*, …​, vanaf *300'000 inwoners* zijn dit er *55*.

De implementatie van deze method mag alvast gebruik maken van volgende arrays…​

```csharp
int[] inwonersAantallen = { 1000, 2000, 3000, 4000, 5000, 7000, 9000, 12000, 15000,
                            20000, 25000, 30000, 35000, 40000, 50000, 60000, 70000,
                            80000, 90000, 100000, 150000, 200000, 250000, 300000 };
int[] raadsledenAantallen = { 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33, 35,
                              37, 39, 41, 43, 45, 47, 49, 51, 53, 55 };
```

`inwonersAantallen` en `raadsledenAantallen` zijn *parallelle arrays*.

Ter herinnering: Waardes die in parallelle arrays op eenzelfde positie staan, horen bij elkaar. Op *index 2* van `inwonersAantallen` vinden we waarde *3000*, dit is het minimum *inwoners aantal* die vereist zijn om *13 raadsleden* (de waarde op *index 2* in `raadsledenAantallen`) aan te duiden.

**Aantal raadsleden per lijst:**

Daarnaast willen we ook bepalen hoeveel raadsleden/zetels elke *lijst* (*politieke partij*) krijgt toegewezen.

Om te bepalen hoeveel zetels elke lijst krijgt, kan je telkens op basis van het hoogste *quotient* (behorende tot een bepaalde lijst) een zetel toewijzen. De *stemcijfers* worden eerst door 1 gedeeld, vervolgens door 2, dan 3, enzovoort…​ .

In onderstaand overzicht krijg je een beeld hoe dit kan worden bepaald…​

| Index | `lijsten` | `stemcijfers` | Zetel 1                               | Zetel 2                               | Zetel 3                               | Zetel 4                               | Zetel 5                               | Zetel 6                                 | Zetel 7                               | `zetels` |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| 0     | Groen     | 60            | 60 (1) | 30 (2)                                | 30 (2) | 20 (3)                                | 20 (3) | 15 (4)                                  | 15 (4) | 4        |
| 1     | Open Vld  | 30            | 30 (1)                                | 30 (1)                                | 30 (1)                                | 30 (1) | 15 (2)                                | 15 (2)                                  | 15 (2)                                | 1        |
| 2     | N-VA      | 31            | 31 (1)                                | 31 (1) | 15,5 (2)                              | 15,5 (2)                              | 15,5 (2)                              | 15,5 (2) | 10,33 (3)                             | 2        |
| 3     | sp.a      | 4             | 4 (1)                                 | 4 (1)                                 | 4 (1)                                 | 4 (1)                                 | 4 (1)                                 | 4 (1)                                   | 4 (1)                                 | 0        |

Voor het toewijzen van elke zetel (*Zetel 1*, *Zetel 2*, …​) wordt gezocht naar het hoogste stemquotient (de hoogste waarde in die kolom). Het hoogste stemquotient werd onderlijnd. De lijst met het hoogste stemquotient bekomt de zetel.

Tussen haakjes zie je het gebruikte deler. Nadat een zetel aan een bepaalde lijst is toegewezen, wordt voor die lijst de deler verhoogd.

Laat opvallen hoe de *derde* en de *zevende* zetel naar *Groen* gaat, en niet *Open Vld*. Ondanks het gelijke stemquotient krijgt *Groen* de zetels toegewezen vanwege het hogere stemcijfer.

Creëer zelf de `Zetels` method die op basis van het aantal `raadleden`, de `lijsten` (de groene kolom), en de `stemcijfers` (de blauwe kolom) een array van *zetel aantallen* (de roze kolom) kan opleveren.

`lijsten`, `stemcijfers` en `zetels` zijn parallelle arrays. Op *index 2* bijvoorbeeld zien we in de arrays hoe *N-VA*, met hun behaalde stemcijfer *31* aan *2* zetels komt.

Het is mogelijk dat je in de implementatie van de `Zetels` method nog extra (parallelle) arrays kan gebruiken.

**Meegegeven `Main` en `Print` methods:**

Je uitgeschreven `Raadsleden` en `Zetels` method moet je kunnen inpassen in volgende code…​

```csharp
class Program {
    static void Main() {
        // Voor fictieve gemeente X:
        int inwonersGemeente = 125;
        int[] lijstNummers = { 1, 2, 3, 4 };
        string[] lijsten = { "Groen", "Open Vld", "N-VA", "sp.a" };
        int[] stemcijfers = { 60, 30, 31, 4 };

        // Voor Gent (gemeenteraadsverkiezingen 2018): // (3)
        // (vervang bovenstaande regels door onderstaande om voor deze gemeente uit te testen)
        /*
        int inwonersGemeente = 259570;
        int[] lijstNummers = { 1, 2, 3, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14 };
        string[] lijsten = { "spa.a-Groen", "N-VA", "CD&V", "VLAAMS BELANG", "Open Vld", "PVDA", "DUW.GENT", "MRP", "SPIEGEL Partij", "BE-ONE", "GENTSE BURGERS", "VMC", "PISS-OFF" };
        int[] stemcijfers = { 53179, 19167, 13979, 12354, 39879, 11178, 3229, 498, 329, 1709, 1633, 480, 931 };
        */

        // Vraag het aantal raadsleden voor de gemeenteraad op:
        int raadsleden = Raadsleden(inwonersGemeente);

        // Vraag het aantal zetels (voor elke lijst) op:
        int[] zetels = Zetels(raadsleden, lijsten, stemcijfers);

        Print(lijstNummers, lijsten, stemcijfers, zetels);
    }

    static void Print(int[] lijstNummers, string[] lijsten, int[] stemcijfers, int[] zetels) {
        for (int i = 0; i < lijstNummers.Length; i++) {
            Console.WriteLine($"{lijstNummers[i],2}: {lijsten[i],15:d}: {zetels[i],2:d} zetels: {stemcijfers[i],6:d} stemmen");
        }
    }

    ... // (1)

    ... // (2)
}
```

1.  Plaats hier de definitie van de `Raadsleden` method.

2.  Plaats hier de definitie van de `Zetels` method.

3.  Indien je voor *Gent* wil uittesten, vervang je `inwonersGemeente`, `lijstNummers`, `lijsten` en `stemcijfers` van *gemeente X* door deze van *Gent*.

De `Print` method levert voor *gemeente X* volgende uitvoer op:

```csharp
 1:           Groen:  4 zetels:     60 stemmen
 2:        Open Vld:  1 zetels:     30 stemmen
 3:            N-VA:  2 zetels:     31 stemmen
 4:            sp.a:  0 zetels:      4 stemmen
```

Voor *Gent* zal de `Print` method het volgende weergeven:

```csharp
 1:     spa.a-Groen: 19 zetels:  53179 stemmen
 2:            N-VA:  6 zetels:  19167 stemmen
 3:            CD&V:  5 zetels:  13979 stemmen
 5:   VLAAMS BELANG:  4 zetels:  12354 stemmen
 6:        Open Vld: 14 zetels:  39879 stemmen
 7:            PVDA:  4 zetels:  11178 stemmen
 8:        DUW.GENT:  1 zetels:   3229 stemmen
 9:             MRP:  0 zetels:    498 stemmen
10:  SPIEGEL Partij:  0 zetels:    329 stemmen
11:          BE-ONE:  0 zetels:   1709 stemmen
12:  GENTSE BURGERS:  0 zetels:   1633 stemmen
13:             VMC:  0 zetels:    480 stemmen
14:        PISS-OFF:  0 zetels:    931 stemmen
```

Test het ook nog eens voor *je eigen gemeente*, de resultaten kan je hier controleren: https://vlaanderenkiest.be/verkiezingen2018/

## Console invoer en methods
### ★ Oefening D12morsebeep
Maak een programma dat meteen van ingetoetste karakers (letter *a* tot en met *z* worden aanvaard) de ***morse code*** gaat beepen.

Om in de console een *beep* af te spelen kan je gebruik maken van de voorgedefinieerde method `Beep`, probeer volgend stukje code eens uit…​

```csharp
const int frequentie = 750;

Console.Beep(frequentie, 500);       // (1)
System.Threading.Thread.Sleep(250);  // (2)
Console.Beep(frequentie, 1000);      // (3)
```

1.  Zal een korte *beep* (van *500 milliseonden*) afspelen.

2.  Na een kleine pauze (van *250 milliseonden*) wordt…​

3.  een lange *beep* (van *1000 milliseonden*) afgespeeld.

Als alles goed is hoor je eerst een *korte beep*, vrij snel gevolgd door een *langere beep*.

> **Opmerking: Ik hoor geen beep**
>
> Controleer eens of je volume wel open staat ;)
>
> De kans is klein, maar sommige hardware configuraties zullen bij sommige *Windows* versie geen geluid produceren. Indien dat het geval is kan je deze oefening ombuigen naar eentje die de tekst *"lang"* of *"kort"* gaat afdrukken op het moment dat een *lange* of *korte beep* wordt verwacht. Eventueel met een *lange* of *korte* pause tussen het afdrukken van deze woorden.

Splits de programmalogica af in verschillende methods:

1.  Een `PlayBeep` method gaat op basis van een bepaalde morse combinatie (bijvoorbeeld `"-."` of `"..-."`) de verschillende morse tekens (`.` of `-`) in een *beep* omzetten.

    ```csharp
    string morse = "-.";
    PlayBeep(morse);   // (1)
    PlayBeep("..-.");  // (2)
    ```

    1.  Speelt een *lange* en *korte beep* af.

    2.  Speelt een *korte*, *korte*, *lange* en *korte beep* af.

2.  De `Morse` method levert op basis van een karakter een tekst op bestaande uit de combinatie van *morse tekens*. Bijvoorbeeld…​

    ```csharp
    char letter = 'h';
    string morse = Morse(letter);
    Console.Write(morse);            // (1)
    Console.Write(Morse('m'));       // (2)
    ```

    1.  Levert `..-.` op.

    2.  Levert `-.` op.

        Maak in deze method alvast gebruik van volgende (parallelle) arrays…​

        ```csharp
        string[] morse = { ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.." };
        char[] letters = { 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z' };
        ```

3.  De `Main` method gaat oneindig lang (`while (true) { …​ }`), vanaf er een toets wordt ingedrukt (`if (Console.KeyAvailable) { …​ }`), indien het gaat om een letter uit het alfabet, zorgen voor afspelen van de juist *beeps*.

    Controleren of het gaat om een letter uit het alfabet, kan door gebruik te maken van de `Key` property van `ConsoleKeyInfo` object. Deze property levert een `ConsoleKey` enumeratiewaarde op.

    Waardes vanaf *a* (`>= ConsoleKey.A`) tot en met *z* (`<= ConsoleKey.Z`) worden aanvaard.

    ```csharp
    ConsoleKeyInfo cki = Console.ReadKey();
    if (cki.Key >= ConsoleKey.A && cki.Key <= ConsoleKey.Z) {
        ...
    }
    ```

    Het afspelen van de verwachte *beeps* gebeurt uiteraard door op gepaste wijze gebruik te maken van de `Morse` en `PlayBeep` methods.
